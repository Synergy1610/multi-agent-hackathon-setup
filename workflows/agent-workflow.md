# Agent Workflow

How three independent OpenCode agents, on three separate machines, build one application together.

The core constraint: **agents share no memory, no conversation, and no local files.** They share only this GitHub repository. Anything not committed or sent to the Team Lead does not exist for the team.

## Roles in the loop

```text
        ┌───────────────────────────────┐
        │  GitHub repo + shared docs    │
        └───────────┬───────────────────┘
                    │
     ┌──────────────┼──────────────┐
     │              │              │
  Team Lead      Veer (UI)     Lavesh (API)
  plans,         builds,       builds,
  reviews,       tests,        tests,
  integrates     pushes        pushes
     │              │              │
     └──────────────┴──────────────┘
            all communication
            goes through the
            Team Lead + repo
```

The agents do not message each other. The Team Lead is the message bus: it approves contract changes, assigns follow-up work, and resolves conflicts.

## 1. Preparation (Team Lead)

1. Create the project repository from the template and clone it.
2. Establish `docs/PROJECT_SPEC.md` with the interview skill.
3. Finalise `docs/ARCHITECTURE.md` and `docs/API_CONTRACT.md`.
4. Break the work into tasks in `docs/TASKS.md`, with owners, priorities, dependencies, and completion criteria.
5. Adapt the developer role documents to this project's actual split.
6. Commit and push the setup to `main`.
7. Send each developer their repository URL and their task IDs.

**Exit condition:** architecture and contract are frozen enough to build against, and every developer has at least one unblocked task.

## 2. Development (developers)

1. Clone the shared repository (once per machine).
2. Start an OpenCode session in the repository root.
3. Run the kickoff command with the assigned task IDs:

   ```text
   /start-veer T-003 T-004
   /start-lavesh T-005 T-006
   ```

   Without IDs, the command reads `docs/TASKS.md` and selects that person's `Not Started` or `In Progress` tasks.
4. Read `AGENTS.md`, the role document, and the four files in `docs/`.
5. Create a feature branch from an up-to-date `main`.
6. Implement only the assigned tasks, respecting the API contract and folder structure.
7. Test locally: build, start, and exercise the changed behaviour.
8. Commit, push the branch, and send the Team Lead the report described in `AGENTS.md` §8.

**Do not** merge into `main`, edit shared documents, or start another owner's task. If a task is blocked, report it and pick an unblocked task from your own list.

## 3. Integration (Team Lead)

1. Read the developer's report and check the diff.
2. Verify the branch against `docs/API_CONTRACT.md` and `docs/ARCHITECTURE.md`.
3. Review the code: scope, correctness, error handling, secrets, dependencies.
4. Run the project and test the changed flow yourself.
5. Apply the checklists in `workflows/integration-checklist.md`.
6. Merge into `main` (direct merge or pull request — see `workflows/git-workflow.md`).
7. Update `docs/TASKS.md` statuses.
8. Tell every developer to synchronise before starting new work.

**Exit condition:** `main` builds, starts, and the integrated flow works — not just the individual branches.

## 4. Parallel work without collisions

Three agents editing one codebase will collide unless ownership is explicit.

- One owner per file or folder. Record it in the task description when it is not obvious.
- Frontend owns `frontend/`; backend owns `backend/`; the Team Lead owns `docs/`, deployment configuration, and environment templates.
- Shared files (root configuration, deployment configuration, API contract) change only through the Team Lead.
- `.env.example` may be updated by the developer who introduces a new variable — placeholder values only — and announced in the report.
- If two tasks must touch the same file, sequence them with a dependency instead of running them in parallel.

## 5. What must be communicated

Anything another agent needs to know must reach the repository or the Team Lead:

| Event | How it is shared |
| --- | --- |
| Contract or schema change | Proposed to the Team Lead, approved, written in `docs/API_CONTRACT.md` |
| New environment variable | Added to `docs/ARCHITECTURE.md` and `.env.example` |
| New dependency | Approved by the Team Lead, recorded in the task or architecture |
| Blocker | Reported to the Team Lead immediately; task set to `Blocked` |
| Task completion | Branch pushed, report sent, Team Lead updates `docs/TASKS.md` |
| Bug found in another area | Reported to the Team Lead, who creates a task |

Never rely on "the other agent probably knows". It does not.

## 6. Handling change during the event

A hackathon will invalidate part of the plan. That is normal, but changes flow one way:

1. The Team Lead decides.
2. `docs/` is updated first.
3. Developers are told what changed.
4. Implementation follows.

An agent must never quietly implement a different contract, stack, or scope because the current one was inconvenient.

## 7. Escalation

Stop and ask the Team Lead when:

- A task depends on something that does not exist yet.
- The contract is ambiguous, incomplete, or impossible to implement.
- The work requires touching another owner's files.
- A library, service, or credential is needed.
- Tests or the build fail for reasons outside the task scope.
- The task is much larger than expected.

Stopping early is cheap. Guessing is expensive: it produces code that cannot be integrated.

## 8. End of day

Before the demo freeze:

- Every developer has pushed, reported, and synced.
- The Team Lead has merged everything that belongs in the demo.
- One person has run the full demo flow on the deployed build.
- `docs/TASKS.md` reflects reality.
