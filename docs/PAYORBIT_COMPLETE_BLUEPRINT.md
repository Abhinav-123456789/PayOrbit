# PayOrbit Blueprint (Rebuilt)

Internal document. No pitch language. No feature shopping list.

## 1. What This Product Actually Is

PayOrbit is a **shared-spend closure app** for people who repeatedly spend together and keep loose mental tabs.

It exists to answer one question fast: **"What is still open between us, and what should be closed now?"**

## 2. Core Use Moment

User opens this app when **they need to close pending shared spends before the next shared plan**.

This is the primary trigger. Not expense logging. Not analytics browsing.

## 3. Core Problems (Refined)

### Problem A: Memory-led money breaks trust
People remember selectively. Small misses become "you always forget" narratives.

### Problem B: UPI and cash create an incomplete ledger
Some spends have digital proof, some are verbal cash claims. Same group, two evidence standards.

### Problem C: Debt reminders create social fatigue
Repeated reminders feel accusatory. People mute or ignore. Then balances become stale.

### Problem D: Unequal capacity creates silent dropout
Equal split looks fair on screen but is unaffordable for some members. They disengage without saying why.

## 4. System Design (Critical)

Only systems below. No generic "add feature" items.

### System 1: Evidence-Weighted Ledger
Solves Problem A + B.

- Input
  - UPI-confirmed transfer references (user-linked)
  - Manually entered expense entries
  - Cash claims from group members
  - Counterparty acknowledgments or disputes

- Processing
  - Assign confidence level per entry: `verified`, `acknowledged`, `unverified`
  - Auto-match likely duplicates (same amount/time/context window)
  - Keep conflict state explicit, never silently overwrite
  - Exclude unverified items from "close now" suggestion until resolved or timeboxed

- Output
  - Ledger row status badge visible to all members
  - "Needs confirmation" queue with one-tap accept/dispute
  - Closure suggestion based only on high-confidence net balances

### System 2: Closure Cycle Engine
Solves Problem C.

- Input
  - Open balances by relationship pair
  - Last action timestamp per pair
  - Upcoming shared-plan marker (trip, rent date, weekly meetup)
  - User reminder preference and response history

- Processing
  - Trigger only near natural closure windows (before next plan), not daily spam
  - Batch all pending items into one closure session per cycle
  - Suppress nudges if counterpart recently engaged
  - Decay reminder frequency after repeated no-response and switch to digest mode

- Output
  - One "Close pending spends" card at the right moment
  - A single closure session: confirm entries -> settle -> done
  - End state signal: "No pending closure for this group"

### System 3: Private Capacity Guardrail
Solves Problem D.

- Input
  - Optional private per-group comfort range (not public)
  - Planned spend estimate for upcoming activity
  - Historic realized spend per member (private + aggregate)

- Processing
  - Compute plan stress score using group aggregate, never exposing individual limits
  - Offer split templates only when stress threshold is crossed
  - Default to neutral language ("high variance risk"), not "X cannot afford"

- Output
  - Organizer sees a planning warning: "Current plan likely to create post-event dues"
  - Suggested lower-risk budget band and split mode
  - Members only see finalized split policy, not private ranges

## 5. Retention Loop

If this loop does not hold, product fails.

**Trigger -> Action -> Reward -> Repeat**

- Trigger: upcoming shared plan or weekly closure window detects open high-confidence dues
- Action: user runs one closure session (confirm disputed items, settle what is clear)
- Reward: social clarity and reduced awkwardness ("nothing pending before we meet")
- Repeat: each new shared plan reintroduces closure need; app becomes pre-plan hygiene

Why users come back weekly:
- Shared spending is recurring for roommates/friend circles
- Closure gives immediate social relief, not abstract reporting
- Session is short and bounded, not endless bookkeeping

## 6. Product Behavior Principles

- Act only when confidence is high or timing is natural.
- Stay silent when data is weak; ask for confirmation instead of guessing.
- Prefer group harmony over strict ledger completeness.
- Resolve ambiguity before suggesting payment.
- Batch friction into one closure moment; avoid continuous micro-interruptions.
- Never expose private affordability signals.

## 7. What We Explicitly Avoid

- No generic AI assistant layer.
- No always-on reminder spam.
- No "track every rupee" mandate.
- No social feed or gamified debt shaming.
- No dependency on scraping every payment notification to function.
- No bloated personal-finance modules unrelated to shared closure.

## 8. Execution Reality

### What is hard to build
- Reliable matching between manual/cash entries and UPI proofs without false merges.
- Multi-party conflict state that stays understandable in UI.
- Timing engine that nudges enough to help, but not enough to be muted.

### What might fail in real usage
- Users may skip confirmation requests if there is no immediate benefit.
- Groups with very low trust will still fight outside the app.
- People may continue settling on UPI directly and forget to close entries.

### Risky assumptions
- Assumption: users will define private comfort ranges. Reality: many will not.
- Assumption: pre-plan closure exists for every group. Reality: some groups are chaotic and ad hoc.
- Assumption: one closure moment is enough. Reality: high-churn groups may need adaptive cadence.

## 9. Minimal Supporting Stack

Keep only what supports the three systems:

- Mobile-first client (React Native) for closure sessions
- API service (Node/Express or equivalent) for ledger, cycle, and guardrail logic
- Postgres for ledger states, confirmations, and closure cycles
- Optional cache/queue for timing jobs and digest delivery

No additional infra decisions in this blueprint.

## ⚡ Product Core & Execution Layer

### Product Core
This product is used when **a user wants to clear pending shared spends before the next shared plan**.

### Retention Loop
Trigger (upcoming plan + open dues) -> Action (run closure session) -> Reward (social clarity, no awkward pending money) -> Repeat (next plan creates next closure need).

### System Designs
- **Evidence-Weighted Ledger:** confidence-tagged entries, confirmation queue, high-confidence closure output.
- **Closure Cycle Engine:** timing-aware batch closure sessions, reminder suppression, clean "all clear" state.
- **Private Capacity Guardrail:** private affordability input, stress scoring, non-exposing split guidance.

### New Product Framing
PayOrbit is **the app you open before meeting your people, so shared money is already clear**.

It is a **closure tool**, not a tracking app.

### Reality Check
- Users will ignore anything that feels like extra admin work.
- If closure sessions are not faster than "just send UPI and forget," adoption drops.
- Without clear value in disputed cash flows, confirmation requests will decay.
