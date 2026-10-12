# Tawjeh Level 1 — Backend .NET Developer

# Sprint Specifications

> **Final Project:** Library Management API
> **Path:** C# Collections & Generics → Delegates & Events → LINQ → EF Core → ASP.NET Core Web API
> **Duration:** 6 Sprints (4 × 14 days + 2 × 15 days · 86 days)

---

# 🗺️ Roadmap Overview

| Sprint | Focus                                      | Days                | Project                                                   |
| ------ | ------------------------------------------ | ------------------- | --------------------------------------------------------- |
| 1      | Collections & Generics                     | Day 1–14 (14 days)  | Job Application Tracker — Part 1: Data Foundation (Console) |
| 2      | Delegates, Lambdas & Events                | Day 15–28 (14 days) | Job Application Tracker — Part 2: Workflows & Notifications (Console) |
| 3      | LINQ                                       | Day 29–42 (14 days) | Sales Data Analytics Dashboard (Console)                  |
| 4      | Entity Framework Core                      | Day 43–56 (14 days) | Library Management System (EF Core)                       |
| 5      | Web API — Foundations, Services & Business Logic | Day 57–71 (15 days) | Library Management API — Part 1 (9 endpoints)       |
| 6      | Web API — Unit of Work & Loans             | Day 72–86 (15 days) | Library Management API — Part 2 (+3 endpoints = 12)       |

Total duration: **86 days** (Sprints 1–4 are 14 days each, Sprints 5–6 are 15 days each).

---

# 🎯 Level 1 Goal

The goal of Level 1 is to take the learner from solid C# fundamentals to building a working, layered Web API on top of a database.

The learner should practice:

```text
Learn
    ↓
Practice
    ↓
Build
    ↓
Debug
    ↓
Explain
```

Every sprint ends with a project, and every project uses what the previous sprints taught.

**How the projects connect:**

* Sprints 1–2 build **one** console project (Job Application Tracker) across two sprints. Sprint 2 continues the exact repository of Sprint 1.
* Sprint 3 is a separate in-memory project (no database).
* Sprint 4 adds a real database with EF Core (Library Management System).
* Sprints 5–6 build **one** Web API (Library Management API) across two sprints. Sprint 6 improves the same solution; it is not a new project.

**Sprint structure:** Sprints 1 and 2 follow a fixed **9 days study & practice, then 5 days project** split. Sprints 3 and 4 are task-based with Easy / Medium / Hard requirements. Sprints 5 and 6 are day-by-day plans where every day ends with a buildable project and a small commit.

---

# 📅 Sprint 1 — Collections & Generics

**Day 1–14 · Project: Job Application Tracker — Part 1 of 2 (Console)**

## 🎯 Sprint Goal

Build a strong C# foundation: choose the right collection, write reusable generic code, and use both to build the **data foundation** of a console application.

**Sprint rules:** no LINQ, no database, no Entity Framework. All data stays in memory.

---

# 📚 What You Will Learn

## 1. Collections

Learn:

* `List<T>`
* `Dictionary<TKey, TValue>`
* `HashSet<T>`
* `Queue<T>`
* `Stack<T>`
* How to choose the right collection (access pattern, uniqueness, ordering)

---

# 2. Generics

Learn:

* Generic methods
* Generic classes
* Constraints (`where`)
* Type parameters and type arguments

---

# 🛠️ Practical Implementation

## Job Application Tracker — Part 1 (Console)

Build the data foundation of a console job portal: store and manage **companies, recruiters, jobs, and candidates** through console menus, using:

* A generic `DataStore<T>` with `where T : class, IEntity`
* A `Dictionary<int, Job>` kept consistent next to the jobs store
* A `HashSet<string>` to check candidate email uniqueness
* Fixed seed data (3 companies, 3 recruiters, 6 jobs, 5 candidates)

---

# 📝 Sprint Tasks

### Part 1 — Study & Practice (Day 1–9)

| Day | Focus | Requirements |
| --- | ----- | ------------ |
| 1–2 | `List<T>`: add, remove, search, update | CL1 |
| 3   | `Dictionary<TKey, TValue>` | CL2 |
| 4   | `HashSet<T>` | CL3 |
| 5   | `Queue<T>` and `Stack<T>` | CL4 |
| 6   | Collections review and the required note | CL1–CL4 |
| 7   | Generic methods | G1 |
| 8   | Generic classes | G2 |
| 9   | Constraints, `IEntity`, and the generics explanation | G1–G2 |

### Part 2 — Build the Project (Day 10–14)

| Day | Focus | Requirements |
| --- | ----- | ------------ |
| 10 | Models, seed data, and console menus | E1, E2 |
| 11 | Generic `DataStore<T>` | M1 |
| 12 | Basic operations for companies, recruiters, and candidates | E3 |
| 13 | Job operations and the Dictionary lookup | E3, E5 |
| 14 | Run all test scenarios, write the README, and tag `sprint-1` | Hand-off |

**Requirements:** CL1–CL4, G1, G2 (practice) · E1, E2, M1, E3, E5 (project) · **Test scenarios:** 1–5

---

# 📦 Sprint Deliverables

* Collections practice (List, Dictionary, HashSet, Queue, Stack) with a short note on why each was chosen
* Generics practice (generic method, generic class, explanation of constraints)
* Job Application Tracker — Part 1 (Console)
* README
* Final commit tagged `sprint-1`

---

# ✅ Definition of Done

* [ ] Right collection chosen for each case
* [ ] Generic method and generic class work, and constraints are explained
* [ ] All four entities are stored in `DataStore<T>` with no raw lists left
* [ ] The jobs Dictionary stays consistent after every add, update, and remove
* [ ] A Pending or Rejected company cannot get new jobs
* [ ] Candidate email is unique (ignoring case), checked with a `HashSet`
* [ ] Invalid input never crashes the application
* [ ] Test scenarios 1–5 pass
* [ ] The Hand-off to Sprint 2 checklist is complete

---

# 🎓 Expected Outcome

By the end of Sprint 1, the learner can choose the right collection for each case, write clean reusable generic code, and has a working data foundation ready for Sprint 2.

---

# 📅 Sprint 2 — Delegates, Lambdas & Events

**Day 15–28 · Project: Job Application Tracker — Part 2 of 2 (Console)**

## 🎯 Sprint Goal

Learn to pass behavior through delegates and lambdas, notify subscribers using events, and use them to finish the Job Application Tracker started in Sprint 1.

**Sprint rules:** no LINQ, no database. You do not start a new project: you open your Sprint 1 repository and add to it. The Sprint 1 Minimum level is the entry gate.

---

# 📚 What You Will Learn

## 1. Delegates & Lambdas

Learn:

* `Func<T, TResult>`
* `Action<T>`
* `Predicate<T>`
* `Comparison<T>`
* Lambda expressions
* When to choose each delegate type

---

# 2. Events

Learn:

* Declaring, raising, and subscribing to events
* `EventHandler<T>`
* Lambda handlers vs named-method handlers
* Unsubscribing
* What happens when a subscriber throws

---

# 🛠️ Practical Implementation

## Job Application Tracker — Part 2 (Console)

Extend the Sprint 1 project with:

* `Find(Predicate<T>)` and `Sort(Comparison<T>)` on `DataStore<T>`
* Search and sort for jobs (title keyword + minimum salary; sort by salary and title)
* Company approval (approve / reject / list by status)
* Applications: the `Application` entity, the `ApplicationStatus` enum, and the apply workflow
* Duplicate-application protection with `HashSet<(int JobId, int CandidateId)>`
* Three events with handlers: `CompanyApproved`, `ApplicationSubmitted`, `ApplicationStatusChanged`

---

# 📝 Sprint Tasks

### Part 1 — Study & Practice (Day 15–23)

| Day | Focus | Requirements |
| --- | ----- | ------------ |
| 15–16 | Delegates and lambda syntax; `Func<T, TResult>` and `Action<T>` | D1, D2 |
| 17    | `Predicate<T>` | D3 |
| 18    | `Comparison<T>` and sorting with a delegate | D4 |
| 19    | Delegates review and the required note | D1–D4 |
| 20    | Events: declare, raise, subscribe; `EventHandler<T>` | EV1 |
| 21    | Handlers: lambda vs named method, and unsubscribing | EV1 |
| 22    | Event practice and review (publisher, subscriber, a subscriber that throws) | EV1 |
| 23    | Review all Sprint 2 topics, re-read Sprint 1 code, plan the changes | — |

### Part 2 — Continue the Project (Day 24–28)

| Day | Focus | Requirements |
| --- | ----- | ------------ |
| 24 | Search and sort on `DataStore<T>` | M3 |
| 25 | Company approval and list by status | E4 |
| 26 | Applications menu and apply workflow | E6 |
| 27 | Duplicate protection (Full), then the three events | M4, M2 |
| 28 | Run all test scenarios, update the README, and tag `sprint-2` | — |

**Requirements:** D1–D4, EV1 (practice) · M3, E4, E6, M2 (Minimum) · M4 (Full) · C1 (optional challenge, not graded) · **Test scenarios:** 1–7 (Minimum), 1–8 (Full)

---

# 📦 Sprint Deliverables

* Delegate practice (`Func`, `Action`, `Predicate`, `Comparison`) with a short note
* Event practice (`EV1`)
* Job Application Tracker — complete (Search, Sort, Approval, Applications, Events)
* README updated
* Final commit tagged `sprint-2`

---

# ✅ Definition of Done

* [ ] The four delegate types are used correctly, and the learner can say when to choose each
* [ ] `DataStore<T>` stays generic and gains `Find` and `Sort` (sorting returns a sorted copy)
* [ ] Company approval and list by status work
* [ ] A candidate can apply only to an open job of an Approved company
* [ ] A job with applications cannot be removed
* [ ] The three events are raised and handled with clear notifications
* [ ] Duplicate applications are rejected (Full level)
* [ ] Sprint 1 behavior still works
* [ ] Test scenarios 1–7 pass (1–8 for Full)

---

# 🎓 Expected Outcome

By the end of Sprint 2, the learner can pass behavior through delegates and lambdas, build event-driven notifications, extend an existing codebase without breaking it, and enforce business rules in workflows.

---

# 📅 Sprint 3 — LINQ

**Day 29–42 · Project: Sales Data Analytics Dashboard (Console)**

## 🎯 Sprint Goal

Learn to query and shape data in memory with LINQ, understand how queries execute, and know the common pitfalls.

**Working rule:** every topic day ends with 5–8 practice problems and adds 1–2 queries to the sprint project. The dashboard grows during the sprint instead of being squeezed into the last days.

---

# 📚 What You Will Learn

## 1. Execution Concepts

Learn:

* Deferred vs immediate execution
* Method syntax vs query syntax
* IEnumerable vs IQueryable (concept)
* Multiple enumeration
* Captured variables in closures

---

# 2. Filtering & Quantifiers

Learn:

* Where
* OfType
* Any
* All
* Contains

---

# 3. Projection & Ordering

Learn:

* Select
* SelectMany
* Anonymous types
* Projecting into records / DTOs
* OrderBy
* ThenBy

---

# 4. Element & Conversion Operators

Learn:

* First
* FirstOrDefault
* Single
* SingleOrDefault
* DefaultIfEmpty
* ToList
* ToArray
* ToDictionary
* ToHashSet
* ToLookup

---

# 5. Paging / Partitioning

Learn:

* Skip
* Take

---

# 6. Grouping

Learn:

* GroupBy
* Composite keys
* Per-group aggregates
* ToLookup

---

# 7. Joining

Learn:

* Join
* GroupJoin
* Left join (GroupJoin + DefaultIfEmpty)

---

# 8. Aggregation

Learn:

* Sum
* Count
* Average
* Min
* Max
* MinBy
* MaxBy
* Aggregate

---

# 9. Set Operations & Combining

Learn:

* Distinct
* DistinctBy
* Union
* Intersect
* Except
* Concat

---

# 10. Custom LINQ

Learn:

* Extension methods on `IEnumerable<T>`
* `yield return`
* `Func` vs `Expression<Func>` (the bridge to EF Core)

---

# 11. Pitfalls & Performance

Learn:

* Any vs Count
* Avoiding multiple enumeration
* When to materialize a query
* LINQ vs foreach

---

# 🛠️ Practical Implementation

## Sales Data Analytics Dashboard (Console)

Build a console dashboard that answers business questions about sales data using LINQ:

* Filtering and searching sales
* Grouping and aggregating results
* Joining related data sets
* Ordering and paging results

---

# 📝 Sprint Tasks

### Task 1 — Execution Concepts

*Day 29.* Demonstrate deferred vs immediate execution, multiple enumeration, and closures.

---

### Task 2 — Filtering & Quantifiers

*Day 30.* Practice Where, OfType, Any, All, and Contains.

---

### Task 3 — Projection & Ordering

*Day 31–32.* Practice Select, SelectMany, projection into records/DTOs, and ordering.

---

### Task 4 — Element, Conversion & Paging

*Day 33.* Practice element operators, conversion operators, Skip, and Take.

---

### Task 5 — Grouping

*Day 34–35.* Practice GroupBy with composite keys, per-group aggregates, and ToLookup.

---

### Task 6 — Joining

*Day 36–37.* Practice Join, GroupJoin, and left join.

---

### Task 7 — Aggregation & Set Operations

*Day 38.* Practice Sum, Count, Average, Min, Max, MinBy, MaxBy, Aggregate, Distinct, DistinctBy, Union, Intersect, Except, and Concat.

---

### Task 8 — Custom LINQ & Pitfalls

*Day 39.* Write extension methods on `IEnumerable<T>`, use `yield return`, compare `Func` with `Expression<Func>`, and review common LINQ pitfalls and performance decisions.

---

### Task 9 — Sprint Project

*Day 40–41.* Finish and polish the Sales Data Analytics Dashboard.

---

### Task 10 — README

*Day 42.* Document the project: overview, features, how to run, the reports and their LINQ operators, sample output, design decisions, and known limitations.

---

# 📦 Sprint Deliverables

* Practice problems for every topic
* Custom LINQ extension methods
* Sales Data Analytics Dashboard (Console)
* README

---

# ✅ Definition of Done

* [ ] Deferred vs immediate execution is understood
* [ ] Filtering, projection, and ordering queries work
* [ ] Grouping and joining queries work
* [ ] Aggregation and set operations work
* [ ] Custom LINQ extension methods work
* [ ] Common pitfalls are understood
* [ ] Sales Data Analytics Dashboard works

---

# 🎓 Expected Outcome

By the end of Sprint 3, the learner can express almost any in-memory data question as a clear LINQ query and understands how that query executes.

---

# 📅 Sprint 4 — Entity Framework Core

**Day 43–56 · Project: Library Management System (EF Core)**

## 🎯 Sprint Goal

Learn to persist and query data with EF Core: model the database, write efficient queries, understand tracking and loading, and read the SQL that EF generates.

**Working rule:** every day, look at the SQL that EF Core generates (logging or `ToQueryString()`) for the queries you wrote.

---

# 📚 What You Will Learn

## 1. Setup

Learn:

* DbContext
* Entities
* Conventions
* Code First
* Migrations (add, apply, rollback, remove, script)

---

# 2. CRUD & Seeding

Learn:

* Create, read, update, delete
* Data seeding
* What SaveChanges actually does

---

# 3. Modeling

Learn:

* One-to-Many
* Many-to-Many (skip navigations and explicit join entity)
* One-to-One
* Fluent API basics (MaxLength, Precision, Required)
* Enums as strings
* Owned entities

---

# 4. LINQ to Entities

Learn:

* How a query becomes SQL
* Building queries with IQueryable
* DTO projection with Select
* Client vs server evaluation
* What EF cannot translate

---

# 5. Loading Strategies

Learn:

* Eager loading (Include / ThenInclude, filtered Include)
* Explicit loading
* Lazy loading (awareness only)
* The N+1 problem
* Split queries vs single query

---

# 6. Change Tracking

Learn:

* Entity states
* AsNoTracking
* Attach / Update
* Connected vs disconnected updates

---

# 7. Raw SQL & Bulk Operations

Learn:

* FromSql
* ExecuteSql
* ExecuteUpdate
* ExecuteDelete

---

# 8. Concurrency & Transactions

Learn:

* RowVersion
* Optimistic concurrency
* Implicit transactions in SaveChanges
* Explicit transactions (Commit / Rollback)

---

# 9. Global Query Filters

Learn:

* Global query filters
* Soft delete

---

# 10. Performance

Learn:

* Reading the generated SQL
* Fixing N+1
* Projecting instead of loading whole entities
* Avoiding early `ToList`

---

# 🛠️ Practical Implementation

## Library Management System (EF Core)

Build the Library Management System with a real database:

* A model with One-to-Many and Many-to-Many relationships
* Migrations and seed data
* CRUD operations
* Queries using eager loading, projection, and AsNoTracking
* Transactions where multiple changes must succeed together

---

# 📝 Sprint Tasks

### Task 1 — Setup

*Day 43.* Create the DbContext and entities, and manage the schema with migrations.

---

### Task 2 — CRUD & Seeding

*Day 44.* Implement CRUD operations and seed data.

---

### Task 3 — Modeling

*Day 45–46.* Model One-to-Many, Many-to-Many, and One-to-One relationships, configure the Fluent API, map enums, and use owned entities.

---

### Task 4 — LINQ to Entities

*Day 47.* Write queries with IQueryable, project into DTOs, and review how each query becomes SQL.

---

### Task 5 — Loading Strategies

*Day 48.* Use Include / ThenInclude, reproduce and fix the N+1 problem, and compare split and single queries.

---

### Task 6 — Change Tracking

*Day 49.* Inspect entity states, use AsNoTracking, and update entities with Attach / Update.

---

### Task 7 — Raw SQL & Bulk Operations

*Day 50.* Run raw SQL and bulk updates and deletes.

---

### Task 8 — Concurrency & Transactions

*Day 51.* Add optimistic concurrency and use explicit transactions with Commit / Rollback.

---

### Task 9 — Global Query Filters

*Day 52.* Implement soft delete with a global query filter.

---

### Task 10 — Performance Review

*Day 53.* Review the project's queries, read the SQL, and fix inefficient queries.

---

### Task 11 — Sprint Project

*Day 54–55.* Finish the Library Management System.

---

### Task 12 — README

*Day 56.* Document the project: overview, features, database setup and migrations, data model, how to run, the queries, sample output, design decisions, and known limitations.

---

# 📦 Sprint Deliverables

* DbContext, entities, and migrations
* Relationships and Fluent API configuration
* Seed data
* Queries using eager loading, projection, and AsNoTracking
* Concurrency and transactions
* Soft delete with a global query filter
* Library Management System (EF Core)
* README

---

# ✅ Definition of Done

* [ ] DbContext and migrations work
* [ ] CRUD operations work
* [ ] One-to-Many and Many-to-Many relationships work
* [ ] Fluent API and owned entities are configured
* [ ] Queries are written with LINQ to Entities and DTO projection
* [ ] N+1 is understood and fixed
* [ ] Change tracking is understood
* [ ] Concurrency and transactions work
* [ ] Soft delete works
* [ ] Learner can read the SQL EF Core generates
* [ ] Library Management System works

---

# 🎓 Expected Outcome

By the end of Sprint 4, the learner can design a database model with EF Core, write efficient queries, and explain what happens between a LINQ query and the SQL that runs.

---

---

# 🏛️ Library Management API — Overview (Sprints 5–6)

The Web API is built over **two sprints of 15 days each**. Sprint 6 improves the same solution built in Sprint 5; it is not a new project.

### Domain

* **Author** (name, bio) — one author has many books
* **Book** (title, ISBN, published year, `IsAvailable` controlled by the server, author)
* **Member** (full name, unique email, optional phone, `JoinedAt` set by the server)
* **Loan** *(Sprint 6)* (book + member, loan date, due date = loan date + 14 days, return date; an active loan has no return date)

### Architecture

* **N-Tier**, three projects created all at once at the start of Sprint 5: **`LibraryApi.API`** (thin controllers, exception middleware, `Program.cs`) → **`LibraryApi.Business`** (services, DTOs, custom exceptions) → **`LibraryApi.Data`** (entities, `DbContext`, migrations, repositories, Unit of Work)
* Dependencies go one way only: `API → Business → Data`
* A controller calls **one service**. Only services use repositories / the Unit of Work. Services return **DTOs**, never entities
* The Business layer never imports EF Core: everything goes through repository methods
* The learner **writes** the Generic Repository (Sprint 5) and the Unit of Work (Sprint 6); the sprints describe *what* to build and which rules to follow, not the code

### All Endpoints (12)

| Sprint | Controller | Endpoints                                                                                       |
| ------ | ---------- | ----------------------------------------------------------------------------------------------- |
| 5      | Authors    | `GET /api/authors` (with `booksCount`) · `POST /api/authors` · `DELETE /api/authors/{id}`        |
| 5      | Books      | `GET /api/books` · `POST /api/books` · `PUT /api/books/{id}`                                     |
| 5      | Members    | `GET /api/members` · `GET /api/members/{id}` · `POST /api/members`                               |
| 6      | Loans      | `POST /api/loans` (borrow) · `PUT /api/loans/{id}/return` · `GET /api/members/{id}/loans` (lives in `LoansController`) |

> The other CRUD endpoints (for example `GET /api/books/{id}`, `DELETE` for books and members, `GET /api/loans`) are intentionally out of scope. Do not implement extra endpoints unless the instructor asks for them.

### Status Code Rules

* An id in the **URL** that does not exist → `404`
* An id in the **body** that does not exist → `400`
* A conflict with the current state of the data (duplicate ISBN or email, deleting an author with books, borrowing an unavailable book, returning a loan twice) → `409`
* Unexpected errors → `500` with a generic message and no stack trace

---

# 📅 Sprint 5 — Web API: Foundations, Services & Business Logic

**Day 57–71 · Project: Library Management API — Part 1**

## 🎯 Sprint Goal

Build the foundation of a layered Web API: a three-project solution, the entities and database (Code First), a Generic Repository, DTOs, custom exceptions with a global exception middleware, and one service per entity that owns its business rules.

---

# 📚 What You Will Learn

## 1. Solution Setup & N-Tier Architecture

Learn:

* Solution vs project, project references
* What a tier (layer) is and why we separate them
* Swagger / OpenAPI

---

# 2. Data Layer

Learn:

* Entities and relationships with EF Core Code First
* `BaseEntity` and one-to-many relationships
* Fluent API vs Data Annotations
* Connection strings and Dependency Injection (scoped lifetime)
* Migrations when the `DbContext` lives in a different project (`--project` / `--startup-project`)

---

# 3. Generic Repository

Learn:

* The Repository pattern
* Generic constraints (`where T : BaseEntity`)
* `Expression<Func<T, object>>` and `Include`

---

# 4. Controllers, DTOs & Validation

Learn:

* Controllers and `[ApiController]`
* DTOs instead of exposing entities
* Validation attributes and automatic `400` responses
* HTTP status codes (`200`, `201`, `204`, `400`, `404`, `409`, `500`)
* `CreatedAtAction`, `PUT` with `204`, and over-posting

---

# 5. Exceptions & Middleware

Learn:

* The ASP.NET Core middleware pipeline and its order
* Custom exceptions (`NotFoundException`, `BadRequestException`, `ConflictException`)
* A global exception middleware with one consistent JSON error format

---

# 6. Business Logic in Services

Learn:

* Business rules in services, not in controllers or repositories
* `404` vs `409` vs `400`
* Uniqueness checks that exclude the record itself on update
* String normalization (trim / lowercase) and server-controlled fields
* Thin controllers and DI extension methods

---

# 🛠️ Practical Implementation

## Library Management API — Part 1

### Entities

`BaseEntity`, `Author`, `Book`, `Member`, `AppDbContext`, unique indexes on `ISBN` and `Email`, and the first migration (in the Data project).

### Author Endpoints

* `GET /api/authors` — list authors with `booksCount`
* `POST /api/authors` — create an author
* `DELETE /api/authors/{id}` — delete an author only if they have no books (`404`, `409`)

### Book Endpoints

* `GET /api/books` — list books with the author name
* `POST /api/books` — create a book (author must exist, ISBN must be unique)
* `PUT /api/books/{id}` — update a book (client can never change `IsAvailable`)

### Member Endpoints

* `GET /api/members` — list members
* `GET /api/members/{id}` — get one member
* `POST /api/members` — create a member (email normalized and unique, `JoinedAt` set by the server)

### Simple Rules (plain checks in the services)

* Invalid DTO → `400`
* A missing URL id → `404`; a missing `AuthorId` in the body → `400`
* A duplicate ISBN or email (excluding the record itself on update) → `409`
* An author with books cannot be deleted → `409`
* Text input is trimmed; emails are stored trimmed and lowercase
* The server controls `IsAvailable` and `JoinedAt`

---

# 📝 Sprint Tasks

| Day | Focus | You finish the day with |
| --- | ----- | ----------------------- |
| 57 | Solution setup | Three projects, references, packages, Swagger |
| 58 | Entities and relationships | `BaseEntity`, `Author`, `Book`, `Member`, `AppDbContext` |
| 59 | Database setup | Connection string, DI registration, migration applied |
| 60 | Generic Repository | `IGenericRepository<T>` and `GenericRepository<T>` |
| 61 | **First working slice: Authors** | DTOs with validation, a minimal `AuthorService`, `GET` and `POST /api/authors` in Swagger |
| 62 | Exceptions and middleware | Custom exceptions, global middleware, consistent error format |
| 63 | Author delete with business rule | `DELETE /api/authors/{id}` with `404` and `409` |
| 64 | Book service: reads and create | List books, create book, author and ISBN checks |
| 65 | Book update | Update a book without letting the client change `IsAvailable` |
| 66 | Member service and endpoints | List members, get member by id, create member |
| 67 | Controllers and DI review | Thin controllers and correct service registrations |
| 68 | Validation tests | Run the essential test scenarios in Swagger |
| 69 | Fixes and regression | Fix failed tests and verify database constraints |
| 70 | Architecture review | Check references, no business logic in controllers, clean code |
| 71 | Wrap-up | Short README, Definition of Done, buffer time |

> Spend the first 1–2 hours of each day on the study topics, then code. Day 59 (migrations across two projects) often takes longer than planned; use the buffer days at the end (69–71) if needed.

---

# 📦 Sprint Deliverables

* Three-project solution (`LibraryApi.API`, `LibraryApi.Business`, `LibraryApi.Data`)
* Entities, `AppDbContext`, and migrations (in the Data project)
* Generic Repository
* One service per entity (`AuthorService`, `BookService`, `MemberService`) with interfaces
* DTOs, custom exceptions, and the global exception middleware
* 9 working endpoints tested in Swagger
* README (how to run, connection string, migrations, known limitations)

---

# ✅ Definition of Done

* [ ] The solution has three projects and the references are exactly `API → Business → Data`
* [ ] Every entity has its own service with an interface
* [ ] Controllers depend only on a service interface and contain no business rules
* [ ] DTOs live in the Business project, and no entity is returned from or accepted by any endpoint
* [ ] No file in `LibraryApi.Business` imports `Microsoft.EntityFrameworkCore`
* [ ] All 9 required endpoints work and were tested in Swagger
* [ ] Every row of the test scenarios table gives the expected status code
* [ ] The exception middleware is registered first, and every error uses the same JSON format
* [ ] Unique indexes exist for `ISBN` and `Email`
* [ ] Known limitations are written in the README

---

# 🎓 Expected Outcome

By the end of Sprint 5, the learner can build a layered Web API with a database, put business rules in services, and explain how a request moves from HTTP to the database and back.

---

# 📅 Sprint 6 — Web API: Unit of Work & Loans

**Day 72–86 · Project: Library Management API — Part 2**

## 🎯 Sprint Goal

Improve the same solution: refactor the data layer to the **Unit of Work** pattern, then add the **Loan** feature (borrow, return, and member loan history) with business rules that combine several entities.

---

# 📚 What You Will Learn

## 1. Unit of Work

Learn:

* The problem caused by saving inside each repository
* The Unit of Work pattern on top of the Generic Repository
* Why an operation that changes several records must be saved **once**
* Who owns the lifetime of the `DbContext` (the DI container)
* Refactoring working code safely, in small steps

---

# 2. Loan Feature

Learn:

* A new entity with two relationships and `Restrict` delete behavior
* A new service (`LoanService`) with its own business rules
* Combining rules from several entities in one service
* The order of business checks
* State-based conflicts (`409`) and the URL-id vs body-id rule

---

# 3. Routing & Controllers

Learn:

* Absolute routes (`[HttpGet("/api/...")]`)
* Why `GET /api/members/{id}/loans` lives in `LoansController` (one controller = one service)

---

# 4. Edge-Case Testing & Review

Learn:

* Designing edge-case tests (a member with exactly 3 active loans, borrowing an unavailable book, returning a loan twice)
* Reviewing transaction boundaries and layer boundaries

---

# 🛠️ Practical Implementation

## Library Management API — Part 2

### Part A — Refactor to Unit of Work

* Repositories only track changes; they no longer call `SaveChangesAsync`
* Add `IUnitOfWork` / `UnitOfWork` and register it in DI
* Switch the services to the Unit of Work; **controllers do not change**
* Run the Sprint 5 tests again as a regression check

### Part B — The Loan Feature

* `GET /api/members/{id}/loans` — list one member's loans (`404` if the member does not exist)
* `POST /api/loans` — borrow a book (`201`; `400` for a missing book or member in the body; `409` if the book is unavailable or the member already has 3 active loans)
* `PUT /api/loans/{id}/return` — return a book (`204`; `404`; `409` if already returned)

### Simple Rules (plain checks in `LoanService`)

* The book and the member (ids in the body) must exist
* The book must be available
* A member can have at most 3 active loans
* `DueDate` = `LoanDate` + 14 days
* Borrow and return each call `SaveChangesAsync()` **once**
* A loan cannot be returned twice

---

# 📝 Sprint Tasks

| Day | Focus | You finish the day with |
| --- | ----- | ----------------------- |
| 72 | Baseline and the problem | `sprint-2` branch, Sprint 5 tests pass, the "two saves" experiment done and deleted |
| 73 | Repository and Unit of Work | Repositories track changes only, `CountAsync` added, `IUnitOfWork` registered in DI |
| 74 | Switch services and regression | Services use the Unit of Work, all Sprint 5 tests pass again |
| 75 | Loan entity and migration | `Loan`, relationships (Restrict), `AddLoanTable` migration, Loan DTOs |
| 76 | Loan read by member | `GetByMemberAsync` and `GET /api/members/{id}/loans` logic |
| 77 | Borrow flow | `BorrowAsync` with availability, member limit, dates, and one save |
| 78 | Return flow | `ReturnAsync` with one save and duplicate-return protection |
| 79 | Loan controller | The three required loan endpoints |
| 80 | Borrow/return tests | Successful borrow/return and important failure cases |
| 81 | Unit of Work review | Each service saves at the correct point; the "one save only" test |
| 82 | Error handling and fixes | Fix failing tests and verify error responses |
| 83 | *Recommended extension:* concurrency, part 1 | `RowVersion` on `Book`, migration, a reproduced double-borrow problem |
| 84 | *Recommended extension:* concurrency, part 2 | Middleware returns `409` for `DbUpdateConcurrencyException` and unique-index errors |
| 85 | Architecture and code review | Remove unused methods and check layer boundaries |
| 86 | Final Review & Project Presentation | Final README, Definition of Done, and buffer time |

> Part A is a small refactor, so it fits in Days 72–74. Days 83–84 are for the optional **Concurrency** extension; if you skip it, use those two days as buffer for the required work.

---

# 📦 Sprint Deliverables

* Unit of Work (`IUnitOfWork` / `UnitOfWork`) and refactored services
* `Loan` entity, relationships, and the `AddLoanTable` migration
* `LoanService` and `LoansController`
* Loan endpoints (3), on top of the 9 endpoints from Sprint 5
* Updated README (architecture, how to run, known limitations)
* The `sprint-2` branch merged into `main`

---

# ✅ Definition of Done

* [ ] Dependencies still go only `API → Business → Data`
* [ ] No repository calls `SaveChangesAsync`; only services call it on the Unit of Work
* [ ] No controller uses `IUnitOfWork`, `DbContext`, repositories, or entities
* [ ] Every controller injects exactly one service, and `GET /api/members/{id}/loans` is in `LoansController`
* [ ] All 9 Sprint 5 endpoints still work
* [ ] Borrow and return each use **one** `SaveChangesAsync`
* [ ] The borrow and return rules are implemented, and every required test row gives the expected status code
* [ ] A missing URL id returns `404`, a missing body id returns `400`, and returning a loan twice returns `409`
* [ ] README is updated and lists the known limitations

---

# 🎁 Optional Bonus Challenges

For learners who finish the required work early. Bonus work is never required for the sprint to be complete.

* **Concurrency** (recommended first bonus): `RowVersion` on `Book`, and a middleware that returns `409` instead of `500`
* **Authentication with JWT:** ASP.NET Core Identity, register / login, `[Authorize]`, roles
* **Other ideas:** AutoMapper, xUnit + Moq, overdue loans endpoint, late fees, lazy repositories in the Unit of Work, soft delete

---

# 🎓 Expected Outcome

By the end of Sprint 6, the learner can build a complete Web API backed by a database, use the Unit of Work to save related changes as one operation, enforce real business rules across entities, and explain how a request moves from HTTP to the database and back.

---

# 🏁 Final Level Completion

A learner completes Level 1 when all six sprints are completed and the Library Management API works:

```text
Collections & Generics
     ↓
Delegates, Lambdas & Events
     ↓
LINQ
     ↓
EF Core
     ↓
Web API Foundations, Services & Business Logic
     ↓
Unit of Work & Loans
     ↓
Library Management API
```

---

# 🎯 Final Project Review

At the end of Level 1, the learner must be able to explain:

## 1. C# Foundations

Why was each collection chosen?

Why does a generic `GetById` need the `IEntity` constraint?

How do delegates, lambdas, and events work?

What is the difference between declaring and raising an event?

---

## 2. LINQ

What is the difference between deferred and immediate execution?

What is the difference between `IEnumerable` and `IQueryable`?

Why should multiple enumeration be avoided?

---

## 3. EF Core

How does a LINQ query become SQL?

What is the difference between tracking and no-tracking queries?

What is the N+1 problem and how was it fixed?

---

## 4. Web API

How does a request move from HTTP to the database and back?

Why are different HTTP methods and status codes used?

Why is an id in the URL a `404` but an id in the body a `400`?

What is the difference between Transient, Scoped, and Singleton?

---

## 5. Architecture

Why is the solution split into API, Business, and Data?

What do the Generic Repository and the Unit of Work do?

Why are DTOs used instead of returning entities?

Why does a controller call only one service?

---

## 6. Business Rules

Where are the Library API business rules enforced?

What is the difference between validation (DTO rules) and business rules?

Why do borrow and return save only once?

How are borrowing rules (availability, 3 active loans, no double return) checked?

---

## 7. Trade-offs

For important decisions, the learner should be able to explain:

```text
Problem
   ↓
Possible Solutions
   ↓
Chosen Solution
   ↓
Why?
   ↓
Trade-offs
```

The learner should not be rewarded for choosing the most sophisticated solution.

The learner should be rewarded for choosing a solution that is:

* Appropriate
* Understandable
* Maintainable
* Justified
* Consistent with the project's requirements

---

# 🧠 Engineering Practice Throughout All Sprints

These are not separate lessons. They are part of how the learner is evaluated throughout the level.

## 1. Debugging

Use:

```text
Symptom
 ↓
Observation
 ↓
Hypothesis
 ↓
Experiment
 ↓
Root Cause
 ↓
Fix
```

The learner should avoid:

> "Change random code until it works."

---

## 2. Reading the Generated SQL

From Sprint 4 onward, the learner checks the SQL that EF Core generates for the queries they write.

---

## 3. Code Review

Throughout the level, review:

* Naming
* Responsibility boundaries
* Duplication
* Error handling
* Status codes
* Query efficiency

---

## 4. Git Discipline

* Commit after every finished requirement, with the requirement ID in the message (Sprints 1–4)
* Commit before every refactor step, and work on a branch for Sprint 6
* Push every project to GitHub with a README

---

# 🚫 Explicitly Out of Scope

The following are NOT required in Level 1:

* Authentication and authorization (JWT, Identity, roles): optional bonus in Sprint 6 only
* AutoMapper and unit testing frameworks (xUnit, Moq): optional bonus ideas only
* File upload
* Reporting endpoints
* Clean Architecture, CQRS, and MediatR
* Caching and background processing
* Docker, CI/CD, and deployment
* Microservices
* Front-end

These topics belong to later levels.

---

# 🏆 Level 1 Philosophy

```text
Learn
  ↓
Practice
  ↓
Build
  ↓
Debug
  ↓
Explain
```

The goal is not:

> How many technologies did the learner use?

The goal is:

> Can the learner take a clear requirement, build a working API on top of a database, enforce real business rules, and explain the decisions behind it?

---

# Final Principle

**One Level.**

**Six Sprints.**

**One Final Project.**

**Real Business Rules.**

**No Technology for Technology's Sake.**
