## Team Evaluation Rubric (41 points max)

### 1.0 Project Overview (max of 3 points)

| Criterion        | Excellent<br>2.1–3.0 points                                                                                                                                                                                   | Satisfactory<br>0.1–1.0 points                                                                                                 | Unsatisfactory<br>0.0 points |
|------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------|------------------------------|
| Project Overview | Provides a clear purpose, description of target users, and scope of the system. Mentions key technical requirements (multi-feature, SaaS, roles, storage, APIs) in context. Reads as a professional overview. | Overview is present but vague or missing either purpose, users, or scope. There is minimal reference to the technical context. | No meaningful overview.      |

### Part 2.0 Core Requirements (max of 20 points)

#### 2.1 User-Based System (max of 2 points)

| Criterion                            | Excellent<br>1.1–2.0 points                                                                                                                         | Satisfactory<br>0.1–2.0 points                            | Unsatisfactory<br>0.0 points                                           |
|--------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------|------------------------------------------------------------------------|
| Core Requirements: User-Based System | Describes authentication and authorization clearly, including registration, login, and access controls. Shows awareness of security considerations. | Mentions authentication/authorization but without detail. | Missing or unclear about handling of authentication and authorization. |

#### 2.2 User Roles (max of 3 points)

| Criterion                     | Excellent<br>2.1–3.0 points                                                                                                     | Satisfactory<br>1.1–2.0 points                                              | Unsatisfactory<br>0.1–1.0 points                                                           | Missing<br>0.0 points |
|-------------------------------|---------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------|--------------------------------------------------------------------------------------------|-----------------------|
| Core Requirements: User Roles | Defines at least 3 distinct roles, each with specific permissions and restrictions. Roles reflect real use cases of the system. | Roles listed, but overlap, lack permissions detail, or feel underdeveloped. | Roles are only named without a meaningful explanation, or fewer than 3 roles were defined. | Roles missing.        |

#### 2.3 Persistent Storage (max of 6 points)

| Criterion           | Excellent<br>5.1–6.0 points                                            | Good<br> 3.1–5.0 points                                                                                                                    | Satisfactory<br>1.1–3.0 points                                                                | Unsatisfactory<br>0.1–1.0 points                                                         | Missing<br>0.0 points         |
|---------------------|------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------|-------------------------------|
| Persistence Storage | All criteria met for both Schema Completeness and Schema Justification | Schema includes 7+ tables with mostly appropriate relationships; justification present but may lack depth or miss some feature connections | Schema has 5-6 tables or has normalization issues; justification is superficial or incomplete | Schema present but has significant structural problems; minimal or unclear justification | Missing or incoherent schema. |

#### 2.4 Modular Architecture (max of 6 points)

| Criterion            | Excellent<br>5.1–6.0 points                                                                        | Good<br> 3.1–5.0 points                                                                           | Satisfactory<br>1.1–3.0 points                                                          | Unsatisfactory<br>0.1–1.0 points                                                | Missing<br>0.0 points                                            |
|----------------------|----------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------|---------------------------------------------------------------------------------|------------------------------------------------------------------|
| Modular Architecture | 3-5 features clearly defined with complete documentation; dependencies fully described and logical | 3-5 features defined, but some documentation gaps; dependencies mentioned but not fully explained | Only 2-3 features defined, or feature boundaries unclear; dependencies missing or vague | Minimal attempt at feature organization; no meaningful dependency documentation | Fewer than 2 features defined, or documentation missing entirely |

#### 2.5 API Interfaces (max of 3 points)

| Criterion      | Excellent<br>2.1–3.0 points                                                                                                            | Satisfactory<br>0.1–1.0 points                                                                                            | Unsatisfactory<br>0.0 points |
|----------------|----------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------|------------------------------|
| API Interfaces | RESTful endpoints listed for each feature. Endpoints follow conventions (verbs, URIs, parameters). Includes role-based access control. | Not all features have endpoints, or endpoints are included but inconsistent, incomplete, or do not follow REST standards. | Not addressed                |

### 3.0 Technical Stack (max of 3 points)

| Criterion       | Excellent<br>2.1–3.0 points                                                                                                           | Satisfactory<br>1.1–2.0 points | Unsatisfactory<br>0.1–1.0 points | Missing<br>0.0 points |
|-----------------|---------------------------------------------------------------------------------------------------------------------------------------|--------------------------------|----------------------------------|-----------------------|
| Technical Stack | Language, framework, database, and testing framework are all specified with an optional brief rationale for why they are appropriate. | Most components listed         | Some components are missing      | Not addressed.        |

### 4.0 Deliverable (max of 1 point)

| Criterion                                        | Met requirement<br>1.0 points | Failed to meet requirement<br>0.0 points |
|--------------------------------------------------|-------------------------------|------------------------------------------|
| Team Deliverable: `docs/specification/README.md` |                               |                                          |

### 5.0 Individual Evaluation Rubric (14 points max)

| Criterion                                                                                                                                                                         | Met requirement<br>2.0 points | Failed to meet requirement<br>0.0 points |
|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------|------------------------------------------|
| Member opened a branch on this repo and edited the `README.md` file to contribute their work.                                                                                     |                               |                                          |
| Member committed their files (with appropriate and professional comments with the commit) and pushed their code to the team's repo.                                               |                               |                                          |
| Member created their **own** pull request for the branch they authored and left a comment in the comments section of the PR tagging all other team members (use the '@' notation) |                               |                                          |
| Member reviewed the contributions (performed a code review) and either (1) commented on the PR or (2) requested changes on the PR                                                 |                               |                                          |
| Member approved the PR for merging after author of PR resolved outstanding comments or change requests                                                                            |                               |                                          |
| Member closed **their own PR**                                                                                                                                                    |                               |                                          |
| Merged merged **their own branch** into the **main** branch, thereby merging their document changes.                                                                              |                               |                                          |
