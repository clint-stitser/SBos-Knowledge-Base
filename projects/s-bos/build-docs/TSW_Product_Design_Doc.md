### N — Project-Based Tool Layer (Claude-Built Tools)

**What it is:** A project-scoped tool-building system where Claude builds custom mini-apps, trackers, checklists, and schedulers for bounded life projects. Tools are created for a specific project stage, used through that stage's lifecycle, then archived for future reference or sharing with others. This is distinct from the recurring life domain framework (Body/Being/Balance/Business) — it handles the temporary, bounded, specific needs that arise throughout life.

**The core insight:** Not everything in life is a habit or a domain. Some things are projects — bounded in time, specific in need, complete when done. A trip to Europe isn't a Body goal. An ear infection isn't a Being ritual. An AP Chemistry test isn't a Business phase gate. These need their own tools, purpose-built for their stage, scoped to their project.

---

#### The Four Project Pillars (universal — applies to every project in both S-BOS and Stitser Way)

Every project — whether a construction development in S-BOS or a family trip in Stitser Way — is structured around the same four pillars. This is existing infrastructure in SmartSuite, developing in Supabase over time, and surfaced in both applications.

| Pillar | What it contains |
|---|---|
| **Budget** | Financial plan, cost tracking, actuals vs. planned — for any project that involves money |
| **Alignment** | Purpose + outcome (why this project exists and what success looks like) + Team (who is involved, what they do, when they do it, why they do it, how much/when they get rewarded) |
| **Schedule** | Timeline, milestones, phases, sequencing — the when |
| **Checklists** | QC / Safety / Decisions / Docs / Routines — structured verification and process within each stage |

**These pillars are universal.** A school year has all four:
- Budget → school fees, supplies, activity costs
- Alignment → purpose of the year, subjects and their goals, family expectations, kids' roles and incentives
- Schedule → semester dates, exam schedule, key milestones
- Checklists → daily homework routine, weekly review, test prep checklist

A family trip to Europe has all four:
- Budget → flights, lodging, activities, food, contingency
- Alignment → purpose of the trip, who's going, what each person's role is, what success looks like for the family
- Schedule → day-by-day itinerary, travel legs, activity booking windows
- Checklists → packing list, document checklist, pre-departure routine

A medical protocol (ear infection) has all four:
- Budget → medication costs, copays, follow-up visits
- Alignment → treatment goal, who is responsible for what (parent gives medication, child reports symptoms), outcome criteria
- Schedule → dosage timing, follow-up appointment dates
- Checklists → medication schedule, symptom tracking, when to call the doctor

---

#### Project Hierarchy

Projects are structured in three levels using the existing S-BOS SmartSuite project infrastructure:

```
Master Project     → School Year 2026–27
  Child Project    → AP Chemistry
    Grandchild     → Midterm Exam — Oct 15
      Tool         → Claude-built: flashcard quiz + spaced review schedule (Study App)

Master Project     → Europe Trip — Summer 2027
  Child Project    → Budget pillar
    Tool           → Claude-built: trip budget tracker with categories + running total
  Child Project    → Schedule pillar
    Tool           → Claude-built: day-by-day itinerary with logistics

Master Project     → Ear Infection — Max, Jun 2026
  Child Project    → Checklists pillar → Routines
    Tool           → Claude-built: medication schedule with dosage + timing reminders
```

Claude-built tools attach to the specific pillar or sub-project they serve — a budget tool lives inside the Budget pillar, a study app lives inside the relevant Checklist or Schedule stage.

---

#### Tool Lifecycle — Four Stages

| Stage | What happens | App behavior |
|---|---|---|
| **Create** | Claude builds the tool scoped to the project pillar/stage | User describes the need → Claude generates the tool → tool attaches to the project record |
| **Active** | Tool is in use for the duration of the project stage | Accessible from project card, relevant tasks surface in Horizon Rings |
| **Complete** | Project stage ends, tool has served its purpose | Tool marked complete, project stage closes, archive prompt fires |
| **Archived** | Tool stored for future reference or sharing | Accessible from project archive, searchable, shareable with other family members or users |

---

#### Tool Types (Claude generates based on need — not a fixed menu)

| Type | Pillar it typically serves | Example |
|---|---|---|
| Budget tracker | Budget | Trip budget, medical cost tracker, school activity fund |
| Study app | Checklists / Schedule | Flashcard quiz, spaced review schedule, concept summary |
| Scheduler / Itinerary | Schedule | Day-by-day trip plan, exam prep calendar, treatment timeline |
| Checklist | Checklists | Packing list, pre-race checklist, medication schedule |
| Team planner | Alignment | Who does what, when, and for what reward on a family project |
| Calculator | Budget | Grade projector, dosage calculator, cost-per-day budget |
| Reference doc | Alignment | Research summary, rules for a sport, protocol reference |

---

#### Data Model

- **Phase 1 (SmartSuite):** Projects live in the existing S-BOS SmartSuite project infrastructure — Master → Child → Grandchild hierarchy. The four pillars (Budget, Alignment, Schedule, Checklists) exist as structured fields/sub-records within each project. Claude-built tools are artifact HTML files linked to the relevant project record.
- **Phase 2 (Supabase):** Project infrastructure migrates to Supabase, developing the four-pillar model with full relational depth. Tool storage migrates with the rest of the data layer.
- **Both S-BOS and Stitser Way read from the same project infrastructure** — S-BOS surfaces business projects (construction, development, brokerage), Stitser Way surfaces personal/family projects (school, travel, health, home). Same four pillars. Different audiences.

---

#### Relationship to the Rest of the App

- **Horizon Rings** — active project tasks surface in the rings by due date and urgency, alongside Goals and GYR follow-ups
- **Life domains** — every project belongs to a domain: school project → Balance (kids/family), trip → Balance or Being, medical protocol → Body, home project → Balance, personal learning → Being
- **Archive as library** — completed tools accumulate into a personal library. A study app built for AP Chemistry adapts to AP Biology. A medication schedule for one ear infection becomes the template for the next. Searchable by project type, domain, family member, tool type.
- **Family Profiles** — tools are shareable across family members. A packing checklist built for one family trip becomes the starting point for the next. A study app Avery used can be shared with Brynn.

---

#### What This Is NOT

- Not S-BOS project management — S-BOS handles business projects (construction, development, brokerage). Stitser Way handles personal/family projects. Same infrastructure, different context.
- Not a pre-built app library — Claude builds each tool fresh for the specific need. The archive stores the outputs.
- Not permanent infrastructure — tools have a lifecycle. Complete → archive. They are not maintained indefinitely.

---

#### Open Question (flag for §9)

What is the UX entry point for creating a new tool?
- (a) Tap into a project pillar → "Build a tool for this stage" → Claude conversation → tool attaches to that pillar
- (b) Claude skill trigger from anywhere ("build me a tool for...") → Kompass identifies the right project and pillar → attaches
- Both may be valid and complementary entry points. Decide before UI/UX doc is written.
