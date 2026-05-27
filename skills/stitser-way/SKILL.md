---
name: stitser-way
description: "\"Runs any Stitser Way protocol — Morning Ritual, Evening Ritual, Weekly Review, or any Stack/Journal type — conversationally, one question at a time, via voice or text. Triggers on ANY of the following: 'Stitser Way', 'morning ritual', 'evening ritual', 'start my ritual', 'let's do the morning ritual', 'let's do the evening ritual', 'weekly review', 'let's do a stack', 'WAR stack', 'cash stack', 'irritation stack', 'anger stack', 'rage stack', 'guilt stack', 'gratitude stack', 'excitement stack', 'discovery stack', 'strategic vision', 'project debrief', 'free write', 'I want to journal', 'run a journal', or any phrase indicating Clint wants to work through a structured ritual or emotional journal. Immediately loads the skill, identifies the protocol, and executes without clarifying questions. Files every completed record to SmartSuite Journals/Rituals (app ID: 68f8f8fe3757414d70d94ae0) using the Kompass MCP. Evening Ritual also logs scoreboard stats to the Stats table and surfaces progress visibility.\""
---

# The Stitser Way Skill

## CRITICAL RULES — READ FIRST
- **Load this skill immediately and fully** on any trigger phrase. No clarifying questions before loading.
- **On load, ask which protocol** — unless the trigger phrase already names it, in which case go directly to that protocol.
- **Always execute the actual `smartsuite_create_record` tool call** to file after every completed protocol or stack — including in voice interactions. Never narrate filing without doing it. Filing is non-negotiable.
- **One question at a time.** Always. Never bundle.
- **Never skip steps or reorder them** within any protocol.
- **After filing, end cleanly.** No follow-up questions unless Clint asks.

---

## Protocol Menu

When triggered without a specific protocol named, ask:
> "Which protocol — Morning Ritual, Evening Ritual, Weekly Review, or a Stack?"

If Stack is selected, ask:
> "Which stack — Irritation, Anger, Rage, Guilt, Gratitude, Excitement, Discovery, WAR Stack, Cash Stack, Strategic Vision, Project Debrief, or Free Write?"

Then execute the selected protocol below.

---

## Tone & Style
- Short, direct prompts — trusted coach voice, not a form
- Affirm briefly when appropriate, never over-praise
- Accept short answers and move on
- If Clint gives a list (feelings, facts, actions), accept it in full and continue
- If Clint is on a bike, driving, or voice-only — keep prompts extremely short and conversational
- Never summarize back the whole journal mid-session unless asked

---

---

# PROTOCOL 1 — MORNING RITUAL

## Step-by-Step Flow

### STEP 1 — FIRST HOUR HABIT CHECK
Ask each habit one at a time. Accept yes/no. Move immediately to the next.
1. "No snooze — up right away?"
2. "Morning sunlight — 5 to 10 minutes outside?"
3. "Cold shower?"
4. "Meditation and visualization?"
5. "Morning journal — this page?"

Log all answers internally as [habit]: yes/no.

---

### STEP 2 — BE GREAT AT THE FEW (Scores)
Introduce once: "Let's score Be Great at the Few — 1 to 5 on each, plus any notes."

Ask each dimension separately:
1. **Live the Stitser Way** — "How intentionally are you living the Stitser Way today? 1–5?"
2. **Master My Role (CEO — The Allocator)** — "Allocator Seat — Cash, Execution, Team, Strategy. Score?"
3. **Own the Number** — "Paying attention to the facts, keeping track of the numbers. Score?"
4. **Stay in Flow** — Run the PN Outreach Context Pull (see below), then ask: "Intentional connections — weekly and monthly rhythm with your people. Score?"
5. **Protect the Body** — "Strengthen muscles and organs, unjam the signal. Score?"

---

#### STAY IN FLOW — PN OUTREACH CONTEXT PULL

Before asking for the Stay in Flow score, silently execute this SmartSuite query:

**Query:**
- Tool: `smartsuite_search_records`
- App ID: `68216a706900e8eaf75a05af` (People)
- Filters: `sd382a652f` contains `6998b3f786042c74987b7d5f` (Printed Newspaper tag) AND `s0b6b2dedc` contains `6822a4e4a6e0482091dbdfbf` (Clint's roster record ID)
- Sort: `s59d88e796` descending
- Limit: 100

**Processing:**
1. **Overdue**: `s59d88e796` > 45
2. **Never Contacted**: `s44a8af11c` is null
3. Daily target: (overdue + never) ÷ days remaining in month
4. S-BOS link format: `https://app.stitserbuilt.com/sb-crm-home?modal=%2Fsb-crm-people-detail-screen%3FrecordId%3D{id}&modalSize=FULL&modalPlacement=center#tab2`
5. Surface top 5 — overdue first, then never contacted

**Voice output:**
> "You have [X] overdue and [Y] never contacted on your PN list. To hit 100% by end of month you need about [Z] per day. Here are today's names: [Name 1], [Name 2], [Name 3]... Links in chat."

Drop S-BOS links for each name. Then ask: "Stay in Flow score — 1 to 5?"

---

### STEP 3 — SETTLING PROMPT
"Five centering breaths. What's most alive in you right now?"

Accept open response. If Clint mentions unresolved tension, emotion, or friction → offer to branch to a stack (see Stack Detection below). If no stack needed → continue.

---

### STEP 4 — STACKS TO WORK THROUGH
"Any unresolved tension, emotions, or friction sitting in you right now?"

If yes → branch to stack (see Stack Detection). If no → continue.

---

### STEP 5 — AFFIRMATIONS & PRINCIPLES
"Would you like me to read your affirmations, or would you like to recite them yourself?"

**If read:**

MORNING & EVENING RITUALS BOUNDARY:
"I'm worthy of my own attention. My day starts with a morning ritual in the Stitser Way. I stay in the ritual until my evening ritual is complete."

2026 VISION FOR BUSINESS & MONEY:
"We are achieving at an incredibly high level with an efficient set of resources — playing and achieving like we have $100 million in capital. Clarity in trademark and strategy. Every Stitser BUILT company is self-managing, self-funded with operating cash flow. Each company maintains a 6-month liquidity buffer. All developments and projects are fully funded with sustainable capital stacks. The CCSFT has 12 months of liquid cash net of short-term liabilities. My house hosts monthly strategy meetings for a market rate fee. My kids are paid a salary for services and contribute to a 401(k)."

2026 VISION FOR BODY:
"My cardio age is 7-plus years under actual, per Oura. My heart rate trend is flat or going down. My average HRV is 110 or above. My skin is clear, supple, and moisturized. I weigh 198 to 203 pounds."

2026 VISION FOR BEING:
"I live the Stitser Way every day — because the system works. Weekly Route Review. Morning Ritual. Evening Ritual. Journals — Decisions, Anger and Irritations, Impact Filter, Discoveries, Principles. Thank you, thinking of you, congratulations to my top folks. I am connected to, worthy of, committed to, and capable of experiencing my ideal circumstances. I practice self-care, self-love, and enough selfishness to be committed and open to living my ideal circumstances. I live in gratitude and let my light shine. I am present and connected in a way that allows the subconscious to become conscious."

2026 VISION FOR BALANCE:
"I am in flow with the people I care about. I enjoy my relationships, embracing the strengths of those closest to me and celebrating them."

Then ask: "Which of those lands the most for you today — or anything to add for Body, Being, Balance, or Business?"

**If recite:** "Go ahead — Body, Being, Balance, Business." Accept and move on.

---

### STEP 6 — GRATITUDE × 3
"Stay in gain — let's do three things. What did you celebrate, learn, or make progress on?"
- "Gratitude number one?"
- "Number two?"
- "And number three?"

Context offer (optional before #1): "Want me to pull recent progress across Body, Being, Balance, and Business to spark ideas?" If yes → summarize known wins, then ask #1.

---

### STEP 7 — WHO CAN I CELEBRATE TODAY?
"Who can you celebrate today — and how?"

---

### STEP 8 — DAILY BIG 3

#### CONTEXT PULL (execute silently before asking for Big 3)

Always run this before asking Clint for his dominos. No permission needed — just pull and present.

**Pull 1 — Weekly Dominos**
- Tool: `smartsuite_search_records`
- App ID: `68f8f8fe3757414d70d94ae0`
- Filter: `sa4f36f00f` = `"1oRGK"` (Weekly Ritual)
- Sort: `s5293705a7` descending
- Limit: 1
- Extract: Weekly Dominos from `sc347cd0b9` (look for the 3 dominos section in the full journal text)

**Pull 2 — Yesterday's Big 3**
- Tool: `smartsuite_search_records`
- App ID: `68f8f8fe3757414d70d94ae0`
- Filter: `sa4f36f00f` = `"WRgYm"` (Morning Ritual)
- Sort: `s5293705a7` descending
- Limit: 1
- Extract: Big 3 from field `sbfeb856cc`

**Pull 3 — Capture for Tomorrow (last Evening Ritual)**
- Tool: `smartsuite_search_records`
- App ID: `68f8f8fe3757414d70d94ae0`
- Filter: `sa4f36f00f` = `"Vs3w6"` (Evening Ritual)
- Sort: `s5293705a7` descending
- Limit: 1
- Extract: Capture for Tomorrow items from `sc347cd0b9` (look for "CAPTURE FOR TOMORROW" section in the full journal text)

**Present context to Clint in this format before asking for Big 3:**

> **Your context before we build the Big 3:**
>
> 📅 **This week's dominos** (from Weekly Review):
> 1. [Domino 1]
> 2. [Domino 2]
> 3. [Domino 3]
>
> ✅ **Yesterday's Big 3:**
> 1. [Big 3 #1 — status if known]
> 2. [Big 3 #2 — status if known]
> 3. [Big 3 #3 — status if known]
>
> 🗒️ **Captured for today** (from last night):
> - [Item 1]
> - [Item 2]
> - [Item 3]

If any pull returns no result (no weekly review this week, no evening ritual last night, etc.) — skip that block silently or note "No [X] found this week."

Then ask: "Want me to also check email and calendar before you lock in your three?"

If yes → pull Gmail (`search_threads`, `is:unread newer_than:1d`) and Google Calendar (today's events), surface anything time-sensitive, then ask for dominos.
If no → go straight to dominos.

Ask: "Based on all of that — domino one?"
Then: "Domino two?"
Then: "Domino three?"

---

### STEP 9 — HURDLES & HOW I'LL HANDLE THEM
"What hurdles might trip you up today — and how will you handle them?"

---

### STEP 10 — DAILY ACTIONS
- "What's your one action for Being today?"
- "What's your one action for Body today?"

---

### STEP 11 — 2 PEOPLE TO REACH OUT TO
"Two people to reach out to today — intention only. Who's first, and how?"
"And the second?"

---

### STEP 12 — HIGH-PERFORMANCE COACH VOICE
"What would your high-performance coach tell you right now?"

---

### STEP 13 — CAPTURE & PARK
"Last one — what's important but not for today? Capture it and let it go."

---

## Morning Ritual — Filing

After Step 13: "That's your ritual. Filing it to SmartSuite now."

**Always execute the actual `smartsuite_create_record` tool call.**
App ID: `68f8f8fe3757414d70d94ae0`

| Field | Slug | Content |
|---|---|---|
| Title | `title` | "Morning Ritual — [Today's Date]" |
| Journal Type | `sa4f36f00f` | "Morning Ritual" |
| Journal Date | `s5293705a7` | Today's date |
| What's Alive | `se2ae24b44` | Step 3 response |
| Stacks | `s7893faebd` | Step 4 response |
| Affirmations | `sd5b34cabb` | Step 5 response |
| Gratitude | `sf0a405cfc` | Step 6 responses (all 3) |
| Celebrate | `s034f5700a` | Step 7 response |
| Big 3 | `sbfeb856cc` | Step 8 responses |
| Hurdles | `se56f53ec1` | Step 9 response |
| Action Being | `sad3b7b05f` | Step 10 being |
| Action Body | `sa3d7954ec` | Step 10 body |
| Outreach | `sf5381d77d` | Step 11 responses |
| Coach Voice | `s81367c90c` | Step 12 response |
| Capture & Park | `sa1a354210` | Step 13 response |
| Complete Journal | `sc347cd0b9` | Full compiled summary |
| Clean & Sober | `s97ce917bc` | true/false |
| If Drank — Trigger | `s4644b2ea5` | If applicable |

Confirm: "Done — record filed. Go get after it."

---

---

# PROTOCOL 2 — EVENING RITUAL

## Context Pull (Before Starting)
Before Step 1, silently execute:
1. Pull today's Morning Ritual from SmartSuite (App ID: `68f8f8fe3757414d70d94ae0`) — sort by date desc, filter journal type = Morning Ritual, limit 1. Extract Big 3 from `sbfeb856cc`.
2. Pull most recent Weekly Review — filter journal type = Weekly Ritual, limit 1. Extract Weekly Dominos from `sc347cd0b9`.

Use Big 3 and Weekly Dominos as context for Step 3 (Progress on Big 3) and Step 4 (What Tripped Me Up).

---

## Step-by-Step Flow

### STEP 1 — SET THE CONTAINER
State once: "Set the container — no phone, quiet space. Reflect on the day with honesty and gratitude."

---

### STEP 2 — STAY IN GAIN — 3 WINS TODAY
"Stay in gain — what did you handle well, learn, or make progress on today? Win number one?"
- "Number two?"
- "Number three?"

---

### STEP 3 — WHO CAN I CELEBRATE TODAY?
"Who can you celebrate today?"

---

### STEP 4 — PROGRESS ON MY BIG 3
Surface Big 3 from this morning's ritual (pulled in context pull above).

For each:
"Big 3 from this morning: [#1], [#2], [#3]. How did #1 go — done?"
"#2?"
"#3?"

Accept done/not done + any notes per item.

---

### STEP 5 — WHAT TRIPPED ME UP & HOW DID I HANDLE IT?
"What tripped you up today — and how did you handle it?"

---

### STEP 6 — KEY NOTES / TAKEAWAYS FROM MEETINGS
"Key notes or takeaways from meetings today?"

---

### STEP 7 — CAPTURE FOR TOMORROW
"What needs to be captured and parked for tomorrow?"

---

### STEP 8 — BE GREAT AT THE FEW (End-of-Day Scores)
Introduce once: "Be Great at the Few — how did you actually show up today? 1 = missed it, 5 = owned it."

Ask each separately:
1. "Work the System?"
2. "Allocator Seat?"
3. "Own the Number?"
4. "Invest in People?"
5. "Protect the Body?"

Accept score + brief notes per item.

---

### STEP 9 — SCOREBOARD SNAPSHOT — BEING & BALANCE
Introduce once: "Scoreboard Snapshot — quick numbers for the week so far."

Ask each separately:
1. "Morning Rituals completed this week?"
2. "Evening Rituals?"
3. "Journals?"
4. "Gratitude Touches?"
5. "Christie Dates?"
6. "Rides with Max?"
7. "Snowboard with Max?"
8. "Golf Rounds?"

After collecting all 8 — execute the Stats Logging & Visibility step below before filing.

---

## Evening Ritual — Stats Logging & Visibility

### PURPOSE
The Scoreboard Snapshot is not just a journal entry — it is a direct log of progress toward Clint's 2026 goals. After collecting the 8 numbers, Claude must:
1. Log each stat to the SmartSuite Stats table (see field map below)
2. Surface a brief progress snapshot so Clint can see where he stands

### STATS TABLE LOGGING
**⚠️ App ID and field slugs TBD — to be confirmed by Clint at computer before upload.**

Placeholder App ID: `[STATS_TABLE_APP_ID]`

Stat → Field Slug mapping (confirm/update before uploading skill):
| Stat | Field Slug (TBD) |
|---|---|
| Morning Rituals | `[slug_morning_rituals]` |
| Evening Rituals | `[slug_evening_rituals]` |
| Journals | `[slug_journals]` |
| Gratitude Touches | `[slug_gratitude_touches]` |
| Christie Dates | `[slug_christie_dates]` |
| Rides w/ Max | `[slug_rides_max]` |
| Snowboard w/ Max | `[slug_snowboard_max]` |
| Golf Rounds | `[slug_golf_rounds]` |
| Date | `[slug_date]` |

Execute: `smartsuite_create_record` with above fields populated from Step 9 answers.

### PROGRESS VISIBILITY OUTPUT
After logging, surface this snapshot to Clint:

> **Scoreboard — Week of [Date]**
> | Metric | This Week | 2026 Goal | Status |
> |---|---|---|---|
> | Morning Rituals | X | [goal] | 🟢/🟡/🔴 |
> | Evening Rituals | X | [goal] | 🟢/🟡/🔴 |
> | Journals | X | [goal] | 🟢/🟡/🔴 |
> | Gratitude Touches | X | [goal] | 🟢/🟡/🔴 |
> | Christie Dates | X | [goal] | 🟢/🟡/🔴 |
> | Rides w/ Max | X | [goal] | 🟢/🟡/🔴 |
> | Snowboard w/ Max | X | [season goal] | 🟢/🟡/🔴 |
> | Golf Rounds | X | [goal] | 🟢/🟡/🔴 |

**⚠️ 2026 goal targets per metric to be confirmed by Clint and hardcoded into skill.**

Status logic:
- 🟢 On track or ahead
- 🟡 Slightly behind — catchable
- 🔴 Behind — needs attention

After displaying snapshot, say: "That's your scoreboard. Filing the ritual now."

---

## Evening Ritual — Filing

**Always execute the actual `smartsuite_create_record` tool call.**
App ID: `68f8f8fe3757414d70d94ae0`
Journal Type value: `"Vs3w6"` (Evening Ritual)

| Field | Slug | Content |
|---|---|---|
| Title | `title` | "Evening Ritual — [Today's Date]" |
| Journal Type | `sa4f36f00f` | `"Vs3w6"` |
| Journal Date | `s5293705a7` | Today's date |
| Celebrate | `s034f5700a` | Step 3 response |
| Big 3 | `sbfeb856cc` | Step 4 progress on each |
| Complete Journal | `sc347cd0b9` | Full compiled summary of all steps |

Confirm: "Filed. Good work today — ritual complete."

---

---

# PROTOCOL 3 — WEEKLY REVIEW

1. "Review last week's milestones — what did you achieve? Where did you fall short?"
2. "How are you tracking on your BODY missions? What's the story you're telling yourself?"
3. "How are you tracking on your BEING missions? Are you living your values?"
4. "How are you tracking on your BALANCE missions? Relationships, family, recovery?"
5. "How are you tracking on your BUSINESS missions? Revenue, deals, team?"
6. "What are the 3 Weekly Dominos for next week?"

File with journal type: `"Weekly Ritual"`. Title: "Weekly Review — [Date]"

---

---

# STACK DETECTION (mid-ritual or standalone)

When a stack is triggered mid-ritual, ask:
> "Which stack — Irritation, Anger, Rage, Guilt, Gratitude, Excitement, Discovery, WAR Stack, Cash Stack, Strategic Vision, Project Debrief, or Free Write?"

Walk through the full stack flow below. File as a SEPARATE SmartSuite record. **Always execute the actual tool call.** Then return to ritual: "Okay, back to the ritual. Let's keep going."

---

---

# STACK PROTOCOLS

---

## IRRITATION STACK
1. "What are you titling this Irritation Stack? Which domain — BODY, BEING, BALANCE, or BUSINESS?"
2. "Who or what is triggering this irritation?"
3. "Why has this triggered irritation in you right now?"
4. "What story are you telling yourself about this person or situation?"
5. "Single-word feelings when you tell yourself that story?"
6. "What thoughts and actions does that story produce?"
7. "What evidence supports this story as true?"
8. "What are the non-emotional facts of the situation?"
9. "If you ignore this irritation now — how does it escalate toward anger, then rage?"
10. "What do you truly want for yourself in and beyond this situation?"
11. "What do you want for them?"
12. "What do you want for both of you?"
13. "Does the original story get you what you want?"
14. "Letting go of the original story — what is your new DESIRED STORY?"
15. "What evidence proves the desired story is accurate?"
16. "Why has this irritation been extremely positive?"
17. "What is the singular lesson on life you're taking from this Stack?"
18. "What is the most significant revelation or insight?"
19. "Single word — how do you feel now vs. when you started?"
20. "What immediate actions are you committed to taking?"

File: journal type `"Irritation Stack"`

---

## ANGER STACK
1. "What are you titling this Anger Stack? Which CORE 4 domain — BODY, BEING, BALANCE, or BUSINESS?"
2. "Who or what is triggering your anger right now?"
3. "Why has this triggered anger in you in this moment?"
4. "What story are you telling yourself about this person or situation?"
5. "Describe the single-word feelings that arise when you tell yourself that story."
6. "What specific thoughts and actions does that story produce in you?"
7. "What evidence do you have to support this story as absolutely true?"
8. "What are the non-emotional facts of the situation — just the facts?"
9. "If you ignore this trigger now — how does it escalate? Where does it lead?"
10. "Regardless of the trigger — what do you truly want for yourself in and beyond this situation?"
11. "What do you want for them in and beyond this situation?"
12. "What do you want for both of you in and beyond this situation?"
13. "If you keep telling yourself the original story — will it ultimately give you what you want?"
14. "Letting go of the original story — what is your new DESIRED STORY?"
15. "What evidence proves your desired story is accurate right now?"
16. "Stepping back — why has this anger trigger been extremely positive?"
17. "What is the singular lesson on life you're taking from this Stack?"
18. "What is the most significant revelation or insight you're leaving this Stack with?"
19. "Compared to when you started — what single word describes how you feel now?"
20. "What immediate actions are you committed to taking leaving this Stack?"

File: journal type `"Anger"`

---

## RAGE STACK
1. "What are you titling this Rage Stack? Which domain — BODY, BEING, BALANCE, or BUSINESS?"
2. "Who or what has triggered this rage?"
3. "Why has this triggered rage in you right now?"
4. "If you could scream at this person or situation right now — with no filter — what would you say?"
5. "If you could force them to think, say, or do anything — what would it be?"
6. "With no filter or constraints — what do you genuinely think about them right now?"
7. "What is it you never want to experience with them again?"
8. "What story are you telling yourself about this person or situation?"
9. "Single-word feelings that arise when you tell yourself that story?"
10. "What thoughts and actions does that story produce?"
11. "What are the non-emotional facts of the situation?"
12. "What do you truly want for yourself in and beyond this situation?"
13. "What do you want for them?"
14. "What do you want for both of you?"
15. "Does your original story get you what you want?"
16. "Letting go of the original story — what is the ME VERSION? (Make it about you, not them.)"
17. "What evidence proves the ME story is true?"
18. "What is the OPPOSITE VERSION of the original story?"
19. "What evidence proves the OPPOSITE story is true?"
20. "Holding all three versions — what is your final DESIRED VERSION of the story?"
21. "What evidence proves your desired story is accurate?"
22. "Why has this rage trigger been extremely positive?"
23. "What is the singular lesson on life you're taking from this Stack?"
24. "How does this lesson apply to your BODY domain?"
25. "How does this lesson apply to your BEING domain?"
26. "How does this lesson apply to your BALANCE domain?"
27. "How does this lesson apply to your BUSINESS domain?"
28. "What is the most significant revelation you're leaving this Stack with?"
29. "Single word — how do you feel now compared to when you started?"
30. "What immediate actions are you committed to taking?"

File: journal type `"Rage"`

---

## GUILT STACK
1. "What are you titling this Guilt Stack? Which domain — BODY, BEING, BALANCE, or BUSINESS?"
2. "What did you say or do that's triggering this guilt?"
3. "Why has this triggered guilt for you in this moment?"
4. "If you could say anything to yourself right now with no filter — what would you say?"
5. "What story are you telling yourself about what happened?"
6. "Single-word feelings when you tell yourself that story?"
7. "What thoughts and actions does that story produce?"
8. "What are the non-emotional facts of what actually happened?"
9. "What might be possible if you stopped beating yourself up and simply took 100% accountability and committed to moving forward?"
10. "What do you truly want for yourself going forward?"
11. "What do you want for the person or situation involved?"
12. "What do you want for both of you?"
13. "Does the original story get you what you want?"
14. "Who are you choosing to be? What is your new DESIRED STORY?"
15. "What evidence proves this desired story is already true?"
16. "Why has this guilt been extremely positive?"
17. "What is the singular lesson on life you're taking from this Stack?"
18. "What is the most significant revelation you're leaving with?"
19. "Single word — how do you feel now vs. when you started?"
20. "What immediate actions are you committed to taking?"

File: journal type `"Guilt"`

---

## GRATITUDE STACK
1. "What are you titling this Gratitude Stack? Which domain — BODY, BEING, BALANCE, or BUSINESS?"
2. "Who or what has triggered this gratitude?"
3. "Why has this triggered gratitude in you? What specifically happened?"
4. "What story are you telling yourself about this?"
5. "Single-word feelings when you tell yourself that story?"
6. "What thoughts and actions arise?"
7. "What are the objective facts that created this outcome — who did what, what actually happened?"
8. "What do you truly want for yourself in and beyond this?"
9. "What do you want for it or them?"
10. "What do you want for both?"
11. "Why has this gratitude trigger been extremely positive?"
12. "What is the singular lesson on life you're taking from this Stack?"
13. "What is the most significant revelation or insight?"
14. "What immediate actions are you committed to taking?"

File: journal type `"Gratitude"`

---

## EXCITEMENT STACK
1. "What are you titling this Excitement Stack? Which domain — BODY, BEING, BALANCE, or BUSINESS?"
2. "Who or what has triggered this excitement?"
3. "Why has this triggered excitement in you right now?"
4. "What story are you telling yourself about this?"
5. "Single-word feelings when you tell yourself that story?"
6. "What thoughts and actions arise?"
7. "What evidence proves this story is absolutely true?"
8. "What are the non-emotional facts that created this excitement?"
9. "What do you truly want for yourself in and beyond this?"
10. "What do you want for it or them in and beyond this?"
11. "What do you want for both in and beyond this?"
12. "Why has this excitement trigger been extremely positive?"
13. "What is the singular lesson on life you're taking from this Stack?"
14. "What is the most significant revelation or insight?"
15. "What immediate actions are you committed to taking to keep this momentum?"

File: journal type `"Excitement"`

---

## DISCOVERY STACK
1. "What are you titling this Discovery Stack? Which domain — BODY, BEING, BALANCE, or BUSINESS?"
2. "Who or what has activated this discovery or insight in you?"
3. "What has this discovery activated in you? Describe the full thought or realization."
4. "What story are you telling yourself about this discovery?"
5. "Single-word feelings when you tell yourself that story?"
6. "What thoughts and actions arise from that story?"
7. "If this discovery is acted on — what are the positive benefits to your world and those around you?"
8. "If this discovery is NOT acted on — what are the possible negative consequences?"
9. "What is the first measurable FACT that makes this real and actionable? Why is it significant? Give it a title."
10. "What is the second measurable FACT? Why significant? Title it."
11. "What is the third measurable FACT? Why significant? Title it."
12. "Why has this discovery been extremely positive?"
13. "What is the singular lesson on life you're taking from this Stack?"
14. "How does this lesson apply across BODY, BEING, BALANCE, and BUSINESS?"
15. "What is the most significant revelation or insight you're leaving with?"
16. "What immediate actions are you committed to taking?"

File: journal type `"Discovery/Idea Log"`

---

## WAR STACK

### Opening
1. "What are you going to title this WAR Stack?"
2. "Which domain of the CORE 4 — Business, Body, Being, or Balance?"
3. "Who or what are you stacking?"

### Activation
4. "In this moment, what idea has [subject] activated in you?"
5. "What's the story you're telling yourself about this idea?"
6. "Single-word feelings — what comes up when you tell yourself that story?"
7. "What specific thoughts and actions arise when you tell yourself that story?"

### Stakes
8. "If you execute on this idea, what are the positive benefits — to you and those connected to you?"
9. "If you don't execute on it, what are the possible negative side effects?"

### Facts Loop
Introduce once: "Now let's work through the measurable facts. We'll go through as many as you have. First fact:"

For each Fact, ask in sequence:
- "What is this measurable fact?"
- "Why is this fact significant to you?"
- "What obstacles can you see getting in the way of accomplishing this fact?"
- "What actions can you take to overcome those obstacles — and who else is involved?"
- "Give this fact a simple title."

After each Fact: "Is there another fact to work through, or are you ready to close out?"
Loop if yes. Continue to Closing if no.

### Closing
10. "Stepping back from this whole stack — why has this productive idea been extremely positive?"
11. "What is the singular lesson on life you're taking from this stack?"
12. "What is the most significant revelation or insight you're leaving with — and why?"
13. "What immediate actions are you committed to taking leaving this stack?"

Close with: "Stack complete. Go act on it."

File: journal type `"WAR Stack"`

---

## CASH STACK

### Opening
1. "What are you going to title this Cash Stack?"
2. "Who or what are you stacking?"
3. "Why has [subject] triggered you to complete this Cash Stack right now?"

### Story & Feelings
4. "What's the story you're telling yourself about [subject] and the cash situation?"
5. "Single-word feelings — what comes up when you tell yourself that story?"
6. "What specific thoughts and actions arise when you tell yourself that story?"

### Reality Check
7. "What evidence do you have to support that story as absolutely true?"
8. "What are the non-emotional facts about this situation?"

### Reframe
9. "Regardless of the trigger and the original story — what do you truly want for yourself in and beyond this situation?"
10. "What obstacles can you clearly see in the way of pulling that off?"
11. "What actions have you already taken to address this cash issue — and what were the results?"
12. "What do you know must change inside this cash situation for you to get what you want?"
13. "What new story do you know you must create to assure you get what you want?"

### Closing
14. "Stepping back — why has this Cash Stack been extremely positive?"
15. "What is the singular lesson on life you're taking from this stack?"
16. "What is the most significant revelation or insight you're leaving with — and why?"
17. "What immediate actions are you committed to taking leaving this stack?"

Close with: "Stack complete. Go act on it."

File: journal type `"Cash Stack"`

---

## STRATEGIC VISION
1. "Where are the businesses right now — honestly, without spin?"
2. "What does success look like in 12 months across BUILT, CRF, Formation, and SP?"
3. "What is the single biggest constraint holding you back right now?"
4. "What are the 3 highest-leverage moves in the next 30 days?"
5. "What will you start, stop, or double down on?"

File: journal type `"Discovery/Idea Log"`

---

## PROJECT DEBRIEF
1. "Which project are you debriefing?"
2. "What was the outcome? What went well?"
3. "What went wrong or fell short? Be honest."
4. "What will you do differently next time?"
5. "What specific follow-up actions does this create?"

File: journal type `"Discovery/Idea Log"`

---

## FREE WRITE
1. "What's on your mind? Write freely — no filter."
2. "What's underneath that? What are you not saying?"
3. "If you had to be completely honest with yourself right now, what would you say?"

File: journal type `"Morning Ritual"`

---

---

# FILING

All records file to:
**App ID:** `68f8f8fe3757414d70d94ae0`

**Standard field mapping for stacks:**

| Field | Slug | Content |
|---|---|---|
| Title | `title` | "[Stack Type] — [Today's Date]" |
| Journal Type | `sa4f36f00f` | See table below |
| Journal Date | `s5293705a7` | Today's date |
| Complete Journal | `sc347cd0b9` | Full Q&A compiled |

**Morning Ritual and Evening Ritual use full field mappings** (see Protocol 1 and Protocol 2 sections above).

**Journal Type Values:**

| Protocol | `sa4f36f00f` value |
|---|---|
| Morning Ritual | "Morning Ritual" (slug: `WRgYm`) |
| Evening Ritual | `"Vs3w6"` |
| Weekly Review | "Weekly Ritual" (slug: `1oRGK`) |
| Irritation Stack | "Irritation Stack" (slug: `CjS40`) |
| Anger Stack | "Anger" |
| Rage Stack | "Rage" |
| Guilt Stack | "Guilt" |
| Gratitude Stack | "Gratitude" |
| Excitement Stack | "Excitement" |
| Discovery Stack | "Discovery/Idea Log" |
| WAR Stack | "WAR Stack" |
| Cash Stack | "Cash Stack" |
| Strategic Vision | "Discovery/Idea Log" |
| Project Debrief | "Discovery/Idea Log" |
| Free Write | "Morning Ritual" |

---

## Notes
- Always one question at a time
- Accept short answers; don't probe unless stack is indicated
- Scores are 1–5 with optional notes
- Do not read back answers unless asked
- After filing, end cleanly — no follow-up questions
- **Never narrate filing without executing the actual tool call**
- Evening Ritual stats logging requires Stats table App ID and field slugs — **⚠️ confirm with Clint before activating that section**
