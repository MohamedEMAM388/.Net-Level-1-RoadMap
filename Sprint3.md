# Sprint 2 — LINQ

> Project: **Sales Data Analytics Dashboard (Console)**
> Duration: **2 weeks (14 days) · Day 15–28**

## Sprint Goal

Learn to query and shape data in memory with LINQ, understand how queries execute, and know the common pitfalls.

## How to use this sprint plan

For every task:

1. Read the **MUST** resources before implementation.
2. Read **DEEP DIVE** only when the concept is unclear or the learner wants deeper understanding.
3. Implement the requested change.
4. Verify the behavior manually.
5. Commit the work in small, meaningful commits.
6. Be able to explain the decision during review.

**Important:** This document does not change the original Sprint scope. It expands the original tasks into an executable checklist.

## Project Brief — Sales Data Analytics Dashboard

A read-only reporting tool over in-memory sales data. There is no CRUD here: build the data once, then answer business questions with LINQ. Every report is a menu option that prints a clean table.

### Data Model

**Product**

| Field | Type |
| --- | --- |
| Id | int |
| Name | string |
| Category | string |
| Price | decimal |
| Stock | int |

**Customer**

| Field | Type |
| --- | --- |
| Id | int |
| Name | string |
| Email | string |
| City | string |

**Order**

| Field | Type |
| --- | --- |
| Id | int |
| CustomerId | int |
| OrderDate | DateTime |

**OrderItem**

| Field | Type |
| --- | --- |
| OrderId, ProductId | int, int |
| Quantity | int |
| UnitPrice | decimal, the price at the time of the order |

### Seed Data (Minimum)

| Collection | Min | Must include |
| --- | --- | --- |
| Products | 25 | 5+ categories, cheap to expensive prices, at least 2 products with Stock = 0 |
| Customers | 15 | 6+ cities, 3+ customers with no orders, two customers sharing one email (for `SingleOrDefault`) |
| Orders | 40 | Spread over at least 6 different months |
| OrderItems | 100 | 1 to 5 items per order; some products never sold |

### Good to Know

Practise alongside the topics (not project features): Method Syntax vs Query Syntax, and Deferred vs Immediate execution.

Add a small **Exercises** menu: the same query in both syntaxes, and a proof of deferred execution (change the list after writing the query, before enumerating it).

### Project Requirements

**Easy** — filter, project, sort, page, check

- **E1** Filter products by Category and by price range (`Where`); min and max typed by the user.
- **E2** Select names and prices into an anonymous type and print them.
- **E3** Sort by Category then Price with `OrderBy` + `ThenBy`; a menu choice for descending.
- **E4** Console paging with `Skip` / `Take`, page size 5, Next / Previous, and "Page 2 of 5".
- **E5** `Any` / `All`: "Is any product out of stock?" and "Are all prices above 0?" Print Yes / No and the product names when relevant.
- **E6** `First` vs `FirstOrDefault` on an empty result (a product that does not exist): show that one returns null and the other throws, and handle both.

**Medium** — grouping, aggregation, set operations

- **M1** `GroupBy` + `Count`: products per Category and customers per City, sorted by count descending.
- **M2** Revenue: total, average order value, largest and smallest order (`Sum`, `Average`, `Max`, `Min`). An order total is the sum of `Quantity * UnitPrice`.
- **M3** Revenue per month (group by year and month, sorted by date).
- **M4** Top 5 best-selling products by quantity (name, quantity, revenue).
- **M5** `Distinct`: unique cities and unique categories.
- **M6** `Union` / `Except`: from two customer lists, print the merged list and the customers who are in A but not in B.
- **M7** `SingleOrDefault`: find a customer by email; on a duplicate, catch `InvalidOperationException` and explain when `Single` and `First` throw.

**Hard** — joins, nested grouping, comparing

- **H1** Full orders report with `Join` over 4 collections: OrderId, customer, date, product, quantity, line total.
- **H2** `GroupJoin`: each customer with their orders and total spent (customers without orders show 0).
- **H3** Customers with no orders, two ways: `GroupJoin` with an empty group, and `Except` on ids. Print both, prove they match, and comment on which you prefer.
- **H4** Top customer per Category with nested grouping: who spent the most in each category.
- **H5** `SelectMany`: flatten Orders into OrderItems and build the best-sellers report from the flat list.
- **H6** A "Run all reports" menu option so a reviewer sees the whole dashboard in one run.

### Quality Rules

- Each report is one method that returns data; printing is separate (no `Console.WriteLine` inside a query method).
- Do not use loops for what LINQ can do; a `foreach` is only for printing.
- Do not call `ToList()` in the middle of a query unless you can explain why.

> **Sprint-specific rule:** no EF Core in this sprint.


## Working Rules

- Work in order: finish **Easy** before **Medium**, and **Medium** before **Hard**. Commit after every finished requirement.
- Every requirement has an ID (E1, M3, H2). Use it in your commit message, for example `M3: add Predicate search`.
- Use only what was taught up to this sprint.
- The app must never crash on bad input. A wrong menu choice, empty text, or a negative number gets a friendly message, then the app continues.
- Push to GitHub with a README (see the README task).

## Sprint Tasks

### Task 1 — Execution Concepts

*Day 15.*

Demonstrate deferred vs immediate execution, multiple enumeration, and closures.

Then create the data model and the seed data, and add the Exercises menu.

**Required output**
- Seed data that meets the minimums in the brief
- Exercises menu: the same query in Method and Query syntax
- Proof of deferred execution (change the list after writing the query, before enumerating it)
- Example of multiple enumeration and how to avoid it

### Resources
- **MUST** — [Language Integrated Query (LINQ)](https://learn.microsoft.com/en-us/dotnet/csharp/linq/)
- **DEEP DIVE** — Jon Skeet — C# in Depth — `Book: C# in Depth`

---

### Task 2 — Filtering & Quantifiers

*Day 16.*

Practice Where, OfType, Any, All, and Contains.

**Apply to the project:** **E1**, **E5** (see Project Requirements above)

**Required output**
- Product filter by category and price range
- Any / All questions with Yes / No output

### Resources
- **MUST** — [Standard query operators overview](https://learn.microsoft.com/en-us/dotnet/csharp/linq/standard-query-operators/)

---

### Task 3 — Projection & Ordering

*Day 17–18.*

Practice Select, SelectMany, projection into records/DTOs, and ordering.

**Apply to the project:** **E2**, **E3** (see Project Requirements above)

**Required output**
- Anonymous-type projection of names and prices
- Sorting by category then price, with a descending option

### Resources
- **MUST** — [Standard query operators overview](https://learn.microsoft.com/en-us/dotnet/csharp/linq/standard-query-operators/)

---

### Task 4 — Element, Conversion & Paging

*Day 19.*

Practice element operators, conversion operators, Skip, and Take.

**Apply to the project:** **E4**, **E6**, **M7** (see Project Requirements above)

**Required output**
- Console paging with Next / Previous and "Page X of Y"
- `First` vs `FirstOrDefault` demo
- `SingleOrDefault` customer lookup that handles the duplicate email

### Resources
- **MUST** — [Standard query operators overview](https://learn.microsoft.com/en-us/dotnet/csharp/linq/standard-query-operators/)

---

### Task 5 — Grouping

*Day 20–21.*

Practice GroupBy with composite keys, per-group aggregates, and ToLookup.

**Apply to the project:** **M1**, **M3**, **M4** (see Project Requirements above)

**Required output**
- Products per category and customers per city
- Revenue per month
- Top 5 best-selling products

### Resources
- **MUST** — [Standard query operators overview](https://learn.microsoft.com/en-us/dotnet/csharp/linq/standard-query-operators/)
- **MUST** — [Grouping data](https://learn.microsoft.com/en-us/dotnet/csharp/linq/standard-query-operators/grouping-data)

---

### Task 6 — Joining

*Day 22–23.*

Practice Join, GroupJoin, and left join.

**Apply to the project:** **H1**, **H2**, **H3** (see Project Requirements above)

**Required output**
- Full orders report with a 4-collection Join
- Customers with their orders and total spent (0 for no orders)
- No-orders customers found two ways, with proof that both match

### Resources
- **MUST** — [Standard query operators overview](https://learn.microsoft.com/en-us/dotnet/csharp/linq/standard-query-operators/)
- **MUST** — [Join operations](https://learn.microsoft.com/en-us/dotnet/csharp/linq/standard-query-operators/join-operations)

---

### Task 7 — Aggregation & Set Operations

*Day 24.*

Practice Sum, Count, Average, Min, Max, MinBy, MaxBy, Aggregate, Distinct, DistinctBy, Union, Intersect, Except, and Concat.

**Apply to the project:** **M2**, **M5**, **M6** (see Project Requirements above)

**Required output**
- Revenue report: total, average order value, largest and smallest order
- Unique cities and categories
- Union / Except report on two customer lists

### Resources
- **MUST** — [Standard query operators overview](https://learn.microsoft.com/en-us/dotnet/csharp/linq/standard-query-operators/)

---

### Task 8 — Custom LINQ & Pitfalls

*Day 25.*

Write extension methods on `IEnumerable<T>`, use `yield return`, compare `Func` with `Expression<Func>`, and review common LINQ pitfalls and performance decisions.

**Apply to the project:** **H4**, **H5** (see Project Requirements above)

**Required output**
- Top customer per category with nested grouping
- Best-sellers report built from a `SelectMany` flat list
- At least one custom LINQ extension method
- Short note: `Func` vs `Expression<Func>` and why it matters for EF Core

### Resources
- **MUST** — [Extension methods](https://learn.microsoft.com/en-us/dotnet/csharp/programming-guide/classes-and-structs/extension-methods)
- **MUST** — [yield statement](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/statements/yield)
- **MUST** — [Expression trees](https://learn.microsoft.com/en-us/dotnet/csharp/advanced-topics/expression-trees/)
- **DEEP DIVE** — Jon Skeet — C# in Depth — `Book: C# in Depth`

---

### Task 9 — Sprint Project — Finish & Polish

*Day 26–27.*

Add the "Run all reports" option, review every report against the Quality Rules.

**Apply to the project:** **H6** (see Project Requirements above)

**Required output**
- "Run all reports" menu option
- Every report is a method that returns data (printing is separate)
- No needless `ToList()` and no loops instead of LINQ

---

### Task 10 — README

*Day 28.*

Document:
- Project overview
- Features (every requirement ID marked done or skipped)
- How to run
- Reports (the business question and the LINQ operators of each report)
- Sample output, including "Run all reports"
- Design decisions (Method vs Query syntax, deferred execution, when to materialize)
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

- [ ] Sales Data Analytics Dashboard (Console) with all Easy, Medium, and Hard requirements (or clearly marked as skipped)
- [ ] Seed data that meets the minimums
- [ ] Exercises menu (Method vs Query syntax, deferred execution proof)
- [ ] README
- [ ] Commit history that follows the requirement IDs

## Definition of Done

- [ ] Seed data meets the minimums
- [ ] All Easy requirements (E1–E6) are done
- [ ] All Medium requirements (M1–M7) are done
- [ ] All Hard requirements (H1–H6) are done, or clearly marked as skipped
- [ ] "Run all reports" shows the whole dashboard in one run
- [ ] The Quality Rules are respected
- [ ] README is complete and commits follow the requirement IDs

## Before You Submit

- [ ] The solution builds with no errors, and seed data loads on first run
- [ ] Every Easy / Medium / Hard requirement is done, or clearly marked as skipped
- [ ] No secrets, passwords, or real connection strings in the repo
- [ ] The README is complete and the commit history follows the requirement IDs

## Review Rule

The learner should be able to explain not only **what** was implemented, but **why it belongs there, what alternatives existed, and what trade-off was accepted**.
