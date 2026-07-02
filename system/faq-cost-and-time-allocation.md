# FAQ: How Time & Cost Allocate — Departments vs. Projects
**Version:** 1.0 | **Maintained by:** S-BOS System Admin | **Last Updated:** July 2026

---

## The question this answers

> "A Marketing employee logs time to a project that's in the **Retail** department. Does that cost land in **Marketing** (where the employee sits) or **Retail** (where the project lives)? Can one time card go to more than one department?"

This comes up constantly because a **department** in S-BOS plays two roles at once:
- an **operating / product-line** department that **projects** roll up to — Retail, Multi-Family, Entry Level Housing, Asset Disposition-Brokerage, General Brokerage, 3rd Party Development & GC; and
- a **functional / overhead** department that **people** sit in — Marketing, HR, IT/Systems, Finance, Construction, etc.

They live in one Departments table, so the tangle is real. Here's the rule.

---

## The rule: separate "whose cost" from "what it benefited"

Two different questions get tangled together. Keep them apart:

| Question | Answered by | Used for |
|---|---|---|
| **Whose cost is it?** | The employee's **home department** (e.g., Marketing) | Payroll, headcount, overhead, comp |
| **What did the work benefit?** | The **project** — and the project's department (e.g., Retail) | Job costing, project economics |

**A time card charges the work, not the worker's org box.** The employee being in Marketing is a fact about the *person*; it is **not** the charge target for project work.

---

## How a time-card line is charged

Each **time-card line** has exactly **one charge target**:

- **Direct (project) hours** → the line charges a **Project**. The department is **derived from the project** (Retail). A Marketing employee's hours on a Retail project therefore land in **Retail's** project economics. Marketing is never the charge target.
- **Overhead hours** (no specific project) → the line charges a **Department** directly (+ the customer served, if applicable). True Marketing overhead stays in Marketing.

**Splitting across departments/projects = multiple lines, not a multi-department field.** If someone works 2 hrs on a Retail project, 3 hrs on a Multi-Family project, and 3 hrs of Marketing overhead, that's **three lines**, each charging one target. This keeps costing clean and reportable.

Time-card dimensions (per line): `Cost Code · Department · Project ID · Customer ID · Account Code · Hours`. For a direct line, `Project ID` is set and the department follows the project; for an overhead line, `Project ID` is null and the department is set explicitly.

---

## Worked example

Jane is in **Marketing**. This week she logs:

| Hours | Charge target | Department it lands in | Why |
|---|---|---|---|
| 2 | Retail project "Maple Plaza" | **Retail** | Direct — follows the project |
| 3 | Multi-Family project "Elm Court" | **Multi-Family** | Direct — follows the project |
| 3 | (none) Marketing overhead | **Marketing** | Overhead — charged to her dept |

Retail's project report shows Jane's 2 hours as project cost. Marketing's report shows the 3 overhead hours. Nobody had to force the project hours into Marketing, and nothing was double-counted.

---

## What about the employee's salary?

Jane's salary sits in **Marketing** overhead (that's whose cost it is). Her **direct project hours are charged out to the projects** at a labor rate — a **labor recovery** that offsets Marketing's cost and loads the project. This is standard job-cost accounting (labor burden / recovery); it's how a person in an overhead department can still contribute cost to a product-line project without distorting either report. *(The recovery mechanics are a separate, later build detail — noted here only so the salary-vs-project-cost relationship is clear.)*

---

## Quick reference

- **Department = two things** (product-line the project rolls up to **+** functional home of the person). Don't conflate them.
- **Time-card line charges the work:** a **Project** (direct — dept follows the project) or a **Department** (overhead).
- **Multiple allocations → multiple lines**, never one line to many departments.
- **Home department is a People attribute** — comp/headcount/overhead — not the project charge target.
- **Project economics roll up to the project's department**, regardless of which department the person sits in.
