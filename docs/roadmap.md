# Roadmap

Where the standard is, and what has to be true before it moves.

## Now — v0.2.0-draft

**Done:**

- Four-layer architecture: Core, renderings, external artefacts, access layer.
- Core with 17 sections, generated JSON Schema, `camelCase` field names.
- Machine-evaluable `policies[]`; `claims[]` and `certifications[].verified`.
- DTCG profile with the `md.brandkit` namespace, including segments.
- Rendering contract for `brandkit.md` and `design.md`.
- Consumption rules for AI systems (§11), each traceable to a measured failure.
- Two extensions: text expressions, design rules.
- Reference profile and minimal document, compiled by the reference
  implementation's own compilers and validated against its schema.

**Open, deliberately:**

- Per-locale content in one document — `supportedLocales` declares reach, not
  translations.
- The asset manifest is referenced but not formalised.
- Design rules are an extension, not Core.

## Next — v0.2.x

Refinement without renaming Core fields.

- **Validator CLI.** Checks the rules a JSON Schema cannot express: a restricted
  claim used without conditions, an unverified certification referenced from
  messaging, a policy without an actionable description, values duplicated
  between `visualLanguage` and the token set.
  *Acceptance:* runs on every example in this repository in CI.

- **Conformance test suite.** Fixtures plus expected renderings, so a second
  Renderer can prove byte-identical output.
  *Acceptance:* an independent implementation passes without reading the
  reference source.

- **Formalise the asset manifest.** Asset ids appear in `products[]`,
  `imagery.referenceAssetIds` and design rules with no defined shape behind them.
  *Acceptance:* a consumer can resolve an asset id without implementation
  knowledge.

- **Single-language renderings in the reference implementation.** Its brandkit
  compiler emits English labels while its token compilers emit German ones, so a
  single exported artefact currently mixes both (visible in
  [`../EXAMPLES/helion-systems.brandkit.md`](../EXAMPLES/helion-systems.brandkit.md)).
  Permitted by §7.5, but it violates the same section's SHOULD.
  *Acceptance:* label sets are locale-selected, and one artefact renders in one
  language end to end.

- **Localisation extension.** The honest gap. Likely per-locale overlay documents
  rather than nested content in the Core.
  *Acceptance:* one brand, three locales, without duplicating stance.

## v1.0 — stability

Requirements, not a date:

1. Field names, enums and required status frozen; a written compatibility
   guarantee.
2. Published validator, with the §6/§8 rules covered.
3. **At least two independent conforming implementations** — one of them not
   written by the authors of this specification.
4. Extension mechanism proven by an extension somebody else wrote.
5. Every `MUST` in the specification either traceable to a measured failure or
   removed.

Point 3 is the real gate. A standard with one implementation is that
implementation's documentation.

## Beyond — tooling

Sequenced by whether the standard needs them or merely benefits:

- **MCP server** exposing brandkit and tokens as tools, so an agent reads brand
  context without a copy-paste step. Architecturally prepared in the reference
  implementation.
- **Generators**: token set from existing artwork, policy candidates from a
  brand guideline — output is proposal-grade, `source` marked, human approval
  required (§11.1).
- **Linters** for content: check a draft against `vocabulary.avoid`, forbidden
  claims and deterministic policies before anything is generated.
- **Editor integration**: schema-aware completion, and a preview of the compiled
  rendering next to the Core.

## Non-goals

Stated so they stop being asked:

- No component library, no UI kit.
- No asset storage or DAM behaviour.
- No prompt templates for specific models — those age in months; a standard must
  not.
- No print production, motion or merchandise specifications.
- No hosted service as part of the standard.
