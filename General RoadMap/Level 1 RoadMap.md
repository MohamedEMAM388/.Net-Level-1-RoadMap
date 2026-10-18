Tawjeh Level 1 — Backend .NET Developer Roadmap
Duration: 6 sprints · 86 days
Learning path: C# Foundations → LINQ → Database Design & SQL → EF Core → ASP.NET Core Web API → Project Defense & Mock Interview
🎯 Level 1 Goal
By the end of Level 1, learners should be able to design a relational database, build a working layered Web API on top of it, enforce business rules, test and debug their work, and explain their technical decisions in a final project defense and mock interview.
Learning cycle: Learn → Practice → Build → Debug → Explain
🗺️ Roadmap Overview
Sprint	Focus	Days	Project
1	Collections & Generics	1–14	Job Application Tracker — Part 1 (Console)
2	Delegates, Lambdas & Events	15–28	Job Application Tracker — Part 2 (same project)
3	LINQ	29–42	Sales Data Analytics Dashboard (Console)
4	Database Design, SQL & EF Core	43–56	Library Management Database + EF Core
5	Web API Foundations, Services & Business Logic	57–71	Library Management API — Part 1
6	Unit of Work, Loans & Final Interview	72–86	Library Management API — Part 2 + Mock Interview


🔗 How the Projects Connect
- Sprints 1–2: Build one console project, the Job Application Tracker. Sprint 2 extends Sprint 1.
- Sprint 3: Build a separate in-memory Sales Data Analytics Dashboard; no database is required.
- Sprint 4: Design the Library Management database, practice SQL against it, then implement the same domain using EF Core.
- Sprints 5–6: Continue the same Library Management domain as a Web API. Sprint 6 improves the solution from Sprint 5; it is not a new project.
The goal is a gradual learning path: each sprint builds on the previous one instead of introducing unrelated projects and concepts.
📅 Sprint 1 — Collections & Generics
Day 1–14 · Project: Job Application Tracker — Part 1 of 2 (Console)
🎯 Sprint Goal
Build a strong C# foundation: choose the right collection, write reusable generic code, and use both to build the data foundation of a console application.
Sprint rules: no LINQ, no database, no Entity Framework. All data stays in memory.
📚 What You Will Learn
1. Collections
Learn:
- List<T>
- Dictionary<TKey, TValue>
- HashSet<T>
- Queue<T>
- Stack<T>
- How to choose the right collection (access pattern, uniqueness, ordering)
2. Generics
Learn:
- Generic methods
- Generic classes
- Constraints (where)
- Type parameters and type arguments
🛠️ Practical Implementation
Job Application Tracker — Part 1 (Console)
Build the data foundation of a console job portal: store and manage companies, recruiters, jobs, and candidates through console menus, using:
- A generic DataStore<T> with where T : class, IEntity
- A Dictionary<int, Job> kept consistent next to the jobs store
- A HashSet<string> to check candidate email uniqueness
- Fixed seed data (3 companies, 3 recruiters, 6 jobs, 5 candidates)
📝 Sprint Tasks
Part 1 — Study & Practice (Day 1–9)
Day	Focus	Requirements
1–2	List<T>: add, remove, search, update	CL1
3	Dictionary<TKey, TValue>	CL2
4	HashSet<T>	CL3
5	Queue<T> and Stack<T>	CL4
6	Collections review and the required note	CL1–CL4
7	Generic methods	G1
8	Generic classes	G2
9	Constraints, IEntity, and the generics explanation	G1–G2


Part 2 — Build the Project (Day 10–14)
Day	Focus	Requirements
10	Models, seed data, and console menus	E1, E2
11	Generic DataStore<T>	M1
12	Basic operations for companies, recruiters, and candidates	E3
13	Job operations and the Dictionary lookup	E3, E5
14	Run all test scenarios, write the README, and tag sprint-1	Hand-off


Requirements: CL1–CL4, G1, G2 (practice) · E1, E2, M1, E3, E5 (project) · Test scenarios: 1–5
📦 Sprint Deliverables
- Collections practice (List, Dictionary, HashSet, Queue, Stack) with a short note on why each was chosen
- Generics practice (generic method, generic class, explanation of constraints)
- Job Application Tracker — Part 1 (Console)
- README
- Final commit tagged sprint-1
✅ Definition of Done
- [ ] Right collection chosen for each case
- [ ] Generic method and generic class work, and constraints are explained
- [ ] All four entities are stored in DataStore<T> with no raw lists left
- [ ] The jobs Dictionary stays consistent after every add, update, and remove
- [ ] A Pending or Rejected company cannot get new jobs
- [ ] Candidate email is unique (ignoring case), checked with a HashSet
- [ ] Invalid input never crashes the application
- [ ] Test scenarios 1–5 pass
- [ ] The Hand-off to Sprint 2 checklist is complete
🎓 Expected Outcome
By the end of Sprint 1, the learner can choose the right collection for each case, write clean reusable generic code, and has a working data foundation ready for Sprint 2.
📅 Sprint 2 — Delegates, Lambdas & Events
Day 15–28 · Project: Job Application Tracker — Part 2 of 2 (Console)
🎯 Sprint Goal
Learn to pass behavior through delegates and lambdas, notify subscribers using events, and use them to finish the Job Application Tracker started in Sprint 1.
Sprint rules: no LINQ, no database. You do not start a new project: you open your Sprint 1 repository and add to it. The Sprint 1 Minimum level is the entry gate.
📚 What You Will Learn
1. Delegates & Lambdas
Learn:
- Func<T, TResult>
- Action<T>
- Predicate<T>
- Comparison<T>
- Lambda expressions
- When to choose each delegate type
2. Events
Learn:
- Declaring, raising, and subscribing to events
- EventHandler<T>
- Lambda handlers vs named-method handlers
- Unsubscribing
- What happens when a subscriber throws
🛠️ Practical Implementation
Job Application Tracker — Part 2 (Console)
Extend the Sprint 1 project with:
- Find(Predicate<T>) and Sort(Comparison<T>) on DataStore<T>
- Search and sort for jobs (title keyword + minimum salary; sort by salary and title)
- Company approval (approve / reject / list by status)
- Applications: the Application entity, the ApplicationStatus enum, and the apply workflow
- Duplicate-application protection with HashSet<(int JobId, int CandidateId)>
- Three events with handlers: CompanyApproved, ApplicationSubmitted, ApplicationStatusChanged
📝 Sprint Tasks
Part 1 — Study & Practice (Day 15–23)
Day	Focus	Requirements
15–16	Delegates and lambda syntax; Func<T, TResult> and Action<T>	D1, D2
17	Predicate<T>	D3
18	Comparison<T> and sorting with a delegate	D4
19	Delegates review and the required note	D1–D4
20	Events: declare, raise, subscribe; EventHandler<T>	EV1
21	Handlers: lambda vs named method, and unsubscribing	EV1
22	Event practice and review (publisher, subscriber, a subscriber that throws)	EV1
23	Review all Sprint 2 topics, re-read Sprint 1 code, plan the changes	—


Part 2 — Continue the Project (Day 24–28)
Day	Focus	Requirements
24	Search and sort on DataStore<T>	M3
25	Company approval and list by status	E4
26	Applications menu and apply workflow	E6
27	Duplicate protection (Full), then the three events	M4, M2
28	Run all test scenarios, update the README, and tag sprint-2	—


Requirements: D1–D4, EV1 (practice) · M3, E4, E6, M2 (Minimum) · M4 (Full) · C1 (optional challenge, not graded) · Test scenarios: 1–7 (Minimum), 1–8 (Full)
📦 Sprint Deliverables
- Delegate practice (Func, Action, Predicate, Comparison) with a short note
- Event practice (EV1)
- Job Application Tracker — complete (Search, Sort, Approval, Applications, Events)
- README updated
- Final commit tagged sprint-2
✅ Definition of Done
- [ ] The four delegate types are used correctly, and the learner can say when to choose each
- [ ] DataStore<T> stays generic and gains Find and Sort (sorting returns a sorted copy)
- [ ] Company approval and list by status work
- [ ] A candidate can apply only to an open job of an Approved company
- [ ] A job with applications cannot be removed
- [ ] The three events are raised and handled with clear notifications
- [ ] Duplicate applications are rejected (Full level)
- [ ] Sprint 1 behavior still works
- [ ] Test scenarios 1–7 pass (1–8 for Full)
🎓 Expected Outcome
By the end of Sprint 2, the learner can pass behavior through delegates and lambdas, build event-driven notifications, extend an existing codebase without breaking it, and enforce business rules in workflows.
📅 Sprint 3 — LINQ
Day 29–42 · Project: Sales Data Analytics Dashboard (Console)
🎯 Sprint Goal
Learn to query and shape data in memory with LINQ, understand how queries execute, and know the common pitfalls.
Working rule: every topic day ends with 5–8 practice problems and adds 1–2 queries to the sprint project. The dashboard grows during the sprint instead of being squeezed into the last days.
📚 What You Will Learn
1. Execution Concepts
Learn:
- Deferred vs immediate execution
- Method syntax vs query syntax
- IEnumerable vs IQueryable (concept)
- Multiple enumeration
- Captured variables in closures
2. Filtering & Quantifiers
Learn:
- Where
- OfType
- Any
- All
- Contains
3. Projection & Ordering
Learn:
- Select
- SelectMany
- Anonymous types
- Projecting into records / DTOs
- OrderBy
- ThenBy
4. Element & Conversion Operators
Learn:
- First
- FirstOrDefault
- Single
- SingleOrDefault
- DefaultIfEmpty
- ToList
- ToArray
- ToDictionary
- ToHashSet
- ToLookup
5. Paging / Partitioning
Learn:
- Skip
- Take
6. Grouping
Learn:
- GroupBy
- Composite keys
- Per-group aggregates
- ToLookup
7. Joining
Learn:
- Join
- GroupJoin
- Left join (GroupJoin + DefaultIfEmpty)
8. Aggregation
Learn:
- Sum
- Count
- Average
- Min
- Max
- MinBy
- MaxBy
- Aggregate
9. Set Operations & Combining
Learn:
- Distinct
- DistinctBy
- Union
- Intersect
- Except
- Concat
10. Custom LINQ
Learn:
- Extension methods on IEnumerable<T>
- yield return
- Func vs Expression<Func> (the bridge to EF Core)
11. Pitfalls & Performance
Learn:
- Any vs Count
- Avoiding multiple enumeration
- When to materialize a query
- LINQ vs foreach
🛠️ Practical Implementation
Sales Data Analytics Dashboard (Console)
Build a console dashboard that answers business questions about sales data using LINQ:
- Filtering and searching sales
- Grouping and aggregating results
- Joining related data sets
- Ordering and paging results
📝 Sprint Tasks
Task 1 — Execution Concepts
Day 29. Demonstrate deferred vs immediate execution, multiple enumeration, and closures.
Task 2 — Filtering & Quantifiers
Day 30. Practice Where, OfType, Any, All, and Contains.
Task 3 — Projection & Ordering
Day 31–32. Practice Select, SelectMany, projection into records/DTOs, and ordering.
Task 4 — Element, Conversion & Paging
Day 33. Practice element operators, conversion operators, Skip, and Take.
Task 5 — Grouping
Day 34–35. Practice GroupBy with composite keys, per-group aggregates, and ToLookup.
Task 6 — Joining
Day 36–37. Practice Join, GroupJoin, and left join.
Task 7 — Aggregation & Set Operations
Day 38. Practice Sum, Count, Average, Min, Max, MinBy, MaxBy, Aggregate, Distinct, DistinctBy, Union, Intersect, Except, and Concat.
Task 8 — Custom LINQ & Pitfalls
Day 39. Write extension methods on IEnumerable<T>, use yield return, compare Func with Expression<Func>, and review common LINQ pitfalls and performance decisions.
Task 9 — Sprint Project
Day 40–41. Finish and polish the Sales Data Analytics Dashboard.
Task 10 — README
Day 42. Document the project: overview, features, how to run, the reports and their LINQ operators, sample output, design decisions, and known limitations.
📦 Sprint Deliverables
- Practice problems for every topic
- Custom LINQ extension methods
- Sales Data Analytics Dashboard (Console)
- README
✅ Definition of Done
- [ ] Deferred vs immediate execution is understood
- [ ] Filtering, projection, and ordering queries work
- [ ] Grouping and joining queries work
- [ ] Aggregation and set operations work
- [ ] Custom LINQ extension methods work
- [ ] Common pitfalls are understood
- [ ] Sales Data Analytics Dashboard works
🎓 Expected Outcome
By the end of Sprint 3, the learner can express almost any in-memory data question as a clear LINQ query and understands how that query executes.
Sprint 4 — Database Design, SQL & Entity Framework Core
Days: 43–56 · 14 days
Project: Library Management Database → Library Management System (EF Core)
Goal
Learners design and query a relational database before accessing it through C#. Then they implement the same domain using EF Core. This makes the connection between tables, C# entities, LINQ queries, and generated SQL clear.
Part A — Database Design & SQL
Design a database for Authors, Books, and Members.
Learn and practice:
- Tables, rows, columns, data types, and NULL
- Primary keys, foreign keys, one-to-many relationships, and referential integrity
- Required/unique constraints and basic normalization
- SELECT, WHERE, ORDER BY, INSERT, UPDATE, and DELETE
- INNER JOIN, LEFT JOIN, GROUP BY, COUNT, and SUM
- Basic indexes and why constraints matter
Database deliverable: Save the schema, seed data, and tested SQL queries in a /Database folder.
Part B — EF Core Implementation
Build the same Library Management domain with EF Core Code First.
Learn and practice:
- Entities, DbContext, connection strings, and dependency injection
- Migrations: create, apply, and inspect
- CRUD and SaveChangesAsync
- Relationships and Fluent API configuration
- Required fields, maximum lengths, precision, and unique indexes
- LINQ to Entities and DTO projection
- Include / ThenInclude where needed
- The N+1 problem and how to identify and improve it
- Tracking vs. AsNoTracking
- Async database operations and basic transaction concepts
- Inspecting generated SQL through logging or ToQueryString()
Important: The SQL schema and EF Core model must represent the same domain. Do not maintain two competing designs.
Day-by-day plan
Day	Focus	Expected outcome
43	Relational database basics	Identify entities, data types, keys, and relationships; sketch the schema
44	SQL CRUD & constraints	Create tables, constraints, and seed data; practice CRUD queries
45	Joins & aggregation	Write joins and grouped queries; explain basic normalization and indexes
46	Database project review	Finish /Database schema and queries; verify relationships and constraints
47	EF Core setup	Create entities and DbContext; configure connection string and Code First
48	Migrations & seeding	Apply migrations, seed data, and verify the generated schema
49	CRUD & SaveChangesAsync	Implement CRUD and explain when changes reach the database
50	Relationships & Fluent API	Configure Author–Book relationships, required fields, max lengths, and unique ISBN
51	LINQ to Entities & projection	Write filtered/projected queries and inspect generated SQL
52	Loading & N+1	Use Include / ThenInclude where needed; reproduce and improve an N+1-style query
53	Tracking & async queries	Compare tracking and AsNoTracking; use async query methods
54	Integration & reliability	Recheck constraints, relationships, error cases, and basic transaction concepts
55	Finish the project	Complete the system and resolve review findings
56	README & handoff	Document schema, queries, setup, migrations, features, design decisions, and limitations


Sprint 4 deliverables
- /Database folder with SQL schema, seed data, and tested SQL queries
- EF Core entities, DbContext, configuration, and migrations
- Working CRUD operations through EF Core
- Relationships and constraints, including unique ISBN enforcement
- LINQ queries with projection and appropriate loading
- Notes/evidence showing generated SQL for selected queries
- Library Management Database + EF Core System README
Definition of Done
- SQL schema has correct primary keys, foreign keys, required fields, and a unique ISBN constraint.
- CRUD, joins, filtering, and aggregation SQL queries return expected results.
- EF Core configuration and migrations work from a clean setup.
- CRUD works through EF Core.
- The learner can explain how the relational schema maps to the EF Core model.
- The learner understands LINQ-to-SQL translation, AsNoTracking, and the N+1 problem.
- README includes setup steps, sample queries, design decisions, and known limitations.
Scope control
Owned entities, explicit/lazy loading, split queries, raw SQL and bulk operations, concurrency/RowVersion, global query filters, and soft delete are extension topics, not required Sprint 4 outcomes. Prioritize understanding the core database and EF Core workflow over covering every advanced topic.
Library Management API — Shared Domain (Sprints 5–6)
Sprints 5 and 6 extend the Library Management domain introduced in Sprint 4. The database and EF Core work are reused rather than discarded.
Domain
- Author: name and bio; one author can have many books
- Book: title, ISBN, published year, availability controlled by the server, and author
- Member: full name, unique email, optional phone, JoinedAt set by the server
- Loan (Sprint 6): book, member, loan date, due date (loan date + 14 days), and optional return date
An active loan has no return date.
Architecture
Create a three-project solution at the start of Sprint 5:
LibraryApi.API       → Controllers, middleware, Program.cs
       ↓
LibraryApi.Business  → Services, DTOs, custom exceptions
       ↓
LibraryApi.Data      → Entities, DbContext, migrations, repositories, Unit of Work
Dependency direction: API → Business → Data
Rules:
- Controllers stay thin and call one service.
- Services own business rules and return DTOs, not entities.
- Only services use repositories / Unit of Work.
- The Business layer does not import EF Core; database access goes through repository methods.
- Learners implement Generic Repository in Sprint 5 and Unit of Work in Sprint 6.
Required endpoints
Sprint	Endpoint	Purpose
5	GET /api/authors	List authors with booksCount
5	POST /api/authors	Create an author
5	DELETE /api/authors/{id}	Delete an author only if they have no books
5	GET /api/books	List books with author name
5	POST /api/books	Create a book
5	PUT /api/books/{id}	Update a book; client cannot change availability
5	GET /api/members	List members
5	GET /api/members/{id}	Get one member
5	POST /api/members	Create a member
6	POST /api/loans	Borrow a book
6	PUT /api/loans/{id}/return	Return a loan
6	GET /api/members/{id}/loans	List a member’s loans


Other CRUD endpoints are intentionally out of scope unless the instructor requests them.
HTTP status and business rules
- Invalid DTO or missing body reference (e.g. unknown AuthorId) → 400
- ID in the URL does not exist → 404
- Conflict with current data/state (duplicate ISBN/email, deleting an author with books, borrowing an unavailable book, returning a loan twice) → 409
- Unexpected errors → 500 with a generic message and no stack trace
- Trim text input; store emails trimmed and lowercase.
- Server controls IsAvailable and JoinedAt.
- A member can have at most 3 active loans.
- Borrowing requires an existing member and an existing, available book.
- DueDate = LoanDate + 14 days.
- Borrow and return each save changes once; a loan cannot be returned twice.
Sprint 5 — Web API Foundations, Services & Business Logic
Days: 57–71 · 15 days
Project: Library Management API — Part 1
Goal
Build the foundation of a layered Web API with three projects, EF Core Code First, Generic Repository, DTOs, validation, consistent exception handling, and service-owned business rules.
Topics
- Solution/project references, N-Tier architecture, and Swagger / OpenAPI
- Entities, relationships, Fluent API, connection strings, and dependency injection
- EF Core migrations when DbContext lives in a different project
- Generic Repository and generic constraints
- Controllers, DTOs, validation, and over-posting
- HTTP methods/status codes: 200, 201, 204, 400, 404, 409, 500
- CreatedAtAction, PUT responses, middleware, and consistent JSON errors
- Custom exceptions and business rules in services
- Email normalization and uniqueness checks
Implementation approach
Build the API in vertical slices to avoid overwhelming learners:
1. Complete the Author flow end-to-end: DTOs → service → repository access → controller → error behavior.
2. Add the Book flow.
3. Add the Member flow.
4. Verify success cases, failure cases, status codes, and generated SQL before moving on.
Suggested day-by-day focus
- Day 57: Create solution, three projects, references, packages, and Swagger
- Day 58: Add BaseEntity, Author, Book, Member, and AppDbContext
- Day 59: Configure connection string, dependency injection, and first migration
- Day 60: Implement Generic Repository
- Day 61: Build Author DTOs/service and GET/POST endpoints
- Day 62: Add custom exceptions and global exception middleware
- Day 63: Add author deletion rule and 404/409 handling
- Day 64: Build Book reads and creation
- Day 65: Add Book update and protect server-controlled availability
- Day 66: Build Member endpoints and email normalization
- Day 67: Verify validation and HTTP status behavior
- Day 68: Test endpoint success and failure cases
- Day 69: Review generated SQL and query efficiency
- Day 70: Fix issues and review architecture boundaries
- Day 71: Finish short README, endpoint checklist, and Sprint 5 handoff
Deliverables
- Three-project API solution and Generic Repository
- Author, Book, and Member services, DTOs, and controllers
- Exception middleware and consistent error responses
- 9 required endpoints
- Endpoint verification checklist with expected and observed status codes
- README with setup instructions and known limitations
Sprint 6 — Unit of Work, Loans & Final Mock Interview
Days: 72–86 · 15 days
Project: Continue the same Library Management API — Part 2
Goal
Improve the existing solution with Unit of Work, add the Loan feature, verify cross-entity business rules, and finish with a project defense and technical mock interview.
Topics
- Why saving inside each repository can cause problems
- Unit of Work on top of Generic Repository
- One save per operation that changes multiple records
- DbContext lifetime and dependency injection
- Loan entity, relationships, delete behavior, and service-owned business rules
- Edge-case tests, regression checks, safe refactoring, and architecture review
Suggested day-by-day focus
- Day 72: Establish baseline; confirm Sprint 5 tests still pass
- Day 73: Add IUnitOfWork / UnitOfWork; repositories track changes without saving
- Day 74: Refactor services to Unit of Work and rerun Sprint 5 tests
- Day 75: Add Loan entity, relationships, migration, and DTOs
- Day 76: Implement member loan history
- Day 77: Implement borrowing rules and one save
- Day 78: Implement return flow and prevent duplicate returns
- Day 79: Add the three Loan endpoints
- Day 80: Test successful borrow/return and important failure cases
- Day 81: Review Unit of Work and one-save behavior
- Day 82: Fix errors and verify responses
- Days 83–84: Optional concurrency extension; if skipped, use as buffer for required work
- Day 85: Architecture and code review; remove unused methods and verify layer boundaries
- Day 86: Final Project Defense & Technical Mock Interview
Final Mock Interview — Day 86
The learner presents the database and API, explains the request flow and architecture, justifies important decisions, answers technical follow-up questions, and identifies what they would improve next.
Interview areas:
- C# collections, generics, delegates, lambdas, and events
- LINQ execution, projection, and query behavior
- Database design, relationships, constraints, and SQL
- EF Core, migrations, tracking, generated SQL, and N+1
- Web API, HTTP status codes, validation, and exception handling
- Dependency injection and architecture boundaries
- Generic Repository, Unit of Work, and business rules
- Debugging, edge cases, trade-offs, and project-specific decisions
The interview evaluates understanding and reasoning, not memorized answers. Each learner receives structured feedback: strengths, gaps, and a next-step action plan.
Deliverables
- Unit of Work and refactored services
- Loan entity, migration, service, and controller
- 3 additional endpoints (12 total across Sprints 5–6)
- Regression and edge-case verification
- Updated README, final project presentation/defense, mock interview, and individual feedback
🏁 Level 1 Completion Criteria
A learner completes Level 1 when:
- All six sprints are completed.
- The Library Management Database and EF Core implementation work.
- The Library Management API works with the 12 required endpoints.
- Core business rules and expected error responses are verified.
- The learner can explain the database, request flow, architecture, and important trade-offs.
- The learner completes the final project defense / mock interview.
🧠 Engineering Practices Throughout the Level
Debugging
Use Symptom → Observation → Hypothesis → Experiment → Root Cause → Fix. Avoid changing random code until it works.
Generated SQL
From Sprint 4 onward, inspect SQL generated by EF Core for selected queries and explain the relationship between the LINQ expression and SQL.
Code review
Review naming, responsibility boundaries, duplication, error handling, status codes, and query efficiency.
Git discipline
- Commit after each finished requirement; include the requirement ID for Sprints 1–4.
- Commit before each refactor step.
- Use a branch for Sprint 6.
- Push each project to GitHub with a README.
🚫 Explicitly Out of Scope
These topics are not required for Level 1:
- Authentication and authorization (JWT, Identity, roles); optional bonus only
- AutoMapper and unit-testing frameworks such as xUnit/Moq; optional bonus ideas only
- File upload and reporting endpoints
- Clean Architecture, CQRS, and MediatR
- Caching and background processing
- Docker, CI/CD, and deployment
- Microservices and front-end development
These topics belong to later levels.
🏆 Final Principle
One Level. Six Sprints. One Final Project. Real Business Rules. No Technology for Technology’s Sake.
