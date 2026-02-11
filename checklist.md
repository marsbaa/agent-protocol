# Preflight Checklist

**Mountable Into**: `.agent/checklist.md`

---

## Execution Phase Gate

**CRITICAL**: All items below MUST be checked before entering the Execution phase.

**Incomplete items BLOCK execution phase entry.**

**Bypass or override is FORBIDDEN.**

---

## Design Phase Complete

- [ ] Design artifact exists in `design/` directory
- [ ] Design artifact contains all mandatory sections:
  - [ ] Context (problem description, background)
  - [ ] Goals (what will change)
  - [ ] Non-Goals (explicit scope boundaries)
  - [ ] Decisions (design choices made)
  - [ ] Risks/Trade-offs (known limitations)
- [ ] Design artifact has no contradictions between sections
- [ ] Design artifact has no ambiguous goals or scope
- [ ] `FROZEN.md` exists in `design/` directory with all required fields:
  - [ ] Timestamp (ISO 8601 format, UTC)
  - [ ] Frozen By (agent ID or human name)
  - [ ] Artifacts (complete list of frozen files)
  - [ ] Rationale (justification for freezing)

**Binary Verification**: Design phase is complete if and only if ALL items above are checked.

---

## Specification Phase Complete

- [ ] Specification artifact exists in `spec/` directory
- [ ] Specification artifact contains all mandatory sections:
  - [ ] Requirements (SHALL/MUST statements)
  - [ ] Scenarios (concrete examples of expected behavior)
- [ ] All requirements are unambiguous and testable
- [ ] Specification artifact has no contradictions with frozen design artifact
- [ ] `FROZEN.md` exists in `spec/` directory with all required fields:
  - [ ] Timestamp (ISO 8601 format, UTC)
  - [ ] Frozen By (agent ID or human name)
  - [ ] Artifacts (complete list of frozen files)
  - [ ] Rationale (justification for freezing)

**Binary Verification**: Specification phase is complete if and only if ALL items above are checked.

---

## Tasks Valid and Complete

- [ ] Task artifact exists in `tasks/` directory
- [ ] Task artifact contains mandatory structure:
  - [ ] Numbered task groups (1, 2, 3...)
  - [ ] Checkboxed sub-tasks (X.Y format)
  - [ ] Template version field (v0.1)
- [ ] All tasks are numbered and checkboxed
- [ ] Tasks cover all requirements from frozen specification
- [ ] Task descriptions are unambiguous
- [ ] Task artifact has no contradictions with frozen design or specification
- [ ] `FROZEN.md` exists in `tasks/` directory with all required fields:
  - [ ] Timestamp (ISO 8601 format, UTC)
  - [ ] Frozen By (agent ID or human name)
  - [ ] Artifacts (complete list of frozen files)
  - [ ] Rationale (justification for freezing)

**Binary Verification**: Tasks are valid and complete if and only if ALL items above are checked.

---

## Execution Phase Entry

**RULE**: Execution phase CANNOT start until all three sections above are complete.

**ENFORCEMENT**: 
- Human verification in v0.1
- Agents MUST refuse to execute tasks if any checklist item is unchecked
- Bypass or override is a protocol violation

**CONSEQUENCE**: Execution performed with incomplete checklist is invalid and must be discarded.
