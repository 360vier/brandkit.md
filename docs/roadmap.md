# Roadmap

## Prioritized implementation phases

### Phase 1 - sharpen structure

- Normalize repository structure around `SPEC`, `EXAMPLES`, `docs`, and `schema`.
- Mark legacy files clearly as deprecated.
- **Acceptance criterion:** external contributors can find definition, rules, and examples in under five minutes.

### Phase 2 - finalize specification

- Fully anchor normative language (`MUST`, `SHOULD`, `MAY`) in `SPEC.md`.
- Finalize required/optional sections and core fields.
- **Acceptance criterion:** a basic validator draft can be derived directly from `SPEC.md`.

### Phase 3 - expand examples

- Provide distinct profiles for startup, enterprise, personal brand, and minimal baseline.
- Document real trade-offs, not cosmetic differences.
- **Acceptance criterion:** all examples can be checked against required fields.

### Phase 4 - docs and boundaries

- Align FAQ, principles, and comparison docs with adoption needs and common misunderstandings.
- **Acceptance criterion:** typical scope and boundary questions are answerable without extra context.

### Phase 5 - prepare v2

- Standardize field mapping and source attribution for expression candidates.
- Define actionable backlog for generator and validator tooling.
- **Acceptance criterion:** v2 work items are immediately implementable without blocking v0.x.

## v0.x - sharpen specification

Goal: establish a lean, credible normative baseline.

- stabilize required/optional structure
- formalize brand-expression rules
- document parser-oriented field definitions
- provide example profiles for real usage scenarios

## v1.0 - Stabilisierung

Goal: provide a reliable base for broader adoption.

- stable field names and section titles
- explicit compatibility guarantees
- clearer validation rules
- maintained breaking-change process

## v2 - Companion Tooling

Goal: translate the specification into practical tooling.

- validator for required sections and core fields
- generator for brand-expression candidates
- parser helper for structured downstream use

### Planned v2 generator capabilities

- proposals for `emoji_signature`
- proposals for `ascii_mark`
- proposals for `terminal_banner`
- proposals for reduced text-based wordmarks

Note: tool output is proposal-grade. Final approval remains human-curated.
