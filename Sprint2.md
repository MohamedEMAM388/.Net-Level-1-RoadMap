<div align="center">

# Sprint 2 — Delegates, Lambdas & Events

### Job Application Tracker (Console) · Part 2 of 2 · Workflows & Notifications

![Language](https://img.shields.io/badge/language-C%23-239120?style=flat-square)
![Platform](https://img.shields.io/badge/platform-.NET%20Console-512BD4?style=flat-square)
![Duration](https://img.shields.io/badge/duration-14%20days-blue?style=flat-square)
![LINQ](https://img.shields.io/badge/LINQ-not%20allowed-red?style=flat-square)

**Delegates & Lambdas · Events**

</div>

---

## Table of Contents

- [Overview](#overview)
- [Sprint Structure](#sprint-structure)
- [What You Reuse from Sprint 1](#what-you-reuse-from-sprint-1)
- [Data Model Changes](#data-model-changes)
- [Business Rules](#business-rules)
- [Requirements](#requirements)
- [Project Details](#project-details)
- [Test Scenarios](#test-scenarios)
- [Working Rules](#working-rules)
- [Acceptance Criteria](#acceptance-criteria)

---

## Overview

**Goal:** learn to pass behavior through delegates and lambdas, and to notify subscribers using events, then use them to finish the Job Application Tracker you started in [Sprint 1](Sprint1.md).

The project is built across **two sprints**:

| Sprint | Topics | What the project gains |
| :--- | :--- | :--- |
| **[Sprint 1](Sprint1.md)** | Collections, Generics | Companies, recruiters, jobs, and candidates: storage, menus, and management |
| **Sprint 2** *(this one)* | Delegates, Lambdas, Events | Company approval, applications, search and sorting, and notifications |

You do not start a new project. You open your Sprint 1 repository and add to it.

### Prerequisites

- Sprint 1 **Minimum** is complete: CL1–CL4, G1, G2, E1, E2, M1, E3, E5, with Sprint 1 test scenarios 1–5 passing.
- Your project is in the state described in the Sprint 1 hand-off checklist.

> [!IMPORTANT]
> If something from the Sprint 1 Minimum is unfinished, finish it **first**. Sprint 2 depends on it.

There is still no database. All data stays in memory.

### Learning outcomes

By the end of the sprint, a student can:

1. Use `Func`, `Action`, `Predicate`, and `Comparison` in practical code.
2. Declare, raise, and subscribe to events.
3. Extend an existing codebase without breaking what already works.
4. Build workflows that enforce business rules.
5. Test business rules and justify design decisions.

### Completion levels

| Level | Requirements | Test scenarios |
| :--- | :--- | :---: |
| **Minimum** | D1–D4, EV1, M3, E4, E6, M2 | 1–7 |
| **Full** | Minimum + M4 | 1–8 |
| **Optional challenge** (not graded) | C1 | — |

> [!NOTE]
> Every core concept of this sprint (delegates and lambdas, events) is covered by the **Minimum** level. M4 and C1 extend the work; they do not decide whether a student met the concepts.

---

## Sprint Structure

The sprint is **14 days**: **9 days of study and practice, then 5 days on the project.** Finish Part 1 before starting Part 2.

| Part | Days | What you do | Requirements |
| :--- | :---: | :--- | :--- |
| **Part 1 — Study & Practice** | 1–9 | Study delegates and lambdas, then events. Practice each in small exercises of your own | D1–D4, EV1 |
| **Part 2 — Continue the Project** | 10–14 | Add search, company approval, applications, and notifications to your Sprint 1 project | M3, E4, E6, M4, M2 |

### Suggested day-by-day plan

The 9 + 5 split is the rule. The plan inside each part is a guide for pacing.

| Day | Focus | Requirements |
| :---: | :--- | :--- |
| 1–2 | Delegates and lambda syntax; `Func<T, TResult>` and `Action<T>` | D1, D2 |
| 3 | `Predicate<T>` | D3 |
| 4 | `Comparison<T>` and sorting with a delegate | D4 |
| 5 | Delegates review: when to choose each, and write the required note | D1–D4 output |
| 6 | Events: declare, raise, subscribe; `EventHandler<T>` | EV1 |
| 7 | Handlers: lambda versus named method, and unsubscribing | EV1 |
| 8 | Event practice and review: publisher and subscriber, and a subscriber that throws | EV1 |
| 9 | Review all Sprint 2 topics, re-read your Sprint 1 code, and plan where the changes go | — |
| 10 | Search and sort on `DataStore<T>` | M3 |
| 11 | Company approval and list by status | E4 |
| 12 | Applications menu and apply workflow | E6 |
| 13 | Duplicate protection (Full), then the three events | M4, M2 |
| 14 | Run all test scenarios, update the README, and tag `sprint-2` | — |

---

## What You Reuse from Sprint 1

| From Sprint 1 | What changes in Sprint 2 |
| :--- | :--- |
| `DataStore<T>` (M1) | Gains `Find(Predicate<T>)` and `Sort(Comparison<T>)`, and also stores applications |
| Jobs menu (E2, E3) | Gains **Search** and **Sort** options |
| Companies menu (E3) | Gains **Approve or reject** and **List by status** |
| Main menu (E2) | Gains an **Applications** menu |
| Jobs Dictionary (E5) | Used by the Apply workflow to find a job quickly |
| Remove Job (E3) | Now refuses a job that already has applications |
| Seed data (E1) | **Unchanged**. Applications start empty |
| Sprint 1 scenarios | Must **still pass** (Scenario 1 here repeats them) |

---

## Data Model Changes

Sprint 2 adds one entity and one enum.

| Entity | Properties | Rules |
| :--- | :--- | :--- |
| **Application** | `Id`, `JobId`, `CandidateId`, `AppliedDate`, `Status` | Starts as Submitted; implements `IEntity` |

**Enum**

- `ApplicationStatus`: Submitted, UnderReview, Interview, Accepted, Rejected

---

## Business Rules

These rules are new in Sprint 2. The Sprint 1 rules still apply.

| # | Rule | Enforced in | Tested by |
| :---: | :--- | :---: | :---: |
| 1 | A candidate can apply only to an **open** job | E6 | Scenario 2 |
| 2 | The job must belong to an **Approved** company | E6 | Scenario 2 |
| 3 | A job that already has applications **cannot be removed** (close it instead) | E6 | Scenario 5 |
| 4 | A candidate cannot apply to the **same job twice** (Full) | M4 | Scenario 8 |

---

## Requirements

> [!IMPORTANT]
> Work in order. Finish and commit each requirement before moving to the next, and put its ID in the commit message, for example `M2: add CompanyApproved event`.

## Part 1 — Study & Practice

Study the topic first, then write the exercise on your own. Each exercise is separate from the project and should be tiny. One small console project per phase is enough.

### Phase A — Delegates and Lambdas

*Minimum · practice.* Write one tiny independent example of each delegate type before using them in the project.

| ID | Requirement | Example |
| :---: | :--- | :--- |
| **D1** | `Func<T, TResult>` | Calculate a value from an input |
| **D2** | `Action<T>` | Print an entity |
| **D3** | `Predicate<T>` | Check whether a job meets a condition |
| **D4** | `Comparison<T>` | Compare two jobs for sorting |

**Required output:** the four examples, and a short note on what each delegate type takes and returns, and when to choose each.

### Phase B — Events Practice

*Minimum · practice.*

| ID | Exercise | What it tests |
| :---: | :--- | :--- |
| **EV1** | A tiny practice event in a domain of your own, like the `Counter` example in [Project Details](#event-example): declare it, raise it, and subscribe to it with a lambda and with a named method | Declaring, raising, and subscribing |

---

## Part 2 — Continue the Project

Now open your Sprint 1 project and use what you studied. Existing behavior must keep working.

### Phase C — Delegates in the Project

*Minimum.* This phase connects delegates to the generic store you built in Sprint 1.

| ID | Requirement | Acceptance criteria |
| :---: | :--- | :--- |
| **M3** | Search and sort jobs | Add `Find(Predicate<T>)` and `Sort(Comparison<T>)` to `DataStore<T>`. Use a `Predicate<Job>` to search by title keyword **and** minimum salary together, and a `Comparison<Job>` to sort by salary and by title |
| **E4** | Company approval | Approve or reject a Pending company. List companies by status, using `Find` with a lambda |

`DataStore<T>` stays generic: it knows nothing about jobs or companies. The specific condition and comparison are lambdas passed in from the menus. Sorting must not change the order of the items stored in `DataStore<T>`; it returns a sorted copy.

### Phase D — Applications

*Minimum, except M4.*

| ID | Requirement | Acceptance criteria |
| :---: | :--- | :--- |
| **E6** | Applications and apply workflow | Add the `Application` entity and `ApplicationStatus` enum, stored in a `DataStore<Application>`. Add an Applications menu with **Apply**, **GetAll**, and **Change status**. Apply checks that the job is open and its company is Approved, then creates the application, or refuses with a clear message. **Remove Job** now refuses a job that already has applications |
| **M4** | Duplicate protection *(Full)* | Use `HashSet<(int JobId, int CandidateId)>`. Add the pair when an application is created, and reject a repeated one with a clear message |

For M4, students must explain why the pair identifies one candidate's application to one job, and why the HashSet check is useful.

### Phase E — Events in the Project

*Minimum.*

| ID | Requirement | Acceptance criteria |
| :---: | :--- | :--- |
| **M2** | Three events with handlers | Add `CompanyApproved` (a company becomes Approved), `ApplicationSubmitted` (a valid application is created), and `ApplicationStatusChanged` (a status changes). Each handler prints a clear notification |

Students must explain the publisher and subscriber, the difference between declaring and raising an event, why an event separates the main action from its notification, and what happens if a subscriber throws an exception.

### Optional challenge

| ID | Requirement |
| :---: | :--- |
| **C1** | *Not graded.* Undo application-status changes with a `Stack`: save the previous status before each change; Undo restores the most recent one. For students who finish early; it must not delay the Minimum. |

---

## Project Details

### Sample session

The exact wording is yours; the flow is what matters.

```text
===== Job Application Tracker =====
1. Companies     2. Jobs     3. Candidates
4. Applications  5. Recruiters (GetAll)     0. Exit
Choose: 2

--- Jobs ---
1. Add  2. GetAll  3. Update  4. Remove  5. Search  6. Sort  0. Back
Choose: 5
Title keyword (empty to skip): developer
Minimum salary (0 to skip): 15000

Id | Title             | Salary | Open
6  | Backend Developer | 18000  | No
```

```text
--- Applications ---
1. Apply  2. GetAll  3. Change status  0. Back
Choose: 1
Candidate Id: 2
Job Id: 5
Cannot apply: this job's company is not approved.
```

### Seed data

Sprint 2 uses the **same seed data as Sprint 1**, unchanged, and the application list starts empty. For reference, the data used by the scenarios:

**Jobs**

| Id | Title | Salary | Open | CompanyId |
| :---: | :--- | :---: | :---: | :---: |
| 1 | Junior .NET Developer | 12000 | Yes | 1 |
| 2 | QA Engineer | 10000 | Yes | 1 |
| 3 | Data Analyst | 14000 | Yes | 2 |
| 4 | Operations Assistant | 8000 | Yes | 2 |
| 5 | Research Assistant | 9000 | Yes | **3 (Pending)** |
| 6 | Backend Developer | 18000 | **No** | 1 |

### Behavior decisions

| Topic | Decision |
| :--- | :--- |
| **Company approval** | Only a Pending company can be approved or rejected. An Approved or Rejected company keeps its status |
| **Apply** | In the Applications menu. Show the candidates and jobs, then the user types both Ids. Find the job through the Dictionary. The application starts as Submitted with today's date |
| **Change status** | Pick an application Id, then any status from the enum |
| **Remove Job** | Refused when the job has applications. The user can close the job instead through Update |
| **Search** | In the Jobs menu. Asks for a title keyword (empty means skip; ignore case) and a minimum salary (0 means skip; a job matches when its salary is **equal to or above** the minimum). Both conditions apply together. Open and closed jobs are both searched |
| **Sort** | In the Jobs menu. Asks for salary or title and prints ascending |
| **No results** | Search with no match prints a clear message, not an empty table |
| **Events** | Subscribe all handlers once at start in `Program.cs`. Each handler prints one line. `CompanyApproved` fires only when a company becomes Approved, not when it is rejected |
| **Where events are raised** | In the service method that performs the action, **after** the action succeeds. A refused action raises nothing |
| **Existing behavior** | Every Sprint 1 rule, message, and menu keeps working as before |

### Event example

A tiny example in a different domain, showing the three steps: declare, raise, subscribe. You may use `EventHandler<T>` or your own delegate type.

```csharp
public class Counter
{
    public event EventHandler<int>? LimitReached;      // 1. declare

    public void Increment(int value)
    {
        if (value >= 10)
            LimitReached?.Invoke(this, value);         // 2. raise
    }
}

var counter = new Counter();
counter.LimitReached += (sender, value) =>             // 3. subscribe
    Console.WriteLine($"Limit reached: {value}");
counter.Increment(10);
```

---

## Test Scenarios

Every student tests these by hand, **in this order**. Scenario 3 approves the Pending company that Scenario 2 needs, and Scenario 5 needs the application created in Scenario 3.

| # | Scenario | Expected result | Level |
| :---: | :--- | :--- | :--- |
| 1 | Repeat Sprint 1 scenarios 1–5 | They behave exactly as before | Minimum |
| 2 | Apply candidate 2 to job 5 (Pending company); then apply candidate 2 to job 6 (closed) | Both refused with a clear message, and **no** `ApplicationSubmitted` notification is printed | Minimum |
| 3 | Approve the Pending company (Id 3), apply candidate 2 to job 5, then change that application's status | `CompanyApproved`, `ApplicationSubmitted`, and `ApplicationStatusChanged` each print a notification | Minimum |
| 4 | Add a new company, then reject it; then try to add a job to it | The company is rejected with **no** `CompanyApproved` notification, and the job is refused | Minimum |
| 5 | Remove job 5, which now has an application; then close it through Update | Remove is refused with a clear message; closing succeeds | Minimum |
| 6 | **Restart the application** so the seed data is back. Search with keyword `developer` and minimum `0`; then `DEVELOPER` and minimum `15000`; then an empty keyword and minimum `10000`; then keyword `xyz` | Jobs 1 and 6; then Job 6 only; then Jobs 1, 2, 3, and 6; then a "no match" message. The keyword ignores case | Minimum |
| 7 | Sort the jobs by salary, then by title | By salary: Jobs 4, 5, 2, 1, 3, 6. By title: Jobs 6, 3, 1, 4, 2, 5. GetAll order is not changed by sorting | Minimum |
| 8 | Apply candidate 1 to job 1 twice | The second application is refused, and **no** second `ApplicationSubmitted` notification is printed | Full |

---

## Working Rules

- **No LINQ** in this sprint. Use loops, delegates, and the collection methods (for example `List<T>.FindAll` and `List<T>.Sort(Comparison<T>)`).
- **Extend, do not rewrite.** Keep the Sprint 1 structure and fix problems in place.
- Use only concepts taught or introduced during the sprint.
- Keep all data in memory: no database, no Entity Framework.
- The application must not crash on invalid input.
- Study one topic at a time, about an hour per session. **Finish Part 1 (study and practice, days 1–9) before starting Part 2 (the project, days 10–14).**
- Commit after every requirement, with its ID in the message.
- Work in the **same GitHub repository** as Sprint 1.
- Students may discuss concepts, but each one implements and explains their own solution.
- Avoid extra architecture patterns; the focus is C# fundamentals and sound decisions.

---

## Acceptance Criteria

A submission is accepted when all of these are true.

**Minimum level**

- [ ] D1–D4, EV1, M3, E4, E6, and M2 are done and meet their acceptance criteria in [Requirements](#requirements)
- [ ] Test scenarios 1–7 pass
- [ ] The solution builds with no errors, still loads the seed data on first run, and never crashes on bad input
- [ ] The Sprint 1 Minimum still works
- [ ] The student can explain the points listed under each phase of the requirements

**Full level**

- [ ] M4 is done and test scenario 8 passes, or M4 is clearly marked as skipped

**Repository**

- [ ] The Sprint 2 work is pushed to the **same repository** as Sprint 1, with a commit per requirement and its ID in the message
- [ ] The `README.md` from Sprint 1 is updated: Sprint 2 requirement IDs marked **Done** or **Skipped**, and 1–2 new design decisions
- [ ] The final commit is tagged `sprint-2`
- [ ] No secrets or real credentials
