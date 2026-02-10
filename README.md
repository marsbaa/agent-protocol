# Agent Protocol

This repository defines the **canonical workflow and artifacts** for agent-driven development.

It is **not application code**.
It is a **process contract** that standardises how design intent is frozen, specified, decomposed, and executed by humans and AI agents.

If there is a conflict between conversation, memory, or assumptions and the contents of this repository, **this repository wins**.

---

## Purpose

The Agent Protocol exists to:

* Prevent agent reinterpretation of intent
* Make work repeatable and reviewable
* Enable parallel execution safely
* Separate *thinking* from *doing*
* Treat artifacts as the source of truth

This protocol supports workflows ranging from:

* single-developer + local scripts
  to
* multi-agent, remote, cloud-orchestrated execution

---

## Core Principle

> **Artifacts are the source of truth. Conversations are not.**

All durable decisions must be captured as files inside `.agent/`.

---

## Folder Structure

This repository is intended to be mounted into a project at:

```
.agent/
```

Expected structure:

```
.agent/
├─ README.md                ← this file (rules & contract)
├─ design/
│  ├─ design_final.png
│  ├─ design_constraints.md
│  ├─ design_decisions.md
│  └─ FROZEN.md
├─ spec/
│  ├─ spec.md
│  └─ FROZEN.md
├─ tasks/
│  ├─ tasks.json
│  ├─ task_001_*.md
│  ├─ task_002_*.md
│  └─ FROZEN.md
├─ runs/                    ← optional snapshots of executions
└─ checklist.md
```

Not all folders must exist on day one, but **once frozen, they are authoritative**.

---

## Phase Model (Mandatory)

Work progresses strictly through these phases:

### 1. Design Phase

Artifacts:

* `design_final.png`
* `design_constraints.md`
* `design_decisions.md`

Purpose:

* Capture *intent*, not implementation
* Make tradeoffs explicit
* Lock UX / flow / structure

Once frozen, **design may not be reinterpreted**.

---

### 2. Specification Phase

Artifacts:

* `spec.md`

Must explicitly define:

* Goals
* Non-goals
* Invariants
* Acceptance criteria

No tasks may be considered final until the spec is frozen.

---

### 3. Task Decomposition Phase

Artifacts:

* `tasks.json` (task graph / DAG)
* `task_XXX_*.md` (one file per task)

Each task **must** define:

* Scope
* File boundaries
* Dependencies
* Acceptance criteria
* Definition of done

Tasks are **instructions**, not suggestions.

---

### 4. Execution Phase

* Tasks are executed by humans or agents
* Agents must cite the task files they followed
* No reinterpretation of design or spec is allowed

---

### 5. Review & Merge Phase

* Outputs are reviewed **against artifacts**
* Failing acceptance criteria is a hard stop
* Do not silently “fix” agent output

---

## Freezing Rules

Each phase folder contains a `FROZEN.md`.

A folder is considered **frozen** when:

* `FROZEN.md` exists
* Its contents reflect the agreed intent

### Changing frozen content requires:

1. Updating the relevant artifact
2. Recording the reason in the file
3. Re-freezing

Silent edits are forbidden.

---

## Anti-Drift Rules (Very Important)

* Agents **must not infer intent**
* Agents **must not fill gaps creatively**
* If something is unclear, it is a **spec failure**, not an agent problem
* All execution must reference artifact filenames

If an agent output contradicts `.agent/`, the output is wrong.

---

## Runs & Snapshots (Optional but Recommended)

The `runs/` folder may contain snapshots of execution attempts:

```
runs/
└─ 2026-02-10_1530/
   ├─ design/
   ├─ spec/
   ├─ tasks/
   └─ notes.md
```

This enables:

* Reproducibility
* Diffing intent changes
* Safe iteration

---

## What This Repo Is NOT

* ❌ A codebase
* ❌ A task tracker
* ❌ A conversation log
* ❌ A place for ad-hoc notes

It is a **contract**.

---

## Versioning

This repository is versioned.

Projects should pin a specific version (e.g. via git submodule tag).
Upgrading the protocol is an explicit decision, not an automatic one.

---

## Enforcement

Any tool, script, or agent consuming this repository must:

* Treat it as authoritative
* Fail loudly on violations
* Prefer boring, deterministic behavior

If this protocol feels strict, that is intentional.

---

## Final Note

> The goal is not speed.
> The goal is **controlled parallelism without loss of intent**.

If you break the protocol, you may move faster —
but you will not be building the same thing anymore.
