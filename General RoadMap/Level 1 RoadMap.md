# Tawjeh Level 1 — Backend .NET Developer

# Sprint Specifications

> **Final Project:** Gym Management API
> **Path:** Advanced C# → LINQ → EF Core → ASP.NET Core Web API
> **Duration:** 6 Sprints × 2 Weeks (14 days each · 84 days)

---

# 🗺️ Roadmap Overview

| Sprint | Focus                                     | Days                | Project                                          |
| ------ | ----------------------------------------- | ------------------- | ------------------------------------------------ |
| 1      | Advanced C#                              | Day 1–14 (14 days)  | Job Application Tracker (Console)                |
| 2      | LINQ                                      | Day 15–28 (14 days) | Sales Data Analytics Dashboard (Console)         |
| 3      | Entity Framework Core                     | Day 29–42 (14 days) | Library Management System (EF Core)              |
| 4      | Web API Foundations — Plans & Trainers    | Day 43–56 (14 days) | Gym Management API — Part 1 (8 endpoints)        |
| 5      | Web API — Members & Sessions              | Day 57–70 (14 days) | Gym Management API — Part 2 (8 endpoints)        |
| 6      | Web API — Memberships & Bookings          | Day 71–84 (14 days) | Gym Management API — Part 3 (7 endpoints)        |

Total duration: **84 days** (6 sprints × 14 days).

---

# 🎯 Level 1 Goal

The goal of Level 1 is to take the learner from solid C# fundamentals to building a working Web API on top of a database.

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

---

# 📅 Sprint 1 — Advanced C#

**Day 1–14 · Project: Job Application Tracker (Console)**

## 🎯 Sprint Goal

Build a strong C# foundation: choosing the right collection, writing reusable generic code, and using delegates and events.

---

# 📚 What You Will Learn

## 1. Collections

Learn:

* List
* Dictionary
* HashSet
* Queue
* Stack
* How to choose the right collection

---

# 2. Generics

Learn:

* Generic classes
* Generic methods
* Constraints

---

# 3. Delegates, Lambdas & Anonymous Methods

Learn:

* Func
* Action
* Predicate
* Lambda expressions
* Anonymous methods

---

# 4. Events

Learn:

* Events
* Event handlers

---

# 🛠️ Practical Implementation

## Job Application Tracker (Console)

Build a console application that tracks job applications and uses:

* Collections and Generics
* Delegates and Lambdas
* Events

---

# 📝 Sprint Tasks

### Task 1 — Collections

*Day 1–2.* Practice List, Dictionary, HashSet, Queue, and Stack, and choose the right one for each case.

---

### Task 2 — Generics

*Day 3–4.* Write generic classes and generic methods with constraints.

---

### Task 3 — Delegates & Lambdas

*Day 5–6.* Use Func, Action, Predicate, lambda expressions, and anonymous methods.

---

### Task 4 — Events

*Day 7.* Create events and event handlers.

---

### Task 5 — Sprint Project

*Day 8–13.* Build the Job Application Tracker.

---

### Task 6 — README

*Day 14.* Document the project: overview, features, how to run, sample output, structure, design decisions, and known limitations.

---

# 📦 Sprint Deliverables

* Collections practice
* Generics practice
* Delegates, lambdas, and events practice
* Job Application Tracker (Console)
* README

---

# ✅ Definition of Done

* [ ] Right collection chosen for each case
* [ ] Generic classes and methods work
* [ ] Delegates and lambdas are used correctly
* [ ] Events and event handlers work
* [ ] Job Application Tracker works

---

# 🎓 Expected Outcome

By the end of Sprint 1, the learner can write clean, reusable C# code using collections, generics, delegates, and events.

---

# 📅 Sprint 2 — LINQ

**Day 15–28 · Project: Sales Data Analytics Dashboard (Console)**

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

*Day 15.* Demonstrate deferred vs immediate execution, multiple enumeration, and closures.

---

### Task 2 — Filtering & Quantifiers

*Day 16.* Practice Where, OfType, Any, All, and Contains.

---

### Task 3 — Projection & Ordering

*Day 17–18.* Practice Select, SelectMany, projection into records/DTOs, and ordering.

---

### Task 4 — Element, Conversion & Paging

*Day 19.* Practice element operators, conversion operators, Skip, and Take.

---

### Task 5 — Grouping

*Day 20–21.* Practice GroupBy with composite keys, per-group aggregates, and ToLookup.

---

### Task 6 — Joining

*Day 22–23.* Practice Join, GroupJoin, and left join.

---

### Task 7 — Aggregation & Set Operations

*Day 24.* Practice Sum, Count, Average, Min, Max, MinBy, MaxBy, Aggregate, Distinct, DistinctBy, Union, Intersect, Except, and Concat.

---

### Task 8 — Custom LINQ & Pitfalls

*Day 25.* Write extension methods on `IEnumerable<T>`, use `yield return`, compare `Func` with `Expression<Func>`, and review common LINQ pitfalls and performance decisions.

---

### Task 9 — Sprint Project

*Day 26–27.* Finish and polish the Sales Data Analytics Dashboard.

---

### Task 10 — README

*Day 28.* Document the project: overview, features, how to run, the reports and their LINQ operators, sample output, design decisions, and known limitations.

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

By the end of Sprint 2, the learner can express almost any in-memory data question as a clear LINQ query and understands how that query executes.

---

# 📅 Sprint 3 — Entity Framework Core

**Day 29–42 · Project: Library Management System (EF Core)**

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

*Day 29.* Create the DbContext and entities, and manage the schema with migrations.

---

### Task 2 — CRUD & Seeding

*Day 30.* Implement CRUD operations and seed data.

---

### Task 3 — Modeling

*Day 31–32.* Model One-to-Many, Many-to-Many, and One-to-One relationships, configure the Fluent API, map enums, and use owned entities.

---

### Task 4 — LINQ to Entities

*Day 33.* Write queries with IQueryable, project into DTOs, and review how each query becomes SQL.

---

### Task 5 — Loading Strategies

*Day 34.* Use Include / ThenInclude, reproduce and fix the N+1 problem, and compare split and single queries.

---

### Task 6 — Change Tracking

*Day 35.* Inspect entity states, use AsNoTracking, and update entities with Attach / Update.

---

### Task 7 — Raw SQL & Bulk Operations

*Day 36.* Run raw SQL and bulk updates and deletes.

---

### Task 8 — Concurrency & Transactions

*Day 37.* Add optimistic concurrency and use explicit transactions with Commit / Rollback.

---

### Task 9 — Global Query Filters

*Day 38.* Implement soft delete with a global query filter.

---

### Task 10 — Performance Review

*Day 39.* Review the project's queries, read the SQL, and fix inefficient queries.

---

### Task 11 — Sprint Project

*Day 40–41.* Finish the Library Management System.

---

### Task 12 — README

*Day 42.* Document the project: overview, features, database setup and migrations, data model, how to run, the queries, sample output, design decisions, and known limitations.

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

By the end of Sprint 3, the learner can design a database model with EF Core, write efficient queries, and explain what happens between a LINQ query and the SQL that runs.

---

# 🏋️ Gym Management API — Overview (Sprints 4–6)

The Web API is built over **three sprints of 14 days each**. Every sprint adds a part of the same project, so the API grows step by step.

### Domain

* **Plan** (membership plan: name, price, duration in days, active flag)
* **Trainer** (with owned Address and a specialty enum)
* **Category** (session category)
* **Member** (with owned Address, One-to-One **HealthRecord**)
* **Session** (category, trainer, capacity, start and end date)
* **MemberShip** (member + plan + start/end date, status computed from the end date)
* **MemberSession** (booking — Many-to-Many between Member and Session)

### Architecture

* 3 layers: **DAL** (entities, DbContext, configurations, repositories) · **BLL** (services, DTOs, AutoMapper profiles) · **PL** (controllers, Program.cs)
* **Provided to the learner:** Generic Repository, Unit of Work, and the seed files (`plans.json`, `categories.json`)

### All Endpoints (23)

| Sprint | Controller    | Endpoints                                                                                  |
| ------ | ------------- | ------------------------------------------------------------------------------------------ |
| 4      | Plan          | `GET /api/Plan` · `GET /api/Plan/{id}` · `PUT /api/Plan/{id}`                              |
| 4      | Trainer       | `GET` · `GET /{id}` · `POST` · `PUT /{id}` · `DELETE /{id}`                                |
| 5      | Member        | `GET` · `GET /{id}` · `POST` (with HealthRecord) · `DELETE /{id}`                          |
| 5      | Session       | `GET` · `GET /{id}` · `POST` · `DELETE /{id}`                                              |
| 6      | MemberShip    | `GET` · `POST` · `DELETE /{memberId}/{planId}`                                             |
| 6      | MemberSession | `GET` (upcoming sessions) · `GET /Upcoming/{sessionId}/Members` · `POST` · `DELETE /{memberId}/{sessionId}` |

> Out of scope for Level 1: authentication and roles, file upload, attendance, reporting endpoints.

---

# 📅 Sprint 4 — Web API Foundations: Plans & Trainers

**Day 43–56 · Project: Gym Management API — Part 1**

## 🎯 Sprint Goal

Understand how a Web API works end to end (HTTP, routing, middleware, dependency injection), build the full data layer for the Gym domain, and deliver the first two controllers: **Plan** and **Trainer**.

---

# 📚 What You Will Learn

## 1. HTTP, REST & Routing

Learn:

* HTTP methods
* Status codes
* REST conventions
* Attribute routing and route constraints (`{id:int}`)
* Swagger (default setup)

---

# 2. Middleware & Program.cs

Learn:

* Middleware pipeline basics
* How a request moves from HTTP to the controller and back
* Program.cs structure

---

# 3. Solution Structure & Dependency Injection

Learn:

* 3-layer solution: DAL, BLL, PL
* Dependency Injection
* Lifetimes (Transient, Scoped, Singleton)
* Registering services and repositories

---

# 4. Repository & Unit of Work (provided)

Learn:

* Why we use a Generic Repository and a Unit of Work
* Reading and using `GetRepository<T>()` and `SaveChanges()`

---

# 5. Data Layer

Learn:

* Entities and DbContext in a Web API
* Fluent API with `IEntityTypeConfiguration`
* Check constraints and unique indexes
* Migrations
* Seeding from JSON files

---

# 6. Controllers, DTOs & Mapping

Learn:

* `[ApiController]` and controllers
* Action results (Ok, NotFound, BadRequest, CreatedAtAction, NoContent)
* DTOs
* AutoMapper profiles

---

# 7. Validation

Learn:

* Data Annotations
* Automatic 400 responses with `[ApiController]`
* Returning the correct status codes

---

# 🛠️ Practical Implementation

## Gym Management API — Part 1

### Data Layer (all entities)

Create all seven entities, the DbContext, the Fluent API configurations, the first migration, and the seed data. The relationships are used in the next sprints.

---

### Plan Endpoints

* `GET /api/Plan` — list all plans
* `GET /api/Plan/{id}` — plan details (400 for invalid id, 404 if not found)
* `PUT /api/Plan/{id}` — update a plan

---

### Trainer Endpoints

* `GET /api/Trainer` — list all trainers
* `GET /api/Trainer/{id}` — trainer details
* `POST /api/Trainer` — create a trainer
* `PUT /api/Trainer/{id}` — update a trainer
* `DELETE /api/Trainer/{id}` — delete a trainer

---

### Simple Rules (plain checks in the service)

* A plan that has an active membership cannot be updated
* Trainer email and phone must be unique
* A trainer who has upcoming sessions cannot be deleted
* Phone must be a valid Egyptian number and email must be valid (Data Annotations)

---

# 📝 Sprint Tasks

### Task 1 — HTTP, REST & Routing

*Day 43–44.* Create the Web API project, practice HTTP methods, status codes, attribute routing, and Swagger.

---

### Task 2 — Middleware, Structure & Dependency Injection

*Day 45–46.* Learn the middleware pipeline, organize the solution into DAL / BLL / PL, register services with the right lifetimes, and read the provided Repository and Unit of Work.

---

### Task 3 — Data Layer

*Day 47–49.* Create the entities and DbContext, configure them with the Fluent API, add the first migration, and seed Plans and Categories from JSON.

---

### Task 4 — DTOs, AutoMapper & Validation

*Day 50–51.* Create the DTOs and AutoMapper profiles, and add Data Annotations validation.

---

### Task 5 — Plan Endpoints

*Day 52.* Build the Plan controller and service.

---

### Task 6 — Trainer Endpoints

*Day 53–54.* Build the Trainer controller and service with the simple rules.

---

### Task 7 — Test & Review

*Day 55.* Test every endpoint with Swagger and fix bugs.

---

### Task 8 — README

*Day 56.* Document the project: overview, architecture, how to run, the endpoint map, concepts explained, and known limitations.

---

# 📦 Sprint Deliverables

* 3-layer solution (DAL / BLL / PL)
* DbContext, Fluent API configurations, migration, and seed data
* Plan and Trainer endpoints (8)
* DTOs, AutoMapper profiles, and validation
* Swagger documentation
* README

---

# ✅ Definition of Done

* [ ] HTTP methods, status codes, and routing are understood
* [ ] Middleware pipeline and DI lifetimes are understood
* [ ] Migration and seeding work
* [ ] Plan endpoints work
* [ ] Trainer endpoints work
* [ ] Simple rules are enforced
* [ ] Validation returns the correct status codes
* [ ] Swagger is available

---

# 🎓 Expected Outcome

By the end of Sprint 4, the learner can build a layered Web API with a database, and explain how a request moves from HTTP to the database and back.

---

# 📅 Sprint 5 — Web API: Members & Sessions

**Day 57–70 · Project: Gym Management API — Part 2**

## 🎯 Sprint Goal

Work with related data in the API: One-to-One and One-to-Many relationships, eager loading, computed fields, and business rules that depend on other tables.

---

# 📚 What You Will Learn

## 1. Relationships in the API

Learn:

* One-to-One (Member and HealthRecord)
* One-to-Many (Trainer → Sessions, Category → Sessions)
* Owned entity (Address)
* Navigation properties

---

# 2. Loading Related Data

Learn:

* Eager loading with `Include`
* Mapping related data into DTOs
* Computed fields (available slots, session status)

---

# 3. Nested DTOs & Validation

Learn:

* Nested DTOs (Member with HealthRecord)
* Validation of nested objects
* Date and range validation

---

# 4. Business Rules in Services

Learn:

* Checking that related records exist before saving
* Rules that depend on dates
* Choosing between 400 BadRequest and 404 NotFound

---

# 🛠️ Practical Implementation

## Gym Management API — Part 2

### Member Endpoints

* `GET /api/Member` — list all members
* `GET /api/Member/{id}` — member details with the active plan name and membership dates (if any)
* `POST /api/Member` — create a member together with the HealthRecord
* `DELETE /api/Member/{id}` — delete a member

---

### Session Endpoints

* `GET /api/Session` — list sessions with category name, trainer name, and available slots
* `GET /api/Session/{id}` — session details
* `POST /api/Session` — create a session
* `DELETE /api/Session/{id}` — delete a session

---

### Simple Rules (plain checks in the service)

* Member email and phone must be unique
* A member with upcoming booked sessions cannot be deleted (deleting a member also removes their memberships)
* A session needs an existing trainer and an existing category
* A session end date must be after its start date
* Session capacity must be between 1 and 25
* A session can be deleted only if it is completed and has no bookings
* Available slots = capacity − number of bookings

---

# 📝 Sprint Tasks

### Task 1 — Relationships & Loading

*Day 57–58.* Review the relationships between Member, HealthRecord, Trainer, Category, and Session, and practice `Include` to load related data into DTOs.

---

### Task 2 — Member Endpoints

*Day 59–63.* Build the Member controller and service: nested DTO with HealthRecord, unique checks, member details with the active membership, and delete rules.

---

### Task 3 — Session Endpoints

*Day 64–68.* Build the Session controller and service: related-data checks, date and capacity validation, available slots, and delete rules.

---

### Task 4 — Test & Review

*Day 69.* Test every endpoint with Swagger and fix bugs.

---

### Task 5 — README

*Day 70.* Update the README: relationships, the extended endpoint map, business rules, and how to run.

---

# 📦 Sprint Deliverables

* Member endpoints (4)
* Session endpoints (4)
* Nested DTOs and validation
* Business rules enforced
* README

---

# ✅ Definition of Done

* [ ] One-to-One and One-to-Many relationships work in the API
* [ ] `Include` is used to load related data
* [ ] A member is created together with its HealthRecord
* [ ] Member endpoints work
* [ ] Session endpoints work
* [ ] Available slots and session status are computed correctly
* [ ] All simple rules are enforced
* [ ] Validation returns the correct status codes

---

# 🎓 Expected Outcome

By the end of Sprint 5, the learner can build endpoints that work with related data and enforce business rules that depend on other tables and on dates.

---

# 📅 Sprint 6 — Web API: Memberships & Bookings

**Day 71–84 · Project: Gym Management API — Part 3**

## 🎯 Sprint Goal

Finish the API with the endpoints that connect everything: memberships and class bookings. This sprint is about Many-to-Many relationships and time-based business rules.

---

# 📚 What You Will Learn

## 1. Many-to-Many with Extra Data

Learn:

* Join entities (MemberShip: Member + Plan, MemberSession: Member + Session)
* Composite keys
* Extra columns on the join entity (start/end date, booking date)

---

# 2. Time-Based Business Rules

Learn:

* Active vs expired memberships (end date vs now)
* Upcoming sessions
* Capacity checks

---

# 3. Multi-Rule Validation in Services

Learn:

* Checking several rules in the right order
* Returning the correct status code for each failure

---

# 4. Testing & Documentation

Learn:

* Testing the full API with Swagger
* Writing a README for an API

---

# 🛠️ Practical Implementation

## Gym Management API — Part 3

### MemberShip Endpoints

* `GET /api/MemberShip` — list memberships with member name, plan name, start date, end date, and status (Active / Expired)
* `POST /api/MemberShip` — subscribe a member to a plan
* `DELETE /api/MemberShip/{memberId}/{planId}` — cancel an active membership

---

### MemberSession (Booking) Endpoints

* `GET /api/MemberSession` — list upcoming sessions with available slots
* `GET /api/MemberSession/Upcoming/{sessionId}/Members` — members booked in an upcoming session, with booking date
* `POST /api/MemberSession` — book a session for a member
* `DELETE /api/MemberSession/{memberId}/{sessionId}` — cancel a booking

---

### Simple Rules (plain checks in the service)

**MemberShip**

* The member and the plan must exist
* The plan must be active
* A member cannot have two active memberships at the same time
* End date = start date + plan duration in days
* Only an active (not expired) membership can be cancelled

**Booking**

* The member and the session must exist
* The session must be upcoming
* The member must have an active membership
* A member cannot book the same session twice
* A session cannot exceed its capacity
* A booking can be cancelled only before the session starts

---

# 📝 Sprint Tasks

### Task 1 — Many-to-Many & Time-Based Rules

*Day 71–72.* Review the join entities and composite keys, and practice date-based checks (active vs expired, upcoming).

---

### Task 2 — MemberShip Endpoints

*Day 73–76.* Build the MemberShip controller and service with all membership rules.

---

### Task 3 — Booking Endpoints

*Day 77–81.* Build the MemberSession controller and service with all booking rules.

---

### Task 4 — Final Testing

*Day 82–83.* Test the full API with Swagger and fix bugs.

---

### Task 5 — README

*Day 84.* Write the final README: overview, architecture, data model, business rules, endpoint map, how to run, the end-to-end flow, and known limitations.

---

# 📦 Sprint Deliverables

* MemberShip endpoints (3)
* Booking endpoints (4)
* All business rules enforced
* Complete Gym Management API (23 endpoints)
* Swagger documentation
* README

---

# ✅ Definition of Done

* [ ] Many-to-Many with composite keys works
* [ ] A member can subscribe to a plan and cancel the membership
* [ ] A member cannot have two active memberships
* [ ] A member can book and cancel a session
* [ ] Booking rules (active membership, capacity, no duplicate) are enforced
* [ ] All status codes are correct
* [ ] Swagger is available
* [ ] README is complete

---

# 🎓 Expected Outcome

By the end of Sprint 6, the learner can build a complete Web API backed by a database, enforce real business rules, and explain how a request moves from HTTP to the database and back.

---

# 🏁 Final Level Completion

A learner completes Level 1 when all six sprints are completed and the Gym Management API works:

```text
Advanced C#
     ↓
LINQ
     ↓
EF Core
     ↓
HTTP & Routing
     ↓
Web API Foundations (Plans & Trainers)
     ↓
Members & Sessions
     ↓
Memberships & Bookings
     ↓
Gym Management API
```

---

# 🎯 Final Project Review

At the end of Level 1, the learner must be able to explain:

## 1. C# Foundations

Why was each collection chosen?

How do delegates, lambdas, and events work?

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

What is the difference between Transient, Scoped, and Singleton?

---

## 5. Architecture

Why is the solution split into DAL, BLL, and PL?

What do the Repository and Unit of Work do?

Why are DTOs used instead of returning entities?

---

## 6. Business Rules

Where are the Gym API business rules enforced?

What is the difference between validation (Data Annotations) and business rules?

How are membership and booking rules checked?

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

From Sprint 3 onward, the learner checks the SQL that EF Core generates for the queries they write.

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

# 🚫 Explicitly Out of Scope

The following are NOT part of Level 1:

* Authentication and authorization (JWT, Identity, roles)
* File upload
* Attendance and reporting endpoints
* Clean Architecture, CQRS, and MediatR
* Caching and background processing
* Unit and integration testing frameworks
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
