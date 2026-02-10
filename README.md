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

Each phase has:
- **Mandatory inputs** (execution forbidden if missing)
- **Mandatory outputs** (phase incomplete if missing)
- **Binary exit conditions** (true = proceed, false = blocked)
- **Explicit failure conditions** (what triggers rejection)

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
