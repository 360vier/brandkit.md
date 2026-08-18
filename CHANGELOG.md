# Changelog

Changes to the specification and the repository structure. Breaking changes are
labelled as such.

## [0.2.0-draft] — 2026-08-18

**BREAKING.** The data model changed. v0.1 treated a hand-written Markdown file
as the model; v0.2 makes a structured Core document the model and Markdown a
generated rendering. Migration: [`docs/migration.md`](docs/migration.md).

### Added

- **Brandkit Core** — canonical structured model, 17 required root sections,
  `camelCase` field names. Specified in `SPEC.md` §5–§8.
- **`schema/brandkit-v0.2-draft.schema.json`** — JSON Schema 2020-12, generated
  from the reference implementation's definitions. `$id` is
  `https://brandkit.md/schema/brandkit-v0.2-draft.schema.json`.
- **`policies[]`** — machine-evaluable rules with `category`, `severity`,
  `scopes`, `evaluationMethod`, `humanReviewRequired`, `enabled` (§8).
- **`products[]`, `claims[]`, `certifications[].verified`** — proof-before-
  assertion rules, so an unverified standard can be recorded and still be
  unusable in generated content (§6).
- **Design token layer** — DTCG (W3C Design Tokens) with the `md.brandkit`
  extension profile: `order`, `group`, `inputType`, `unit`, `tags`, `options`,
  and per-token `segments` (§9).
- **Segments** — one token foundation with N expressions, declared once at set
  level (§9.4).
- **Rendering contract** — exact structure of the compiled `brandkit.md`, including
  the status-carrying renderings for unverified certifications and placeholder
  products (§7).
- **`design.md`** — token rendering with per-segment values and CSS custom
  properties, `:root` plus `[data-segment]` overrides (§9.5).
- **Interchange rules** — every export carries brand *and* design values; JSON
  envelope `{ brandkit, designTokens }`; an empty token set must never displace a
  real one (§10).
- **Consumption rules for AI systems** (§11) — approval gates generation, hard
  constraints survive truncation, reproducibility via brand version plus token
  version plus context hash, and separation of generation from composition.
- **Conformance classes** — Producer, Renderer, Consumer, Extension (§3.1).
- **Extension mechanism** (§12) with two extensions: `ext/text-expressions.md`,
  `ext/design-rules.md`.
- **`docs/architecture.md`**, **`docs/design-tokens.md`**, **`docs/policies.md`**,
  **`docs/context-compilation.md`** — new documentation.
- **`EXAMPLES/helion-systems.*`** — fictional reference brand: Core, token set
  with two segments, and both compiled renderings.
- **`EXAMPLES/minimal.brandkit.json`** — smallest valid Core document.
- **`templates/tokens-template.json`**, **`templates/authoring.brandkit.md`**.
- Named the **brandkit.md Portal** as the reference implementation.

### Changed

- `SPEC.md` rewritten for the four-layer architecture.
- `README.md` rewritten around the Core, the token layer and enforceability.
- `docs/principles.md`, `docs/comparison.md`, `docs/faq.md`, `docs/roadmap.md`
  rewritten for v0.2.
- `docs/migration.md` now maps v0.1 → v0.2 (the earlier two-file migration note
  moved to `spec/v0.1/migration-from-two-file-model.md`).
- `templates/brandkit-template.json` replaces the blank Markdown starters as the
  primary authoring entry point.
- Examples and renderings are now produced by the reference implementation's own
  compilers and validate against its schema, so the standard and the running tool
  cannot drift apart quietly.

### Moved

- v0.1 archived under `spec/v0.1/` — specification, field map, four example
  profiles, Markdown templates, and the legacy `brand.md` / `styles.md` notices.

### Removed from the Core

- Text-only brand expressions (`fallback_wordmark`, `plain_text_signature`,
  `ascii_mark`, `terminal_banner`, `emoji_signature`, `cli_prompt_style`) — moved
  to `ext/text-expressions.md`, unchanged in substance.
- The v0.1 required sections as such; they are absorbed into the Core's
  structured sections (`docs/migration.md` has the mapping).
- `snake_case` field labels, in favour of `camelCase` JSON keys.

## [0.1.0-draft] — 2026-03-14

### Added

- `SPEC.md` as the normative v0.1 draft specification
- `EXAMPLES/` with four reference profiles
- `docs/principles.md`, `docs/faq.md`, `docs/comparison.md`, `docs/roadmap.md`
- `schema/brandkit-v0.1-fields.md` as parser-oriented field groundwork
- `CONTRIBUTING.md` with contribution rules

### Changed

- `README.md` rewritten to a clear standard-in-progress entry point

### Deprecated

- Two-file core model (`brand.md` + `styles.md`) deprecated as canonical
  standard path
