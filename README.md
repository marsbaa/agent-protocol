# Agent Protocol v0.1

## Philosophy

AI agents reinterpret intent because conversations are ephemeral. This causes implementation drift and unpredictable outcomes. **Files must be the canonical source of truth.** Agents must not reinterpret frozen artifacts under any circumstances.

This protocol enforces a **strict, non-negotiable** artifact-driven workflow that eliminates ambiguity and ensures deterministic outcomes.

## Version

**Protocol Version**: v0.1

**Verification Mode**: Human-executed with manual verification. No automation or scripts are assumed in v0.1.

## Five-Phase Workflow

This protocol enforces a gated, strictly-ordered workflow:

1. **Design** → Define context, goals, decisions, and trade-offs
2. **Specification** → Define requirements and scenarios
3. **Task Decomposition** → Break work into numbered, checkboxed tasks
4. **Execution** → Implement tasks exactly as specified
5. **Review** → Validate outputs against frozen artifacts

**FORBIDDEN**: Starting phase N+1 before phase N is frozen.

## Phase Definitions

Each phase has mandatory inputs, outputs, exit conditions, and failure conditions. Phase N+1 cannot start until phase N is frozen.

### Design Phase

**Inputs**:
- Problem statement or change request
- Existing system context (if applicable)

**Outputs**:
- Design artifact with mandatory sections:
  - Context (problem description, background)
  - Goals (what will change)
  - Non-Goals (explicit scope boundaries)
  - Decisions (design choices made)
  - Risks/Trade-offs (known limitations)

**Exit Conditions**:
- All mandatory sections present
- Design artifact frozen (FROZEN.md created)

**Failure Conditions**:
- Missing mandatory sections
- Ambiguous goals or scope
- Contradictions between sections

### Specification Phase

**Inputs**:
- Frozen design artifact

**Outputs**:
- Specification artifact with mandatory sections:
  - Requirements (SHALL/MUST statements)
  - Scenarios (concrete examples of expected behavior)

**Exit Conditions**:
- All mandatory sections present
- Requirements are unambiguous and testable
- Specification artifact frozen (FROZEN.md created)

**Failure Conditions**:
- Missing mandatory sections
- Ambiguous or untestable requirements
- Contradictions with frozen design artifact

### Task Decomposition Phase

**Inputs**:
- Frozen design artifact
- Frozen specification artifact

**Outputs**:
- Task artifact with mandatory structure:
  - Numbered task groups (1, 2, 3...)
  - Checkboxed sub-tasks (X.Y format)
  - Dependency annotations where applicable

**Exit Conditions**:
- All tasks numbered and checkboxed
- Tasks cover all requirements from specification
- Task artifact frozen (FROZEN.md created)

**Failure Conditions**:
- Missing tasks for specification requirements
- Ambiguous task descriptions
- Contradictions with frozen design or specification

### Execution Phase

**Inputs**:
- Frozen design artifact
- Frozen specification artifact
- Frozen task artifact
- Completed preflight checklist

**Outputs**:
- Completed task checkboxes
- Implementation artifacts (code, configuration, etc.)
- Execution log or summary

**Exit Conditions**:
- All tasks checked as complete
- Implementation satisfies specification requirements
- No deviations from frozen artifacts

**Failure Conditions**:
- Incomplete tasks
- Implementation contradicts frozen artifacts
- Deviation from task instructions without unfreeze

### Review Phase

**Inputs**:
- All frozen artifacts (design, specification, tasks)
- Execution outputs

**Outputs**:
- Verification report
- Pass/fail determination
- List of deviations (if any)

**Exit Conditions**:
- Execution outputs match specification requirements
- All tasks verified as complete
- No contradictions with frozen artifacts

**Failure Conditions**:
- Execution outputs contradict frozen artifacts
- Incomplete task execution
- Ambiguity in verification results

## Authority Hierarchy

When artifacts conflict, the following hierarchy applies (highest to lowest authority):

1. **Protocol rules** (this specification)
2. **Frozen design artifacts**
3. **Frozen specification artifacts**
4. **Frozen task artifacts**
5. **Execution output**
6. **Conversation** (non-authoritative)

**If lower-order artifacts contradict higher-order artifacts, execution is invalid.**

Agents **MUST** halt when contradictions are detected. Resolution requires unfreezing and correcting the lower-authority artifact.

## Freeze Mechanism

Freezing makes artifacts immutable, authoritative, and non-reinterpretable. Frozen artifacts cannot be modified or reinterpreted without explicit unfreeze.

### FROZEN.md Marker File

Each frozen artifact directory must contain a `FROZEN.md` file with the following required fields:

```markdown
# Frozen Artifact

**Timestamp**: YYYY-MM-DD HH:MM:SS UTC

**Frozen By**: [Agent ID or Human Name]

**Artifacts**:
- artifact1.md
- artifact2.md
- ...

**Rationale**: [Brief explanation of why this artifact is frozen]
```

**Required Fields**:
- **Timestamp**: ISO 8601 format, UTC timezone
- **Frozen By**: Identity of agent or human who froze the artifact
- **Artifacts**: Complete list of files covered by this freeze
- **Rationale**: Justification for freezing (e.g., "Design phase complete", "Specification approved")

**Omission of any required field invalidates the freeze.**

### Freeze Procedure

1. Verify artifact completeness (all mandatory sections present)
2. Create `FROZEN.md` in the artifact directory with all required fields
3. Commit freeze marker to version control (if applicable)
4. Announce freeze to all stakeholders

**Frozen artifacts become authoritative and non-reinterpretable.**

### Forbidden Actions on Frozen Artifacts

The following actions are **FORBIDDEN** on frozen artifacts:

1. **Modification**: Editing, appending, or deleting content
2. **Reinterpretation**: Inferring new meaning or intent beyond literal text
3. **Creative Deviation**: Implementing "spirit" rather than "letter" of frozen text
4. **Implicit Unfreeze**: Treating frozen artifacts as mutable without explicit unfreeze

**Violation of any forbidden action invalidates all subsequent work.**

### Unfreeze Procedure

Unfreezing requires explicit action and rationale documentation:

1. **Remove FROZEN.md marker** from artifact directory
2. **Document rationale** for unfreeze in commit message or change log:
   - What was wrong with the frozen artifact?
   - What will be corrected?
   - Who authorized the unfreeze?
3. **Make corrections** to the artifact
4. **Re-freeze** using standard freeze procedure

**Unfreeze without documented rationale is forbidden.**

**After unfreeze, all downstream artifacts (higher-numbered phases) must be reviewed for invalidation.**

## Protocol Invariants

These rules are binary, enforceable predicates. Violation triggers immediate consequences.

### 1. Artifacts Override Conversation

**Rule**: Frozen artifacts are authoritative. Conversation is non-authoritative.

**Violation Consequence**: Agent output that contradicts frozen artifacts is rejected.

**Enforcement**: Human verification in v0.1. Agents must cite artifact sources for all decisions.

### 2. Frozen Artifacts Cannot Be Reinterpreted

**Rule**: Frozen artifacts are immutable and non-reinterpretable. Their meaning is fixed at freeze time.

**Violation Consequence**: Execution halts immediately. No work proceeds until artifact is unfrozen and corrected.

**Enforcement**: Agents must refuse to proceed when frozen artifact interpretation is required. Unfreeze requires explicit marker removal + rationale documentation.

### 3. Tasks Are Instructions, Not Suggestions

**Rule**: Task artifacts define mandatory steps. Deviation is forbidden.

**Violation Consequence**: Execution that deviates from tasks is invalid and must be discarded.

**Enforcement**: Human verification compares execution output to task checklist. Incomplete or deviated tasks fail review.

### 4. Ambiguity Is Spec Failure

**Rule**: Ambiguous specifications are invalid. Agents must reject them, not interpret creatively.

**Violation Consequence**: Execution based on ambiguous specs is invalid. Spec must be unfrozen, clarified, and refrozen.

**Enforcement**: Agents must halt and request clarification when encountering ambiguity. Proceeding with assumptions is forbidden.

## Agent Obligations

### Agents MUST:

1. **HALT** execution if any invariant is violated
2. **REJECT** ambiguous specifications rather than infer intent
3. **FORBID** modification of frozen artifacts without explicit unfreeze
4. **BLOCK** phase progression until exit conditions are satisfied
5. **TREAT** all "MUST" statements as hard constraints, not recommendations

### Agents MUST NOT:

1. Reinterpret frozen artifacts under any circumstances
2. Proceed with partial or incomplete phase outputs
3. Bypass preflight checklist validation
4. Treat tasks as suggestions or guidelines
5. Optimize for convenience over protocol compliance

**Violation of any obligation invalidates all subsequent work.**

## Out of Scope

This protocol does **NOT** define:

- Runtime system behavior
- Code architecture decisions
- Tooling implementation details
- Performance optimization strategies
- Language-specific conventions
- Deployment procedures

**Any inference beyond defined artifacts is forbidden.**

Agents **MUST NOT** extrapolate protocol rules to out-of-scope domains.

## Repository Structure

The protocol defines the following mountable directories:

- `design/` - Design artifacts (mountable into `.agent/design/`)
- `spec/` - Specification artifacts (mountable into `.agent/spec/`)
- `tasks/` - Task decomposition artifacts (mountable into `.agent/tasks/`)

This README is mountable into `.agent/README.md` for consumer projects.

## Impact

- **Projects**: Any project importing this protocol **MUST** adopt the strict workflow.

- **Agents**: AI agents **MUST** treat artifacts as authoritative over conversation, halt on frozen artifact violation, and reject ambiguous specs rather than interpret creatively.

- **Developer Experience**: Developers gain deterministic, predictable outcomes. Flexibility is sacrificed for correctness. This is intentional.

## License

See [LICENSE](LICENSE) for details.
