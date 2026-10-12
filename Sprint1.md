<div align="center">

# Sprint 1 — Collections & Generics

### Job Application Tracker (Console) · Part 1 of 2 · Data Foundation

![Language](https://img.shields.io/badge/language-C%23-239120?style=flat-square)
![Platform](https://img.shields.io/badge/platform-.NET%20Console-512BD4?style=flat-square)
![Duration](https://img.shields.io/badge/duration-14%20days-blue?style=flat-square)
![LINQ](https://img.shields.io/badge/LINQ-not%20allowed-red?style=flat-square)

**Collections · Generics**

</div>

---

## Table of Contents

- [Overview](#overview)
- [Sprint Structure](#sprint-structure)
- [Project Brief](#project-brief)
- [Data Model](#data-model)
- [Business Rules](#business-rules)
- [Requirements](#requirements)
- [Project Details](#project-details)
- [Test Scenarios](#test-scenarios)
- [Hand-off to Sprint 2](#hand-off-to-sprint-2)
- [Working Rules](#working-rules)
- [Acceptance Criteria](#acceptance-criteria)

---

## Overview

**Goal:** build a strong C# foundation by learning to choose the right collection and write reusable generic code, then using both to build the **data foundation** of a console application.

The Job Application Tracker is built across **two sprints**:

| Sprint | Topics | What the project gains |
| :--- | :--- | :--- |
| **Sprint 1** *(this one)* | Collections, Generics | Companies, recruiters, jobs, and candidates: storage, menus, and management |
| **[Sprint 2](Sprint2.md)** | Delegates, Lambdas, Events | Company approval, applications, search and sorting, and notifications |

Keep your code clean and in one repository: Sprint 2 continues this exact project.

### Prerequisites

- C# fundamentals and control flow.
- Object-oriented programming: classes, objects, interfaces, and enums.

There is no database in this sprint. All data stays in memory so the focus stays on C#.

### Learning outcomes

By the end of the sprint, a student can:

1. Choose a suitable collection based on access pattern, uniqueness, and ordering.
2. Build CRUD operations with safe console input.
3. Write generic classes and methods with constraints.
4. Explain basic trade-offs: lookup cost, uniqueness, and keeping data consistent.
5. Test business rules and justify design decisions.

### Completion level

| Level | Requirements | Test scenarios |
| :--- | :--- | :---: |
| **Minimum** | CL1–CL4, G1, G2, E1, E2, M1, E3, E5 | 1–5 |

> [!NOTE]
> Every core concept of this sprint (collections and generics) is covered by the Minimum level.

> [!IMPORTANT]
> Sprint 2 starts from your Sprint 1 project. The Minimum level is the gate to begin it.

---

## Sprint Structure

The sprint is **14 days**: **9 days of study and practice, then 5 days on the project.** Finish Part 1 before starting Part 2.

| Part | Days | What you do | Requirements |
| :--- | :---: | :--- | :--- |
| **Part 1 — Study & Practice** | 1–9 | Study collections, then generics. Practice each in small exercises of your own | CL1–CL4, G1, G2 |
| **Part 2 — Build the Project** | 10–14 | Build the data foundation of the Job Application Tracker using what you studied | E1, E2, M1, E3, E5 |

### Suggested day-by-day plan

The 9 + 5 split is the rule. The plan inside each part is a guide for pacing.

| Day | Focus | Requirements |
| :---: | :--- | :--- |
| 1–2 | `List<T>`: add, remove, search, update | CL1 |
| 3 | `Dictionary<TKey, TValue>`: lookup by key, repeated keys | CL2 |
| 4 | `HashSet<T>`: uniqueness and membership | CL3 |
| 5 | `Queue<T>` and `Stack<T>` | CL4 |
| 6 | Collections review: when to choose each, and write the required note | CL1–CL4 output |
| 7 | Generic methods | G1 |
| 8 | Generic classes | G2 |
| 9 | Constraints (`where`), `IEntity`, and write the generics explanation | G1–G2 output |
| 10 | Models, seed data, and console menus | E1, E2 |
| 11 | Generic `DataStore<T>` | M1 |
| 12 | Basic operations for companies, recruiters, and candidates (email check with `HashSet`) | E3 |
| 13 | Job operations and the Dictionary lookup | E3, E5 |
| 14 | Run all test scenarios, write the README, and tag `sprint-1` | Hand-off |

---

## Project Brief

The full project is a console job portal. An admin approves companies, recruiters have jobs that belong to their companies, and candidates apply for available jobs.

**In this sprint** you build the data foundation: you can store and manage companies, recruiters, jobs, and candidates through console menus. Approval, applications, search, and notifications come in Sprint 2.

The application uses menus and sample data. It needs no login, database, web interface, or external API.

**Suggested structure**

```text
JobApplicationTracker/
├── Models/
├── Data/
├── Services/
├── Program.cs
└── README.md
```

Sprint 2 adds an `Events/` folder to this structure. Students may change the structure when they can explain why. The goal is understandable code, not a specific architecture.

> [!TIP]
> Write **one method to read a number and one to read text**, and use them everywhere. This is the cheapest way to meet the "never crash on bad input" rule.

---

## Data Model

| Entity | Properties | Rules |
| :--- | :--- | :--- |
| **Company** | `Id`, `Name`, `Industry`, `City`, `Status` | Name required; starts as Pending |
| **Recruiter** | `Id`, `Name`, `Email`, `CompanyId` | Name required |
| **Job** | `Id`, `Title`, `Description`, `Salary`, `Location`, `IsOpen`, `PostedDate`, `CompanyId`, `RecruiterId` | Title required; salary above 0; open by default |
| **Candidate** | `Id`, `FullName`, `Email`, `Phone`, `YearsOfExperience` | Name required; email unique; experience 0 or more |

**Enum**

- `CompanyStatus`: Pending, Approved, Rejected

Sprint 2 adds the `Application` entity and the `ApplicationStatus` enum.

**Ids** are generated by the program (the next number), not typed by the user.

**Seed data** (loaded at start): 3 companies, 3 recruiters, 6 jobs, and 5 candidates. Every student uses the fixed data in [Project Details](#project-details), so every rule can be tested right away.

---

## Business Rules

| # | Rule | Enforced in | Tested by |
| :---: | :--- | :---: | :---: |
| 1 | A **Pending or Rejected** company cannot get new jobs | E3 (Add Job) | Scenario 3 |
| 2 | A candidate's **email must be unique** (ignoring case) | E3 (Add Candidate) | Scenario 5 |
| 3 | Invalid input never crashes the application | All menus | Scenario 2 |

> [!NOTE]
> There is no login in this sprint, so recruiter-ownership rules are out of scope.

---

## Requirements

> [!IMPORTANT]
> Work in order. Finish and commit each requirement before moving to the next, and put its ID in the commit message, for example `M1: implement generic DataStore`.

## Part 1 — Study & Practice

Study the topic first, then write the exercise on your own. Each exercise is separate from the project and should be small, about 15–20 lines. One small console project per phase is enough.

### Phase A — Collections Lab

*Minimum · practice.*

| ID | Exercise | What it tests |
| :---: | :--- | :--- |
| **CL1** | **List:** store student scores; add, remove, search, and update one | Ordered collection and basic operations |
| **CL2** | **Dictionary:** store students by ID and retrieve one by ID; handle a repeated key safely | Key lookup and unique keys |
| **CL3** | **HashSet:** track registered student IDs and reject a repeated registration | Uniqueness and membership checks |
| **CL4** | **Queue and Stack:** process waiting candidates in arrival order (Queue); undo recent actions, most recent first (Stack) | FIFO and LIFO |

**Required output:** the four exercises, plus a short note on why each collection was chosen and the basic cost difference between walking a list, a dictionary lookup, and a hash-based check (no formal analysis needed).

### Phase B — Generics Practice

*Minimum · practice.*

| ID | Requirement | Acceptance criteria |
| :---: | :--- | :--- |
| **G1** | Generic method | One method works with several types without copying its code |
| **G2** | Generic class | A small generic class with `Add` and `GetAll` |

**Required output:** one generic method, one generic class, and a short explanation of type parameters, type arguments, and constraints (`where`).

---

## Part 2 — Build the Project

Now use what you studied. The project uses collections and generics from the first line of code.

### Phase C — Data Foundation

*Minimum.*

| ID | Requirement | Acceptance criteria |
| :---: | :--- | :--- |
| **E1** | Models and sample data | The four entities and the `CompanyStatus` enum exist; the seed data loads at start |
| **E2** | Console menus | A main menu with submenus for Companies, Jobs, and Candidates, plus a GetAll option for Recruiters |
| **M1** | Generic `DataStore<T>` | `Add`, `Update`, `Remove`, `GetById`, `GetAll`, with `where T : class, IEntity`. An `IEntity` interface exposes `Id`. One store is used by all four entities, with no raw lists left |
| **E3** | Basic operations | See the table below |
| **E5** | Dictionary lookup | Keep a `Dictionary<int, Job>` next to the jobs in `DataStore<Job>` and use it to find jobs by ID. Add, update, and remove keep both consistent |

Students must explain why `GetById` needs the `IEntity` constraint.

**E3 operations**

| Entity | Operations |
| :--- | :--- |
| Companies | Add, GetAll |
| Recruiters | GetAll |
| Jobs | Add, GetAll, Update (title, salary, open or closed), Remove |
| Candidates | Add, GetAll |

**GetAll** shows every item of that entity as a readable table and calls `DataStore<T>.GetAll()`.

Two extra rules for E3: **Add Job** refuses a Pending or Rejected company, and **Add Candidate** refuses an email that already exists, checked with a `HashSet<string>`.

GetAll output should be readable and aligned; plain `Console.WriteLine` formatting is enough. Menu choices, empty text, numbers, IDs, and negative values must never end the program.

> [!TIP]
> Reach `DataStore<T>` only through service methods. Then Sprint 2 can add search, sorting, and events in one place instead of across the whole project.

---

## Project Details

These details remove guesswork, so every student builds the same behavior and the reviewer can test every project the same way.

### Sample session

The exact wording is yours; the flow is what matters.

```text
===== Job Application Tracker =====
1. Companies     2. Jobs     3. Candidates
4. Recruiters (GetAll)       0. Exit
Choose: abc
Please enter a number between 0 and 4.
Choose: 2

--- Jobs ---
1. Add  2. GetAll  3. Update  4. Remove  0. Back
Choose: 2

Id | Title                 | Salary | Open
1  | Junior .NET Developer | 12000  | Yes
2  | QA Engineer           | 10000  | Yes
3  | Data Analyst          | 14000  | Yes
4  | Operations Assistant  | 8000   | Yes
5  | Research Assistant    | 9000   | Yes
6  | Backend Developer     | 18000  | No
```

```text
--- Jobs ---
Choose: 1
Company Id: 3
Cannot add a job: this company is not approved.
```

### Seed data

Use exactly this data. Company 3 is Pending (Scenario 3), and Job 6 is closed. Descriptions, locations, phone numbers, and posted dates can be any values.

**Companies**

| Id | Name | Industry | City | Status |
| :---: | :--- | :--- | :--- | :--- |
| 1 | Nile Tech | Software | Cairo | Approved |
| 2 | Delta Foods | Food | Mansoura | Approved |
| 3 | Pyramid Labs | Research | Giza | **Pending** |

**Recruiters**

| Id | Name | Email | CompanyId |
| :---: | :--- | :--- | :---: |
| 1 | Mona Samir | mona@niletech.com | 1 |
| 2 | Omar Adel | omar@deltafoods.com | 2 |
| 3 | Hana Yassin | hana@pyramidlabs.com | 3 |

**Jobs**

| Id | Title | Salary | Open | CompanyId | RecruiterId |
| :---: | :--- | :---: | :---: | :---: | :---: |
| 1 | Junior .NET Developer | 12000 | Yes | 1 | 1 |
| 2 | QA Engineer | 10000 | Yes | 1 | 1 |
| 3 | Data Analyst | 14000 | Yes | 2 | 2 |
| 4 | Operations Assistant | 8000 | Yes | 2 | 2 |
| 5 | Research Assistant | 9000 | Yes | 3 | 3 |
| 6 | Backend Developer | 18000 | **No** | 1 | 1 |

**Candidates**

| Id | Full name | Email | Years of experience |
| :---: | :--- | :--- | :---: |
| 1 | Sara Ahmed | sara@mail.com | 2 |
| 2 | Youssef Hany | youssef@mail.com | 0 |
| 3 | Mariam Tarek | mariam@mail.com | 4 |
| 4 | Karim Nabil | karim@mail.com | 1 |
| 5 | Nour Salah | nour@mail.com | 3 |

### Behavior decisions

| Topic | Decision |
| :--- | :--- |
| **Ids** | Each entity has its own counter that continues after the seeded Ids. An Id is never reused after a removal |
| **Add Company** | The company starts as Pending. It cannot get jobs until it is approved in Sprint 2 |
| **Add Job** | Show the companies and recruiters, then the user types a `CompanyId` and a `RecruiterId`. The company must be Approved (rule 1) |
| **Update Job** | The user types the job Id, then the new title, salary, and open or closed value |
| **Add Candidate** | The email must be new, ignoring case (rule 2). The seeded emails count as already used |
| **Dictionary and `DataStore<T>`** | `DataStore<T>` keeps its items in a `List<T>`, so its `GetById` walks the list. The jobs Dictionary stays as the fast lookup, and every job add, update, and remove changes **both** |

---

## Test Scenarios

Every student tests these by hand, **in this order**.

| # | Scenario | Expected result | Level |
| :---: | :--- | :--- | :--- |
| 1 | Start the app and open GetAll for companies, recruiters, jobs, and candidates | 3 companies, 3 recruiters, 6 jobs, and 5 candidates, matching the seed data | Minimum |
| 2 | Type `abc` where a number is expected, enter an invalid ID, or enter a negative salary | Friendly message; the menu shows again | Minimum |
| 3 | Add a job to the Pending company (Id 3); then add a job to an Approved company | The first is refused with a clear message; the second succeeds | Minimum |
| 4 | Add a job, update it, then remove it, looking it up by ID after each step | The lookup always returns the current data, and the removed job is gone from both the list and the Dictionary | Minimum |
| 5 | Add a candidate with an email that already exists (try `SARA@mail.com`) | Refused with a clear message; a new email is accepted | Minimum |

---

## Hand-off to Sprint 2

Sprint 2 extends this exact project. Before you finish, check that your code is ready for it:

- [ ] All four entities are stored in `DataStore<T>`, with no raw lists left.
- [ ] The jobs Dictionary stays consistent after every add, update, and remove.
- [ ] Every operation (add, update, remove, GetAll) lives in a **service method**, not inside the menu code.
- [ ] The main menu has room for one more option (Applications), and the Jobs menu for two (Search and Sort).
- [ ] The final commit is tagged `sprint-1`.

---

## Working Rules

- **No LINQ** in this sprint. Use loops and the collection methods.
- Use only concepts taught or introduced during the sprint.
- Keep all data in memory: no database, no Entity Framework.
- The application must not crash on invalid input.
- Study one topic at a time, about an hour per session. **Finish Part 1 (study and practice, days 1–9) before starting Part 2 (the project, days 10–14).**
- Commit after every requirement, with its ID in the message.
- Push the project to GitHub with a short README.
- Students may discuss concepts, but each one implements and explains their own solution.
- Avoid extra architecture patterns; the focus is C# fundamentals and sound decisions.

---

## Acceptance Criteria

A submission is accepted when all of these are true.

**Minimum level**

- [ ] CL1–CL4, G1, G2, E1, E2, M1, E3, and E5 are done and meet their acceptance criteria in [Requirements](#requirements)
- [ ] Test scenarios 1–5 pass
- [ ] The solution builds with no errors, loads the seed data on first run, and never crashes on bad input
- [ ] The student can explain the points listed under each phase of the requirements

**Repository**

- [ ] The project is pushed to GitHub, with a commit per requirement and its ID in the message
- [ ] A short `README.md` with a 2-line overview, how to run, every requirement ID marked **Done** or **Skipped**, and 2–3 design decisions
- [ ] The [Hand-off to Sprint 2](#hand-off-to-sprint-2) checklist is complete
- [ ] No secrets or real credentials
