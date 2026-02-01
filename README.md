# lovable-incident-reports
Infinite incident reports from building a multi-tenant financial app with Lovable
# Dignus – Lovable Incident Reports (Financial / Multi‑Tenant App)

This repository documents a series of **critical incidents** encountered while building a multi‑tenant financial application called **Dignus** using **Lovable**.

The project has been in development for **several months**, during which I have spent **many hours and a significant amount of paid credits** fixing bugs introduced or suggested by the tool itself. These issues include:

- Repeated regressions in previously working features.
- Multi‑tenancy isolation problems (`home_id` missing or misused).
- Incorrect financial summaries and month‑end closings.
- Unstable behaviour in critical flows such as “Create New Month”.

In this repo I am **only documenting the incidents that occurred today** (one single day), to illustrate the density and severity of errors that still appear after months of work. The goal is not to list every historical problem, but to show that even at this late stage the system continues to generate serious issues in core financial functionality.

---

## Scope of This Repository

- **What is included here**
  - Detailed incident reports from **today’s session only** (date: YYYY‑MM‑DD).
  - Each report focuses on a concrete error, its root cause and the proposed fix.
  - Incidents mostly affect **financial logic**, **multi‑tenancy**, and **month management** (closing and creating accounting periods).

- **What is *not* fully included here**
  - All past incidents from previous months (there have been many).
  - All the time and credits spent trying to stabilize the app before today.
  - The complete codebase of Dignus (only snippets as needed for context).

Even though this repo covers just one day of work, it already contains **multiple critical incidents**, which should give an idea of the ongoing instability.

---

## Context: The Dignus App

- Multi‑tenant financial system for several residential homes.  
- Each **home** must have completely isolated data (`home_id`‑based separation).  
- Core features:
  - Month‑end closing.
  - Budgets and variances.
  - Financial summaries per residential home.
  - Creation of new accounting months.

This type of software requires a **very high level of data integrity and predictability**. Any mix of data between homes or incorrect month totals makes the system unusable as a reliable financial tool.

---

## Today’s Incident Reports

All detailed reports are in the `incidents/` folder.

Examples of what is documented (adapt the filenames to what you create):

- `incidents/INCIDENT_04_month_end_closing_multi_tenancy.md`  
  - Month‑end summary combining transactions from **all homes** instead of the selected one (missing `home_id` filters in queries).
- `incidents/INCIDENT_05_close_month_rpc_multi_tenancy.md`  
  - PostgreSQL RPC `close_month` implemented without `home_id` parameter, wrong `ON CONFLICT` key, and no filtering by home, making month closing incorrect or impossible.
- `incidents/INCIDENT_07_create_new_month_hooks_and_role_loading.md`  
  - React component `CreateNewMonth.tsx` violating hooks rules (early return before `useEffect`) and mishandling role loading, causing unpredictable redirects and broken “Create Month” flow.
- `incidents/INCIDENT_08_create_new_month_loading_and_fake_months.md`  
  - “Create New Month” showing months without valid data and going into a loading state that returns to the dashboard without clearly asking which month to create or checking if it already exists.

> **Important:** All of these incidents happened **on the same day**, after months of previous work and fixes.

---

## Why I Am Publishing This

- To provide a **concrete, technical record** of the kinds of issues I am still seeing in a Lovable‑generated financial app after months of development.
- To help other developers evaluate whether Lovable is suitable for **financial / multi‑tenant use cases**, given the risks around:
  - Data isolation (`home_id`)  
  - Month‑end closing logic  
  - Reliability of financial reports
- To give Lovable a reproducible case they can use to improve:
  - Their templates and internal patterns for multi‑tenant apps.
  - Their safeguards around financial correctness.
  - Their handling of regressions that consume user credits.

---

## Notes on Time and Credits

While this repository only covers today’s incidents, it is the result of **months of accumulated work** and **dozens of errors** that had to be corrected along the way.

- Many of these bugs were introduced or suggested by Lovable itself.
- Fixing them required multiple iterations, each one consuming additional **paid credits**.
- In some cases, credits spent to “fix” an issue ended up creating **new bugs** in the same critical area (for example, financial month management).

Because of this, I have currently **paused** using this version of Dignus for any real financial data, and I am evaluating alternative platforms for future development.

---

## Languages

- Incident reports: primarily **English** (so the Lovable team and international developers can read them).  
- This README may include short notes or links in **Spanish** for local context.

## Link to Original Business Rules

The full business rules for Dignus (including Rule 2 – Salary Advances) are defined here:

- https://github.com/hoyyosoy/dignus-app/blob/main/BUSINESS_RULES.md

These rules were clearly documented **before** the incidents described in this repo. The problems shown here are implementation and consistency issues, not a lack of specification.
