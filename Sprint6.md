# 📚 Library Management API: Sprint 2

> **Unit of Work + Loan Service (Borrowing System)**
> Duration: 15 days | Level: Beginner → Intermediate

---

## 📑 Table of Contents

1. [Overview](#-overview)
2. [Learning Objectives](#-learning-objectives)
3. [Daily Plan](#-daily-plan)
4. [Architecture](#-architecture)
5. [Before You Start](#-before-you-start)
6. [Part A: Refactor to Unit of Work](#-part-a-refactor-to-unit-of-work)
7. [Part B: The Loan Feature](#-part-b-the-loan-feature)
8. [Business Logic per Service](#-business-logic-per-service)
9. [Full API Specification](#-full-api-specification)
10. [Test Scenarios](#-test-scenarios)
11. [Known Limitations](#-known-limitations)
12. [Definition of Done](#-definition-of-done)
13. [Deliverables](#-deliverables)
14. [Bonus Challenges](#-bonus-challenges)

---

## 📌 Overview

In Sprint 1 you built the three-project solution (**API**, **Business**, **Data**), a Generic Repository, and the 9 required endpoints. In Sprint 2 you improve the same solution. This is **not** a new project.

| Part | What you do |
|---|---|
| **A** | Refactor the **Data** layer: remove `SaveChanges` from repositories and add the **Unit of Work**. The services switch to it. The controllers do not change |
| **B** | Add the **Loan** entity and a new **`LoanService`** with the **borrow**, **return**, and member loan-history logic |

> This document describes **what** to build and **which rules** to follow. It does not give you the code. Writing it is the exercise. If you get stuck, ask your instructor.

---

## 🎯 Learning Objectives

- [ ] Explain the problem caused by saving inside each repository
- [ ] Implement the **Unit of Work** pattern on top of the Generic Repository
- [ ] **Refactor** working code safely, in small steps, without breaking it
- [ ] Explain why a business operation that changes several records must be saved **once**
- [ ] Explain who owns the lifetime of the `DbContext` (the DI container, not your code)
- [ ] Design a new service (`LoanService`) with its own business rules
- [ ] Combine rules from several entities inside one service
- [ ] Add rules to existing services without breaking their current behavior

---

## 📅 Daily Plan

This sprint is planned for **15 days**. Finish and test Part A before moving to the Loan feature.

| Day | Focus | You finish the day with | Study topics (read/watch before coding) |
|---|---|---|---|
| 1 | Baseline and the problem | `sprint-2` branch, Sprint 1's 9 required tests pass, the throwaway "two saves" experiment from A.1 done and deleted | Git branches, why saving inside each repository is a problem |
| 2 | Repository and Unit of Work | Repositories track changes only, `CountAsync` added, `IUnitOfWork` and `UnitOfWork` written and registered in DI | Unit of Work pattern, `DbContext` change tracker, scoped lifetime and who disposes the `DbContext` |
| 3 | Switch services and regression | Services use `_uow` and call `SaveChangesAsync()`, all Sprint 1 required tests pass again | Safe refactoring in small steps, `Task`/`async` vs sync methods |
| 4 | Loan entity and migration | `Loan`, relationships (Restrict), `Loans` in the Unit of Work, `AddLoanTable` migration, Loan DTOs | EF Core relationships and delete behaviors, migrations |
| 5 | Loan read by member | `GetByMemberAsync` and `GET /api/members/{id}/loans` logic | `Include` / `ThenInclude`, filtering by foreign key, DTO mapping |
| 6 | Borrow flow | `BorrowAsync` with availability, member limit, dates, and one save | A single `SaveChangesAsync()` = one database transaction, `DateTime.UtcNow`, order of business checks |
| 7 | Return flow | `ReturnAsync` with one save and duplicate-return protection | State-based conflicts (`409`), updating two related entities |
| 8 | Loan controller | The three required loan endpoints | Attribute routing, absolute routes (`[HttpGet("/api/...")]`), `201 Created` |
| 9 | Borrow/return tests | Successful borrow/return and important failure cases (L1 to L14) | Designing edge-case tests |
| 10 | Unit of Work review | Verify each service saves at the correct point; run the "one save only" test (S1) | Reviewing transaction boundaries |
| 11 | Error handling and fixes | Fix failing tests and verify error responses | Reading error responses, middleware behavior |
| 12 | *Recommended extension:* concurrency, part 1 | `RowVersion` on `Book`, migration, and a reproduced double-borrow problem | Race conditions, optimistic concurrency, concurrency tokens |
| 13 | *Recommended extension:* concurrency, part 2 | Middleware returns `409` for `DbUpdateConcurrencyException` and unique-index errors | Catching EF Core exceptions in middleware, SQL Server error numbers `2601` and `2627` |
| 14 | Architecture and code review | Remove unused methods and check layer boundaries | Code review checklist, layer boundaries |
| 15 | Wrap-up | README, Definition of Done, and buffer time | Writing a README, documenting known limitations |

> **About the schedule:** Part A is a small refactor, so it fits in **Days 1 to 3**. Days 12 and 13 are for the **Concurrency** extension (see the Bonus Challenges section). It is optional: if you skip it, use those two days as buffer for the required work.

---

## 🏛️ Architecture

The architecture does not change. The Data layer gets one new piece, the **Unit of Work**, and the Business layer gets one new service, **`LoanService`**.

```
┌────────────────────────────────────┐
│          LibraryApi.API            │   Presentation layer
│  Controllers (thin), Middleware,   │   HTTP in, HTTP out. No logic
│  Program.cs                        │
└──────────────────┬─────────────────┘
                   │ references
                   ▼
┌────────────────────────────────────┐
│        LibraryApi.Business         │   Business layer (BLL)
│  Services, DTOs, Custom Exceptions │   Rules, validation, mapping
└──────────────────┬─────────────────┘
                   │ references
                   ▼
┌────────────────────────────────────┐
│          LibraryApi.Data           │   Data access layer (DAL)
│  Entities, DbContext, Migrations,  │   Database access only
│  Repositories, Unit of Work        │
└────────────────────────────────────┘
```

### Who is responsible for what?

| Layer | Responsible for | Must NOT |
|---|---|---|
| **API** | Routes, HTTP status codes, receiving DTOs, calling **one service**, the exception middleware | Contain business rules, use `IUnitOfWork`, `DbContext`, or entities |
| **Business** | Business rules, checks, mapping between entities and DTOs, throwing custom exceptions, **deciding when to save** | Know about HTTP (no `ActionResult`, no `HttpContext`) or about EF Core (no `IQueryable`, no `using Microsoft.EntityFrameworkCore`) |
| **Data** | Entities, `DbContext`, repositories, Unit of Work, migrations | Contain business rules |

### Rules

1. Dependencies go one way: `API → Business → Data`. Never the other way.
2. A controller calls **one service** and returns the result. No exceptions: that is why `GET /api/members/{id}/loans` lives in `LoansController`.
3. Only services use `IUnitOfWork`.
4. Services return **DTOs**, never entities.
5. Business never imports `Microsoft.EntityFrameworkCore`. Everything goes through repository methods.

---

## ✅ Before You Start

Sprint 1 must meet its Definition of Done. Before you touch anything:

- Make sure you have **no uncommitted changes**.
- Create a new branch named `sprint-2` and work on it.
- Run the whole **Test Scenarios** table from Sprint 1 in Swagger. You will repeat the same tests as a baseline after each refactor step.

---

## 🔄 Part A: Refactor to Unit of Work

> All work in this part happens in **LibraryApi.Data** and in the **services**. Controllers do not change.

### A.1 The problem

Right now each repository saves by itself. Imagine borrowing a book, which needs **two** changes:

1. Add the loan to the database (this saves immediately: **save #1**).
2. Mark the book as unavailable and update it (this saves again: **save #2**).

If the second save fails, the database says a loan exists **but the book is still available**. The data is now inconsistent.

**Try it yourself:** write a throwaway endpoint that adds an author and then deliberately throws an exception before a second change. Check the database and see what was saved. Delete the endpoint afterwards.

### A.2 The solution

A **Unit of Work** groups all changes made during one operation and saves them **together, once**. Either everything is saved or nothing is.

```
Service ──> IUnitOfWork ──> Repositories (track changes only)
                 │
                 └──> SaveChangesAsync()  (one single save)
```

### A.3 Step 1: Update the repository

Repositories **stop saving**. They only track changes in the `DbContext`.

| Operation | Change |
|---|---|
| `CountAsync(predicate)` | **New.** Returns how many entities match the condition. It works like `AnyAsync`. Used in Part B to count a member's active loans |
| `GetAllAsync(includes)` | No change |
| `GetByIdAsync(id, includes)` | No change |
| `AnyAsync(predicate)` | No change |
| `AddAsync(entity)` | **No `SaveChanges` inside anymore** |
| `Update(entity)` | Now **synchronous** (no `Async`, no `Task`) and **no `SaveChanges` inside** |
| `Delete(entity)` | Now **synchronous** and **no `SaveChanges` inside** |

> Do **not** expose `IQueryable<T>` from the repository. It would leak EF Core into the Business layer. When a service needs a new kind of question (a count, an existence check), add a small method to the repository instead.

### A.4 Step 2: Create the Unit of Work

In `LibraryApi.Data/UnitOfWork/` create an `IUnitOfWork` interface and a `UnitOfWork` class.

**`IUnitOfWork` exposes:**

| Member | Purpose |
|---|---|
| `Authors`, `Books`, `Members` | One `IGenericRepository<T>` per entity |
| `Loans` | Added in Part B |
| `SaveChangesAsync()` | The **single** save point. Returns the number of affected rows |

**`UnitOfWork` must:**

- Receive the `AppDbContext` through its constructor.
- Create **one repository per entity**, all using the **same** `DbContext` instance.
- Implement `SaveChangesAsync()` by calling the context's `SaveChangesAsync()`.
- **Not** dispose the context, and not implement `IDisposable`. See the note below.

> **Why "same DbContext"?** One `DbContext` tracks all changes. One `SaveChangesAsync()` call saves all of them inside a single database transaction.

> **Who disposes the `DbContext`?** The DI container created it (scoped), so the container disposes it at the end of the request. `UnitOfWork` only *uses* it. If `UnitOfWork` disposed it too, it would be disposing something it does not own. So `UnitOfWork` does not need `IDisposable` at all.

> Until Part B, leave the `Loans` repository out. The `Loan` class does not exist yet.

### A.5 Step 3: Register it

In `DataServiceRegistration` (Data project), **replace** the open-generic registration of `IGenericRepository<>` with a scoped registration of `IUnitOfWork` mapped to `UnitOfWork`. Keep the `AppDbContext` registration. `UnitOfWork` creates the repositories itself, so they no longer need their own registration. `BusinessServiceRegistration` does not change.

### A.6 Step 4: Switch the services to the Unit of Work

Update `AuthorService`, `BookService`, and `MemberService`. **The controllers and the business rules do not change.** Only the way the services reach the data changes.

- Inject `IUnitOfWork` instead of the individual repositories.
- Use `_uow.Authors`, `_uow.Books`, and `_uow.Members` for data access.
- `Update` and `Delete` are no longer awaited (they are not async anymore).
- Every create, update, and delete must call `SaveChangesAsync()` on the Unit of Work. **The service decides when to save.**
- **Forgetting `SaveChangesAsync()` is the #1 bug:** the endpoint returns success but nothing is saved.

### A.7 Step 5: Regression check

Repeat the **9 required Sprint 1 tests** in Swagger. Every required scenario must behave exactly as before. Commit. Do **not** continue to Part B until this passes.

---

## 🧾 Part B: The Loan Feature

### B.1 The Loan entity (Data project)

`Loan` inherits `BaseEntity` and has these properties:

| Property | Type | Rules |
|---|---|---|
| `BookId` / `Book` | foreign key / navigation | The borrowed book |
| `MemberId` / `Member` | foreign key / navigation | The member who borrowed it |
| `LoanDate` | `DateTime` | Set by the server: the current UTC time |
| `DueDate` | `DateTime` | `LoanDate` + **14 days** |
| `ReturnDate` | `DateTime?` | `null` until the book is returned |

An **active loan** is a loan where `ReturnDate` is `null`.

### B.2 Update existing entities and the context

- `Book` and `Member` each get a **`Loans` collection** (initialized empty).
- `AppDbContext` gets a **`Loans` set**.
- In the model configuration, add two relationships, both with **Restrict** as the delete behavior:
  - `Loan → Book` (one book has many loans, foreign key `BookId`)
  - `Loan → Member` (one member has many loans, foreign key `MemberId`)
- Add the `Loans` repository to `IUnitOfWork` and `UnitOfWork` (the part you left out in Part A).
- Create a migration named **`AddLoanTable`** and apply it to the database, the same way you did in Sprint 1 (the `DbContext` is in Data, and the startup project is the API):

```
dotnet ef migrations add AddLoanTable --project LibraryApi.Data --startup-project LibraryApi.API
dotnet ef database update --project LibraryApi.Data --startup-project LibraryApi.API
```

### B.3 DTOs (Business project)

| DTO | Fields | Validation |
|---|---|---|
| `LoanCreateDto` | `BookId`, `MemberId` | Both must be positive numbers (1 or greater). Enforce this in code on the DTO |
| `LoanReadDto` | `Id`, `BookId`, `BookTitle`, `MemberId`, `MemberName`, `LoanDate`, `DueDate`, `ReturnDate?`, `IsReturned` | none |

`IsReturned` is calculated: it is `true` when `ReturnDate` has a value.

### B.4 `LoanService`: contract and business logic

Create `ILoanService` and `LoanService` in the Business project, and register the service with the other services. The service uses `IUnitOfWork`. To show names in `LoanReadDto`, load each loan together with its book and member.

| Operation | Returns | Business logic | Throws |
|---|---|---|---|
| `GetByMemberAsync(memberId)` | List of `LoanReadDto` | Check that the member exists, then return only that member's loans | `NotFoundException` if the member does not exist |
| `BorrowAsync(dto)` | The created `LoanReadDto` | See **B.5** | `BadRequestException`, `ConflictException` |
| `ReturnAsync(loanId)` | nothing | See **B.6** | `NotFoundException`, `ConflictException` |

> **Why `BadRequestException` in borrow but `NotFoundException` in return?** It is the same id rule as Sprint 1. An id in the **URL** (`loanId`, or the member id in `/members/{id}/loans`) that does not exist is `404`. An id in the **body** (`bookId` and `memberId` in `POST /api/loans`) that points to nothing is `400`.

### B.5 The borrow flow (`BorrowAsync`)

Follow these steps **in this order**:

1. Load the book. If it does not exist → `BadRequestException` (the id comes from the body).
2. Load the member. If it does not exist → `BadRequestException` (the id comes from the body).
3. If the book is **not available** → `ConflictException`.
4. If the member already has **3 active loans** → `ConflictException`.
5. Create the loan: `LoanDate` = now (UTC), `DueDate` = now + 14 days.
6. Mark the book as unavailable and call `Update` on it.
7. Add the loan through the `Loans` repository.
8. Call `SaveChangesAsync()` **once**, for steps 5 to 7 together.
9. Return a `LoanReadDto`.

**Counting active loans (step 4):** call `CountAsync` on the `Loans` repository with a condition: the member id matches and `ReturnDate` is `null`. The repository hides EF Core, so the Business project needs no EF imports.

> Two borrow requests for the same book at the same instant can both pass steps 3 and 4. This is a known limitation. See [Known Limitations](#-known-limitations).

### B.6 The return flow (`ReturnAsync`)

1. Load the loan **together with its book**. If it does not exist → `NotFoundException`.
2. If `ReturnDate` is already set → `ConflictException` ("already returned"). It conflicts with the current state of the data, like borrowing a book that is not available.
3. Set `ReturnDate` to the current UTC time and call `Update` on the loan.
4. Mark the loan's book as available again and call `Update` on the book.
5. Call `SaveChangesAsync()` **once**, for both changes.

> Call `Update` explicitly on both entities, as you did in the borrow flow. It keeps the code correct even if an entity was loaded without change tracking.

### B.7 Keep the reduced scope

To keep this sprint achievable in 15 days, do **not** add new delete rules to `BookService` or `MemberService`. The reduced Sprint 1 API does not expose delete endpoints for books or members, so these rules and their endpoints are out of scope. The author delete rule from Sprint 1 remains.

### B.8 Controllers

- Create `LoansController` with the three required endpoints below. It injects only `ILoanService`.
- `GET /api/members/{id}/loans` lives in `LoansController` and calls `GetByMemberAsync`. Because the URL starts with `/api/members`, use an explicit absolute route. `MembersController` does not change.
- `POST /api/loans` returns `201 Created` with the created `LoanReadDto`. Since the reduced scope does not include `GET /api/loans/{id}`, a `Location` header is not required.
- Every controller injects only its service interface.

---

## 🧠 Business Logic per Service

After this sprint, every service owns a clear set of rules. Use this table to check that nothing is missing.

| Service | Business logic it owns |
|---|---|
| **`AuthorService`** | Trims input. An author with books cannot be deleted. Computes `BooksCount` |
| **`BookService`** | The author must exist. The ISBN must be unique (excluding itself on update). The server sets `IsAvailable` on create and never lets update change it. Fills `AuthorName` |
| **`MemberService`** | Normalizes email (trim, lowercase) and other text fields. The email must be unique on create. The server sets `JoinedAt` |
| **`LoanService`** *(new)* | The book and the member (ids in the body) must exist, otherwise `400`. The book must be available (`409`). A member can have at most 3 active loans (`409`). Sets `LoanDate` and `DueDate` (14 days). A loan cannot be returned twice (`409`). Borrow and return each save **once**, keeping the loan and `Book.IsAvailable` consistent |

All the Sprint 1 rules still apply.

---

## 🔌 Full API Specification

The sprint adds only **3 endpoints**. The existing 9 Sprint 1 endpoints remain unchanged.

| Method | Route | Description | Success | Errors |
|---|---|---|---|---|
| POST | `/api/loans` | Borrow a book | `201` | `400` (invalid body or missing book/member), `409` (book unavailable or member has 3 active loans) |
| PUT | `/api/loans/{id}/return` | Return a book | `204` | `404`, `409` (already returned) |
| GET | `/api/members/{id}/loans` | List one member's loans | `200` | `404` |

> The `GET /api/members/{id}/loans` action is implemented in `LoansController` and uses an explicit absolute route. There is intentionally no `GET /api/loans` or `GET /api/loans/{id}` in this reduced version.

### Example: borrow a book

`POST /api/loans` takes `bookId` and `memberId`.

**Success: `201 Created`.** Return a `LoanReadDto` with `bookTitle`, `memberName`, `loanDate` (current UTC time), `dueDate` (14 days later), `returnDate = null`, and `isReturned = false`.

- Borrowing the same book again returns `409 Conflict`.
- Borrowing with a nonexistent `bookId` or `memberId` returns `400 Bad Request` because the ids are in the request body.
- Returning a loan that was already returned returns `409 Conflict`.

---

## 🧪 Test Scenarios

First, repeat the **9 required Sprint 1 tests**. Then run these loan tests. Use at least four books and one member to test the three-active-loan limit. For every error, check `statusCode`, `message`, and `timestamp` in the response body.

### Borrow and return

| # | Request | Expected |
|---|---|---|
| L1 | `POST /api/loans` with an available book and existing member | `201`, `isReturned` is `false`, `dueDate` is 14 days after `loanDate` |
| L2 | `GET /api/books` after borrowing | The borrowed book has `isAvailable = false` |
| L3 | `POST /api/loans` for the same book again | `409` |
| L4 | `POST /api/loans` with `bookId` `9999` | `400` |
| L5 | `POST /api/loans` with `memberId` `9999` | `400` |
| L6 | `POST /api/loans` with `bookId` `0` | `400` |
| L7 | One member borrows 3 different books, then a 4th | First 3 return `201`; 4th returns `409` |
| L8 | `PUT /api/loans/{id}/return` for an active loan | `204` |
| L9 | `GET /api/books` after returning the book | That book has `isAvailable = true` |
| L10 | `PUT /api/loans/{id}/return` for the same loan again | `409` |
| L11 | `PUT /api/loans/9999/return` | `404` |
| L12 | After returning a book, the member borrows another book | `201` |
| L13 | `GET /api/members/{id}/loans` | `200`, only that member's loans, including returned loans |
| L14 | `GET /api/members/9999/loans` | `404` |

### One save only

| # | Test | Expected |
|---|---|---|
| S1 | Temporarily throw an exception in `BorrowAsync` after tracking the loan and book changes but before `SaveChangesAsync()`, then call `POST /api/loans` | `500`, no new loan is stored and the book remains available. Remove the test exception afterwards. |


## ⚠️ Known Limitations

These are known weaknesses. You do **not** need to fix them in this sprint, but write them in your README. The first two have a bonus solution below.

- **Double borrow.** Two requests for the same book at the same instant can both read `IsAvailable = true` and both succeed. The same can happen to the 3-loan limit.
- **Duplicate ISBN or email race.** Two requests with the same ISBN (or email) at the same instant can both pass the service check. The unique index rejects the second one with a database exception, and the middleware returns `500` instead of `409`. The data stays correct. Only the status code is wrong.

---

## ✔️ Definition of Done

**Architecture**
- [ ] Dependencies still go only `API → Business → Data`
- [ ] No controller uses `IUnitOfWork`, `DbContext`, repositories, or entities
- [ ] Controllers contain no business rules (no `if` checks for rules)
- [ ] Every controller injects exactly **one** service, and `GET /api/members/{id}/loans` is in `LoansController`
- [ ] No file in `LibraryApi.Business` imports `Microsoft.EntityFrameworkCore`
- [ ] `LoanService` exists with its interface and is registered with the other services

**Data layer**
- [ ] No repository calls `SaveChangesAsync`
- [ ] Only services call `SaveChangesAsync()` on the Unit of Work
- [ ] `UnitOfWork` does not dispose the `DbContext`
- [ ] The repository exposes `CountAsync(predicate)` and no `IQueryable`
- [ ] Migrations are in the Data project and committed

**Behavior**
- [ ] All 9 required Sprint 1 endpoints still work
- [ ] Borrow and return each use **one** `SaveChangesAsync`
- [ ] The `LoanService` borrow and return rules are implemented, and every required row of the Sprint 1 and Sprint 2 test tables gives the expected status code
- [ ] A missing member/loan id in a URL returns `404`, a missing book/member id in the borrow body returns `400`, and returning a loan twice returns `409`
- [ ] `POST /api/loans` returns `201 Created` with the created loan DTO

**Quality**
- [ ] Code is clean with English comments
- [ ] Your repository `README` is updated, explains the architecture and how to run the project, and lists the known limitations

---

## 📦 Deliverables

1. Updated source code on GitHub (merge your `sprint-2` branch into `main`)
2. The updated `Migrations` folder
3. An updated `README` describing the architecture, how to run the project, and any known limitations
4. (Optional) Screenshots from Swagger: a successful borrow, the "already borrowed" error, and the 4th-book error

---

## 🏆 Bonus Challenges

> For students who finish the required work early. Bonus work is **optional** and is never required for the sprint to be complete. Quality matters more than quantity.

### 🛡️ Concurrency (fixes the first two known limitations, recommended first bonus)

| Goal | Details |
|---|---|
| Stop a double borrow | Add a `RowVersion` property (`byte[]`, marked as a concurrency token) to `Book` and create a migration. When two requests change the same book, the second `SaveChangesAsync()` throws `DbUpdateConcurrencyException` |
| Return `409` instead of `500` | Catch `DbUpdateConcurrencyException` in the exception middleware and return `409`. Also catch the `DbUpdateException` caused by a unique index (SQL Server error numbers `2601` and `2627`) and return `409` |

> For this bonus only, the middleware (API) may use EF Core exception types. The Business layer must still **not** import EF Core. `ConflictException` lives in Business, so the Unit of Work (Data) cannot throw it.

### 🔐 Authentication with JWT

| Level | Goal | Details |
|---|---|---|
| **1** | Register, login, protect endpoints | Add ASP.NET Core Identity with an `ApplicationUser` (Data). An `IAuthService` / `AuthService` (Business) generates the JWT. `POST /api/auth/register` and `POST /api/auth/login` in an `AuthController` (API). Add `[Authorize]` to all controllers except auth |
| **2** | Roles | Create `Admin` and `Member` roles. Only `Admin` can create, update, or delete books and authors. A member can only see **their own** loans (link `Member` to `ApplicationUser`) |
| **3** | Advanced | Refresh tokens, and seed a default admin user and the roles at startup |

**Your instructor provides** these as ready-made boilerplate: the `JwtSettings` section in `appsettings.json`, the JWT bearer authentication setup in `Program.cs`, and the Swagger **Authorize** button configuration.

**You write:** the register and login logic and the token generation method.

Useful packages: `Microsoft.AspNetCore.Identity.EntityFrameworkCore` (in Data) and `Microsoft.AspNetCore.Authentication.JwtBearer` (in API).

> **Heads up:** Identity needs `AppDbContext` to inherit from `IdentityDbContext<ApplicationUser>` instead of `DbContext`, and you must create a new migration.

### ✨ Other ideas

- Replace manual mapping with **AutoMapper**
- Write unit tests with **xUnit** and **Moq**. Services are easy to test because they depend on `IUnitOfWork`, which you can mock
- `GET /api/loans/overdue`: active loans where `DueDate` has passed
- Calculate a late fee when a book is returned after its `DueDate`
- Make `UnitOfWork` create repositories **lazily** with a generic `Repository<T>()` method and a dictionary cache
- Replace hard delete with a **soft delete** (an `IsDeleted` flag), so books and members with loan history can be "deleted" safely

---

## 💡 Tips

- **Commit before every refactor step.** If something breaks, you can go back.
- Refactor in **small steps** and run the project after each one.
- Write each business rule, then test it in Swagger **before** writing the next one.
- A good test for a thin controller: if you can delete an `if` from it without changing behavior, it belonged in a service.
- Think about edge cases: a member with exactly 3 active loans, borrowing an unavailable book, and returning the same loan twice.
- If you are stuck for more than 30 to 45 minutes, ask for help.
