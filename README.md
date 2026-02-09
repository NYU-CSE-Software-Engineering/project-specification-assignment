# **Project Specification Template and Rubric** 

### **1.0 Project Overview (max of 3 points)**

* ## **3 points:** Provides a clear purpose, description of target users, and scope of the system. Mentions key technical requirements (multi-feature, SaaS, roles, storage, APIs) in context. Reads as a professional overview.

* ## **1-2 points** Overview is present but vague or missing either purpose, users, or scope. There is minimal reference to the technical context.

* ## **0 points:** No meaningful overview.

## **2.0 Core Requirements (max of 20 points)**

### **2.1 User-Based System (max of 2 points)**

* **2 points:** Describes authentication and authorization clearly, including registration, login, and access controls. Shows awareness of security considerations.

* **1 point:** Mentions authentication/authorization but without detail.

* **0 points:** Missing or unclear about handling of authentication and authorization.

### **2.2 User Roles (max of 3 points)**

* **3 points:** Defines at least 3 distinct roles, each with specific permissions and restrictions. Roles reflect real use cases of the system.

* **2 points:** Roles listed, but overlap, lack permissions detail, or feel underdeveloped.

* **1 point:** Roles are only named without a meaningful explanation, or fewer than 3 roles were defined.

* **0 points:** Roles missing.

### **2.3 Persistent Storage (max of 6 points)**

* **6 points: All criteria met for both Schema Completeness and Schema Justification**  
  * **Schema Completeness (max of 3 points)**  
    * The database schema must include a minimum of 7 tables.

    * Tables should be logically organized and follow database normalization principles (avoiding redundant data, ensuring appropriate relationships between entities).

    * Schema design must directly support all functional requirements of the application (each table should serve a clear purpose within the application's feature set).

    * Appropriate use of primary keys, foreign keys, and associations (has\_many, belongs\_to, has\_and\_belongs\_to\_many, etc.).

    * Tables should represent distinct entities or join tables for many-to-many relationships where appropriate.

  * **Schema Justification (max of 3 points)**  
    * Provide a written explanation describing how your database schema supports the application's core features and functionality.

    * Identify which tables and relationships are used to implement each major feature of your application.

    * If your application includes different user roles (e.g., admin, regular user, guest), explain how the schema accommodates these roles and their different permissions or capabilities.

    * Demonstrate understanding of why specific design choices were made (e.g., why certain tables were separated, why specific associations were chosen, why particular fields were included).

    * Show the connection between user stories or functional requirements and the underlying data model.

* **4-5 points:** Schema includes 7+ tables with mostly appropriate relationships; justification present but may lack depth or miss some feature connections

* **2-3 points:** Schema has 5-6 tables or has normalization issues; justification is superficial or incomplete

* **1 point:** Schema present but has significant structural problems; minimal or unclear justification

* **0 points:** Missing or incoherent schema.

### **2.4 Modular Architecture (max of 6 points)**

* **6 points: 3-5 features clearly defined with complete documentation; dependencies fully described and logical**  
  * **Feature Definition (max of 3 points):** Application functionality is organized into 3-5 distinct major features (feature areas), each clearly named and documented. For each feature, provide:  
    * A descriptive name that reflects its purpose (e.g., "User Authentication," "Product Catalog," "Order Management").

    * A summary of the feature's core functionality and responsibilities.

    * Clear boundaries identifying which models, views, and controllers belong to this feature area.

    * Brief description of the primary user-facing capabilities this feature provides.

  * **Feature Dependencies (max of 3 points):** Documentation explicitly describes how features interact with or depend on one another, including:  
    * Which features rely on data or functionality from other features

    * The nature of these dependencies (e.g., "Order Management depends on User Authentication to identify the customer placing an order").

    * Dependencies are logical and demonstrate thoughtful application architecture.

* **4-5 points:** 3-5 features defined, but some documentation gaps; dependencies mentioned but not fully explained

* **2-3 points:** Only 2-3 features defined, or feature boundaries unclear; dependencies missing or vague

* **1 point:** Minimal attempt at feature organization; no meaningful dependency documentation

* **0 points:** Fewer than 2 features defined, or documentation missing entirely

  ### **2.5 API Interfaces (max of 3 points)**

* **3 points:** RESTful endpoints listed for each feature. Endpoints follow conventions (verbs, URIs, parameters). Includes role-based access control.

* **1-2 points:** Not all features have endpoints, or endpoints are included but inconsistent, incomplete, or do not follow REST standards.

* **0 points:** Not addressed.

## **3.0 Technical Stack (max of 3 points)**

* **3 points:** Language, framework, database, and testing framework are all specified with an optional brief rationale for why they are appropriate.

* **2 points:** Most components listed.

* **1 point:** Some components are missing.

* **0 points:** Not addressed.


  