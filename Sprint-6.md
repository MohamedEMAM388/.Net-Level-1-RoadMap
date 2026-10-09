# Sprint 6 — Web API — Memberships & Bookings

> Project: **Gym Management API — Part 3 (7 endpoints)**
> Duration: **2 weeks (14 days) · Day 71–84**

## Sprint Goal

Finish the API with the endpoints that connect everything: memberships and class bookings. This sprint is about Many-to-Many relationships and time-based business rules.

## How to use this sprint plan

For every task:

1. Read the **MUST** resources before implementation.
2. Read **DEEP DIVE** only when the concept is unclear or the learner wants deeper understanding.
3. Implement the requested change.
4. Verify the behavior manually.
5. Commit the work in small, meaningful commits.
6. Be able to explain the decision during review.

**Important:** This document does not change the original Sprint scope. It expands the original tasks into an executable checklist.

## Project Brief — Gym Management API, Part 3

This sprint finishes the API with the endpoints that connect everything: **MemberShip** (a member subscribes to a plan) and **MemberSession** (a member books a session). The rules are about dates, capacity, and duplicates.

The full project brief (architecture, data model, and endpoint map) is in **Sprint 4**.

### Data You Work With

**MemberShip** — joins Member and Plan (composite key `MemberId + PlanId`). `StartDate` = `CreatedAt`. `EndDate` = start date + plan duration in days. `Status` is computed: `Expired` when `EndDate <= now`, otherwise `Active`.

**MemberSession** — joins Member and Session (composite key `MemberId + SessionId`). `BookingDate` = `CreatedAt`.

> **Response convention (same as the reference solution):** success returns `200` with the data or a short success message; an invalid id or a failed rule / validation returns `400` with a message; a missing record returns `404`. Model-validation errors are returned automatically as `400` by `[ApiController]`.


## Working Rules

- Finish each task before starting the next one, and commit after every finished task.
- Business rules live in the **services**, never in the controllers.
- Controllers return DTOs, never entities.
- Test every endpoint in Swagger, for both success and failure cases.
- Push to GitHub with a README (see the README task).

## Sprint Tasks

### Task 1 — Many-to-Many & Time-Based Rules

*Day 71–72.*

**Goal** — Understand the join entities and how date checks decide what is allowed.

**Tasks**
- [ ] README: explain MemberShip (Member + Plan) and MemberSession (Member + Session) as join entities with extra data and composite keys.
- [ ] Practise the date checks you will need: an active vs expired membership (`EndDate` vs now) and an upcoming session (`StartDate` vs now).
- [ ] Write the helper methods once (for example `HasActiveMemberShip(memberId)` and `IsUpcoming(session)`) and reuse them.

**Done When**
- [ ] You can explain why the two join entities use composite keys
- [ ] Helper methods exist and are reused

### Resources
- **MUST** — [Relationships (many-to-many)](https://learn.microsoft.com/en-us/ef/core/modeling/relationships)
- **DEEP DIVE** — Jon P. Smith — Entity Framework Core in Action — `Book: Entity Framework Core in Action`

---

### Task 2 — MemberShip Endpoints

*Day 73–76.*

**Goal** — Subscribe members to plans and cancel active memberships.

**Tasks**
- [ ] Create the DTOs: `MemberShipDTO` (MemberId, PlanId, MemberName, PlanName, StartDate, EndDate, Status) and `CreateMemberShipDTO` (MemberId, PlanId).
- [ ] Create `IMemberShipService` / `MemberShipService` and `MemberShipController`.
- [ ] Subscribe rules: the member and the plan must exist; the plan must be active; a member cannot have two active memberships at the same time; `EndDate` = start date + plan duration in days.
- [ ] Cancel rule: only an active (not expired) membership can be cancelled.

**Endpoints**

| Method & Route | Access | OK | Errors / Notes |
| --- | --- | --- | --- |
| `GET api/MemberShip` | Public | 200 | List with member name, plan name, start date, end date, and status (Active / Expired) |
| `POST api/MemberShip` | Public | 200 | 400 on validation errors, member or plan not found, plan not active, or the member already has an active membership |
| `DELETE api/MemberShip/{memberId}/{planId}` | Public | 200 | 400 when the membership does not exist or is already expired |

**Sample request — `POST api/MemberShip`**

```json
{
  "memberId": 1,
  "planId": 2
}
```

**Done When**
- [ ] All 3 endpoints work in Swagger
- [ ] Every subscribe rule is enforced in the service
- [ ] The status is computed, not stored

### Resources
- **MUST** — [Controller action return types](https://learn.microsoft.com/en-us/aspnet/core/web-api/action-return-types)
- **MUST** — [Loading related data](https://learn.microsoft.com/en-us/ef/core/querying/related-data/)

---

### Task 3 — Booking Endpoints

*Day 77–81.*

**Goal** — Let members book and cancel sessions, with capacity and membership rules.

**Tasks**
- [ ] Create the DTOs: `CreateMemberSessionDTO` (MemberId, SessionId), `BookedMemberDTO` (MemberId, SessionId, MemberName, BookingDate), and reuse `SessionDTO` for the upcoming sessions list.
- [ ] Create `IMemberSessionService` / `MemberSessionService` and `MemberSessionController`.
- [ ] Booking rules: the member and the session must exist; the session must be upcoming; the member must have an active membership; a member cannot book the same session twice; a session cannot exceed its capacity.
- [ ] Cancel rule: a booking can be cancelled only before the session starts.

**Endpoints**

| Method & Route | Access | OK | Errors / Notes |
| --- | --- | --- | --- |
| `GET api/MemberSession` | Public | 200 | Upcoming sessions with available slots |
| `GET api/MemberSession/Upcoming/{sessionId}/Members` | Public | 200 | 404 when the session does not exist or is not upcoming |
| `POST api/MemberSession` | Public | 200 | 400 on validation errors or any booking rule violation |
| `DELETE api/MemberSession/{memberId}/{sessionId}` | Public | 200 | 400 when the booking does not exist or the session has already started |

**Sample request — `POST api/MemberSession`**

```json
{
  "memberId": 1,
  "sessionId": 3
}
```

**Done When**
- [ ] All 4 endpoints work in Swagger
- [ ] All booking rules are enforced in the service
- [ ] After a booking, the session's available slots drop by one

### Resources
- **MUST** — [Controller action return types](https://learn.microsoft.com/en-us/aspnet/core/web-api/action-return-types)

---

### Task 4 — Final Testing

*Day 82–83.*

**Goal** — The whole flow works end to end.

**Tasks**
- [ ] Test the full API with Swagger: all 23 endpoints, success and failure cases. Fix the bugs you find.
- [ ] Test the whole flow: create a plan, trainer, and member → create a session → subscribe the member → book the session → check available slots → cancel the booking.

**Done When**
- [ ] The full flow works in Swagger
- [ ] No secrets or real credentials are committed

---

### Task 5 — README

*Day 84.*

Document:
- Project overview
- Architecture
- Data model (all 7 entities, with the composite keys of the join entities)
- Business rules, grouped by entity
- Endpoint map for all 23 endpoints
- How to run (connection-string placeholder, migration command, run command, Swagger URL)
- End-to-end flow (plan → trainer → member → session → subscribe → book → cancel)
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
| 1 | Subscribe a member to an inactive plan | 400, nothing created |
| 2 | Subscribe a member who already has an active membership | 400, nothing created |
| 3 | Cancel an expired membership | 400 |
| 4 | Book a session for a member without an active membership | 400 |
| 5 | Book the same session twice | 400 on the second request |
| 6 | Book a session that is already full | 400 |
| 7 | Book a session that already started | 400 |
| 8 | Cancel a booking after the session started | 400 |
| 9 | Book a session, then `GET api/MemberSession` | Available slots dropped by one |

## What the Reviewer Checks

| Area | What is checked |
| --- | --- |
| Architecture | Three projects with the right references; controllers only call services; no `DbContext` outside the DAL |
| Correctness | Right status codes; business rules enforced in the services; DTOs returned, never entities |
| Data | Migration works; seeding runs once; constraints and keys exist in SQL Server |
| Errors | Validation messages are clear; failed rules return `400`; missing records return `404` |
| Docs | README complete; Swagger shows every endpoint |

## Sprint Deliverables

- [ ] MemberShip endpoints (3)
- [ ] Booking endpoints (4)
- [ ] All business rules enforced
- [ ] Complete Gym Management API (23 endpoints)
- [ ] Swagger documentation
- [ ] README

## Definition of Done

- [ ] Many-to-Many with composite keys works
- [ ] A member can subscribe to a plan and cancel the membership
- [ ] A member cannot have two active memberships
- [ ] A member can book and cancel a session
- [ ] Booking rules (active membership, capacity, no duplicate) are enforced
- [ ] All status codes are correct
- [ ] Swagger is available
- [ ] README is complete

## Before You Submit

- [ ] The solution builds with no errors, and the migration and seed data work on a fresh database
- [ ] Every endpoint in this sprint works in Swagger and returns the status codes in its endpoint table
- [ ] No passwords or real connection-string credentials are in the repo
- [ ] The README is complete and the commit history is meaningful

## Review Rule

The learner should be able to explain not only **what** was implemented, but **why it belongs there, what alternatives existed, and what trade-off was accepted**.
