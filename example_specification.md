# **Example Project Specification: Course Management System**

---

**ACADEMIC INTEGRITY NOTICE:**
This example document was AI-generated and reviewed by the instructor and course staff solely as a formatting and structural template. The technical content may contain errors, inconsistencies, or nonsensical design choices because AI cannot reason about system architecture the way engineers do. **YOUR PROJECT PROPOSAL MUST BE YOUR OWN WORK.** This means:

- DO NOT use AI tools to generate your system design, database schema, or technical specifications
- DO NOT trust AI-generated architecture - it frequently produces designs that look plausible but are technically flawed, poorly integrated, or unnecessarily complex
- DO use your team's collective engineering judgment to design a system that makes sense for your specific application
- DO think critically about the relationships between your features, the structure of your database, and the requirements of your users

AI-generated content is easily identifiable and will be treated as academic dishonesty. We expect thoughtful, team-created designs that demonstrate genuine understanding of web application architecture, not generic templates filled with buzzwords.

Your proposal must reflect original thinking, deliberate design choices, and your team's reasoned approach to solving your application's specific requirements. If you cannot explain and defend every technical decision in your specification, it is not ready for submission.

---
## **1.0 Project Overview**

The **Course Management System (CMS)** is a web-based platform designed to facilitate online course delivery and management for 
educational institutions. The system serves three primary user groups: instructors who create and manage courses, students who 
enroll in and complete coursework, and administrators who oversee the entire platform.

This multi-feature SaaS application enables instructors to create courses with modules and assignments, students to enroll in courses 
and submit work, and administrators to manage users and monitor platform activity. The system implements role-based access controls 
to ensure appropriate permissions for each user type, maintains persistent storage of course content and user progress, and provides 
RESTful API endpoints for all major operations.

**Key Technical Requirements:**
- Multi-feature architecture with distinct functional areas
- User authentication and authorization with role-based access
- Persistent database storage with normalized schema
- RESTful API design following standard conventions
- Secure handling of user data and submissions

---

## **2.0 Core Requirements**

### **2.1 User-Based System**

**Authentication:**
- Users register with email, password, and role selection (student/instructor)
- Passwords are encrypted using bcrypt before storage
- Users log in with email/password credentials
- Session management using Rails session cookies with secure flags
- Password reset functionality via email token

**Authorization:**
- Role-based access control implemented through User model role attribute
- Before filters in controllers verify user permissions for actions
- Students can only view courses they're enrolled in
- Instructors can only edit courses they created
- Administrators have full access to all resources
- Unauthenticated users can only view the public course catalog

**Security Considerations:**
- Strong password requirements (minimum 8 characters, mixed case, numbers)
- Use of REGEX for validating the password rules
- Protection against SQL injection via ActiveRecord parameterization
- CSRF token validation on all state-changing requests
- Session timeout after 2 hours of inactivity
- Rate limiting on login attempts to prevent brute force attacks

---

### **2.2 User Roles**

**Student Role:**
- **Permissions:**
  - Browse public course catalog
  - Enroll in available courses
  - View enrolled course content (modules, assignments)
  - Submit assignments for enrolled courses
  - View own grades and feedback
  - Update own profile information
- **Restrictions:**
  - Cannot create or edit courses
  - Cannot view other students' submissions or grades
  - Cannot access administrative functions
  - Cannot enroll in courses at capacity

**Instructor Role:**
- **Permissions:**
  - All student permissions
  - Create new courses with syllabi and learning objectives
  - Add/edit/delete modules within own courses
  - Create assignments with due dates and rubrics
  - View all submissions for own courses
  - Grade assignments and provide feedback
  - View enrollment lists and student progress
  - Publish/unpublish courses
- **Restrictions:**
  - Cannot edit courses created by other instructors
  - Cannot access grade submissions for courses they don't teach
  - Cannot delete user accounts
  - Cannot view system-wide analytics

**Administrator Role:**
- **Permissions:**
  - All instructor permissions
  - View all courses and user accounts
  - Create/edit/delete any user account
  - Assign or modify user roles
  - Access system-wide analytics and reports
  - Manage course categories and tags
  - Remove inappropriate content
  - Configure platform settings
- **Restrictions:**
  - Should not directly grade student work (delegated to instructors)

---

### **2.3 Persistent Storage**

**Database Schema (PostgreSQL)**

**Schema Completeness:**

The database consists of 9 normalized tables that eliminate data redundancy and establish clear relationships between entities. Selected, commonly used fields are indexed  for peformant lookups in read-heavy workflows, e.g, index on first name and last name since instructors may frequently look up specific students

1. **users** - Stores user account information
   - Primary key: id
   - Indexed fields: email, first_name, last_name
   - Fields: email (unique), encrypted_password, role (enum: student/instructor/admin), first_name, last_name, created_at, updated_at

2. **courses** - Stores course information
   - Primary key: id
   - Foreign key: instructor_id (references users)
   - Indexed fields: title
   - Fields: title, description, syllabus (text), published (boolean), max_enrollment, created_at, updated_at

3. **enrollments** - Join table for many-to-many relationship between users and courses
   - Primary key: id
   - Foreign keys: user_id (references users), course_id (references courses)
   - Fields: enrolled_at, status (enum: active/dropped/completed)
   - Composite unique index on (user_id, course_id)

4. **modules** - Stores course content modules
   - Primary key: id
   - Foreign key: course_id (references courses)
   - Indexed fields: title
   - Fields: title, description, order_position, created_at, updated_at

5. **assignments** - Stores assignment details
   - Primary key: id
   - Foreign key: module_id (references modules)
   - Fields: title, description, due_date, max_points, created_at, updated_at

6. **submissions** - Stores student assignment submissions
   - Primary key: id
   - Foreign keys: assignment_id (references assignments), user_id (references users)
   - Fields: content (text), file_url, submitted_at, updated_at
   - Composite unique index on (assignment_id, user_id)

7. **grades** - Stores grading information for submissions
   - Primary key: id
   - Foreign keys: submission_id (references submissions), graded_by_id (references users)
   - Fields: points_earned, feedback (text), graded_at

8. **categories** - Stores course categories for organization
   - Primary key: id
   - Fields: name, description

9. **course_categories** - Join table for many-to-many relationship between courses and categories
   - Primary key: id
   - Foreign keys: course_id (references courses), category_id (references categories)
   - Composite unique index on (course_id, category_id)

**Associations:**
- User has_many :courses (as instructor)
- User has_many :enrollments
- User has_many :enrolled_courses, through: :enrollments, source: :course
- Course belongs_to :instructor, class_name: 'User'
- Course has_many :enrollments
- Course has_many :students, through: :enrollments, source: :user
- Course has_many :modules
- Course has_and_belongs_to_many :categories
- Module belongs_to :course
- Module has_many :assignments
- Assignment belongs_to :module
- Assignment has_many :submissions
- Submission belongs_to :assignment
- Submission belongs_to :user
- Submission has_one :grade

**Schema Justification:**

**User Management & Roles:**
The `users` table includes a `role` field that supports the three-tier permission system. This single-table inheritance approach simplifies authentication while the role attribute drives authorization logic throughout the application. All user-initiated actions (course creation, enrollment, grading) reference this table via foreign keys.

**Course Catalog & Enrollment Feature:**
The `courses` and `enrollments` tables implement the course browsing and enrollment features. The many-to-many relationship through `enrollments` allows students to join multiple courses while enabling instructors to have many students. The `published` boolean on courses supports draft/published workflows, and `max_enrollment` enforces capacity limits. The `course_categories` join table enables courses to belong to multiple categories for better organization and filtering in the course catalog.

**Course Content Management Feature:**
The `modules` and `assignments` tables create a hierarchical content structure. Modules belong to courses and contain assignments, supporting the organization of course material into logical units. The `order_position` field in modules allows instructors to sequence content appropriately.

**Assignment Submission & Grading Feature:**
The `submissions` table connects students to assignments with a unique constraint ensuring one submission per student per assignment. The separate `grades` table normalizes grading information, preventing null values in submissions and allowing the grading process to be tracked independently. The `graded_by_id` field maintains an audit trail of who graded each submission.

**Design Rationale:**
- **Separation of submissions and grades:** This prevents mixing ungraded submissions (null values) with graded ones and enables future features like re-grading or multiple graders
- **Enrollment as a separate table:** Rather than a simple join table, enrollments include status and timestamp data, supporting features like tracking when students drop courses
- **Categories join table:** Enables flexible categorization without duplicating course data, supporting future tag-based filtering
- **Foreign key for instructor_id in courses:** While users can have multiple roles, separating the instructor relationship makes queries for "courses taught by X" more efficient and semantically clear

---

### **2.4 Modular Architecture**

**Feature Areas:**

**1. User Authentication & Authorization**
- **Models:** User
- **Controllers:** SessionsController, RegistrationsController, PasswordResetsController
- **Views:** sessions/new (login), registrations/new (signup), registrations/edit (profile), password_resets/new, password_resets/edit
- **Core Functionality:**
  - User registration with email validation
  - Secure login/logout with session management
  - Password encryption and reset functionality
  - Role-based authorization checks
- **User-Facing Capabilities:**
  - Create an account and choose a role
  - Log in and out securely
  - Reset forgotten password via email
  - Update profile information

**2. Course Catalog & Enrollment**
- **Models:** Course, Enrollment, Category, CourseCategory
- **Controllers:** CoursesController (index, show), EnrollmentsController, CategoriesController
- **Views:** courses/index (browse catalog), courses/show (course detail), enrollments/index (my courses)
- **Core Functionality:**
  - Browse published courses with filtering by category
  - View course details, including instructor and syllabus
  - Enroll in available courses with capacity checks
  - Track enrollment status and manage course list
- **User-Facing Capabilities:**
  - Search and filter the course catalog
  - View course descriptions and prerequisites
  - Enroll in or drop courses
  - See the list of enrolled courses

**3. Course Content Management**
- **Models:** Course, Module, Assignment
- **Controllers:** Instructor::CoursesController, Instructor::ModulesController, Instructor::AssignmentsController
- **Views:** instructor/courses/new, instructor/courses/edit, instructor/modules/form, instructor/assignments/form
- **Core Functionality:**
  - Create and edit course structure
  - Organize content into modules
  - Create assignments with due dates and point values
  - Publish/unpublish courses
  - Reorder modules and assignments
- **User-Facing Capabilities:**
  - Instructors build course curricula
  - Define learning modules and sequencing
  - Set assignment requirements and deadlines
  - Control course visibility

**4. Assignment Submission & Grading**
- **Models:** Submission, Grade, Assignment
- **Controllers:** SubmissionsController, GradesController
- **Views:** assignments/show (with submission form), submissions/index (instructor view), grades/edit (grading form)
- **Core Functionality:**
  - Students submit assignment work (text or file upload)
  - Instructors view all submissions for their courses
  - Grading interface with point entry and feedback
  - Grade calculation and display
- **User-Facing Capabilities:**
  - Students submit work before deadlines
  - Students view grades and feedback
  - Instructors grade submissions with rubrics
  - Instructors provide detailed feedback

**5. Administrative Dashboard**
- **Models:** User, Course, Enrollment (read-only analytics)
- **Controllers:** Admin::UsersController, Admin::CoursesController, Admin::AnalyticsController
- **Views:** admin/dashboard (overview), admin/users/index, admin/courses/index, admin/analytics/show
- **Core Functionality:**
  - User account management and role assignment
  - System-wide course oversight
  - Platform analytics and reporting
  - Content moderation
- **User-Facing Capabilities:**
  - Manage all user accounts
  - Override course settings when needed
  - View enrollment and activity metrics
  - Generate platform reports

**Feature Dependencies:**

**Course Catalog & Enrollment depends on User Authentication:**
- Enrollment requires an authenticated user to link the student to the course
- Course browsing is personalized based on the user's role and enrolled courses
- Data flow: User authentication → role verification → enrollment permissions

**Course Content Management depends on User Authentication:**
- Only authenticated instructors can create/edit courses
- Course ownership tied to the instructor's user_id
- Authorization checks verify the instructor owns the course before allowing edits
- Data flow: User login → role check (instructor) → course ownership verification

**Assignment Submission & Grading depends on Course Catalog & Enrollment:**
- Students can only submit assignments in enrolled courses
- Submission validation checks enrollment status before accepting work
- Instructors can only grade submissions for courses they teach
- Data flow: Enrollment record → assignment access → submission creation

**Assignment Submission & Grading depends on Course Content Management:**
- Assignments must exist (created by the instructor) before submissions are possible
- Assignment attributes (due_date, max_points) drive submission validation and grading
- Data flow: Assignment creation → student submission → instructor grading

**Administrative Dashboard depends on all other features:**
- Dashboard displays aggregated data from courses, enrollments, and submissions
- User management affects authentication across all features
- Administrative overrides require reading data from all modules
- Data flow: All feature data → analytics aggregation → dashboard display

**Architectural Justification:**
The dependency structure creates a logical hierarchy: authentication forms the foundation, enabling enrollment, which enables content 
access and submission. This prevents circular dependencies and ensures clean separation of concerns. Each feature area can be 
developed and tested independently while maintaining clear integration points.

---

### **2.5 API Interfaces**

All endpoints follow RESTful conventions and require authentication via session cookie unless noted otherwise.

**User Authentication & Authorization Feature:**

| Method | Endpoint | Parameters | Role Access | Description |
|--------|----------|------------|-------------|-------------|
| POST | /api/v1/register | email, password, password_confirmation, role, first_name, last_name | Public | Create new user account |
| POST | /api/v1/login | email, password | Public | Authenticate and create session |
| DELETE | /api/v1/logout | - | All authenticated | Destroy session |
| GET | /api/v1/users/:id | - | Owner or Admin | Get user profile |
| PATCH | /api/v1/users/:id | email, first_name, last_name | Owner or Admin | Update user profile |
| POST | /api/v1/password_reset | email | Public | Request password reset token |
| PATCH | /api/v1/password_reset | token, password, password_confirmation | Public | Reset password with token |

**Course Catalog & Enrollment Feature:**

| Method | Endpoint | Parameters | Role Access | Description |
|--------|----------|------------|-------------|-------------|
| GET | /api/v1/courses | category_id, search, page | All authenticated | List published courses with filters |
| GET | /api/v1/courses/:id | - | All authenticated | Get course details |
| POST | /api/v1/courses | title, description, syllabus, category_ids | Instructor, Admin | Create new course |
| PATCH | /api/v1/courses/:id | title, description, syllabus, published, max_enrollment | Course owner, Admin | Update course |
| DELETE | /api/v1/courses/:id | - | Course owner, Admin | Delete course |
| GET | /api/v1/enrollments | - | All authenticated | List user's enrollments |
| POST | /api/v1/enrollments | course_id | Student, Instructor | Enroll in course |
| DELETE | /api/v1/enrollments/:id | - | Enrollment owner, Admin | Drop course |
| GET | /api/v1/categories | - | All authenticated | List all categories |

**Course Content Management Feature:**

| Method | Endpoint | Parameters | Role Access | Description |
|--------|----------|------------|-------------|-------------|
| GET | /api/v1/courses/:course_id/modules | - | Enrolled users, Course owner, Admin | List course modules |
| POST | /api/v1/courses/:course_id/modules | title, description, order_position | Course owner, Admin | Create module |
| PATCH | /api/v1/modules/:id | title, description, order_position | Course owner, Admin | Update module |
| DELETE | /api/v1/modules/:id | - | Course owner, Admin | Delete module |
| GET | /api/v1/modules/:module_id/assignments | - | Enrolled users, Course owner, Admin | List module assignments |
| POST | /api/v1/modules/:module_id/assignments | title, description, due_date, max_points | Course owner, Admin | Create assignment |
| PATCH | /api/v1/assignments/:id | title, description, due_date, max_points | Course owner, Admin | Update assignment |
| DELETE | /api/v1/assignments/:id | - | Course owner, Admin | Delete assignment |

**Assignment Submission & Grading Feature:**

| Method | Endpoint | Parameters | Role Access | Description |
|--------|----------|------------|-------------|-------------|
| GET | /api/v1/assignments/:assignment_id/submissions | - | Course instructor, Admin | List all submissions |
| GET | /api/v1/submissions/:id | - | Submission owner, Course instructor, Admin | Get submission details |
| POST | /api/v1/submissions | assignment_id, content, file | Enrolled student | Submit assignment |
| PATCH | /api/v1/submissions/:id | content, file | Submission owner | Update submission (if before due date) |
| POST | /api/v1/submissions/:submission_id/grade | points_earned, feedback | Course instructor, Admin | Grade submission |
| PATCH | /api/v1/grades/:id | points_earned, feedback | Original grader, Admin | Update grade |
| GET | /api/v1/users/:user_id/grades | course_id | Grade owner, Course instructor, Admin | List user's grades |

**Administrative Dashboard Feature:**

| Method | Endpoint | Parameters | Role Access | Description |
|--------|----------|------------|-------------|-------------|
| GET | /api/v1/admin/users | role, search, page | Admin | List all users |
| PATCH | /api/v1/admin/users/:id | role, active | Admin | Update user role or status |
| DELETE | /api/v1/admin/users/:id | - | Admin | Delete user account |
| GET | /api/v1/admin/analytics | start_date, end_date | Admin | Get platform analytics |
| GET | /api/v1/admin/courses | published, instructor_id, page | Admin | List all courses (including unpublished) |

**API Conventions:**
- All endpoints return JSON responses
- Success responses include appropriate HTTP status codes (200 OK, 201 Created, 204 No Content)
- Error responses use standard codes (400 Bad Request, 401 Unauthorized, 403 Forbidden, 404 Not Found, 422 Unprocessable Entity) with error message in JSON
- Pagination implemented via page parameter, returns metadata (total_pages, current_page, total_count)
- Authentication failures return 401, authorization failures return 403
- Timestamps returned in ISO 8601 format

---

## **3.0 Technical Stack**

**Language:** Ruby 3.3.8 or better
- Chosen for its expressiveness and strong ecosystem for web development
- Compatible with the latest Rails features and security patches

**Framework:** Ruby on Rails 7.1 or better
- Provides robust MVC architecture aligned with the course curriculum
- Built-in security features (CSRF protection, SQL injection prevention)
- Active Record ORM simplifies database interactions
- Convention over configuration reduces boilerplate code
- Rich ecosystem of gems for common functionality

**Database:** PostgreSQL 15
- Production-grade relational database with strong ACID compliance
- Superior support for complex queries and relationships
- JSON field support for flexible data storage where needed
- Widely used in industry, valuable learning experience

**Testing Framework:** RSpec with FactoryBot and Capybara
- RSpec provides readable, BDD-style test syntax
- FactoryBot simplifies test data creation
- Capybara enables integration testing of user workflows
- Strong Rails community support and documentation

**Additional Tools:**
- **Authentication:** Devise gem for robust user authentication
- **Authorization:** Pundit gem for policy-based authorization
- **File Uploads:** ActiveStorage with AWS S3 for assignment submissions
- **Background Jobs:** Sidekiq for email delivery and analytics processing
- **API Documentation:** Swagger/OpenAPI for endpoint documentation

**Rationale:**
This stack represents current industry standards for Rails development while remaining accessible to students learning web application 
fundamentals. PostgreSQL provides more robust features than SQLite for a multi-user application, and RSpec is the de facto standard for 
Rails testing. The chosen gems (Devise, Pundit) are well-documented and teach best practices for authentication and authorization 
patterns that students will encounter professionally.
