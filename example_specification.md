# Course Management System (CourseFlow) — EXAMPLE Project Specification

>[!WARNING]
> **ACADEMIC INTEGRITY NOTICE:**
> This example document was AI-generated and reviewed by the instructor and course staff solely as a formatting and structural template. The technical content may contain errors, inconsistencies, or nonsensical design choices because AI cannot reason about system architecture the way engineers do. **YOUR PROJECT PROPOSAL MUST BE YOUR OWN WORK.** This means:
>
> - DO NOT use AI tools to generate your system design, database schema, or technical specifications
> - DO NOT trust AI-generated architecture - it frequently produces designs that look plausible but are technically flawed, poorly integrated, or unnecessarily complex
> - DO use your team's collective engineering judgment to design a system that makes sense for your specific application
> - DO think critically about the relationships between your features, the structure of your database, and the requirements of your users

AI-generated content is easily identifiable and will be treated as academic dishonesty. We expect thoughtful, team-created designs that demonstrate genuine understanding of web application architecture, not generic templates filled with buzzwords.

Your proposal must reflect original thinking, deliberate design choices, and your team's reasoned approach to solving your application's specific requirements. If you cannot explain and defend every technical decision in your specification, it is not ready for submission.

## Table of Contents (`docs/project_specification/README.md`)
The links below should link to your various files (subdocuments) and are provided as an example.

* [1.0 Project Overview](project_overview.md)
* [2.0 Core Requirements](core_requirements.md)
  * [2.1 Language / Framework](core_requirements.md#21-language--framework)
  * [2.2 Role-Based Access Control (RBAC)](core_requirements.md#22-role-based-access-control-rbac)
  * [2.3 Non-Trivial External API Integration](core_requirements.md#23-non-trivial-external-api-integration)
  * [2.4 Persistent Data Storage & Migrations](core_requirements.md#24-persistent-data-storage--migrations)
  * [2.5 Modern Authentication & Security](core_requirements.md#25-modern-authentication--security)
  * [2.6 Documented RESTful API](core_requirements.md#26-documented-restful-api)
  * [2.7 Subscription Tiers & Feature Toggling](core_requirements.md#27-subscription-tiers--feature-toggling)
  * [2.8 Usage Tracking or Auditing](core_requirements.md#28-usage-tracking-or-auditing)
* [3.0 Technical Stack](technical_stack.md)
* [4.0 Comprehensive Feature List](feature_list.md)

# 1.0 Project Overview 
**File: [`docs/project_specification/project_overview.md`]**

**CourseFlow** is a modern SaaS-based Course Management System (CMS) designed for educational institutions, independent instructors, and online bootcamps. The system enables instructors to create, organize, and manage course materials (modules, assignments, video lessons, and quizzes) while providing students with an intuitive interface to enroll, complete coursework, receive automated feedback, and track academic progress.

### Target Users
1. **Platform Administrators:** Manage global platform configurations, subscription tiers, institutional onboarding, and overall audit logs.
2. **Instructors / Teaching Assistants:** Create and publish courses, manage student enrollments, issue assignments, and leverage AI-assisted grading feedback.
3. **Students:** Browse available course catalogs, enroll in courses, submit assignments, take automated quizzes, and view progress dashboards.

### System Scope & Technical Context
CourseFlow is built as a multi-tenant web application using Ruby on Rails for the backend REST API and server-rendered HTML views, with support for third-party mobile clients. The platform incorporates hierarchical Role-Based Access Control (RBAC), multi-tier subscriptions (Free, Pro, Enterprise), persistent relational data modeling with Rails migrations, secure JWT and OAuth 2.0 authentication, and structured audit logging. Additionally, CourseFlow integrates two-way asynchronous LLM API services to offer automated assignment grading recommendations and interactive AI tutoring.

---
# 2.0 Core Requirements
**File: [`docs/project_specification/core_requirements.md`]**

### 2.1 Language / Framework
* **Backend:** Built using **Ruby on Rails 7.1+** operating in API-centric mode, serving JSON endpoints for all core application workflows while providing Hotwire-enabled (Turbo/Stimulus) HTML views for standard web access.
* **Frontend Interfaces:**
  * **Web Client:** Server-rendered HTML templates dynamicized with Tailwind CSS and Hotwire to allow reactive UI updates without full page reloads.
  * **Mobile / External Clients:** A decoupled native mobile client (iOS/Android) or single-page app (SPA) interacts exclusively with the backend via secure REST API endpoints exposed under `/api/v1/`.

### 2.2 Role-Based Access Control (RBAC)
CourseFlow uses a 3-tier hierarchical role-based authorization strategy (enforced server-side via the `pundit` gem):

| Role | Hierarchy Level | Capabilities & Access Permissions | Restrictions |
| :--- | :--- | :--- | :--- |
| **Platform Admin** | Level 3 (Highest) | Full system access. Can modify global platform settings, manage institutional accounts, view system-wide audit logs, reassign user roles, and manage billing tiers. | None. |
| **Instructor / TA** | Level 2 | Can create, edit, and archive courses; manage assignments, quizzes, and gradebooks; invite students; trigger automated AI grading tools; and export course usage reports. | Cannot alter system configurations, view platform audit logs outside their assigned courses, or modify user roles. |
| **Student** | Level 1 | Can view enrolled course content, submit coursework, participate in discussion forums, access AI tutoring endpoints, and view personal grades. | Cannot view unpublished course content, access gradebooks of other students, or create/edit course materials. |

### 2.3 Non-Trivial External API Integration
CourseFlow integrates directly with the **OpenAI API (GPT-4o)** to provide complex, two-way data processing for automated assignment assessment and interactive feedback:

1. **Submission Processing Pipeline:** When a student submits a text-based essay or coding assignment, an asynchronous background job (`ActiveJob` via `Sidekiq`) constructs a structured prompt combining the assignment rubric, student response, and historical context.
2. **Two-Way Processing:** The job dispatches an asynchronous API call to OpenAI's structured outputs endpoint.
3. **Data Parsing & Actions:** Upon receiving the JSON response, Rails parses the feedback score and breakdown, persists the generated suggestions into the `assignment_submissions` record, alerts the instructor via webhooks/notifications for review, and updates the student's learning analytics.

### 2.4 Persistent Data Storage & Migrations
* **Database Engine:** PostgreSQL 15+ is used as the relational database engine.
* **Schema Design:** Key entities include `Users`, `Roles`, `Courses`, `Enrollments`, `Modules`, `Assignments`, `Submissions`, `Subscriptions`, and `AuditLogs`.
* **Migration Strategy:** All database structure modifications are controlled through versioned Rails database migrations (`db/migrate/`). Structural consistency across development, staging, and production environments is enforced via `db/schema.rb` and validated in the continuous integration (CI) pipeline using `bin/rails db:migrate:status`.

### 2.5 Modern Authentication & Security
* **Authentication Mechanics:**
  * **OAuth 2.0 (SSO):** Supports Single Sign-On via Google Workspace and GitHub OAuth providers using the `omniauth` gem.
  * **JWT Authentication:** Native API client access uses JSON Web Tokens signed with HMAC-SHA256 (`jwt` gem), stored securely in `HttpOnly`, `SameSite=Lax`, `Secure` cookies to mitigate Cross-Site Scripting (XSS) and Cross-Site Request Forgery (CSRF).
* **Password Hashing:** Local user accounts use `bcrypt` (via Rails `has_secure_password`) with a configurable work factor (cost factor = 12 in production).
* **Security Safeguards:** API endpoint rate-limiting via `rack-attack`, strong parameter filtering, and automatic SQL-injection prevention using Rails' Active Record ORM.

### 2.6 Documented RESTful API
* **Architecture:** Formally adheres to REST principles using proper HTTP verbs (`GET`, `POST`, `PUT`/`PATCH`, `DELETE`), predictable resource-oriented URLs (e.g., `POST /api/v1/courses/:course_id/assignments`), and standardized HTTP status codes (`200 OK`, `201 Created`, `400 Bad Request`, `401 Unauthorized`, `403 Forbidden`, `404 Not Found`, `422 Unprocessable Entity`).
* **Automated Documentation:** The API is documented using OpenAPI 3.0 via the `rswag` gem. Request/response specs are defined alongside integration tests, automatically generating interactive Swagger UI documentation hosted live at `/api-docs`.

### 2.7 Subscription Tiers & Feature Toggling
CourseFlow enforces three operational tiers using a feature toggling mechanism (`flipper` gem coupled with custom policy objects):

| Tier | Feature Access | Usage Restrictions / Limits |
| :--- | :--- | :--- |
| **Free Tier** | Basic course enrollment, standard assignment submissions, public discussion boards. | Max 2 active courses; no AI automated grading or advanced analytics; capped at 50 MB total file storage. |
| **Pro Tier** | All Free features + AI automated assignment feedback, exportable gradebooks, live quiz creation. | Max 15 active courses; up to 100 AI grading requests per month; 10 GB file storage. |
| **Enterprise Tier** | All Pro features + unlimited courses/storage, dedicated SSO configuration, real-time audit logging, custom reporting, unlimited AI usage. | Unlimited capacity; priority queue execution for background job processing. |

### 2.8 Usage Tracking or Auditing
- Auditing Mechanism: Uses the audited gem to maintain an immutable log table (audit_logs) recording key system events.

- Tracked Events:

| Auditable Type | Example |
| :--- | :--- |
| User authentication events | Logins, failed attempts, password resets |
| RBAC modifications | Changing user roles or administrative permissions |
| Course & grade updates | Creating/deleting courses, overriding grades, modifying rubrics |
| External API consumption | Tracking OpenAI API token counts per course for billing/usage analysis |

Log data structure: Each log entry captures `user_id`, `action`, `auditable_type`, `auditable_id`, `changes_json`, `ip_address`, and `timestamp`.

---
# 3.0 Technical Stack Component Listing, Description, and Rationale
**File: [`docs/project_specification/technical_stack.md`]**

| Component | Technology | Rationale |
|---|---|---|
|Language|???|Offers high developer productivity, clean syntax, and strong ecosystem support for web service development.|
|Web Framework|???|Provides out-of-the-box support for RESTful API conventions, database migrations, security best practices, and background processing.|
|Database|???|???|
|Cache & Queue Store|???|???|
|Authentication|???|???|
|Testing Framework|???|???|
|API Documentation|???|???|
|External API|???|???|
|Deployment / Hosting|???|???|

---

# 4.0 Comprehensive Feature List 
**File: [`docs/project_specification/feature_list.md`]**

|Feature Name|Target User Role|Description|Tier Availability|
|---|---|---|---|
|Automated AI Assignment Grading|Instructor / TA|Evaluates student text or code submissions against assignment rubrics using OpenAI GPT-4o, generating auto-scores and suggestions for instructor review.|Pro, Enterprise|
|Interactive Student Progress Dashboard|Student|Provides real-time visual tracking of completed course modules, quiz scores, pending deadlines, and overall grade performance across all active enrollments.|Free, Pro, Enterprise|
|Interactive Course Builder & Quiz Creator|Instructor / TA|Enables drag-and-drop course module arrangement, video lesson publishing, rich-text assignment creation, and auto-graded multiple-choice quiz setup.|Free (capped at 2 courses), Pro, Enterprise|
|System-Wide Security & Audit Logging|Platform Admin|Real-time tracking and immutable audit trails of sensitive actions, including role reassignments, grade modifications, SSO events, and external API token usage.|Enterprise|
|Etc.|You will have many more rows....|

