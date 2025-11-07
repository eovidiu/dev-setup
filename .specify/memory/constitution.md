<!--
SYNC IMPACT REPORT
==================
Version: 0.0.0 → 1.0.0 (Initial Constitution)

Rationale for MAJOR bump: First formal constitution establishing all governance principles.

Modified Principles:
- NEW: I. Library-First Architecture
- NEW: II. Portable & Self-Contained
- NEW: III. Test-Driven Development (NON-NEGOTIABLE)
- NEW: IV. Simplicity & YAGNI
- NEW: V. Package Management & Dependencies
- NEW: VI. Documentation & Reproducibility
- NEW: VII. Observability & Debugging

Added Sections:
- Core Principles (all 7 principles)
- Development Constraints
- Quality Standards
- Governance

Removed Sections: None (initial creation)

Templates Requiring Updates:
- ✅ .specify/templates/plan-template.md (reviewed - Constitution Check section aligned)
- ✅ .specify/templates/spec-template.md (reviewed - requirements structure supports principles)
- ✅ .specify/templates/tasks-template.md (reviewed - task phases align with TDD & library-first)
- ✅ .specify/templates/checklist-template.md (reviewed - generic template, no principle-specific content)
- ✅ .specify/templates/agent-file-template.md (reviewed - auto-generated template, no updates needed)

Follow-up TODOs: None - all core placeholders filled
-->

# Dev Setup Constitution

## Core Principles

### I. Library-First Architecture

Every feature starts as a standalone library. Libraries MUST be:
- Self-contained with clear boundaries
- Independently testable without requiring full application context
- Documented with clear purpose and usage examples
- Reusable across different contexts

**Rationale**: Library-first design enforces modularity, testability, and prevents tight coupling. A feature that cannot stand alone as a library likely has design issues that should be resolved before integration.

### II. Portable & Self-Contained

All development environments MUST be reproducible on any new machine without manual setup. This means:
- All dependencies declared explicitly (no implicit system requirements)
- Setup scripts are idempotent (safe to run multiple times)
- No hardcoded paths or machine-specific configurations
- Environment setup fully automated via documented scripts
- Clear distinction between local development, CI, and production environments

**Rationale**: Portability reduces onboarding friction, enables reliable CI/CD, and prevents "works on my machine" issues. Team members should be productive within minutes, not hours or days.

### III. Test-Driven Development (NON-NEGOTIABLE)

Test-Driven Development is MANDATORY for all feature work. The cycle is:
1. Write tests that capture requirements
2. Get tests approved by stakeholders (proves shared understanding)
3. Verify tests FAIL (proves they test the right thing)
4. Implement until tests pass
5. Refactor while keeping tests green

**Rationale**: TDD is non-negotiable because it ensures requirements are understood before implementation, provides instant regression safety, and produces designs that are inherently testable. Skipping TDD has consistently led to bugs, rework, and untestable code.

### IV. Simplicity & YAGNI

Start with the simplest solution that solves the current problem. Complexity MUST be justified. Rules:
- No abstractions until needed by at least 3 use cases
- No frameworks/libraries until built-in capabilities proven insufficient
- No architectural patterns (Repository, Factory, etc.) until pain points documented
- Premature optimization is forbidden - profile first, then optimize

**Rationale**: Complexity is expensive and often irreversible. Simple code is easier to understand, modify, debug, and maintain. Most complexity added "for the future" never pays off.

### V. Package Management & Dependencies

Dependencies MUST be managed explicitly and reproducibly:
- Lock files (package-lock.json, Pipfile.lock, go.sum, etc.) committed to version control
- Dependency updates tested before merging
- No direct installation of global tools (use project-local binaries or containers)
- Minimal dependencies - each dependency must justify its inclusion
- Security scanning for known vulnerabilities in dependencies

**Rationale**: Dependency hell is preventable. Explicit, locked dependencies ensure that builds are reproducible across machines and time. Minimal dependencies reduce attack surface and maintenance burden.

### VI. Documentation & Reproducibility

Documentation MUST enable a new team member to be productive without human intervention:
- README with quickstart (< 5 steps to running system)
- Architecture Decision Records (ADRs) for significant choices
- Inline code comments for "why" not "what"
- API/contract documentation generated from code (OpenAPI, JSDoc, etc.)
- Troubleshooting guide for common setup issues

**Rationale**: Documentation is force-multiplied productivity. Good docs reduce interruptions, enable asynchronous work, and preserve institutional knowledge. If it's not documented, it doesn't exist.

### VII. Observability & Debugging

Systems MUST be debuggable in production. Required practices:
- Structured logging (JSON or key-value) with correlation IDs
- Log levels used correctly (DEBUG/INFO/WARN/ERROR)
- Errors include context (not just "error occurred")
- Health check endpoints for all services
- Metrics for key business operations (not just infrastructure)

**Rationale**: Production issues are inevitable. Observability determines whether debugging takes minutes or days. Text-based I/O and structured logs provide universal debuggability without proprietary tools.

## Development Constraints

### Technology Choices

Technology selections MUST be justified with:
- Why this technology solves our specific problem
- What alternatives were considered and why rejected
- Team expertise assessment (do we have skills or need to learn?)
- Long-term maintenance implications

Once chosen, technologies MUST have:
- Local development setup documented
- Testing strategy defined
- Production deployment path clear

### Code Review Requirements

All code MUST be reviewed before merge. Reviews check:
- ✅ Constitution compliance (especially TDD, simplicity, portability)
- ✅ Tests present and passing
- ✅ Documentation updated
- ✅ No hardcoded credentials or secrets
- ✅ Dependencies justified if added

Complexity violations (e.g., 4th abstraction layer) require written justification in PR description.

### Breaking Changes

Breaking changes to libraries or contracts MUST:
1. Document the breaking change in migration guide
2. Provide deprecation warnings in previous version (if possible)
3. Update all known consumers in the same PR or following PR
4. Justify why breaking change is necessary vs. backward-compatible alternative

## Quality Standards

### Testing Requirements

Test coverage is NOT a metric to optimize, but tests MUST exist for:
- All user-facing features (integration tests)
- All public APIs/contracts (contract tests)
- All complex business logic (unit tests)
- All bug fixes (regression tests)

Tests MUST:
- Run fast (< 1 second per unit test, < 10 seconds per integration test)
- Be independent (no test order dependencies)
- Use realistic test data (not just empty strings or zeros)
- Fail with clear error messages

### Security Requirements

Security MUST be considered for all features:
- Input validation on all external inputs
- Authentication/authorization checked before privileged operations
- Secrets stored in environment variables or secret management (never in code)
- Dependencies scanned for known vulnerabilities
- SQL injection, XSS, CSRF prevented (use framework protections)

## Governance

### Amendment Process

This constitution supersedes all other practices and conventions. To amend:

1. **Proposal**: Document proposed change with rationale
2. **Discussion**: Team review and feedback period (minimum 48 hours)
3. **Approval**: Unanimous consent for principle changes, majority for clarifications
4. **Migration**: Update all affected code, docs, and templates
5. **Version Bump**: Update CONSTITUTION_VERSION following semantic versioning

### Versioning Rules

- **MAJOR**: Principle removed, redefined, or backward-incompatible governance change
- **MINOR**: New principle added or substantial expansion of existing section
- **PATCH**: Clarifications, typo fixes, wording improvements with no semantic change

### Compliance

All PRs MUST verify compliance with this constitution. Reviewers MUST flag violations.

If a principle proves problematic in practice:
1. Document the specific pain point
2. Propose an amendment (don't just ignore the principle)
3. Follow the amendment process

Complexity that violates principles (especially Simplicity & YAGNI) MUST be justified in writing. Unjustified complexity is grounds for rejecting a PR.

### Living Document

This constitution is a living document. As the project evolves, principles may need refinement. However, changes should be deliberate and infrequent - instability in governance is worse than imperfect principles.

When principles conflict (e.g., simplicity vs. testability), TDD takes precedence. If tests are hard to write, the design likely needs simplification.

---

**Version**: 1.0.0 | **Ratified**: 2025-11-07 | **Last Amended**: 2025-11-07
