# Sprint 4 — Web API Foundations — Plans & Trainers

> Project: **Gym Management API — Part 1 (8 endpoints)**
> Duration: **2 weeks (14 days) · Day 43–56**

## Sprint Goal

Understand how a Web API works end to end (HTTP, routing, middleware, dependency injection), build the full data layer for the Gym domain, and deliver the first two controllers: **Plan** and **Trainer**.

## How to use this sprint plan

For every task:

1. Read the **MUST** resources before implementation.
2. Read **DEEP DIVE** only when the concept is unclear or the learner wants deeper understanding.
3. Implement the requested change.
4. Verify the behavior manually.
5. Commit the work in small, meaningful commits.
6. Be able to explain the decision during review.

**Important:** This document does not change the original Sprint scope. It expands the original tasks into an executable checklist.

## Project Brief — Gym Management API

A gym backend: manage membership plans and trainers, register members with their health record, schedule sessions, subscribe members to plans, and let them book sessions. The API is built over **three sprints**; each sprint ends with something you can run and test in Swagger.

Stack: .NET · EF Core + SQL Server · AutoMapper · Swagger

### Architecture: Three Projects, One Job Each

| Project | Responsibility | References |
| --- | --- | --- |
| **GymManagementPL** | Controllers, `Program.cs`, Swagger, `wwwroot/Files` (seed JSON) | GymManagementBLL, GymManagementDAL |
| **GymManagementBLL** | Services + interfaces, DTOs, AutoMapper profiles, business rules | GymManagementDAL |
| **GymManagementDAL** | Entities, `DbContext`, Fluent API configurations, migrations, seeding, Repository + Unit of Work | nothing |

**Golden rule:** controllers never touch a `DbContext` or a repository directly; they call a service. Services use `IUnitOfWork`, never the `DbContext`.

**Provided to the learner:** Generic Repository, Unit of Work, and the seed files (`plans.json`, `categories.json`).

### Data Model

All entities have `Id` (int), `CreatedAt` (DateTime), and `UpdatedAt` (DateTime?).

**GymUser** (abstract base of Trainer and Member)

| Field | Type / Rule |
| --- | --- |
| Name | string, required |
| Email | string, unique |
| Phone | string, unique |
| DateOfBirth | DateOnly |
| Gender | enum { Male = 1, Female = 2 } |
| Address | owned entity: BuildingNumber (int), Street, City |

**Trainer** (extends GymUser) — `Specialties` enum { GeneralFitness = 1, Yoga = 2, Boxing = 3, Crossfit = 4 }. One Trainer has many Sessions.

**Member** (extends GymUser) — `JoinDate` = `CreatedAt`. One-to-One with HealthRecord. Many MemberShips and many MemberSessions.

**HealthRecord**

| Field | Type / Rule |
| --- | --- |
| Height | decimal |
| Weight | decimal |
| BloodType | string, max 3 characters |
| Note | string?, optional |

One-to-One with Member (same key as the member).

**Plan**

| Field | Type / Rule |
| --- | --- |
| Name | string, max 50 |
| Description | string, max 200 |
| DurationDays | int, between 1 and 365 |
| Price | decimal |
| IsActive | bool |

**Category** — `CategoryName` (string, max 20). One Category has many Sessions.

**Session**

| Field | Type / Rule |
| --- | --- |
| Description | string |
| Capacity | int, between 1 and 25 |
| StartDate, EndDate | DateTime, `EndDate` must be after `StartDate` |
| CategoryId | int, FK to Category |
| TrainerId | int, FK to Trainer |

**MemberShip** (join: Member + Plan, composite key `MemberId + PlanId`)

| Field | Type / Rule |
| --- | --- |
| StartDate | = `CreatedAt` |
| EndDate | DateTime, start date + plan duration in days |
| Status | computed, not stored: `Expired` when `EndDate <= now`, otherwise `Active` |

**MemberSession** (join: Member + Session, composite key `MemberId + SessionId`) — `BookingDate` = `CreatedAt`.

### Seed Data

`plans.json` and `categories.json` in `wwwroot/Files`. Seed each table only when it is empty. Category ids start from 1, so no endpoint is needed to list them.

### Endpoint Map: What the Finished API Exposes (23)

| Sprint | Controller | Endpoints |
| --- | --- | --- |
| 4 | Plan | `GET api/Plan` · `GET api/Plan/{id}` · `PUT api/Plan/{id}` |
| 4 | Trainer | `GET api/Trainer` · `GET api/Trainer/{id}` · `POST api/Trainer` · `PUT api/Trainer/{id}` · `DELETE api/Trainer/{id}` |
| 5 | Member | `GET api/Member` · `GET api/Member/{id}` · `POST api/Member` · `DELETE api/Member/{id}` |
| 5 | Session | `GET api/Session` · `GET api/Session/{id}` · `POST api/Session` · `DELETE api/Session/{id}` |
| 6 | MemberShip | `GET api/MemberShip` · `POST api/MemberShip` · `DELETE api/MemberShip/{memberId}/{planId}` |
| 6 | MemberSession | `GET api/MemberSession` · `GET api/MemberSession/Upcoming/{sessionId}/Members` · `POST api/MemberSession` · `DELETE api/MemberSession/{memberId}/{sessionId}` |

> **Response convention (same as the reference solution):** success returns `200` with the data or a short success message; an invalid id or a failed rule / validation returns `400` with a message; a missing record returns `404`. Model-validation errors are returned automatically as `400` by `[ApiController]`.

### How the Project Is Split

| Sprint | Days | What you finish |
| --- | --- | --- |
| 4 | Day 43–56 | Foundations, the full data layer (all 7 entities), Plan and Trainer endpoints |
| 5 | Day 57–70 | Member (+ HealthRecord) and Session endpoints |
| 6 | Day 71–84 | MemberShip and booking endpoints, final testing, README |

> **Out of scope for this level:** authentication and roles, file upload, attendance, reporting endpoints.

### Validation Rules for Input DTOs

| Field | Rule |
| --- | --- |
| Name | required, 2–50 characters, letters and spaces only |
| Email | required, valid format, 5–100 characters |
| Phone | required, valid Egyptian number: starts with 010 / 011 / 012 / 015 followed by 8 digits |
| DateOfBirth, Gender | required |
| BuildingNumber | required, between 1 and 9000 |
| Street | required, 2–30 characters |
| City | required, 2–30 characters, letters and spaces only |


## Working Rules

- Finish each task before starting the next one, and commit after every finished task.
- Business rules live in the **services**, never in the controllers.
- Controllers return DTOs, never entities.
- Test every endpoint in Swagger, for both success and failure cases.
- Push to GitHub with a README (see the README task).

## Sprint Tasks

### Task 1 — HTTP, REST & Routing

*Day 43–44.*

**Goal** — A running empty API with the right project and a Swagger page.

**Tasks**
- [ ] Explain in your README: what an API is, REST vs SOAP vs GraphQL (3 lines each), and the status code each Gym endpoint should return.
- [ ] Create the ASP.NET Core Web API project `GymManagementPL`.
- [ ] Add a temporary `PingController` (`GET api/Ping`) to practise attribute routing; add a route with a constraint (`{id:int}`). Remove the Ping controller at the end of the task.
- [ ] Make sure Swagger is available at `/swagger`.
- [ ] Test the same endpoint with Swagger, Postman, and a `.http` file.

**Endpoints**

| Method & Route | Access | OK | Errors / Notes |
| --- | --- | --- | --- |
| `GET api/Ping` | Public | 200 | Temporary; returns text |

**Done When**
- [ ] `dotnet run` shows Swagger at `/swagger`
- [ ] The three test tools return the same result from the same endpoint
- [ ] No password or real connection-string credentials are committed

### Resources
- **MUST** — [HTTP request methods (MDN)](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods)
- **MUST** — [HTTP response status codes (MDN)](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status)
- **MUST** — [Web API design best practices](https://learn.microsoft.com/en-us/azure/architecture/best-practices/api-design)
- **MUST** — [Tutorial: Create a web API with ASP.NET Core](https://learn.microsoft.com/en-us/aspnet/core/tutorials/first-web-api)
- **MUST** — [Routing to controller actions](https://learn.microsoft.com/en-us/aspnet/core/mvc/controllers/routing)
- **DEEP DIVE** — Andrew Lock — ASP.NET Core in Action — `Book: ASP.NET Core in Action`

---

### Task 2 — Middleware, Structure & Dependency Injection

*Day 45–46.*

**Goal** — A layered solution with correct references and a clean `Program.cs`.

**Tasks**
- [ ] Create the solution with 3 projects (`GymManagementDAL`, `GymManagementBLL`, `GymManagementPL`) and set the references as in the architecture table. DAL references nothing.
- [ ] Read the provided Generic Repository and Unit of Work. In your README, write 5 lines: what each one does and why services use `IUnitOfWork` instead of the `DbContext`.
- [ ] Clean up `Program.cs`: services first, then middleware in the right order. List the order in the README.
- [ ] Register the `DbContext` (connection string in `appsettings.Development.json`), the Unit of Work, and the services with the right lifetimes.
- [ ] README: why `DbContext` is Scoped (not Singleton), and what Transient, Scoped, and Singleton mean.

**Done When**
- [ ] The solution builds and the references match the architecture table
- [ ] No controller uses a `DbContext` or a repository directly

### Resources
- **MUST** — [ASP.NET Core Middleware](https://learn.microsoft.com/en-us/aspnet/core/fundamentals/middleware/)
- **MUST** — [Dependency injection in ASP.NET Core](https://learn.microsoft.com/en-us/aspnet/core/fundamentals/dependency-injection)
- **MUST** — [Implementing the Repository and Unit of Work patterns](https://learn.microsoft.com/en-us/aspnet/mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application)

---

### Task 3 — Data Layer

*Day 47–49.*

**Goal** — Gym data lives in SQL Server, is created by migrations, and Plans and Categories are filled from JSON at startup.

**Tasks**
- [ ] Create `BaseEntity` and all the entities in the Data Model: GymUser (abstract), Trainer, Member, HealthRecord, Plan, Category, Session, MemberShip, MemberSession, plus the `Gender` and `Specialties` enums and the owned `Address`.
- [ ] Create `GymDbContext` and Fluent API configurations (`IEntityTypeConfiguration<T>`): required fields, max lengths, unique indexes on `Email` and `Phone`, check constraints (`DurationDays` between 1 and 365, `Capacity` between 1 and 25, `EndDate > StartDate`), and the composite keys of MemberShip and MemberSession.
- [ ] Configure the relationships: Trainer–Session, Category–Session, Member–HealthRecord (One-to-One), Member–MemberShip–Plan, Member–MemberSession–Session.
- [ ] Add the connection string, create the first migration, and update the database.
- [ ] Seed Plans and Categories from `plans.json` and `categories.json`. Seed only when the tables are empty.

**Done When**
- [ ] The first run creates the database, applies the migration, and seeds both tables; the second run does not duplicate data
- [ ] The tables, keys, and constraints in SQL Server match the Data Model

### Resources
- **MUST** — [Relationships](https://learn.microsoft.com/en-us/ef/core/modeling/relationships)
- **MUST** — [Creating and configuring a model](https://learn.microsoft.com/en-us/ef/core/modeling/)
- **MUST** — [Migrations overview](https://learn.microsoft.com/en-us/ef/core/managing-schemas/migrations/)
- **MUST** — [Data seeding](https://learn.microsoft.com/en-us/ef/core/modeling/data-seeding)
- **DEEP DIVE** — Jon P. Smith — Entity Framework Core in Action — `Book: Entity Framework Core in Action`

---

### Task 4 — DTOs, AutoMapper & Validation

*Day 50–51.*

**Goal** — Controllers never expose entities, and input is validated before it reaches a service.

**Tasks**
- [ ] Create the Plan DTOs: `PlanDTO` (Id, Name, Description, DurationDays, Price, IsActive) and `PlanToUpdateDTO` (Name, Description 5–50 characters, DurationDays 1–365, Price 0.1–10000).
- [ ] Create the Trainer DTOs: `TrainerDTO` (Id, Name, Email, Phone, Specialization), `TrainerDetailsDTO` (+ DateOfBirth, Address as text), `CreateTrainerDTO`, and `TrainerToUpdateDTO`.
- [ ] Add Data Annotations using the validation table in the brief (name, email, Egyptian phone, address fields, `Specialties` enum).
- [ ] Create the AutoMapper profile for Plan and Trainer (including the address fields and the enum shown as text).
- [ ] README: why entities are not returned directly from the API.

**Done When**
- [ ] An invalid phone, a short name, or a missing field returns `400` with the validation message
- [ ] Responses contain DTOs only, never entities

### Resources
- **MUST** — [AutoMapper — Getting started](https://docs.automapper.org/en/stable/Getting-started.html)
- **MUST** — [Model validation in ASP.NET Core](https://learn.microsoft.com/en-us/aspnet/core/mvc/models/validation)
- **MUST** — [Create web APIs with ASP.NET Core](https://learn.microsoft.com/en-us/aspnet/core/web-api/)

---

### Task 5 — Plan Endpoints

*Day 52.*

**Goal** — The first real controller: list, read, and update plans.

**Tasks**
- [ ] Create `IPlanService` / `PlanService` and `PlanController`.
- [ ] Business rule: a plan that has an active membership cannot be updated.
- [ ] Return the correct status code for every case (see the table).

**Endpoints**

| Method & Route | Access | OK | Errors / Notes |
| --- | --- | --- | --- |
| `GET api/Plan` | Public | 200 | List of all plans |
| `GET api/Plan/{id}` | Public | 200 | 400 when the id is zero or negative, 404 when the plan does not exist |
| `PUT api/Plan/{id}` | Public | 200 | 400 on validation errors, plan not found, or the plan has an active membership |

**Done When**
- [ ] All 3 endpoints work in Swagger
- [ ] The rule is enforced in the service, not in the controller

### Resources
- **MUST** — [Controller action return types](https://learn.microsoft.com/en-us/aspnet/core/web-api/action-return-types)

---

### Task 6 — Trainer Endpoints

*Day 53–54.*

**Goal** — Full CRUD for trainers with uniqueness and delete rules.

**Tasks**
- [ ] Create `ITrainerService` / `TrainerService` and `TrainerController`.
- [ ] Rules: trainer email and phone must be unique (on create, and on update against other trainers). A trainer who has upcoming sessions cannot be deleted.
- [ ] Return the correct status code for every case (see the table).

**Endpoints**

| Method & Route | Access | OK | Errors / Notes |
| --- | --- | --- | --- |
| `GET api/Trainer` | Public | 200 | List of all trainers |
| `GET api/Trainer/{id}` | Public | 200 | 400 id zero or negative, 404 not found |
| `POST api/Trainer` | Public | 200 | 400 on validation errors or duplicate email / phone |
| `PUT api/Trainer/{id}` | Public | 200 | 400 on validation errors, duplicate email / phone, or trainer not found |
| `DELETE api/Trainer/{id}` | Public | 200 | 400 id invalid or trainer has upcoming sessions, 404 not found |

**Sample request — `POST api/Trainer`**

```json
{
  "name": "Ahmed Hassan",
  "email": "ahmed.hassan@gym.com",
  "phone": "01012345678",
  "specialties": 2,
  "gender": 1,
  "dateOfBirth": "1992-05-14",
  "buildingNumber": 12,
  "street": "Nile Street",
  "city": "Cairo"
}
```

`gender`: 1 = Male, 2 = Female. `specialties`: 1 = GeneralFitness, 2 = Yoga, 3 = Boxing, 4 = Crossfit.

**Done When**
- [ ] All 5 endpoints work in Swagger
- [ ] Duplicate email or phone is refused
- [ ] A trainer with upcoming sessions cannot be deleted

### Resources
- **MUST** — [Controller action return types](https://learn.microsoft.com/en-us/aspnet/core/web-api/action-return-types)

---

### Task 7 — Test & Review

*Day 55.*

**Goal** — Every endpoint is tested.

**Tasks**
- [ ] Test every endpoint with Swagger (success and failure cases) and fix the bugs you find.

**Done When**
- [ ] All 8 endpoints behave as in the endpoint tables

---

### Task 8 — README

*Day 56.*

Document:
- Project overview
- Architecture (DAL / BLL / PL and the references between them)
- How to run (connection-string placeholder, migration command, run command)
- Endpoint map for all 8 endpoints
- Concepts explained (REST vs SOAP vs GraphQL, Repository and Unit of Work, middleware order, DI lifetimes, why entities are not returned)
- Known limitations

**Required output**
- Endpoint map as a table: Method, Route, Access, Success code, Error codes
- A `README.md` in the repository root
- Commands in code blocks that run as written
- Screenshots or sample output stored in the repo
- No secrets or real credentials

### Resources
- **MUST** — [GitHub — About READMEs](https://docs.github.com/en/repositories/managing-your-repositorys-settings-and-features/customizing-your-repository/about-readmes)

---

## Test Scenarios the Reviewer Will Try

| # | Scenario | Expected result |
| --- | --- | --- |
| 1 | `GET api/Plan/0` | 400 with a message |
| 2 | `GET api/Plan/999` | 404 `Plan Not Found` |
| 3 | `PUT api/Plan/{id}` for a plan that has an active membership | Refused (400), plan unchanged |
| 4 | `POST api/Trainer` with phone `0123` | 400 validation message |
| 5 | `POST api/Trainer` with an email that already exists | 400, nothing created |
| 6 | `DELETE api/Trainer/{id}` for a trainer with upcoming sessions | 400, trainer still exists |
| 7 | Run the app twice | No duplicate Plans or Categories |

## What the Reviewer Checks

| Area | What is checked |
| --- | --- |
| Architecture | Three projects with the right references; controllers only call services; no `DbContext` outside the DAL |
| Correctness | Right status codes; business rules enforced in the services; DTOs returned, never entities |
| Data | Migration works; seeding runs once; constraints and keys exist in SQL Server |
| Errors | Validation messages are clear; failed rules return `400`; missing records return `404` |
| Docs | README complete; Swagger shows every endpoint |

## Sprint Deliverables

- [ ] 3-layer solution (DAL / BLL / PL)
- [ ] DbContext, Fluent API configurations, migration, and seed data
- [ ] Plan and Trainer endpoints (8)
- [ ] DTOs, AutoMapper profiles, and validation
- [ ] Swagger documentation
- [ ] README

## Definition of Done

- [ ] HTTP methods, status codes, and routing are understood
- [ ] Middleware pipeline and DI lifetimes are understood
- [ ] Migration and seeding work
- [ ] Plan endpoints work
- [ ] Trainer endpoints work
- [ ] Simple rules are enforced
- [ ] Validation returns the correct status codes
- [ ] Swagger is available

## Before You Submit

- [ ] The solution builds with no errors, and the migration and seed data work on a fresh database
- [ ] Every endpoint in this sprint works in Swagger and returns the status codes in its endpoint table
- [ ] No passwords or real connection-string credentials are in the repo
- [ ] The README is complete and the commit history is meaningful

## Review Rule

The learner should be able to explain not only **what** was implemented, but **why it belongs there, what alternatives existed, and what trade-off was accepted**.
