# Lab 02 — Worksheet: AI Product Scoping (Vin Smart Future)

---

# Phase 1 — SCAN

### My Problem List:

| # | Subsidiary | Lens | Brief Problem Description |
|---|---|---|---|
| 1 | **Vinmec** | Time-consuming tasks | Doctor spends 20–30 min/patient manually writing discharge summaries from electronic medical records — each ward handles 15–20 discharges per day. |
| 2 | **Xanh SM** | Repetitive tasks | Dispatchers manually look up available charging stations and compose routing messages for drivers during battery emergencies — ~80 incidents per day in Hanoi, taking 12–15 min each. |
| 3 | **Vinhomes** | Potential AI-upgrade | Resident complaint tickets, such aswater outage, broken elevator, and noise, are manually classified and routed via email — average resonse time: 12 hours/ticket. |
| 4 | **VinFast** | Stakeholder Pains | Customers describe car symptoms in natural language (for example: *"braking makes a clicking sound"*) — technicians have to manually look up the fault's codes and call back, wasting 10–15 min before scheduling a maintance session. |
| 5 | **Vinpearl** | Time-consuming tasks | Staff manually process group booking emails from travel agencies — each email contains 10–30 room requests and takes the staff 25–40 minutes to check availability and draft a booking order. |

---

# Phase 2 — QUICK-ASSESS 

> **Selected problems:** #1 (Vinmec), #2 (Xanh SM), #3 (Vinhomes)

---

## Quick Problem Card #1 — Vinmec: Discharge Summary Drafting

```
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #1                                       │
│                                                             │
│ Problem: Vinmec doctors spend 20–30 min/patient manually    │
│ writing discharge summaries from electronic medical         │
│ records, lab results, and clinical notes.                   │
│                                                             │
│ Subsidiary: [x] Vinmec                                      │
│                                                             │
│ Who is in pain (Actor)?                                     │
│   Attending physicians (overloaded, frequently complain),   │
│   Patients (waiting for paperwork before leaving).          │
│                                                             │
│ Current manual workflow (5 steps):                          │
│   1. Doctor opens the electronic medical record (EMR)       │
│   ──> 2. Reviews all clinical notes, lab results, imaging   │
│   ──> 3. Manually writes discharge summary in Word/HIS      │
│   ──> 4. Department head reviews and signs off              │
│   ──> 5. Document is printed and handed to patient          │
│                                                             │
│ Slowest / most error-prone step?                            │
│   Steps 2–3 (20–25 min/case)                             │
│   — Doctor must consolidate scattered data from multiple    │
│     sources; information is often missed or inconsistent.   │
│                                                             │
│ Where can AI step in?                                       │
│   Steps 2–3: Auto-extract key clinical data from EMR        │
│   and draft a patient-friendly discharge summary.           │
│                                                             │
│ Success metric (with numbers)?                              │
│   Reduce drafting time from 25 min ──> under 5 min.         │
│   ≥ 85% of AI drafts approved by doctors with no major      │
│   edits required.                                           │
│                                                             │
│ Quick Architecture: [x] LLM Feature                        │
│   (Structured draft from existing data; doctor reviews)     │
└─────────────────────────────────────────────────────────────┘
```

---

## Quick Problem Card #2 — Xanh SM: Field Battery Emergency Handling

```
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #2                                       │
│                                                             │
│ Problem: When Xanh SM drivers report low battery on the     │
│ road, dispatchers manually look up available charging       │
│ stations and compose routing instructions — ~80 cases/day,  │
│ taking 12–15 min each.                                      │
│                                                             │
│ Subsidiary: [x] Xanh SM (GSM)                              │
│                                                             │
│ Who is in pain (Actor)?                                     │
│   Drivers (stranded on the road, unable to pick up riders), │
│   Dispatchers (manual lookups under high volume pressure).  │
│                                                             │
│ Current manual workflow (5 steps):                          │
│   1. Driver calls dispatch hotline to report low battery    │
│   ──> 2. Dispatcher locates vehicle GPS on internal map     │
│   ──> 3. Manually checks VinFast stations for open slots    │
│   ──> 4. Composes routing message and sends via Driver App  │
│   ──> 5. Calls roadside rescue if battery is below 5%       │
│                                                             │
│ Slowest / most error-prone step?                            │
│   Steps 3–4 (10–12 min/case)                             │
│   — Risk of recommending wrong charger type (CCS2 vs GBT)  │
│     for the specific vehicle model (VF5 / VF8 / VF9).      │
│                                                             │
│ Where can AI step in?                                       │
│   Steps 3–4: Auto-pull vehicle GPS + available stations,    │
│   draft a clear Vietnamese routing message for the driver.  │
│                                                             │
│ Success metric (with numbers)?                              │
│   Reduce handling time from 15 min ──> under 3 min.         │
│   ≥ 98% of guidance matches the correct charger type        │
│   for the vehicle model.                                    │
│                                                             │
│ Quick Architecture: [x] LLM Feature                        │
│   (Fixed workflow, high risk if AI sends without approval)  │
└─────────────────────────────────────────────────────────────┘
```

---

## Quick Problem Card #3 — Vinhomes: Resident Complaint Classification & Routing

```
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #3                                       │
│                                                             │
│ Problem: Resident complaints submitted via the Vinhomes     │
│ Resident App are manually classified and routed to the      │
│ correct building management team — average response time    │
│ is 12 hours/ticket, with 50–80 tickets/day.                 │
│                                                             │
│ Subsidiary: [x] Vinhomes                                    │
│                                                             │
│ Who is in pain (Actor)?                                     │
│   Customer service staff (manual triage, frequent misroute),│
│   Residents (long wait times, complaints on social media).  │
│                                                             │
│ Current manual workflow (4 steps):                          │
│   1. Resident submits complaint via Vinhomes Resident App   │
│   ──> 2. CS staff reads and manually classifies the ticket  │
│   ──> 3. Forwards email to the correct building mgmt team   │
│   ──> 4. Team receives and begins handling                  │
│                                                             │
│ Slowest / most error-prone step?                            │
│   Steps 2–3 (8–10 min/ticket, backlog of 50–80/day)      │
│   — Easy to misroute: technical vs. cleaning vs. security.  │
│                                                             │
│ Where can AI step in?                                       │
│   Steps 2–3: Auto-classify complaint category + route to    │
│   the correct team, with priority tag (urgent / standard).  │
│                                                             │
│ Success metric (with numbers)?                              │
│   Reduce response time from 12 hours ──> under 30 minutes. │
│   ≥ 90% classification accuracy to the correct department. │
│                                                             │
│ Quick Architecture: [x] LLM Feature                        │
│   (Text classification + routing; no autonomous agent)      │
└─────────────────────────────────────────────────────────────┘
```

---

## 🗳️ Recommended Problem for Phase 3 Deep-Dive

| Card | Strengths | Watch out for |
|---|---|---|
| **#1 Vinmec** | Clear metric, high time savings for doctors | Medical data is sensitive — HITL is mandatory, AI must never auto-send |
| **#2 Xanh SM** | Fixed workflow, easy to prototype, worked example available | Needs mock GPS + charging station API for testing |
| **#3 Vinhomes** | Simple text input, easiest to test immediately with an LLM | Requires a clear complaint taxonomy before building |

> **Recommendation:** If your team wants to build and test the Phase 4 prototype quickly, **Card #3 (Vinhomes)** is the easiest — it only needs Vietnamese text input and no complex mock APIs.