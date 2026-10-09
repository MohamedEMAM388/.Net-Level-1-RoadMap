# Sprint 3 — Entity Framework Core

> Project: **Library Management System (EF Core)**
> Duration: **2 weeks (14 days) · Day 29–42**

## Sprint Goal

Learn to persist and query data with EF Core: model the database, write efficient queries, understand tracking and loading, and read the SQL that EF generates.

## How to use this sprint plan

For every task:

1. Read the **MUST** resources before implementation.
2. Read **DEEP DIVE** only when the concept is unclear or the learner wants deeper understanding.
3. Implement the requested change.
4. Verify the behavior manually.
5. Commit the work in small, meaningful commits.
6. Be able to explain the decision during review.

**Important:** This document does not change the original Sprint scope. It expands the original tasks into an executable checklist.

## Project Brief — Library Management System

A library database: books, authors, categories, and members, then borrowing, returning, and fines. The project practises Code First, Fluent API, migrations, seeding, the Repository pattern, eager loading, and transactions.

Relationships: Category 1–N Book · Author 1–N Book · Member 1–N Loan · Book 1–N Loan · Loan 1–1 Fine

### Data Model

**Category**

| Field | Type / Rule |
| --- | --- |
| Id | int |
| Name | required, unique, max 100 |

**Author**

| Field | Type / Rule |
| --- | --- |
| Id | int |
| Name | required, max 150 |
| Country | max 60 |

**Book**

| Field | Type / Rule |
| --- | --- |
| Id | int |
| Title | required, max 200 |
| ISBN | unique, max 20 |
| PublishedYear | int |
| Status | enum { Available, Borrowed, Lost }, stored as text label |
| CategoryId, AuthorId | int, int |

**Member**

| Field | Type / Rule |
| --- | --- |
| Id | int |
| FullName | required |
| Email | unique, valid format |
| Phone | valid format |
| JoinDate | default = today |
| Status | enum { Active, Suspended }, stored as text |
| Address | owned entity (Street, City) |

**Loan**

| Field | Type / Rule |
| --- | --- |
| Id | int |
| BookId, MemberId | int, int |
| LoanDate | DateTime |
| DueDate | LoanDate + 14 days |
| ReturnDate | DateTime?, null while the book is out |

**Fine**

| Field | Type / Rule |
| --- | --- |
| Id | int |
| LoanId | int, one fine per late loan |
| Amount | decimal(10,2) |
| IssuedDate | DateTime |
| IsPaid | bool, default false |

### Project Requirements

**Easy** — setup, first migration, basic CRUD

- **E1** Create the project, install the EF Core packages, and add `LibraryDbContext` with `DbSet` for Book, Category, Author, Member.
- **E2** Write Fluent API configurations in separate `IEntityTypeConfiguration<T>` classes: required, max length, unique index.
- **E3** Relationships: Category has many Books; Author has many Books. Choose delete behaviour so deleting an author does not cascade to books.
- **E4** Create the first migration and update the database. Seed 4 categories and 6 authors with `HasData`.
- **E5** Console CRUD for Category and Author. Block deleting one that still has books, with a friendly message.

**Medium** — constraints, owned entity, repository

- **M1** Seed 15 Books and 8 Members (with Address values).
- **M2** Add a check constraint (`PublishedYear` between 1000 and the current year), default values (`JoinDate`, book `Status = Available`), and store enums as text labels.
- **M3** Validate Member email and phone (Data Annotations or manual checks) before saving.
- **M4** Owned entity Address on Member. Add the second migration for all these changes.
- **M5** A generic `Repository<T>` (Add, GetById, GetAll, Update, Delete) used for Book, Member, Category, and Author.
- **M6** Query 1: all Available books with author and category name, sorted by title, using eager loading (`Include` / `ThenInclude`).
- **M7** Query 2: number of books per category. Use `AsNoTracking()` on this and on every read-only query. Print the SQL once with `ToQueryString()`.

**Hard** — loans, fines, rules, transactions

- **H1** Add Loan and Fine entities, configurations, and the third migration.
- **H2** Borrow: allowed only if the member is Active, the book is Available, and the member has fewer than 3 active loans. `DueDate` = today + 14 days; the book becomes Borrowed.
- **H3** Return: set `ReturnDate`; the book becomes Available. If late, create a Fine automatically (5 per late day, kept in one constant).
- **H4** Auto-suspend: a member with an unpaid Fine becomes Suspended and cannot borrow. Paying all fines makes them Active again.
- **H5** Query 3: members with active loans and the titles they hold.
- **H6** Query 4: overdue loans: member, book, days overdue.
- **H7** Query 5: members with unpaid fines and the total each owes, highest first.
- **H8** Transaction: Borrow and Return each run in one transaction (`BeginTransaction`, `Commit`, `Rollback`). Prove it: force an error after saving the loan and show nothing was stored.

**Bonus** (optional, only if time allows)

- Explicit loading: load one member's Loans with `Entry(member).Collection(m => m.Loans).Load()` and compare it with `Include`.
- Show `IEnumerable` vs `IQueryable` with one query and the SQL each one produces.

### Test Scenarios the Reviewer Will Try

| # | Scenario | Expected result |
| --- | --- | --- |
| 1 | Borrow a book that is already Borrowed | Refused with a clear message |
| 2 | A member with 3 active loans borrows again | Refused |
| 3 | Return a book 4 days late | Fine created automatically, member Suspended |
| 4 | A Suspended member tries to borrow | Refused until the fine is paid |
| 5 | Delete an Author that has books | Blocked with a message, no exception |
| 6 | A failure in the middle of Borrow | Rollback: no loan row, the book is still Available |


## Working Rules

- Work in order: finish **Easy** before **Medium**, and **Medium** before **Hard**. Commit after every finished requirement.
- Every requirement has an ID (E1, M3, H2). Use it in your commit message, for example `M3: add Predicate search`.
- Use only what was taught up to this sprint.
- The app must never crash on bad input. A wrong menu choice, empty text, or a negative number gets a friendly message, then the app continues.
- Push to GitHub with a README (see the README task).

## Sprint Tasks

### Task 1 — Setup

*Day 29.*

Create the DbContext and entities, and manage the schema with migrations.

Then create the project and configure Book, Category, and Author.

**Apply to the project:** **E1**, **E2**, **E3** (see Project Requirements above)

**Required output**
- Project with EF Core packages and `LibraryDbContext`
- Fluent API configurations in separate `IEntityTypeConfiguration<T>` classes
- Category–Book and Author–Book relationships with a safe delete behaviour
- Practice: add, apply, rollback, remove, and script a migration

### Resources
- **MUST** — [EF Core overview](https://learn.microsoft.com/en-us/ef/core/)
- **MUST** — [Migrations overview](https://learn.microsoft.com/en-us/ef/core/managing-schemas/migrations/)
- **DEEP DIVE** — Jon P. Smith — Entity Framework Core in Action — `Book: Entity Framework Core in Action`

---

### Task 2 — CRUD & Seeding

*Day 30.*

Implement CRUD operations and seed data.

**Apply to the project:** **E4**, **E5**, **M1** (see Project Requirements above)

**Required output**
- First migration applied; 4 categories and 6 authors seeded with `HasData`
- Console CRUD for Category and Author with the delete block
- 15 books and 8 members seeded
- Explanation of what `SaveChanges` actually does

### Resources
- **MUST** — [Saving data](https://learn.microsoft.com/en-us/ef/core/saving/)
- **MUST** — [Data seeding](https://learn.microsoft.com/en-us/ef/core/modeling/data-seeding)

---

### Task 3 — Modeling

*Day 31–32.*

Model One-to-Many, Many-to-Many, and One-to-One relationships, configure the Fluent API, map enums, and use owned entities.

**Apply to the project:** **M2**, **M3**, **M4**, **H1** (see Project Requirements above)

**Required output**
- Check constraint, default values, and enums stored as text
- Email and phone validation
- Owned entity Address on Member and the second migration
- Loan and Fine entities with the Loan–Fine One-to-One, and the third migration

### Resources
- **MUST** — [Relationships](https://learn.microsoft.com/en-us/ef/core/modeling/relationships)
- **MUST** — [Owned entity types](https://learn.microsoft.com/en-us/ef/core/modeling/owned-entities)
- **MUST** — [Value conversions](https://learn.microsoft.com/en-us/ef/core/modeling/value-conversions)
- **DEEP DIVE** — Jon P. Smith — Entity Framework Core in Action — `Book: Entity Framework Core in Action`

---

### Task 4 — LINQ to Entities

*Day 33.*

Write queries with IQueryable, project into DTOs, and review how each query becomes SQL.

Then build the generic repository.

**Apply to the project:** **M5** (see Project Requirements above)

**Required output**
- Generic `Repository<T>` used for Book, Member, Category, and Author
- Queries built with `IQueryable` and the generated SQL reviewed
- Example of a query EF cannot translate

### Resources
- **MUST** — [Querying data](https://learn.microsoft.com/en-us/ef/core/querying/)

---

### Task 5 — Loading Strategies

*Day 34.*

Use Include / ThenInclude, reproduce and fix the N+1 problem, and compare split and single queries.

**Apply to the project:** **M6** (see Project Requirements above)

**Required output**
- Query 1: available books with author and category name, sorted by title
- N+1 reproduced and fixed
- Split query vs single query comparison

### Resources
- **MUST** — [Loading related data](https://learn.microsoft.com/en-us/ef/core/querying/related-data/)
- **MUST** — [Efficient querying](https://learn.microsoft.com/en-us/ef/core/performance/efficient-querying)

---

### Task 6 — Change Tracking

*Day 35.*

Inspect entity states, use AsNoTracking, and update entities with Attach / Update.

Then implement Borrow and Return.

**Apply to the project:** **M7**, **H2**, **H3** (see Project Requirements above)

**Required output**
- Query 2: books per category, with `AsNoTracking()` and the SQL printed once with `ToQueryString()`
- Borrow with all its rules (Active member, Available book, fewer than 3 active loans)
- Return with the automatic fine (5 per late day, kept in one constant)

### Resources
- **MUST** — [Change tracking](https://learn.microsoft.com/en-us/ef/core/change-tracking/)
- **MUST** — [Tracking vs no-tracking queries](https://learn.microsoft.com/en-us/ef/core/querying/tracking)

---

### Task 7 — Raw SQL & Bulk Operations

*Day 36.*

Run raw SQL and bulk updates and deletes.

Then implement the auto-suspend rule (use `ExecuteUpdate` where it fits).

**Apply to the project:** **H4** (see Project Requirements above)

**Required output**
- A raw SQL query with `FromSql` and `ExecuteSql` / `ExecuteUpdate` / `ExecuteDelete` examples
- A member with an unpaid fine becomes Suspended; paying all fines makes them Active again

### Resources
- **MUST** — [SQL queries](https://learn.microsoft.com/en-us/ef/core/querying/sql-queries)
- **MUST** — [ExecuteUpdate and ExecuteDelete](https://learn.microsoft.com/en-us/ef/core/saving/execute-insert-update-delete)

---

### Task 8 — Concurrency & Transactions

*Day 37.*

Add optimistic concurrency and use explicit transactions with Commit / Rollback.

Then wrap Borrow and Return in transactions.

**Apply to the project:** **H8** (see Project Requirements above)

**Required output**
- Borrow and Return each run in one transaction
- Proof: force an error after saving the loan and show nothing was stored
- `RowVersion` concurrency token and a concurrency conflict reproduced (practice)

### Resources
- **MUST** — [Handling concurrency conflicts](https://learn.microsoft.com/en-us/ef/core/saving/concurrency)
- **MUST** — [Transactions](https://learn.microsoft.com/en-us/ef/core/saving/transactions)

---

### Task 9 — Global Query Filters

*Day 38.*

Implement soft delete with a global query filter.

This is a practice topic and is not one of the project requirements.

**Required output**
- Soft delete on a practice entity with a global query filter

### Resources
- **MUST** — [Global query filters](https://learn.microsoft.com/en-us/ef/core/querying/filters)

---

### Task 10 — Performance Review

*Day 39.*

Review the project's queries, read the SQL, and fix inefficient queries.

**Required output**
- List of inefficient queries found and how each was fixed
- Early `ToList` calls removed
- `AsNoTracking()` on every read-only query

### Resources
- **MUST** — [Performance](https://learn.microsoft.com/en-us/ef/core/performance/)

---

### Task 11 — Sprint Project — Finish the Hard Requirements

*Day 40–41.*

Finish the remaining Hard queries, run all test scenarios.

**Apply to the project:** **H5**, **H6**, **H7** (see Project Requirements above)

**Required output**
- Query 3: members with active loans and the titles they hold
- Query 4: overdue loans with days overdue
- Query 5: members with unpaid fines and the total each owes, highest first
- All 6 test scenarios pass

---

### Task 12 — README

*Day 42.*

Document:
- Project overview
- Features (every requirement ID marked done or skipped)
- Database setup (provider, connection-string placeholder, migration commands)
- Data model and relationships
- How to run
- Queries (loading strategy and `AsNoTracking()` use for each query)
- Sample output, including a transaction rollback
- Design decisions (loading strategies, tracking, transactions)
- Known limitations

**Required output**
- A `README.md` in the repository root
- Commands in code blocks that run as written
- Screenshots or sample output stored in the repo
- No secrets or real credentials

### Resources
- **MUST** — [GitHub — About READMEs](https://docs.github.com/en/repositories/managing-your-repositorys-settings-and-features/customizing-your-repository/about-readmes)

---

## Sprint Deliverables

- [ ] Library Management System (EF Core) with all Easy, Medium, and Hard requirements (or clearly marked as skipped)
- [ ] Three migrations and seed data
- [ ] All 5 queries and the transactions
- [ ] Practice code for raw SQL, concurrency, and global query filters
- [ ] README
- [ ] Commit history that follows the requirement IDs

## Definition of Done

- [ ] All Easy requirements (E1–E5) are done
- [ ] All Medium requirements (M1–M7) are done
- [ ] All Hard requirements (H1–H8) are done, or clearly marked as skipped
- [ ] All 6 test scenarios pass
- [ ] Borrow and Return are transactional (rollback proven)
- [ ] Every read-only query uses `AsNoTracking()`
- [ ] README is complete and commits follow the requirement IDs

## Before You Submit

- [ ] The solution builds with no errors, and seed data loads on first run
- [ ] Every Easy / Medium / Hard requirement is done, or clearly marked as skipped
- [ ] No secrets, passwords, or real connection strings in the repo
- [ ] The README is complete and the commit history follows the requirement IDs

## Review Rule

The learner should be able to explain not only **what** was implemented, but **why it belongs there, what alternatives existed, and what trade-off was accepted**.
