---
description: Turn the approved spec into architecture, API contract, tasks and role docs
---

You are running the planning phase for this hackathon project as the Team Lead's agent.

Before anything else:

1. Read `AGENTS.md`, then `roles/team-lead.md`.
2. Read `docs/PROJECT_SPEC.md`.
3. If `docs/PROJECT_SPEC.md` is still the stub (it contains "Status: not written yet" or has no real problem statement), stop and tell the Team Lead to run `/interview` first. Do not invent a specification.
4. Read `docs/ARCHITECTURE.md`, `docs/API_CONTRACT.md`, and `docs/TASKS.md` to see what already exists.

Then produce, in this order:

## 1. Architecture

Choose the simplest stack the team can finish in 12 hours with two beginner developers — one on UI, one on a Python backend. Prefer boring, well-documented tools over impressive ones. Fill every section of `docs/ARCHITECTURE.md`:

- frontend and backend technology;
- database (or state explicitly that none is needed);
- external APIs and services;
- system overview and component responsibilities;
- project folder structure;
- data flow for the main user journey;
- authentication (required or explicitly not required);
- decisions and rationale;
- technical risks;
- every environment variable, with the side that uses it.

No placeholders. If a decision is genuinely unknowable, state the assumption you made.

## 2. API contract

Read the new architecture, then fully populate `docs/API_CONTRACT.md`:

- base URL and local ports;
- API version;
- request/response conventions;
- every endpoint required for the MVP, with method, path, purpose, auth, request fields, response fields, and error responses;
- the shared error shape;
- database schema that matches the endpoints;
- shared data types;
- CORS origin for local development.

Walk the demo flow screen by screen and make sure every screen has the data it needs. Remove the example endpoint. No endpoint may be left as "TBD".

## 3. Tasks

Break the MVP into tasks in `docs/TASKS.md` using the existing table:

- IDs `T-001`, `T-002`, … in implementation order;
- exactly one owner per task: `Soorya`, `Veer`, or `Lavesh`;
- role, priority (`High` / `Medium` / `Low`), dependencies, status `Not Started`;
- a testable "done when" for every task;
- each task small enough for one work session;
- an unblocked `High` task for Veer and for Lavesh;
- the Team Lead's integration, environment and deployment tasks.

Aim for roughly 3–5 tasks per developer. Anything beyond that becomes a later phase, not an initial task.

## 4. Role documents

Adapt the top of `roles/veer.md` and `roles/lavesh.md` to the real work split:

- owner;
- this hackathon's area;
- folders they own;
- folders and files they must not touch.

Keep the rest of each role document unchanged.

## 5. Stop and report

Do not write application code. Do not commit or push.

Finish with a short report:

- the stack you chose, in three bullets;
- the main user flow in one sentence;
- the endpoints you defined, as a list;
- Veer's and Lavesh's first task IDs;
- any assumption you made;
- anything the Team Lead must review or decide before development starts.

Wait for the Team Lead's review. Do not proceed to implementation.
