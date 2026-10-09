# Sprint 1 — Advanced C#

> Project: **Job Application Tracker (Console)**
> Duration: **2 weeks (14 days) · Day 1–14**

## Sprint Goal

Build a strong C# foundation: choosing the right collection, writing reusable generic code, and using delegates and events.

## How to use this sprint plan

For every task:

1. Read the **MUST** resources before implementation.
2. Read **DEEP DIVE** only when the concept is unclear or the learner wants deeper understanding.
3. Implement the requested change.
4. Verify the behavior manually.
5. Commit the work in small, meaningful commits.
6. Be able to explain the decision during review.

**Important:** This document does not change the original Sprint scope. It expands the original tasks into an executable checklist.

## Project Brief — Job Application Tracker

A job portal in the console. Companies are approved by an admin, recruiters post jobs for their company, and candidates apply. The project practises collections, generics, delegates, events, and lambdas.

Suggested folders: `Models/` `Services/` `Data/` `Events/` `UI/` `Program.cs`

### Data Model

**Company**

| Field | Type / Rule |
| --- | --- |
| Id | int, unique |
| Name | string, required |
| Industry | string |
| City | string |
| Status | enum { Pending, Approved, Rejected }, starts as Pending |

**Recruiter**

| Field | Type / Rule |
| --- | --- |
| Id | int |
| Name | string, required |
| Email | string, valid format |
| CompanyId | int, the recruiter's company |

**Job**

| Field | Type / Rule |
| --- | --- |
| Id | int |
| Title | string, required |
| Description | string |
| Salary | decimal, above 0 |
| Location | string |
| IsOpen | bool, default true |
| PostedDate | DateTime |
| CompanyId, RecruiterId | int, int |

**Candidate**

| Field | Type / Rule |
| --- | --- |
| Id | int |
| FullName | string, required |
| Email | string, valid format, unique |
| Phone | string |
| YearsOfExperience | int, 0 or more |

**Application**

| Field | Type / Rule |
| --- | --- |
| Id | int |
| JobId, CandidateId | int, int |
| AppliedDate | DateTime |
| Status | enum { Submitted, UnderReview, Interview, Accepted, Rejected } |

### Business Rules

- A candidate can apply only to a job that is open and belongs to a company with status Approved.
- A candidate cannot apply to the same job twice.
- A recruiter can create, edit, close, and remove jobs of their own company only.
- A company that is Pending or Rejected cannot post new jobs.

### Project Requirements

**Easy** — lists, menus, and safe input (everyone must finish it)

- **E1** Store the 5 entities in `List<T>` (one list per entity). Load sample data at start: 3 companies, 3 recruiters, 6 jobs, 5 candidates.
- **E2** Main menu with a sub-menu per entity: Add / Update / List / Remove. The list view prints a clean, aligned table.
- **E3** Company approval: change a company from Pending to Approved or Rejected. List companies by status.
- **E4** Apply to a job: pick a candidate and a job. Refuse with a clear message if the job is closed or the company is not Approved.
- **E5** Keep a `Dictionary<int, Job>` next to the job list and use it to find a job by Id.

**Medium** — generics, events, lambdas

- **M1** Create a generic `DataStore<T>` (Add, Update, Remove, GetById, GetAll) with a constraint like `where T : class, IEntity`. Replace the 5 raw lists with it.
- **M2** Declare 3 events: `CompanyApproved`, `ApplicationSubmitted`, `ApplicationStatusChanged`. Subscribe handlers that print a notification.
- **M3** Search jobs with a lambda + `Predicate<Job>`: title keyword, minimum salary, company, open / closed. Combine at least two filters.
- **M4** Sort jobs with a lambda + `Comparison<Job>`: salary, posted date, title.
- **M5** A recruiter picks their name from a list and manages only their own company's jobs.

**Hard** — duplicates, undo, audit log

- **H1** Prevent duplicate applications with a `HashSet` of `(JobId, CandidateId)`. Keep it in sync on add and remove.
- **H2** Undo for status changes with a `Stack`: push the old state on every change; Undo pops it, restores the status, and fires the event again.
- **H3** Audit Log via events: one logger subscribed to all 3 events records time, event, and details. Keep it in memory and show it in a menu option.

### Test Scenarios the Reviewer Will Try

| # | Scenario | Expected result |
| --- | --- | --- |
| 1 | Type `abc` where a number is expected | Friendly message, the menu shows again |
| 2 | Apply to a job of a Pending company | Clear message: the company is not approved |
| 3 | Apply to a closed job | Clear message: the job is closed |
| 4 | Same candidate applies twice to one job | Clear message: already applied |
| 5 | Change a status, then Undo | Old status is back; the log has both entries |

> **Sprint-specific rule:** no LINQ in this sprint.


## Working Rules

- Work in order: finish **Easy** before **Medium**, and **Medium** before **Hard**. Commit after every finished requirement.
- Every requirement has an ID (E1, M3, H2). Use it in your commit message, for example `M3: add Predicate search`.
- Use only what was taught up to this sprint.
- The app must never crash on bad input. A wrong menu choice, empty text, or a negative number gets a friendly message, then the app continues.
- Push to GitHub with a README (see the README task).

## Sprint Tasks

### Task 1 — Collections

*Day 1–2.*

Practice List, Dictionary, HashSet, Queue, and Stack, and choose the right one for each case.

Then start the project: create the 5 entities, store them in lists, load the sample data, build the menus, and use a Dictionary to find jobs by Id.

**Apply to the project:** **E1**, **E2**, **E5** (see Project Requirements above)

**Required output**
- Practice project with at least one example per collection
- Short note explaining why each collection was chosen
- Project skeleton with models, sample data, and menus

### Resources
- **MUST** — [Collections and data structures](https://learn.microsoft.com/en-us/dotnet/standard/collections/)
- **MUST** — [Selecting a collection class](https://learn.microsoft.com/en-us/dotnet/standard/collections/selecting-a-collection-class)
- **DEEP DIVE** — Jon Skeet — C# in Depth — `Book: C# in Depth`

---

### Task 2 — Generics

*Day 3–4.*

Write generic classes and generic methods with constraints.

Then build the generic `DataStore<T>` and replace the raw lists. Add the company approval flow.

**Apply to the project:** **M1**, **E3** (see Project Requirements above)

**Required output**
- A generic class and a generic method
- At least one constraint (`where T : class, IEntity`)
- `DataStore<T>` used for all 5 entities

### Resources
- **MUST** — [Generics in C#](https://learn.microsoft.com/en-us/dotnet/csharp/fundamentals/types/generics)
- **MUST** — [Constraints on type parameters](https://learn.microsoft.com/en-us/dotnet/csharp/programming-guide/generics/constraints-on-type-parameters)

---

### Task 3 — Delegates & Lambdas

*Day 5–6.*

Use Func, Action, Predicate, lambda expressions, and anonymous methods.

Then add job search and sorting, and the apply flow.

**Apply to the project:** **M3**, **M4**, **E4** (see Project Requirements above)

**Required output**
- One example for each: Func, Action, Predicate, Comparison
- Job search with at least two combined filters
- Job sorting by salary, posted date, and title
- Apply flow refuses closed jobs and non-approved companies

### Resources
- **MUST** — [Delegates](https://learn.microsoft.com/en-us/dotnet/csharp/programming-guide/delegates/)
- **MUST** — [Lambda expressions](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/operators/lambda-expressions)

---

### Task 4 — Events

*Day 7.*

Create events and event handlers.

Then add the 3 project events and their handlers.

**Apply to the project:** **M2** (see Project Requirements above)

**Required output**
- `CompanyApproved`, `ApplicationSubmitted`, `ApplicationStatusChanged` events
- Handlers that print a notification

### Resources
- **MUST** — [Events](https://learn.microsoft.com/en-us/dotnet/csharp/programming-guide/events/)

---

### Task 5 — Sprint Project — Finish the Hard Requirements

*Day 8–13.*

Finish the recruiter flow and all Hard requirements, then polish.

**Apply to the project:** **M5**, **H1**, **H2**, **H3** (see Project Requirements above)

**Required output**
- Recruiter manages only their own company's jobs
- Duplicate-application protection with a `HashSet`
- Undo for status changes with a `Stack`
- Audit log shown in the menu
- All 5 test scenarios pass

---

### Task 6 — README

*Day 14.*

Document:
- Project overview
- Features (every requirement ID marked done or skipped)
- How to run
- Usage and sample output
- Project structure
- Design decisions (collections, generics, delegates, events)
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

- [ ] Job Application Tracker (Console) with all Easy, Medium, and Hard requirements (or clearly marked as skipped)
- [ ] Practice code for every topic (collections, generics, delegates, events)
- [ ] README
- [ ] Commit history that follows the requirement IDs

## Definition of Done

- [ ] All Easy requirements (E1–E5) are done
- [ ] All Medium requirements (M1–M5) are done
- [ ] All Hard requirements (H1–H3) are done, or clearly marked as skipped
- [ ] All 5 test scenarios pass
- [ ] The app never crashes on bad input
- [ ] README is complete and commits follow the requirement IDs

## Before You Submit

- [ ] The solution builds with no errors, and seed data loads on first run
- [ ] Every Easy / Medium / Hard requirement is done, or clearly marked as skipped
- [ ] No secrets, passwords, or real connection strings in the repo
- [ ] The README is complete and the commit history follows the requirement IDs

## Review Rule

The learner should be able to explain not only **what** was implemented, but **why it belongs there, what alternatives existed, and what trade-off was accepted**.
