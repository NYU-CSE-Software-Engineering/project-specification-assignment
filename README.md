# **Project Specification Template and Rubric** 

The project should adhere to the previously published core requirements (see Brightspace). These include the following areas (as a reminder).
|Area|Description|
|---|---|
| **Language / Framework** | You will be using Ruby and Rails (aka Ruby on Rails) for your application's back end. I am less concerned with the frontend, though an HTML web interface should be supported. If you have a stretch goal of a mobile interface, you can use whatever you like to implement it, but it should be using your API endpoints to interact with the system. |
| **Role-Based Access Control (RBAC)** | The system must support at least three distinct user roles with hierarchical permissions: A solid trio would be Platform Admin (full system access), Moderator/Support (can manage user disputes, content, or basic billing but not system settings), and Standard User. Alternatively, you could map roles to the subscription tiers: Admin, Pro User, and Free User, where permissions dictate what features they can access. |
| **Non-Trivial External API Integration** | The application must consume at least one external API in a meaningful way. The integration must involve complex data processing or two-way communication (e.g., Stripe for payments, Twilio for SMS, SendGrid for transactional emails, or an LLM API). A simple read-only weather or stock ticker widget is insufficient. |
| **Persistent Data Storage & Migrations** | The application must use a robust relational/SQL database. It must implement a formal schema and use a database migration system (built into Rails) to version-control database changes. |
| **Modern Authentication & Security** | Secure user registration, login, and session management: Must use industry-standard practices, such as JWT (JSON Web Tokens) with secure HTTP-only cookies, or OAuth 2.0 (SSO via Google, GitHub, etc.). Passwords must be securely hashed (e.g., bcrypt/Argon2) before storage. |
| **Documented RESTful API** | The backend must be exposed as a cleanly designed REST API (proper HTTP verbs, status codes, and resource routing). The API must be automatically documented using a standard like OpenAPI/Swagger. |
| **Subscription Tiers & Feature Toggling** | The system should implement at least two service tiers (e.g., "Free" and "Pro"). Programmatically restrict access to certain premium features or usage limits based on the user's or tenant's active tier. (Actual payment processing can be mocked or put in "test mode").|
| **Usage Tracking or Auditing** | The system should track key user actions or usage metrics, either for billing purposes (e.g., number of API calls made) or security auditing (e.g., an activity log showing who deleted a resource). |

### 1.0 Project Overview (max of 3 points) [`docs/project_specification/project_overview.md`]

* 3 points: Provides a clear purpose, description of target users, and scope of the system. Mentions key technical requirements (multi-feature, SaaS, roles, storage, APIs) in context. Reads as a professional overview.

* 1-2 points: Overview is present but vague or missing either purpose, users, or scope. There is minimal reference to the technical context.

* 0 points: No meaningful overview.

### 2.0 Core Requirements (max of 20 points) [`docs/project_specification/core_requirements.md`]

Describe how your system will address each of the core requirement areas below.

--- 

#### 2.1 Language / Framework (max of 2 points)

* **2 points:** Clearly outlines the use of Ruby on Rails for the backend, summarizes the planned front-end interface, and explicitly specifies how any additional stretch interfaces (e.g., mobile) consume the Rails API endpoints.
* **1 point:** Mentions using Ruby on Rails, but details on the front-end interface or API integration for additional clients are vague or incomplete.
* **0 points:** Fails to specify Ruby on Rails as the backend framework or omits interface architecture details.

---

#### 2.2 Role-Based Access Control (RBAC) (max of 3 points)

* **3 points:** Defines at least 3 distinct user roles with clear hierarchical permissions and restrictions (e.g., Admin, Moderator, Standard User) that reflect realistic application use cases.
* **2 points:** Defines 3 roles, but permissions overlap, lack granular detail, or are underdeveloped.
* **1 point:** Lists fewer than 3 roles, or roles are only named without describing associated permissions.
* **0 points:** RBAC design is missing or completely unaddressed.

---

#### 2.3 Non-Trivial External API Integration (max of 3 points)

* **3 points:** Fully details an integration with a third-party API (e.g., Stripe, Twilio, LLM, etc.) involving complex data processing or two-way communication rather than simple read-only data fetching.
* **2 points:** Describes an external API integration, but the implementation lacks complexity or two-way data flow.
* **1 point:** Uses a trivial external API (e.g., basic weather or stock ticker) or lacks detail on how the application processes the external data.
* **0 points:** No external API integration is proposed.

---

#### 2.4 Persistent Data Storage & Migrations (max of 2 points)

* **2 points:** Describes a relational/SQL database schema design and details a clear strategy for version-controlling database changes using Rails migrations.
* **1 point:** Mentions using a relational database, but details regarding schema design or the migration system are minimal.
* **0 points:** Missing or unclear about relational database storage and migration strategy.

---

#### 2.5 Modern Authentication & Security (max of 3 points)

* **3 points:** Proposes industry-standard authentication (e.g., OAuth 2.0 or JWT with secure HTTP-only cookies), outlines secure password hashing (e.g., bcrypt/Argon2), and demonstrates strong security practices.
* **2 points:** Mentions secure authentication and password hashing, but details on session/token management or security safeguards are incomplete.
* **1 point:** Uses outdated or insecure authentication methods without proper session management or secure password hashing.
* **0 points:** Authentication and security mechanisms are missing.

---

#### 2.6 Documented RESTful API (max of 2 points)

* **2 points:** Clearly defines a RESTful API architecture (proper HTTP verbs, status codes, resource routing) and includes automatic API documentation generation (e.g., OpenAPI/Swagger).
* **1 point:** Describes a REST API structure, but omits specifics on HTTP verbs/routing standards, or lacks automatic API documentation generation.
* **0 points:** API architecture is not RESTful, poorly defined, or is non-existent.

---

#### 2.7 Subscription Tiers & Feature Toggling (max of 3 points)

* **3 points:** Details at least two service tiers (e.g., Free vs. Pro) and provides a clear mechanism for programmatically enforcing feature access, usage limits, or feature toggling based on tier status.
* **2 points:** Mentions subscription tiers, but the implementation plan for feature restriction or toggling lacks technical detail.
* **1 point:** Defines tiers conceptually without explaining how restrictions are programmatically enforced within the application.
* **0 points:** Subscription tiers or feature toggling are omitted entirely.

---

#### 2.8 Usage Tracking or Auditing (max of 2 points)

* **2 points:** Clearly outlines a mechanism for logging user actions or tracking usage metrics for security auditing or usage-based billing.
* **1 point:** Mentions logging or tracking, but lacks specifics on what events are audited or how data is stored/utilized.
* **0 points:** No plan provided for audit logging or usage tracking.
  
### 3.0 Technical Stack (max of 3 points) [`docs/project_specification/technical_stack.md`]

* 3 points: Language, framework, database, and testing framework are all specified with a brief rationale for why they are appropriate.
* 2 points: Most components listed.
* 1 point: Some components are missing.
* 0 points: Not addressed.

### 4.0 Comprehensive Feature List (max of 3 points) [`docs/project_specification/feature_list.md`]
Define the system's functional features in a structured Markdown table, ensuring each feature explicitly addresses all required metadata fields (Feature Name, Target User Role, Description, and Tier Availability).

* 3 points: Outlines at least 6+ (strive to include all) distinct application features using a Markdown table. Every entry explicitly details all required attributes: Feature Name, Target User Role (e.g., Platform Admin, Instructor, Student), Description (clear functional overview), and Tier Availability (e.g., Free, Pro, Enterprise).
* 2 points: Lists 3-5 features in a table, but fails to include one of the required columns (e.g., missing Tier Availability or Target User Role), or descriptions are overly brief.
* 1 point: Lists features in plain text or bullet points instead of a Markdown table, or defines fewer than 3 features with missing metadata fields.
* 0 points: Feature list section is omitted entirely.

# Submitting the specification (see [the grading rubric](grading_rubric.md))
The team will collaboratively develop the specification document. It is expected that all team members have a major (equal) role in the discussion of the specification, the design of the system,
and the creation of the specification documents. Our expectations break down into the following:
- The team will create (in the team's repo) a `docs/` directory at the top level of the repo. This and future documentation (for other assignments) will reside in this directory.
- The team will create a subdirectory under the `docs/` directory that is named `project_specification/`. This directory will contain the deliverable for this task/assignment.
- The team will create a `README.md` [Markdown file](https://docs.github.com/en/get-started/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax) in the `docs/project_specification/` directory that is the table of contents for the overall project specification document.
- The `README.md` page will be a table of contents that lists the major subsections of the document (listed above) with links to each of those subsections. Each subsection is a separate Markdown file as specified above.
- The development of these files is a team effort. Anyone who does not contribute in any fashion (in the manner described below) will receive a 0 grade for the task/assignment.
  - Each member will open a branch on this repo and edit the `README.md` and other subdocument files to contribute their work. Each branch is partially named for the author, using their NYU NetID (e.g., `in-class-assignment-5-<nyu-netid>`). If necessary, multiple branches are ok, but each should contain the NYU NetID in the name for tracking purposes.
  - Each member will commit their files (with appropriate and professional comments with the commit) and then push the code to the team's repo.
  - Each member will create their own pull request for the branch/es they authored and leave a comment in the comments section of the PR tagging all other team members (use the '@' notation)
  - Every member will review and comment on one other member's PR, requesting changes or approving the PR for merging. Multiple team members can be reviewing the branches in this step; we are not implying there is only one reviewer.
  - Once the PR is approved, each member of the team should merge their _own_ branch/es.

If performed correctly, every member of the team will have created a branch (or more), committed changes, created a PR, reviewed and approved someone else's PR, and merged their _own_ documents. Merge conflicts should be resolved before merging; penalties will result from incorrect merging or the inclusion of [merge conflict markers](https://codersnexus.com/tutorials/github-complete-course/merge-conflicts-causes-and-conflict-markers).

Since these files will be developed somewhat simultaneously, we strongly advise that `git pull`s are repeatedly performed on the `main` branch. This will keep your local copy up to date with the changes merged by others. However, your branch may lag behind the HEAD of the `main` branch. Thus, you might want to explore the use of the `git stash` and `git rebase` commands. But it is still likely you might encounter the issue of git _merge conflicts_. It's something we all go through in cases like this, and you'll want to carefully resolve these conflicts.  Seek help from the course staff if this becomes a challenge for you. Your prior work in GitKit should have prepared you for this. If you are still struggling, do not wait until the last hours to resolve these issues and your comprehension thereof.

Once the document is "done" (check Brightspace for the date and time), you can expect that the course staff will grade your team's work.  Edits after the due date of the document **WILL NOT** be considered as part of the document, so be mindful of the due date and time.

Again, we expect this to be a team effort, and the git repo will show this to us clearly. If you fail to contribute, expect a 0 grade. If you fail to contribute in a _meaningful_ way, expect to receive a poor grade. (That is, if you only add a small amount of text to the document, then your grade will be small.) Outside of these deductions, each member of the team would normally receive the same grade for the task/assignment.

# Example specification
Be sure to reference the [example specification](https://github.com/NYU-CSE-Software-Engineering/project-specification-assignment/blob/main/example_specification.md) that was discussed in class. 

# Grading Rubric
You can review the [grading rubric here](grading_rubric.md).
