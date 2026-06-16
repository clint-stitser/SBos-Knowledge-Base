# Pre-Design Thought List: Before You Open the PDD

> ## 🛠️ Read Before Filling
>
> **Template version:** AI-optimized (minimal — human-only thinking doc)
> **Fill order:** First step in the design pipeline. Before the PDD. Before any other design doc.
>
> **This template is different from every other in the suite.** It is a **human-only thinking doc**. There is no agent enforcement. There are no 🤖 blocks inside the sections. There are no gates. The PDD is where answers get formalized — this file is where the human gets their thinking straight.
>
> **Agent role:** Conversational thinking partner only. The agent asks questions, surfaces blind spots, and helps the human work through the questions below. The agent does NOT:
> - Fill in answers on the human's behalf
> - Cross-check this file against anything
> - Enforce completeness (the human decides when they're done thinking)
> - Generate downstream artifacts from this file (the PDD pulls from the human's mind, not from this file)
>
> **Format note:** Notes only. This is not a deliverable. Jot down whatever comes to mind. Bullet points, fragments, stream-of-consciousness — all fine. The PDD is where prose and structure happen.
>
> **When this file is "done":** When the human feels ready to open the PDD. There is no formal sign-off. There is no "complete" state. Some questions may be answered; some may be flagged as "need to find out"; some may be skipped because they don't apply. All three outcomes are valid.
>
> **When the next file in the pipeline opens:** Move on to `[AppName]_Product_Design_Doc.md` (the PDD). The PDD's first sections will pull from the thinking captured here, but the PDD agent does NOT read this file directly — the human writes the PDD answers based on what they thought through here.

**Purpose:** Get your thinking straight before touching any design doc. These are the questions that derail projects mid-build when left unanswered. 30 minutes here saves days later.

**How to use:** Work through this alone or with Claude before starting the Product Design Doc. You don't need final answers to everything — but you need to *know* what you don't know.

**Format:** Notes only. This is not a deliverable. Jot down whatever comes to mind. The PDD is where answers get formalized.

---

## 1. The Problem

The most common design mistake is building a solution before nailing the problem.

- What problem am I actually solving? Write it in one sentence.
- Who has this problem? Be specific — not "users," but what kind of person, in what context.
- How are they solving it today? (Spreadsheet? Another app? Doing nothing? Workaround?)
- Why is the current solution bad enough that they'd switch?
- What does "solved" look like to them? How would they know the problem is gone?
- Am I solving a real pain, or building something I think is cool? (Be honest.)

---

## 2. The Users

Vague users = vague features = scope creep.

- Who is the primary user? One person, one role.
- Is there a secondary user? (Admin, manager, customer, etc.)
- Do these users have conflicting needs? (If yes, which one wins?)
- How technical are they? Will they read docs, or do they need zero onboarding?
- How often will they use this? Daily driver or occasional tool?
- What device are they on — desktop, mobile, both?
- What would make them abandon the app?

---

## 3. The Scope

The hardest part of design is deciding what NOT to build.

- What is the absolute minimum this needs to do to be worth using?
- What features am I tempted to add that aren't actually necessary for V1?
- What does this app explicitly NOT do? (Write it down — "out of scope" is a design decision.)
- Is this a tool (does one thing well) or a platform (does many things)? Know which one before you start.
- What's the one thing that, if I got wrong, would make the whole app fail?

---

## 4. The Data

Apps are just UIs on top of data. Know your data before you design your UI.

- What are the core "things" in this system? (The nouns — User, Project, Order, Appointment, etc.)
- Which of those things does the app create, read, update, or delete?
- Is any data shared between users, or is everything private per user?
- Does anything have history that needs to be tracked? (Audit log, version history, etc.)
- Is there any data I need to import or migrate from somewhere else?
- What data would be catastrophic to lose? How important is backup / recovery?

---

## 5. The Workflows

Features aren't features until you can trace the path a user walks to use them.

- Walk through the 3 most important things a user will do. Step by step.
  - What triggers each one? What do they click or tap first?
  - What does the system do in response?
  - What can go wrong? What does the user see when it does?
- Are there any multi-user workflows? (User A does X, then User B does Y?)
- Are there any time-based or background processes? (Scheduled jobs, reminders, expiration, etc.)

---

## 6. The Constraints

Ignoring constraints early = painful pivots late.

- Is there a technology I'm locked into? (Platform, language, existing codebase?)
- Is there a budget or time constraint that limits scope?
- Are there any legal, compliance, or regulatory requirements? (HIPAA, GDPR, PCI, etc.)
- Do I need to integrate with any existing systems or third-party services?
- Will this need to scale, or is this a small/internal tool?
- Is there an existing codebase this connects to? If so, what are its constraints?

---

## 7. Third-Party Dependencies

Third-party services aren't implementation details — they're architectural decisions. The wrong integration choice, discovered mid-build, can require redesigning your data model.

- What external services are non-negotiable? (Auth provider, payment processor, email, maps, etc.)
- For each one: have I used it before? If not, have I read the docs enough to know what it requires?
- Does any integration dictate my data model? (e.g., Stripe requires a `customer_id` on User; Twilio requires E.164 phone format)
- Does any integration require a webhook endpoint? What does that mean for my API surface?
- What are the rate limits on each service I plan to use? What happens to my app when I hit them?
- Are any of these services paid? Do I have API keys, or do I need to sign up first?
- Is there a sandbox / test mode? Can I develop without hitting real APIs?
- What happens to my app if one of these services goes down? Is that acceptable, or do I need a fallback?
- Am I locked in to any of these, or could I swap them out later if needed?

> ⚠️ **Gate check:** If you're integrating with Stripe, an OAuth provider, a mapping API, or any service with its own data model requirements — sketch out what fields that forces onto your entities *before* opening the DB Schema Doc. Discovering "Stripe needs a `customer_id` on User" after you've designed the schema means rework.

---

## 8. Success

If you can't define success, you can't know when you're done.

- What does a successful V1 look like, concretely?
- What's the one metric that, if I hit it, I'd call this a win?
- What would make me decide to keep building vs. shut it down?
- Is there a launch deadline? Why does that date matter?

---

## 9. The Honest Questions

These are the ones people skip because they're uncomfortable. Don't skip them.

- Has this been built before? What can I learn from what already exists?
- Why will users choose this over what they're already doing?
- What's the riskiest assumption I'm making? What happens if it's wrong?
- Am I the right person to build this? Do I understand the user's problem from their perspective?
- If this project stalled in 3 months, what would be the most likely reason?

---

## Before You Start the PDD

You're ready when you can answer:

1. **The problem in one sentence** — not a solution, the actual problem
2. **Who the primary user is** — specific enough that you could describe a real person
3. **The 3 most important features** — and why these 3, not others
4. **What's out of scope** — at least 3 things you're explicitly not building
5. **What "done" looks like for V1** — a concrete, checkable condition

If any of these are blank, spend more time here before opening the PDD.
