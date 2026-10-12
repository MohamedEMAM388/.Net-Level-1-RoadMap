# 📚 Library Management API: Sprint 1

> **Foundations + 3-Project Solution + Services with Business Logic**
> Duration: 15 days | Level: Beginner

---

## 📑 Table of Contents

1. [Overview](#-overview)
2. [Learning Objectives](#-learning-objectives)
3. [Daily Plan](#-daily-plan)
4. [Architecture](#-architecture)
5. [Solution Setup](#-solution-setup)
6. [Solution Structure](#-solution-structure)
7. [Domain Model](#-domain-model)
8. [Database Context, Registration and Migrations](#-database-context-registration-and-migrations)
9. [Generic Repository](#-generic-repository)
10. [DTOs](#-dtos)
11. [Exceptions and the Global Exception Middleware](#-exceptions-and-the-global-exception-middleware)
12. [Services and Business Logic](#-services-and-business-logic)
13. [API Specification](#-api-specification)
14. [Validation and Business Rules](#-validation-and-business-rules)
15. [Sample Seed Data](#-sample-seed-data)
16. [Test Scenarios](#-test-scenarios)
17. [Known Limitations](#-known-limitations)
18. [Definition of Done](#-definition-of-done)
19. [Deliverables](#-deliverables)
20. [What Comes Next](#-what-comes-next)

---

## 📌 Overview

Over the next month you will build a **Library Management API** with ASP.NET Core Web API. The system manages **authors**, **books**, and **library members**, and in Sprint 2 it will also handle **borrowing and returning books**.

In **Sprint 1** you build the foundation:

- A **three-project solution** (N-Tier) created **all at once at the very start**: API, Business (BLL), and Data (DAL), with the project references set from the start
- The entities and the database (Code First)
- A **Generic Repository** that hides data-access code
- **One service per entity** (`AuthorService`, `BookService`, `MemberService`) in the Business layer. Each service owns the business logic of its entity
- **DTOs**, custom exceptions, and a **global exception middleware** with a consistent error format
- **Thin controllers** that only call a service and return an HTTP response

By the end of this sprint you will have a working API with **9 required endpoints** that you can test from Swagger. The endpoint list is intentionally reduced so the student can focus on architecture, services, and core business rules.

> This document describes **what** to build and **which rules** to follow. It does not give you the code. Writing it is the exercise. If you get stuck, ask your instructor.

---

## 🎯 Learning Objectives

By the end of this sprint you will be able to:

- [ ] Create a **solution with multiple projects** and connect them with project references
- [ ] Explain what a **tier (layer)** is and why we separate them
- [ ] Design entities and relationships with EF Core (Code First)
- [ ] Create and apply **Migrations** when the DbContext lives in a different project
- [ ] Explain and implement the **Generic Repository Pattern**
- [ ] Put **business logic in services**, not in controllers or repositories
- [ ] Use **DTOs** instead of exposing entities directly
- [ ] Signal errors with **custom exceptions** and turn them into HTTP responses with a **middleware**
- [ ] Return correct HTTP status codes (`200`, `201`, `204`, `400`, `404`, `409`, `500`)

---

## 📅 Daily Plan

This sprint is planned for **15 days**. Each day should end with a buildable project and a small commit.

| Day | Focus | You finish the day with | Study topics (read/watch before coding) |
|---|---|---|---|
| 1 | Solution setup | Three projects, references, packages, Swagger | Solution vs project, project references, what N-Tier is, Swagger / OpenAPI |
| 2 | Entities and relationships | `BaseEntity`, `Author`, `Book`, `Member`, `AppDbContext` | EF Core Code First, one-to-many relationships, Fluent API vs Data Annotations |
| 3 | Database setup | Connection string, DI registration, migration applied | Dependency Injection and scoped lifetime, connection strings, `dotnet ef` with `--project` / `--startup-project` |
| 4 | Generic Repository | `IGenericRepository<T>` and `GenericRepository<T>` with only required methods | Repository pattern, generic constraints (`where T : BaseEntity`), `Expression<Func<T, object>>`, `Include` |
| 5 | **First working slice: Authors** | `AuthorCreateDto`, `AuthorReadDto` (with validation), a minimal `AuthorService`, `GET` and `POST /api/authors` working in Swagger | Controllers and `[ApiController]`, DTOs, validation attributes, HTTP status codes (`200`, `201`, `400`) |
| 6 | Exceptions and middleware | Custom exceptions, global middleware, consistent error format (test it with a deliberate error) | ASP.NET Core middleware pipeline and order, `try/catch`, custom exceptions, writing a JSON response manually |
| 7 | Author delete with business rule | `DELETE /api/authors/{id}` with `404` and `409`; Author tests A1 to A6 pass | Business rules in services, `404` vs `409`, `DeleteBehavior.Restrict` |
| 8 | Book service: reads and create | List books, create book, author and ISBN checks | `Include` for related data, manual mapping, unique indexes, `400` vs `409` |
| 9 | Book update | Update a book without allowing the client to change `IsAvailable` | `PUT` and `204`, excluding the record itself in uniqueness checks, over-posting |
| 10 | Member service and endpoints | List members, get member by id, create member | String normalization (trim/lowercase), `CreatedAtAction`, server-controlled fields |
| 11 | Controllers and DI review | Thin controllers and correct service registrations | Extension methods for DI, service lifetimes, what "thin controller" means |
| 12 | Validation tests | Run the essential test scenarios in Swagger | Writing test cases, expected status codes, reading error responses |
| 13 | Fixes and regression | Fix failed tests and verify database constraints | Debugging, inspecting the database and indexes |
| 14 | Architecture review | Check references, no business logic in controllers, clean code | Layer boundaries, `dotnet list reference`, code review checklist |
| 15 | Wrap-up | Short README, Definition of Done, buffer time | Writing a README, listing known limitations |

> **About the study topics:** spend the first 1 to 2 hours of each day on them, then start coding. They tell you *what to learn*, not what to copy.
>
> **About the schedule:** the first working endpoint is on **Day 5** on purpose, so you see something real in Swagger early. Day 3 (migrations across two projects) often takes longer than planned. If it does, use the buffer days at the end (13 to 15).

> The removed CRUD operations are intentionally out of scope. Do not implement extra endpoints unless the instructor asks for them.

---

## 🏛️ Architecture

This project uses an **N-Tier architecture**. We split the code into projects, and each project has one job. You create **all three projects now**, so you never have to add a project or re-wire references later.

```
┌────────────────────────────────────┐
│          LibraryApi.API            │   Presentation layer
│  Controllers (thin), Middleware,   │   HTTP in, HTTP out. No logic
│  Program.cs                        │
└──────────────────┬─────────────────┘
                   │ references
                   ▼
┌────────────────────────────────────┐
│        LibraryApi.Business         │   Business Logic Layer (BLL)
│  Services, DTOs, Custom Exceptions │   Rules, checks, mapping
└──────────────────┬─────────────────┘
                   │ references
                   ▼
┌────────────────────────────────────┐
│          LibraryApi.Data           │   Data Access Layer (DAL)
│  Entities, DbContext, Migrations,  │   Database access only
│  Repositories                      │
└────────────────────────────────────┘
```

### Who is responsible for what?

| Layer | Responsible for | Must NOT |
|---|---|---|
| **API** | Routes, HTTP status codes, receiving DTOs, calling **one service**, the exception middleware | Contain business rules, use repositories or `DbContext`, or touch entities |
| **Business** | Business rules, checks, mapping between entities and DTOs, throwing custom exceptions | Know about HTTP (no `ActionResult`, no `HttpContext`) or about EF Core (no `IQueryable`, no `using Microsoft.EntityFrameworkCore`) |
| **Data** | Entities, `DbContext`, repositories, migrations | Contain business rules |

### The rules

1. **Dependencies go one way:** `API → Business → Data`. A lower project must **never** reference a higher one (Data never references Business or API, and Business never references API).
2. **A controller calls one service** and returns the result. That is all.
3. **Only services use repositories.** Controllers never use `DbContext` or repositories.
4. **Services return DTOs**, never entities. Entities never leave the Business layer.
5. **Business never uses EF Core directly.** Everything it needs from the database goes through the repository methods (`GetAllAsync`, `GetByIdAsync`, `AnyAsync`, and so on).

> .NET project references are **transitive**, so the API can technically *see* Data types through Business. The architecture rule is yours to follow: **no controller may import anything from `LibraryApi.Data`**. `Program.cs` is the only API file that may do it, to call the Data registration method. In the same way, Business can technically see EF Core through Data, but **no file in Business imports `Microsoft.EntityFrameworkCore`**.

---

## ⚙️ Solution Setup

Start from an empty folder and follow these steps **in order**. All three projects are created at the very start.

| # | Step | Details |
|---|---|---|
| 1 | Create the solution | Name: `LibraryApi` |
| 2 | Create the **API** project | Name: `LibraryApi.API`. Type: ASP.NET Core Web API, **with controllers** |
| 3 | Create the **Business** project | Name: `LibraryApi.Business`. Type: class library. DTOs, exceptions, and services will live here |
| 4 | Create the **Data** project | Name: `LibraryApi.Data`. Type: class library. Entities, the context, and repositories will live here |
| 5 | Add all three projects to the solution | |
| 6 | Set the project references | **API → Business** and **Business → Data**. Nothing else. The Data project references no other project |
| 7 | Install the packages | The SQL Server provider for EF Core (`Microsoft.EntityFrameworkCore.SqlServer`) in **Data**. `Microsoft.EntityFrameworkCore.Design` in **API** |
| 8 | Install the EF Core command-line tool (`dotnet-ef`) globally | Once per machine. Skip it if you already have it |
| 9 | Clean up the templates | Delete the default `WeatherForecast` controller and model, and the default `Class1.cs` from both class libraries |
| 10 | Build and run | The solution must build, and Swagger must open when you run the API |

> **Why `Microsoft.EntityFrameworkCore.Design` in the API?** The API is the *startup project*, and the EF tools need this package there to run migrations.

> If your template does not include Swagger, add the `Swashbuckle.AspNetCore` package to the API project and register it in `Program.cs`.

### Check your references

Before you write any other code, confirm the reference chain:

- `LibraryApi.API` references **only** `LibraryApi.Business`
- `LibraryApi.Business` references **only** `LibraryApi.Data`
- `LibraryApi.Data` references **nothing**

You can check this by opening each `.csproj` file or by using the `dotnet list reference` command on each project.

---

## 🗂️ Solution Structure

```
LibraryApi/
├── LibraryApi.sln
│
├── LibraryApi.API/                       ← Presentation layer
│   ├── Controllers/
│   │   ├── AuthorsController.cs
│   │   ├── BooksController.cs
│   │   └── MembersController.cs
│   ├── Middleware/
│   │   └── ExceptionHandlingMiddleware.cs
│   ├── appsettings.json
│   └── Program.cs
│
├── LibraryApi.Business/                  ← Business layer (BLL)
│   ├── DTOs/
│   │   ├── Authors/   (AuthorCreateDto, AuthorUpdateDto, AuthorReadDto)
│   │   ├── Books/     (BookCreateDto, BookUpdateDto, BookReadDto)
│   │   └── Members/   (MemberCreateDto, MemberUpdateDto, MemberReadDto)
│   ├── Exceptions/
│   │   ├── NotFoundException.cs
│   │   ├── ConflictException.cs
│   │   └── BadRequestException.cs
│   ├── Services/
│   │   ├── Interfaces/
│   │   │   ├── IAuthorService.cs
│   │   │   ├── IBookService.cs
│   │   │   └── IMemberService.cs
│   │   ├── AuthorService.cs
│   │   ├── BookService.cs
│   │   └── MemberService.cs
│   └── BusinessServiceRegistration.cs
│
└── LibraryApi.Data/                      ← Data access layer (DAL)
    ├── Entities/
    │   ├── BaseEntity.cs
    │   ├── Author.cs
    │   ├── Book.cs
    │   └── Member.cs
    ├── Context/
    │   └── AppDbContext.cs
    ├── Repositories/
    │   ├── IGenericRepository.cs
    │   └── GenericRepository.cs
    ├── Migrations/                       (generated by EF Core)
    └── DataServiceRegistration.cs
```

Use matching namespaces, for example `LibraryApi.Data.Entities`, `LibraryApi.Business.Services`, and `LibraryApi.Business.DTOs.Authors`.

---

## 🧩 Domain Model

### Relationship diagram

```
Author (1) ───────< (N) Book

Member   (standalone in Sprint 1; linked to books through Loan in Sprint 2)
```

### BaseEntity

An **abstract** class with one property, an integer `Id`. Every entity inherits from it. This is what lets the generic repository work with any entity.

### Author

Inherits `BaseEntity`. It has a **navigation property**: a collection of `Book` (one author has many books), initialized to an empty collection.

| Property | Type | Rules |
|---|---|---|
| `Name` | `string` | Required, max 100 characters |
| `Bio` | `string?` | Optional, max 500 characters |

### Book

Inherits `BaseEntity`. It has a **foreign key** (`AuthorId`) and a **navigation property** (`Author`) pointing to its author.

| Property | Type | Rules |
|---|---|---|
| `Title` | `string` | Required, max 200 characters |
| `ISBN` | `string` | Required, max 20 characters, **unique** |
| `PublishedYear` | `int` | Between 1000 and 2100 |
| `IsAvailable` | `bool` | Defaults to `true`. **Not** settable by the client |
| `AuthorId` | `int` | Required, must reference an existing author |

### Member

Inherits `BaseEntity`. It is standalone in this sprint.

| Property | Type | Rules |
|---|---|---|
| `FullName` | `string` | Required, max 100 characters |
| `Email` | `string` | Required, valid email format, max 150 characters, **unique** |
| `Phone` | `string?` | Optional, valid phone format, max 20 characters |
| `JoinedAt` | `DateTime` | Defaults to the current UTC time. Set by the server. **Not** settable by the client |

> String properties that are required should default to an empty string, and optional ones should be nullable, so the compiler's nullability checks stay happy.

---

## 🗄️ Database Context, Registration and Migrations

Create `AppDbContext` in `LibraryApi.Data/Context/`. It inherits from `DbContext` and takes the options through its constructor.

**It must expose:** one `DbSet` for each entity: `Authors`, `Books`, and `Members`.

**It must configure:**

| What | Rule |
|---|---|
| Unique index on `Book.ISBN` | No two books can share an ISBN |
| Unique index on `Member.Email` | No two members can share an email |
| `Author → Books` relationship | One author has many books, with `AuthorId` as the foreign key, and **Restrict** as the delete behavior (the database blocks deleting an author who still has books) |
| Maximum lengths and required fields | As listed in the Domain Model tables |

### Connection string

Add a connection string named `DefaultConnection` to `appsettings.json` in the **API** project. For a local database you can use SQL Server LocalDB:

```json
"ConnectionStrings": {
  "DefaultConnection": "Server=(localdb)\\MSSQLLocalDB;Database=LibraryApiDb;Trusted_Connection=True;TrustServerCertificate=True"
}
```

The connection string lives in the API because the API is the startup project. Never commit real passwords to GitHub.

### Registering everything (Dependency Injection)

Each project registers its own pieces in **one extension method on `IServiceCollection`**, so `Program.cs` stays short and each project hides how it is built.

| File | Project | What it registers |
|---|---|---|
| `DataServiceRegistration.cs` | Data | `AppDbContext` with the SQL Server provider and the connection string (the method receives the connection string or the configuration as a parameter), and `IGenericRepository<>` mapped to `GenericRepository<>`. This is an **open generic** registration, so one line covers every entity |
| `BusinessServiceRegistration.cs` | Business | Each service interface mapped to its class: `IAuthorService`, `IBookService`, `IMemberService` |
| `Program.cs` | API | Calls the two extension methods, adds controllers and Swagger, and registers the exception middleware first |

Use the **scoped** lifetime for the repository and the services. Scoped means one instance per HTTP request, so everything in one request shares the same `AppDbContext`. (`AddDbContext` is already scoped by default.)

### Creating and applying the migration

The `DbContext` lives in **Data**, but the EF tool needs a **startup project** (the one with `Program.cs` and the connection string). So every command names both. Run these from the solution folder:

```
dotnet ef migrations add InitialCreate --project LibraryApi.Data --startup-project LibraryApi.API
dotnet ef database update --project LibraryApi.Data --startup-project LibraryApi.API
```

| Option | Meaning |
|---|---|
| `--project` | The project that **contains** the `DbContext`. The `Migrations` folder is created here |
| `--startup-project` | The project the tool **runs** to read the configuration and the DI registration |

After the first command, check that a `Migrations` folder appears in `LibraryApi.Data`. After the second one, open the database (SQL Server Object Explorer or any client) and check that the three tables and the two unique indexes exist.

> If you see "Unable to create a 'DbContext'", check that the startup project is the API, that `Microsoft.EntityFrameworkCore.Design` is installed in the API, that the connection string name matches, and that the Data registration is called in `Program.cs`.
>
> Every time you change an entity or the context configuration, add a **new** migration with a clear name (for example `AddBookIndexes`). Never edit a migration that was already applied.

---

## 🧱 Generic Repository

### Why do we need it?

Without a repository, every service repeats the same `DbContext` code (get all, get by id, add, update, delete) for every entity. A **Generic Repository** writes that code **once** and reuses it for any entity type. The repository contains **no business rules**, only data access.

### The contract

Create `IGenericRepository<T>` and `GenericRepository<T>` in `LibraryApi.Data/Repositories/`. `T` is restricted to types that inherit from `BaseEntity`. The repository receives the `AppDbContext` through its constructor.

| Operation | What it does | Notes |
|---|---|---|
| `GetAllAsync(includes)` | Returns all entities of type `T` | Optional list of related entities to load. This query is read-only, so it can skip change tracking |
| `GetByIdAsync(id, includes)` | Returns one entity, or `null` if it does not exist | Do **not** use `FindAsync` here, because it cannot be combined with includes. Filter by `Id` and take the first or default. **Keep change tracking on**, because services change the returned entity and then update it |
| `AnyAsync(predicate)` | Returns `true` if any entity matches the condition | Used by services for duplicate checks (ISBN, email) and "has books" checks |
| `AddAsync(entity)` | Adds a new entity and saves | |
| `UpdateAsync(entity)` | Marks the entity as modified and saves | |
| `DeleteAsync(entity)` | Removes the entity and saves | |

### How `includes` works

The `includes` parameter is a list of expressions that point to **navigation properties** (for example "the author of a book"). The repository loops over them and tells EF to load each one. This solves the "Book needs its Author's name" problem without writing a separate `BookRepository`: the service asks for all books and includes the author.

> **Hint:** the `includes` parameter is a `params` array of `Expression<Func<T, object>>`. Inside the repository, loop over it and call `Include(...)` on the query. A service passes it like `b => b.Author`. Take 30 minutes to understand why it is an *expression* and not a plain `Func` (EF Core must translate it to SQL). If you are stuck, ask your instructor.

### Important notes for this sprint

- In Sprint 1 the repository calls `SaveChangesAsync()` **inside** `AddAsync`, `UpdateAsync`, and `DeleteAsync`. This keeps things simple. Sprint 2 explains the problem with this approach and fixes it.
- Services use `IGenericRepository<T>` and **never** `AppDbContext` directly. They never import `Microsoft.EntityFrameworkCore` either.

---

## 📨 DTOs

Never return or accept entities outside the Business layer. Create DTOs in `LibraryApi.Business/DTOs` and map them **manually** (no AutoMapper yet). Each service has a small private mapping method (entity to read DTO).

> You do **not** create all DTOs on one day. Create each entity's DTOs when you reach its service: Authors on Day 5, Books on Day 8, Members on Day 10. The tables below are the reference.

The **Validation** column says **what** must be checked. You decide **how** to enforce it in code on the DTOs. Invalid input must be rejected with `400` before it reaches the service.

### Authors

| DTO | Fields | Validation |
|---|---|---|
| `AuthorCreateDto` | `Name`, `Bio?` | `Name`: required, max 100 characters · `Bio`: optional, max 500 characters |
| `AuthorReadDto` | `Id`, `Name`, `Bio?`, `BooksCount` | none |

### Books

| DTO | Fields | Validation |
|---|---|---|
| `BookCreateDto` | `Title`, `ISBN`, `PublishedYear`, `AuthorId` | `Title`: required, max 200 characters · `ISBN`: required, max 20 characters · `PublishedYear`: a number between 1000 and 2100 · `AuthorId`: a positive number (1 or greater) |
| `BookUpdateDto` | `Title`, `ISBN`, `PublishedYear`, `AuthorId` | Same as create |
| `BookReadDto` | `Id`, `Title`, `ISBN`, `PublishedYear`, `IsAvailable`, `AuthorId`, `AuthorName` | none |

### Members

| DTO | Fields | Validation |
|---|---|---|
| `MemberCreateDto` | `FullName`, `Email`, `Phone?` | `FullName`: required, max 100 characters · `Email`: required, valid email format, max 150 characters · `Phone`: optional, valid phone format, max 20 characters |
| `MemberReadDto` | `Id`, `FullName`, `Email`, `Phone?`, `JoinedAt` | none |

> `IsAvailable` and `JoinedAt` do **not** appear in create/update DTOs. The server controls them.
>
> `BooksCount` (in `AuthorReadDto`) is the number of books the author has. `AuthorName` (in `BookReadDto`) comes from the book's author. The service loads the related data with `includes`.

---

## 🚨 Exceptions and the Global Exception Middleware

### Custom exceptions

Services signal errors by **throwing** these. They know nothing about HTTP. They live in `LibraryApi.Business/Exceptions`. Each one inherits from `Exception` and takes a message.

| Exception | Meaning | Mapped by the middleware to |
|---|---|---|
| `NotFoundException` | The requested record does not exist | `404` |
| `ConflictException` | The action conflicts with the current state of the data (duplicate, still in use, already returned, already borrowed) | `409` |
| `BadRequestException` | The request is invalid in a way the DTO validation cannot catch (for example, an id in the **body** that points to a record that does not exist) | `400` |

### The middleware

The middleware turns exceptions into consistent JSON responses. Create `LibraryApi.API/Middleware/ExceptionHandlingMiddleware.cs`.

**How it works:**

1. It wraps the rest of the request pipeline in a try/catch.
2. If nothing is thrown, the request continues normally.
3. If an exception is thrown, it picks the status code and message from the table below and writes a JSON response.

| Exception thrown | Status code | Message sent to the client |
|---|---|---|
| `NotFoundException` | `404` | The exception's own message |
| `ConflictException` | `409` | The exception's own message |
| `BadRequestException` | `400` | The exception's own message |
| Anything else | `500` | A generic text: "An unexpected error occurred." |

For unexpected errors (`500`), **log the full exception** on the server. The client never sees the details.

Register the middleware **first** in the pipeline in `Program.cs`, before everything else.

**Error response format used by the whole API.** Every error is a JSON object with three properties:

| Property | Content |
|---|---|
| `statusCode` | The HTTP status code (for example `409`) |
| `message` | A clear text (for example, that a book with a given ISBN already exists) |
| `timestamp` | The current UTC time |

> No stack trace is ever sent to the client. The automatic `400` returned for failed DTO validation uses ASP.NET Core's built-in format. That is fine for this project.

---

## 🧠 Services and Business Logic

Every entity has one service in `LibraryApi.Business/Services`, with an interface in `Services/Interfaces`. A service owns its business rules, accepts/returns DTOs only, and throws custom exceptions. Controllers call services only.

### Required service operations

| Service | Required operations | Main rules |
|---|---|---|
| `AuthorService` | `GetAllAsync`, `CreateAsync`, `DeleteAsync` | Trim input; an author with books cannot be deleted; compute `BooksCount` for list results |
| `BookService` | `GetAllAsync`, `CreateAsync`, `UpdateAsync` | Author must exist; ISBN must be unique; server sets `IsAvailable = true` on create and update must not change it; include `AuthorName` in reads |
| `MemberService` | `GetAllAsync`, `GetByIdAsync`, `CreateAsync` | Normalize email (trim/lowercase); email must be unique; server sets `JoinedAt` |

Do not implement service methods that exist only to support removed endpoints. In particular, `AuthorService` does not need get-by-id or update, `BookService` does not need get-by-id or delete, and `MemberService` does not need update or delete in this sprint.

### Order of checks

For an operation that uses an id in the URL, first check that the record exists (`404`). For create/update inputs, validate the request (`400`) and then check conflicts such as duplicate ISBN/email (`409`).

### AuthorService

Uses the author repository and book repository.

| Operation | Business logic |
|---|---|
| `GetAllAsync` | Load authors with their books and map each to a DTO containing `BooksCount` |
| `CreateAsync` | Trim name and bio, add the author, and return the created DTO (`BooksCount = 0`) |
| `DeleteAsync` | If the author does not exist, throw `NotFoundException`. If any book belongs to the author, throw `ConflictException`; otherwise delete |

### BookService

Uses the book repository and author repository.

| Operation | Business logic |
|---|---|
| `GetAllAsync` | Load books with their author and include `AuthorName` in each read DTO |
| `CreateAsync` | Trim title and ISBN. The author must exist (`BadRequestException`) and ISBN must be unique (`ConflictException`). Set `IsAvailable = true` on the server |
| `UpdateAsync` | Find the book or throw `NotFoundException`. Validate author existence and ISBN uniqueness (excluding this book). Update title, ISBN, year, and author only; do not change `IsAvailable` |

### MemberService

Uses the member repository.

| Operation | Business logic |
|---|---|
| `GetAllAsync` | Load all members and map each one |
| `GetByIdAsync` | Throw `NotFoundException` if the member does not exist; otherwise return its DTO |
| `CreateAsync` | Trim name/phone, trim and lowercase email, reject duplicate email with `ConflictException`, and set `JoinedAt` to current UTC time |

---

## 🔌 API Specification

All routes start with `/api`. All bodies are JSON, and property names use camelCase.

> **Scope:** Only the following **9 endpoints** are required. Other CRUD endpoints are intentionally removed from this sprint.

### Authors — 3 endpoints

| Method | Route | Description | Success | Errors |
|---|---|---|---|---|
| GET | `/api/authors` | List authors with `booksCount` | `200` | none |
| POST | `/api/authors` | Create an author | `201` | `400` |
| DELETE | `/api/authors/{id}` | Delete author if it has no books | `204` | `404`, `409` |

### Books — 3 endpoints

| Method | Route | Description | Success | Errors |
|---|---|---|---|---|
| GET | `/api/books` | List books with author name | `200` | none |
| POST | `/api/books` | Create a book | `201` | `400`, `409` |
| PUT | `/api/books/{id}` | Update a book | `204` | `400`, `404`, `409` |

### Members — 3 endpoints

| Method | Route | Description | Success | Errors |
|---|---|---|---|---|
| GET | `/api/members` | List members | `200` | none |
| GET | `/api/members/{id}` | Get one member | `200` | `404` |
| POST | `/api/members` | Create a member | `201` | `400`, `409` |

For each POST, return the created DTO with `201 Created`. A `Location` header is optional in this reduced scope because the get-by-id endpoint is intentionally not implemented for authors or books. For members, `CreatedAtAction` may point to `GetById`.

### Thin controllers

- Mark controllers with `[ApiController]` and route `api/[controller]`.
- Inject only the relevant service interface.
- Each action calls the service and returns the appropriate HTTP response.
- Do not put business rules or repository calls in controllers.

## 📏 Validation and Business Rules

| # | Rule | Enforced by | Response |
|---|---|---|---|
| 1 | The DTO breaks its validation rules (missing name, bad email, year out of range, ...) | The DTO validation (automatic in a controller marked `[ApiController]`) | `400` |
| 2 | Any `GET/PUT/DELETE` where the id in the **URL** does not exist | Every service | `404` (`NotFoundException`) |
| 3 | Creating a book or updating one with an `AuthorId` in the **body** that does not exist | `BookService` | `400` (`BadRequestException`) |
| 4 | Creating a book or updating one with an ISBN used by **another** book | `BookService` | `409` (`ConflictException`) |
| 5 | Creating a member with an email used by **another** member | `MemberService` | `409` (`ConflictException`) |
| 6 | Deleting an author who still has books | `AuthorService` | `409` (`ConflictException`) |
| 7 | Emails are stored **trimmed and lowercase**, so `Mona@x.com` and `mona@x.com` are the same email | `MemberService` | n/a |
| 8 | Text input (names, titles, ISBN, bio, phone) is **trimmed** before saving | All services | n/a |
| 9 | The server controls `IsAvailable` (`true` on create, never changed by update) and `JoinedAt` (set on create, never changed by update) | `BookService`, `MemberService` | n/a |
| 10 | An unexpected error returns a generic message and **no stack trace** | Middleware | `500` |

> **Id rule (rules 2 and 3):** an id in the URL that points to nothing is `404`, because the resource you asked for does not exist. An id in the body that points to nothing is `400`, because the request itself is wrong. Sprint 2 follows the same rule.

> **Updating tip (rules 4 and 5):** when a book is updated with its own current ISBN, that is not a conflict. When checking ISBN duplicates during book update, **exclude the record itself**.

All error responses use the format from the middleware section: `statusCode`, `message`, `timestamp`.

---

## 🌱 Sample Seed Data

Use this data to test quickly through Swagger.

**Authors**

| Name | Bio |
|---|---|
| Robert C. Martin | Author of Clean Code |
| Martin Fowler | Author of Refactoring |
| Jon Skeet | Author of C# in Depth |

**Books**

| Title | ISBN | Year | Author |
|---|---|---|---|
| Clean Code | 9780132350884 | 2008 | Robert C. Martin |
| Clean Architecture | 9780134494166 | 2017 | Robert C. Martin |
| Refactoring | 9780134757599 | 2018 | Martin Fowler |
| C# in Depth | 9781617294532 | 2019 | Jon Skeet |

**Members**

| Full Name | Email |
|---|---|
| Mona Ali | mona.ali@example.com |
| Omar Hassan | omar.hassan@example.com |
| Sara Ahmed | sara.ahmed@example.com |

> Optional: add this data with `HasData(...)` in the context's model configuration, or just create it by calling your own POST endpoints.
>
> If you use `HasData`:
> - Give every row an explicit `Id`, and set each book's `AuthorId` with those same ids.
> - Give `JoinedAt` a **fixed** value (for example `new DateTime(2025, 1, 1)`). **Never** use `DateTime.UtcNow` there. EF compares the model with the last snapshot, so a value that changes on every build puts a fake change to the seed rows in every new migration, and in newer EF Core versions it can even make applying migrations fail.
> - Write the emails in lowercase, as rule 7 requires.
> - Add the seed in its **own** migration (for example `SeedData`).

---

## 🧪 Test Scenarios

Run these essential tests in Swagger. Replace `{id}` with a real id. For each error, check that the response body contains `statusCode`, `message`, and `timestamp`.

### Authors

| # | Request | Expected |
|---|---|---|
| A1 | `GET /api/authors` | `200`, each author includes `booksCount` |
| A2 | `POST /api/authors` with a valid body | `201`, `booksCount` is `0` |
| A3 | `POST /api/authors` with an empty name | `400` |
| A4 | `DELETE /api/authors/{id}` for an author with no books | `204` |
| A5 | `DELETE /api/authors/{id}` for an author who has books | `409` |
| A6 | `DELETE /api/authors/9999` | `404` |

### Books

| # | Request | Expected |
|---|---|---|
| B1 | `GET /api/books` | `200`, each book includes `authorName` |
| B2 | `POST /api/books` with a valid body | `201`, `isAvailable` is `true` |
| B3 | `POST /api/books` with `authorId` `9999` | `400` |
| B4 | `POST /api/books` with an ISBN that already exists | `409` |
| B5 | `POST /api/books` with `publishedYear` `999` | `400` |
| B6 | `PUT /api/books/{id}` with a valid body | `204` |
| B7 | `PUT /api/books/{id}` keeping the book's own ISBN | `204` |
| B8 | `PUT /api/books/{id}` with the ISBN of another book | `409` |
| B9 | `PUT /api/books/9999` with a valid body | `404` |
| B10 | Add `"isAvailable": false` to a create request | `201`, and returned `isAvailable` is still `true` |

### Members

| # | Request | Expected |
|---|---|---|
| M1 | `GET /api/members` | `200` |
| M2 | `GET /api/members/{id}` for an existing member | `200` |
| M3 | `GET /api/members/9999` | `404` |
| M4 | `POST /api/members` with a valid email with spaces and uppercase letters | `201`, email is trimmed/lowercase and `joinedAt` is filled |
| M5 | `POST /api/members` with an invalid email | `400` |
| M6 | `POST /api/members` with an existing email in different letter case | `409` |

### General

| # | Test | Expected |
|---|---|---|
| G1 | Send malformed JSON | `400` |
| G2 | Trigger a database connection error | `500`, generic message, no stack trace in response |

---

## ⚠️ Known Limitations

These are known weaknesses. You do **not** need to fix them in this sprint, but write them in your README so you know they exist.

- **Duplicate check race.** `BookService` checks the ISBN with `AnyAsync` and then saves. If two requests with the same ISBN arrive at the same moment, both can pass the check. The unique index then rejects the second one with a database exception, and the middleware returns `500` instead of `409`. The same applies to member emails. The index keeps the data correct. Only the status code is wrong.
- **Saving inside the repository.** Each `AddAsync`, `UpdateAsync`, and `DeleteAsync` saves by itself, so one operation cannot be saved as a group. Sprint 2 fixes this with the Unit of Work.

---

## ✔️ Definition of Done

**Architecture**
- [ ] The solution has three projects: `LibraryApi.API`, `LibraryApi.Business`, and `LibraryApi.Data`
- [ ] The references are exactly `API → Business → Data`: `Data` references neither `Business` nor `API`, and `Business` does not reference `API`
- [ ] Every entity has its own service (`AuthorService`, `BookService`, `MemberService`) with an interface
- [ ] Controllers depend **only** on a service interface (no repositories, no `AppDbContext`, no entities)
- [ ] Controllers contain no business rules (no `if` checks for rules)
- [ ] DTOs live in the Business project, and no entity is returned from or accepted by any endpoint
- [ ] No file in `LibraryApi.Business` imports `Microsoft.EntityFrameworkCore`

**Behavior**
- [ ] The solution builds and the API runs with no errors
- [ ] All 9 required endpoints work and were tested in Swagger
- [ ] The required business rules are implemented, and every row of the [Test Scenarios](#-test-scenarios) table gives the expected status code
- [ ] A missing URL id for an implemented endpoint returns `404`, and a missing id in the request body returns `400`
- [ ] The exception middleware is registered first and every error uses the same JSON format
- [ ] Unique indexes exist for `ISBN` and `Email`
- [ ] Migrations are in the **Data** project

**Quality**
- [ ] Code is clean: clear names, no unused code, English comments
- [ ] The `README.md` of your own repository explains how to run the project and lists the known limitations

---

## 📦 Deliverables

1. The solution source code on GitHub
2. The `Migrations` folder (inside `LibraryApi.Data`)
3. A short `README` explaining how to run the project: the connection string, how to apply the migrations, how to run the API project, and the known limitations
4. (Optional) A screenshot of Swagger showing the 9 required endpoints

---

## 🔭 What Comes Next

In **Sprint 2** you will:

1. **Refactor** the data layer: remove `SaveChangesAsync` from the repository and introduce the **Unit of Work** pattern. Your services will switch from repositories to the Unit of Work, and the controllers will not change
2. Add the **Loan** entity and a new **`LoanService`** with the **borrow / return** business logic, plus an endpoint for the loans of one member (it lives in `LoansController`, so every controller still calls one service)
3. Add the Loan feature with borrow, return, and member loan-history operations

Make sure everything in this sprint works before you start Sprint 2.
