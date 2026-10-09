# Sprint 5 — Web API — Members & Sessions

> Project: **Gym Management API — Part 2 (8 endpoints)**
> Duration: **2 weeks (14 days) · Day 57–70**

## Sprint Goal

Work with related data in the API: One-to-One and One-to-Many relationships, eager loading, computed fields, and business rules that depend on other tables.

## How to use this sprint plan

For every task:

1. Read the **MUST** resources before implementation.
2. Read **DEEP DIVE** only when the concept is unclear or the learner wants deeper understanding.
3. Implement the requested change.
4. Verify the behavior manually.
5. Commit the work in small, meaningful commits.
6. Be able to explain the decision during review.

**Important:** This document does not change the original Sprint scope. It expands the original tasks into an executable checklist.

## Project Brief — Gym Management API, Part 2

This sprint adds the endpoints that depend on other tables: **Member** (with its HealthRecord) and **Session** (with its Trainer and Category).

The full project brief (architecture, data model, validation rules, and endpoint map) is in **Sprint 4**.

### Data You Work With

**Member** — Name, Email, Phone, DateOfBirth, Gender, Address (owned), and a One-to-One **HealthRecord** (Height, Weight, BloodType up to 3 characters, optional Note). `JoinDate` = `CreatedAt`.

**Session** — Description, Capacity (1–25), StartDate, EndDate, CategoryId, TrainerId. Available slots = capacity − number of bookings.

### Validation Rules

| Input | Rule |
| --- | --- |
| Member (create) | Same rules as Trainer (name, email, Egyptian phone, address); `HealthRecord` is required |
| HealthRecord | Height between 0.1 and 300, Weight between 0.1 and 500, BloodType up to 3 characters |
| Session (create) | Description 10–500 characters, Capacity 1–25, StartDate, EndDate, TrainerId, CategoryId required |

> **Response convention (same as the reference solution):** success returns `200` with the data or a short success message; an invalid id or a failed rule / validation returns `400` with a message; a missing record returns `404`. Model-validation errors are returned automatically as `400` by `[ApiController]`.


## Working Rules

- Finish each task before starting the next one, and commit after every finished task.
- Business rules live in the **services**, never in the controllers.
- Controllers return DTOs, never entities.
- Test every endpoint in Swagger, for both success and failure cases.
- Push to GitHub with a README (see the README task).

## Sprint Tasks

### Task 1 — Relationships & Loading

*Day 57–58.*

**Goal** — Understand the relationships between the tables and load related data into DTOs.

**Tasks**
- [ ] README: explain the One-to-One (Member and HealthRecord) and One-to-Many (Trainer → Sessions, Category → Sessions) relationships, and the owned `Address`.
- [ ] Practise `Include` / `ThenInclude`: write a query that loads sessions with their category and trainer, and map the result into a DTO.
- [ ] Print the SQL of that query and check that it is a single query with joins (no N+1).

**Done When**
- [ ] The sessions query runs as one SQL query with joins
- [ ] You can explain what `Include` does and what happens without it

### Resources
- **MUST** — [Relationships](https://learn.microsoft.com/en-us/ef/core/modeling/relationships)
- **MUST** — [Loading related data](https://learn.microsoft.com/en-us/ef/core/querying/related-data/)
- **MUST** — [Owned entity types](https://learn.microsoft.com/en-us/ef/core/modeling/owned-entities)
- **DEEP DIVE** — Jon P. Smith — Entity Framework Core in Action — `Book: Entity Framework Core in Action`

---

### Task 2 — Member Endpoints

*Day 59–63.*

**Goal** — Register members together with their health record, read them with their active plan, and delete them safely.

**Tasks**
- [ ] Create the Member DTOs: `MemberDTO` (Id, Name, Email, Phone, Gender, DateOfBirth, Address, PlanName, MembershipStartDate, MembershipEndDate), `CreateMemberDTO` (with a nested `HealthRecordDTO`), and `HealthRecordDTO`.
- [ ] Create `IMemberService` / `MemberService` and `MemberController`.
- [ ] Rules: member email and phone must be unique. A member with upcoming booked sessions cannot be deleted. Deleting a member also removes their memberships.
- [ ] `GET api/Member/{id}` shows the active plan name and the membership dates when the member has an active membership.

**Endpoints**

| Method & Route | Access | OK | Errors / Notes |
| --- | --- | --- | --- |
| `GET api/Member` | Public | 200 | List of all members |
| `GET api/Member/{id}` | Public | 200 | 400 id zero or negative, 404 not found |
| `POST api/Member` | Public | 200 | 400 on validation errors (including a missing HealthRecord) or duplicate email / phone |
| `DELETE api/Member/{id}` | Public | 200 | 400 id invalid or member has upcoming booked sessions, 404 not found |

**Sample request — `POST api/Member`**

```json
{
  "name": "Sara Ali",
  "email": "sara.ali@mail.com",
  "phone": "01123456789",
  "dateOfBirth": "1998-03-21",
  "gender": 2,
  "buildingNumber": 5,
  "street": "Tahrir Street",
  "city": "Giza",
  "healthRecord": {
    "height": 165,
    "weight": 58,
    "bloodType": "A+",
    "note": "No known issues"
  }
}
```

**Done When**
- [ ] All 4 endpoints work in Swagger
- [ ] A member is created together with its HealthRecord in one request
- [ ] The delete rule is enforced in the service

### Resources
- **MUST** — [Model validation in ASP.NET Core](https://learn.microsoft.com/en-us/aspnet/core/mvc/models/validation)
- **MUST** — [Controller action return types](https://learn.microsoft.com/en-us/aspnet/core/web-api/action-return-types)
- **MUST** — [AutoMapper — Getting started](https://docs.automapper.org/en/stable/Getting-started.html)

---

### Task 3 — Session Endpoints

*Day 64–68.*

**Goal** — Schedule sessions with a trainer and a category, and show how many slots are left.

**Tasks**
- [ ] Create the Session DTOs: `SessionDTO` (Id, CategoryName, Description, TrainerName, StartDate, EndDate, Capacity, AvailableSlots) and `CreateSessionDTO`.
- [ ] Create `ISessionService` / `SessionService` and `SessionController`.
- [ ] Rules: a session needs an existing trainer and an existing category. `EndDate` must be after `StartDate`. Capacity must be between 1 and 25. A session can be deleted only if it is completed and has no bookings.
- [ ] Compute `AvailableSlots` = capacity − number of bookings.

**Endpoints**

| Method & Route | Access | OK | Errors / Notes |
| --- | --- | --- | --- |
| `GET api/Session` | Public | 200 | List with category name, trainer name, and available slots |
| `GET api/Session/{id}` | Public | 200 | 400 invalid id, 404 not found |
| `POST api/Session` | Public | 200 | 400 on validation errors, missing trainer or category, or invalid dates / capacity |
| `DELETE api/Session/{id}` | Public | 200 | 400 invalid id or the session is not deletable, 404 not found |

**Sample request — `POST api/Session`**

```json
{
  "description": "Morning yoga for beginners",
  "capacity": 15,
  "startDate": "2026-12-01T09:00:00",
  "endDate": "2026-12-01T10:00:00",
  "trainerId": 1,
  "categoryId": 1
}
```

**Done When**
- [ ] All 4 endpoints work in Swagger
- [ ] Available slots are computed correctly
- [ ] All the rules are enforced in the service

### Resources
- **MUST** — [Controller action return types](https://learn.microsoft.com/en-us/aspnet/core/web-api/action-return-types)
- **MUST** — [Loading related data](https://learn.microsoft.com/en-us/ef/core/querying/related-data/)

---

### Task 4 — Test & Review

*Day 69.*

**Goal** — Every endpoint is tested.

**Tasks**
- [ ] Test every endpoint with Swagger (success and failure cases) and fix the bugs you find.

**Done When**
- [ ] All 8 endpoints behave as in the endpoint tables

---

### Task 5 — README

*Day 70.*

Document:
- Project overview and architecture (updated)
- Data model and relationships (One-to-One, One-to-Many, owned `Address`)
- Endpoint map for all 16 endpoints
- Business rules of Members and Sessions
- How to run (updated migrations)
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
| 1 | `POST api/Member` with an email that already exists | 400, nothing created |
| 2 | `POST api/Member` without a `healthRecord` | 400 validation message |
| 3 | `GET api/Member/{id}` for a member with an active membership | Plan name and membership dates are shown |
| 4 | `DELETE api/Member/{id}` for a member with an upcoming booked session | 400, member still exists |
| 5 | `POST api/Session` with `endDate` before `startDate` | 400, nothing created |
| 6 | `POST api/Session` with capacity `0` or `26` | 400 validation message |
| 7 | `POST api/Session` with a trainer id that does not exist | 400, nothing created |
| 8 | `DELETE api/Session/{id}` for an upcoming session | 400, session still exists |

## What the Reviewer Checks

| Area | What is checked |
| --- | --- |
| Architecture | Three projects with the right references; controllers only call services; no `DbContext` outside the DAL |
| Correctness | Right status codes; business rules enforced in the services; DTOs returned, never entities |
| Data | Migration works; seeding runs once; constraints and keys exist in SQL Server |
| Errors | Validation messages are clear; failed rules return `400`; missing records return `404` |
| Docs | README complete; Swagger shows every endpoint |

## Sprint Deliverables

- [ ] Member endpoints (4)
- [ ] Session endpoints (4)
- [ ] Nested DTOs and validation
- [ ] Business rules enforced
- [ ] README

## Definition of Done

- [ ] One-to-One and One-to-Many relationships work in the API
- [ ] `Include` is used to load related data
- [ ] A member is created together with its HealthRecord
- [ ] Member endpoints work
- [ ] Session endpoints work
- [ ] Available slots and session status are computed correctly
- [ ] All simple rules are enforced
- [ ] Validation returns the correct status codes

## Before You Submit

- [ ] The solution builds with no errors, and the migration and seed data work on a fresh database
- [ ] Every endpoint in this sprint works in Swagger and returns the status codes in its endpoint table
- [ ] No passwords or real connection-string credentials are in the repo
- [ ] The README is complete and the commit history is meaningful

## Review Rule

The learner should be able to explain not only **what** was implemented, but **why it belongs there, what alternatives existed, and what trade-off was accepted**.
