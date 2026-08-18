# brandkit.md Specification v0.2.0-draft

## 1. Purpose

`brandkit.md` defines how a brand is expressed to humans, language models and
agents in a way that is portable, versionable and reproducible.

v0.1 treated a single Markdown file as the data model. Practice showed that this
does not survive contact with production: a model needs the *value* `#101B2D`,
not prose about a dark navy; a review workflow needs to know whether a rule is
approved; an auditor needs to know which certification is verified. v0.2
therefore separates the **canonical data model** from its **renderings**.

This specification is written for:

- teams maintaining brand context for AI workflows,
- implementers building portals, validators, generators or MCP servers,
- agents consuming brand context at generation time.

## 2. Normative Language

`MUST`, `MUST NOT`, `SHOULD`, `SHOULD NOT` and `MAY` are normative as described
in RFC 2119.

## 3. Architecture

Four layers, each with one job:

```
1  Brandkit Core        canonical structured model of one brand version
                        JSON, validated against brandkit-v0.2-draft.schema.json
                        ↓ compiled (deterministic, pure)
2  Renderings           brandkit.md  — brand context for humans and models
                        design.md    — token values for humans and models
3  External artefacts   design tokens as DTCG JSON · assets as asset manifest
                        (referenced, never re-invented)
4  Access layer         UI, file export, HTTP API, MCP
```

Three kinds of knowledge are kept apart, and MUST NOT be duplicated across
layers:

| Layer | Answers | Lives in |
| --- | --- | --- |
| Design tokens | **What values?** — colour, typeface, spacing step | DTCG token set (§9) |
| Brandkit Core | **What stance?** — voice, positioning, claims, policies | Core document (§5–§8) |
| Design rules | **How applied?** — where the logo sits, which grid holds | Optional extension (§12) |

A value that exists twice will diverge. Where a Core field needs a concrete
value, it MUST reference the token set rather than restate the value.

### 3.1 Conformance classes

An implementation MAY conform as one or more of:

- **Producer** — emits Core documents that validate against the JSON Schema
  and satisfy §5–§8.
- **Renderer** — compiles a Core document to `brandkit.md` exactly as specified
  in §7, deterministically.
- **Consumer** — reads Core documents or renderings and satisfies §11.
- **Extension** — implements a documented extension under §12 without
  violating any rule in §5–§11.

Claiming conformance without naming the class is meaningless; implementations
SHOULD state which classes they meet.

## 4. Canonical Sources

| Artefact | Path | Role |
| --- | --- | --- |
| Normative prose | `SPEC.md` | This document. Wins on conflict. |
| Machine schema | `schema/brandkit-v0.2-draft.schema.json` | Validation. `$id` is `https://brandkit.md/schema/brandkit-v0.2-draft.schema.json`. |
| Reference profile | `EXAMPLES/helion-systems.*` | Fictional brand, full feature coverage. |
| Minimal document | `EXAMPLES/minimal.brandkit.json` | Smallest valid Core. |
| Authoring template | `templates/brandkit-template.json` | All sections, placeholder values. |
| Extensions | `ext/` | Optional, separately versioned. |
| v0.1 archive | `spec/v0.1/` | Historical. Not normative. |

## 5. Brandkit Core

### 5.1 Identity and versioning

- A Core document MUST be a single JSON object.
- The file name SHOULD be `<brand>.brandkit.json`.
- `schemaVersion` MUST be present and MUST be `0.2.0-draft` for this revision.
- A document SHOULD carry `$schema` pointing at the schema `$id`.
- Keys beginning with `$` (such as `$schema`, `$comment`) are documentation
  affordances; consumers MUST ignore them when interpreting brand content.
- One document describes exactly **one version of one brand**. Multi-brand or
  multi-version containers are out of scope.

Version lifecycle:

- `metadata.status` is one of `draft`, `review`, `approved`, `archived`.
- A document with `status: approved` MUST be treated as immutable. Changes MUST
  create a new document with a new `metadata.version`, `status: draft` and
  `metadata.supersedesVersion` set to the version it replaces.
- Retired brand language SHOULD stay in the document with `enabled: false`
  (policies) or `status: forbidden` (claims) rather than being deleted, so that
  tooling keeps rejecting it.

### 5.2 Required root sections

All seventeen keys MUST be present. Collection sections MUST be present as
arrays; an empty array is valid and means "nothing declared".

| Key | Type | Notes |
| --- | --- | --- |
| `schemaVersion` | string | §5.1 |
| `metadata` | object | §5.3 |
| `identity` | object | §5.4 |
| `positioning` | object | §5.4 |
| `audiences` | array | §5.5 |
| `messaging` | object | §5.6 |
| `voice` | object | §5.6 |
| `visualLanguage` | object | §5.7 |
| `imagery` | object | §5.7 |
| `products` | array | §6 |
| `claims` | array | §6 |
| `policies` | array | §8 |
| `channels` | array | §5.8 |
| `markets` | array | §5.8 |
| `aiInstructions` | object | §5.9 |
| `governance` | object | §5.9 |
| `references` | array | §5.9 |

Field names are `camelCase` and are case-sensitive. Unknown keys MAY be
preserved by consumers but MUST NOT be required by them.

### 5.3 metadata

| Field | Required | Type |
| --- | --- | --- |
| `brandId` | MUST | string, stable slug |
| `organizationId` | MUST | string |
| `name` | MUST | string, display name |
| `version` | MUST | string, SemVer recommended |
| `status` | MUST | `draft` \| `review` \| `approved` \| `archived` |
| `defaultLocale` | MUST | BCP 47 tag |
| `supportedLocales` | MUST | array of BCP 47 tags |
| `createdAt`, `updatedAt` | MUST | ISO 8601 timestamp |
| `effectiveFrom`, `reviewDueAt` | MAY | ISO 8601 timestamp |
| `owner`, `approvedBy`, `supersedesVersion` | MAY | string |

Content in a Core document is authored in `defaultLocale`.
`supportedLocales` is a declaration of where the brand operates, not a
translation container — per-locale content is out of scope for v0.2 and MUST be
handled as separate documents until a localisation extension exists.

### 5.4 identity, positioning

`identity`: `mission` (MUST), `values` (MUST, array of `{name, description}`,
possibly empty), and optionally `vision`, `purpose`, `story`, `tagline`.

`positioning`: `statement` (MUST), `differentiators` (MUST, array),
`proofPoints` (MUST, array), and optionally `category`, `competitiveContext`.

`proofPoints` are the factual backing for generated text. A proof point SHOULD
carry its own measurement basis in the string itself ("9 days across 40
documented deployments") — a number without its basis cannot be used safely
downstream.

### 5.5 audiences

Array of `{id, name, description}` (all MUST), plus `needs`, `painPoints`
(arrays, default empty) and `toneAdjustments` (MAY).

`id` MUST be unique in the document and is the target of
`messaging.keyMessages[].audienceIds` and policy scopes of type `audience`.

### 5.6 messaging, voice

`messaging`: `coreMessage` (MUST), `keyMessages` (MUST, array of
`{id, message}` with optional `audienceIds`, `proofPoints`), optionally
`elevatorPitch` and `boilerplates` (`short`, `medium`, `long`).

`voice`: `toneAttributes` (MUST, array), `principles` (MUST, array of
`{name, description, do[], dont[]}`), optionally `vocabulary`
(`preferred[]`, `avoid[]`).

Voice principles carry `do`/`dont` as data rather than prose because consumers
evaluate them. A `dont` entry SHOULD name the observable pattern
("open with an outcome adjective"), not a mood.

### 5.7 visualLanguage, imagery

`visualLanguage`:

| Field | Required | Notes |
| --- | --- | --- |
| `logo.usage` | MUST | string |
| `logo.misuse` | MUST | array, may be empty |
| `logo.clearspace`, `logo.minSize` | MAY | string |
| `colorPrinciples`, `typographyPrinciples`, `layoutPrinciples`, `iconographyStyle` | MAY | string |
| `designTokenSetRef` | SHOULD | reference to the DTCG token set (§9) |

`visualLanguage` holds **principles**, never values. A concrete colour or
typeface in `colorPrinciples` is a specification violation in spirit and a
maintenance defect in practice: consumers read values from the token set, so a
value restated here will silently go stale. Producers SHOULD set
`designTokenSetRef`; consumers that need values MUST resolve them from the
referenced set.

`imagery`: `style` (MUST), `subjects`, `moods`, `avoid`, `referenceAssetIds`
(arrays, default empty), optionally `composition`, `lighting`, `colorGrading`.

`imagery.avoid` is the single most load-bearing array for image generation.
Entries MUST name objects and depictions concretely. A general prohibition is
read as a general suggestion by image models: "no text in the image" is not
sufficient, "no text, wordmarks or printed labels on objects inside the scene"
is.

### 5.8 channels, markets

`channels`: `{id, name}` (MUST), optionally `type`, `formatNotes`,
`toneAdjustments`, plus `constraints` (array).

`markets`: `{id, name}` (MUST), plus `locales` (array), optionally
`legalNotes`, `culturalNotes`.

Both `id` values are targets for policy scopes (§8).

### 5.9 aiInstructions, governance, references

`aiInstructions`:

| Field | Required | Notes |
| --- | --- | --- |
| `systemGuidelines` | MUST | System-prompt-grade instruction, written in the imperative |
| `fallbackBehavior` | MUST | What to do when the brandkit does not cover the case |
| `allowedContexts`, `restrictedContexts` | MUST (array) | Where output is permitted / must not be produced |
| `imageGenerationRules`, `textGenerationRules` | MUST (array) | Modality-specific hard rules |

`fallbackBehavior` MUST describe an action, not an attitude. The specified
behaviour SHOULD be: produce the most conservative version inside approved
facts, mark the uncertain passage, route to human review. It MUST NOT be
"use your best judgement" — that is the failure mode this field exists to
prevent.

`governance`: `owner` (MUST), optionally `reviewCycle`, `approvalPolicy`,
`changePolicy`, `contact`.

`references`: `{id, title, type}` (MUST) with `type` one of `guideline`,
`document`, `url`, `asset`; optionally `url`, `assetId`, `description`.
Full brand guidelines, print specifications and motion systems belong here as
references — they are explicitly out of scope for the Core (§13).

## 6. Products and Claims — proof before assertion

`products[]`:

| Field | Required | Notes |
| --- | --- | --- |
| `id`, `name`, `shortDescription` | MUST | |
| `slug` | MAY | |
| `approvedClaims`, `forbiddenClaims`, `safetyNotes`, `assetIds`, `markets`, `languages`, `certifications` | MUST (array, may be empty) | |
| `isPlaceholder` | MUST | boolean, default `false` |

`certifications[]`: `norm` (MUST), `verified` (MUST, boolean, default `false`),
optionally `scope`, `source`.

`claims[]`: `id`, `text`, `status` (`approved` \| `restricted` \| `forbidden`)
MUST be present; `conditions` and `markets` MAY be.

Normative consumption rules:

1. A norm, standard or certification MUST NOT appear in generated content
   unless its `certifications[]` entry has `verified: true`. An unverified
   entry exists in order to be excluded, and MUST NOT be paraphrased into an
   implication.
2. A `restricted` claim MUST only be used together with its `conditions`.
3. A `forbidden` claim MUST NOT appear, in any paraphrase.
4. Product facts, figures and capabilities MUST come from `approvedClaims`,
   `positioning.proofPoints` or `messaging.keyMessages[].proofPoints`. If a
   required fact is absent, a consumer MUST report the gap and MUST NOT supply
   a plausible substitute.
5. `isPlaceholder: true` marks a product whose facts are not verified. Content
   generated from it MUST NOT be published without human review.
6. `safetyNotes` MUST be treated as hard constraints, not tone guidance.

These six rules are the reason the Core is structured rather than prose. Free
text cannot express "this norm exists but must not be claimed".

## 7. brandkit.md — the rendering contract

`brandkit.md` is **generated output**, not a data source. A Renderer MUST NOT
be the only place a field exists, and consumers MUST NOT round-trip Markdown
back into a Core document as an authoring path.

### 7.1 Determinism

Compilation MUST be a pure function of the Core document: same input, byte-identical
output. Section order MUST be fixed and MUST NOT depend on object key iteration
order. Determinism is a hard requirement because consumers hash the compiled
context to make a generated artefact reproducible (§11.3).

### 7.2 Document shape

```
# {metadata.name} — Brandkit
> Version {metadata.version} · Status: {metadata.status} · Schema {schemaVersion}
**Default locale:** …
**Supported locales:** …
**Owner:** …
**Updated:** …
```

Sections follow in this order, joined by a horizontal rule (`\n\n---\n\n`):

| # | Heading | Emitted |
| --- | --- | --- |
| 1 | `## Identity` | always |
| 2 | `## Positioning` | always |
| 3 | `## Audiences` | when `audiences` is non-empty |
| 4 | `## Messaging` | always |
| 5 | `## Voice & Tone` | always |
| 6 | `## Visual Language` | always |
| 7 | `## Imagery` | always |
| 8 | `## Products` | when `products` is non-empty |
| 9 | `## Claims` | when `claims` is non-empty |
| 10 | `## Policies` | when at least one policy has `enabled: true` |
| 11 | `## Channels` | when `channels` is non-empty |
| 12 | `## Markets` | when `markets` is non-empty |
| 13 | `## Usage Notes for AI Systems` | always |
| 14 | `## Governance` | always |
| 15 | `## References` | when `references` is non-empty |

The document MUST end with a provenance footer:

```
_Generated from Brandkit Core {schemaVersion} · brand {metadata.brandId} · version {metadata.version}_
```

### 7.3 Field and entity conventions

- Scalar fields render as `**Label:** value`; empty fields are omitted entirely
  rather than rendered empty.
- List fields render as a bold label followed by a `-` list.
- Collection entries render as `### {name} (\`{id}\`)` so that the machine
  identifier stays visible to a reader and recoverable by a parser.
- Heading depth MUST stay within `#`…`###`, and levels MUST NOT be skipped.

### 7.4 Status-carrying renderings

Three renderings carry evaluation semantics and MUST be preserved verbatim by
Renderers:

| Source | Rendering |
| --- | --- |
| `certifications[]` with `verified: true` | `{norm} ({scope}) — verified, source: {source}` |
| `certifications[]` with `verified: false` | `{norm} ({scope}) — UNVERIFIED, must not be claimed` |
| `products[]` with `isPlaceholder: true` | heading suffix `— _placeholder data_` |

Policies render with their evaluation metadata on one line:

```
**Category:** {category} · **Severity:** {severity} · **Evaluation:** {evaluationMethod}[ · human review required]
```

Claims render grouped as `**Approved:**`, `**Restricted:**` (each with its
conditions in italics) and `**Forbidden:**`. A Renderer MUST NOT drop the
forbidden group: a consumer needs to know what not to say.

Policies with `enabled: false` MUST NOT be rendered.

### 7.5 Prose language

Renderings are for humans and models, so their labels and prose are
implementation- and locale-defined. The reference renderer emits English
labels. What is normative is the **structure** of §7.2–§7.4, not the language of
the surrounding prose. Implementations SHOULD render one artefact in one
language throughout.

## 8. Policies

A policy is a machine-evaluable rule, not a sentence in a style guide.

| Field | Required | Values |
| --- | --- | --- |
| `id`, `title`, `description` | MUST | string |
| `category` | MUST | `visual` \| `voice` \| `messaging` \| `product` \| `safety` \| `legal` \| `channel` \| `ai` |
| `severity` | MUST | `info` \| `warning` \| `error` \| `critical` |
| `evaluationMethod` | MUST | `human` \| `deterministic` \| `language-model` \| `multimodal-model` \| `hybrid` |
| `humanReviewRequired` | MUST | boolean |
| `enabled` | MUST | boolean |
| `scopes` | MUST (array) | `{type: global \| channel \| market \| product \| audience, value?}` |

Normative rules:

1. `enabled: false` means the policy is inactive for generation but retained
   for recognition. Consumers MUST NOT apply it as a rule and SHOULD use it to
   detect retired brand language.
2. `scopes` filter applicability. A policy with a `channel`/`market`/
   `product`/`audience` scope MUST NOT be applied outside it — a newsletter
   length limit that leaks into a poster is a defect, not caution.
3. `severity: critical` marks a policy whose violation invalidates the artefact.
   Consumers that evaluate output MUST treat it as a hard fail, not a score
   deduction.
4. `evaluationMethod` declares *how* a violation is detectable, so that a
   pipeline routes the check to the right evaluator. It does not weaken the
   rule: `human` means no automated evaluator may claim the policy was checked.
5. `humanReviewRequired: true` means an artefact touching this policy MUST NOT
   be published without a human decision, regardless of automated verdicts.
6. A policy MUST be actionable as written. "Be professional" is not a policy;
   "no more than three hashtags" is.

## 9. Design tokens

Design values live in a **DTCG (W3C Design Tokens) JSON** document, referenced
from `visualLanguage.designTokenSetRef`. The standard does not define a token
format; it defines a profile on top of DTCG.

### 9.1 Category roots

Top-level keys carry the category and MUST be processed in this order:

`color`, `typography`, `spacing`, `size`, `border`, `shadow`, `opacity`

A legacy top-level `radius` root MAY appear in older sets and MUST be treated
as belonging to `border`.

### 9.2 The `md.brandkit` extension namespace

Profile data lives under `$extensions["md.brandkit"]`, so a set stays readable
by any standard DTCG tool.

Set level (root `$extensions`):

| Key | Type | Meaning |
| --- | --- | --- |
| `segments` | array of `{id, label}` | The brand's segment registry (§9.4) |
| `tags` | array of string | Tag vocabulary of the set |

Token level:

| Key | Type | Meaning |
| --- | --- | --- |
| `order` | integer | Author order. REQUIRED for stable presentation, because JSON object key order is not preserved by common storage (notably Postgres `jsonb`, which normalises keys shortest-first then alphabetically). |
| `group` | string | Sub-grouping inside the category |
| `inputType` | enum | `color` \| `dim` \| `weight` \| `ratio` \| `pct` \| `select` \| `font` \| `shadow` |
| `unit` | string | Unit for `dim` values (`px`, `rem`, `ch`, …) |
| `tags` | array | Subset of the set-level vocabulary |
| `options` | array | Permitted values for `inputType: select` |
| `segments` | object | Per-segment values, keyed by segment `id` (§9.4) |

`inputType` describes the authoring control and the value's semantics;
`$type` stays the DTCG type. The mapping is:

| `inputType` | `$type` |
| --- | --- |
| `color` | `color` |
| `dim` | `dimension` |
| `weight` | `fontWeight` |
| `font` | `fontFamily` |
| `shadow` | `shadow` |
| `ratio`, `pct` | `number` |
| `select` | `string` |

### 9.3 Value forms

- Dimensions serialise as a string of number plus unit: `"16px"`.
- `fontFamily` values are a CSS stack (array or comma-separated string). The
  **first non-generic entry** is the brand typeface; generic families
  (`sans-serif`, `serif`, `monospace`, `system-ui`, `ui-sans-serif`) MUST NOT be
  treated as brand information.
- Shadows serialise as `{offsetX, offsetY, blur, spread, alpha}` with `alpha`
  in percent (0–100).
- A value of the form `{token.path}` is a **reference**. References MUST be
  resolved recursively, resolution MUST be segment-aware (§9.4), and
  implementations MUST bound recursion (the reference implementation stops at
  depth 10). An unresolvable reference MUST be left visible rather than
  silently replaced by a default.

### 9.4 Segments — one foundation, N expressions

A brand MAY declare segments (sub-brands, product lines, tonal variants). The
registry is declared **once**, at set level. A token that differs by segment
carries its values in its own `$extensions["md.brandkit"].segments` map.

- Segment values MUST NOT be stored anywhere else. Duplicating them into
  channels, templates or content types means every future CD change has to be
  chased through several places.
- For a segmented token, `$value` MUST hold one of the segment values (the
  reference implementation uses the first) so that standard DTCG consumers
  still read a valid value. The authoritative per-segment truth is the
  `segments` map.
- A consumer resolving a set for a segment MUST substitute `$value` from the
  segment map where present, and MUST fall back to the shared value otherwise.
- Sets SHOULD keep segmented tokens rare. The value of the model is that most
  values are shared; a set where every token is segmented is N brands wearing
  one file.

### 9.5 design.md

A token set SHOULD have a Markdown rendering (`design.md`) for humans and
models. Where present it MUST contain:

- one table per category root, listing token path, value, `$type` and
  description,
- per-segment values shown explicitly where a token is segmented,
- a CSS custom property block: shared values under `:root`, segment deviations
  as `[data-segment="{id}"]` override blocks,
- an explicit statement that the values are binding.

Custom property names derive from the token path with `.` replaced by `-`
(`color.brand.primary` → `--color-brand-primary`).

## 10. Interchange

### 10.1 Every export carries brand *and* design

A brandkit export without concrete design values is not actionable for an
external agent: principles without values cannot produce a brand-compliant
page. Therefore:

- A Markdown export MUST consist of the compiled `brandkit.md` plus a
  `## Design Tokens` section containing the concrete values. The section MAY be
  capped per category (the reference implementation caps at 24) and MUST then
  state what was omitted and where the full set lives.
- A JSON export MUST use the envelope:

```json
{ "brandkit": { … }, "designTokens": { "version": 4, "tokens": { … } } }
```

- `designTokens` MUST be `null` rather than an empty object when no set exists.
  A consumer importing an envelope MUST NOT replace an existing token set with
  an empty or placeholder one — a valueless version displacing real CD values
  is data loss disguised as an update.
- An importer SHOULD accept both the envelope and a bare Core document.

### 10.2 Distributable artefacts

An access layer SHOULD be able to emit, for any brand version:

| Artefact | Content |
| --- | --- |
| `brandkit.md` | compiled rendering plus token summary (§10.1) |
| Core JSON | envelope of §10.1 |
| `design.md` | token rendering (§9.5) |
| Token JSON | the DTCG set |
| Template | empty Core with all sections, importable |
| Schema | `brandkit-v0.2-draft.schema.json` |

The template MUST NOT ship placeholder design values (see §10.1).

## 11. Consumption by AI systems

This section is normative for Consumers. Every rule here exists because its
absence produced a measurable failure in the reference implementation.

### 11.1 Approval gates generation

- Only `approved` brand material MAY reach a prompt. Draft content, and
  machine-extracted candidates in particular, MUST NOT be treated as brand
  truth before human approval.
- Automatically derived rules SHOULD be stored with their provenance (that they
  were machine-extracted) so approval remains a visible act.

### 11.2 Hard rules must survive truncation

Model context is finite and brand context is long. A compiled brand context
routinely exceeds an image model's prompt budget by several times.

- A Consumer that truncates context MUST NOT truncate hard constraints.
  Policies, `imagery.avoid`, `safetyNotes` and the concrete brand values
  (§9) MUST be rendered into a compact, protected region that is never
  shortened, and MUST NOT rely on their position inside the long-form context.
- Concrete values are non-negotiable content: a colour name in prose is not a
  substitute for `#101B2D` in the protected region.
- Where instructions compete, the later and more specific one wins in practice.
  Consumers SHOULD place the protected region and any user-supplied revision
  request at the **end** of the prompt.
- When a Consumer must drop material, it MUST drop descriptive context before
  constraints, and SHOULD report what it omitted.

### 11.3 Reproducibility

A Consumer SHOULD record, for every generated artefact: the brand version, the
token set version, and a hash of the compiled context. Without those three, an
output cannot be explained after the fact — and brand governance that cannot
explain an output is not governance.

### 11.4 Separation of generation and composition

Identity elements MUST NOT be generated. A logo, wordmark or headline MUST be
placed from its source asset or composed deterministically. A generative model
MUST NOT be asked to render them, and `logo.misuse` SHOULD state this
explicitly.

Consequently, layout knowledge (where the logo sits, which crop shape applies,
where a text zone lives) MUST NOT be sent to an image model as prompt text.
Instructed to respect a logo slot, image models paint a logo. Layout belongs to
the deterministic composition step; see `ext/design-rules.md` for the
`appliesTo` mechanism that keeps it out of prompts.

## 12. Extensions

Extensions add optional capability without enlarging the Core. An extension:

- MUST live in its own document under `ext/`,
- MUST declare its own version and its status,
- MUST NOT redefine or contradict a Core field,
- MUST be namespaced when it adds data — inside a Core document under an
  `x-` prefixed key, inside a DTCG set under its own `$extensions` namespace.

Consumers MUST ignore unknown extension data rather than fail.

Defined extensions:

| Extension | Status | Purpose |
| --- | --- | --- |
| `ext/text-expressions.md` | draft | Text-only brand renderings: fallback wordmark, ASCII mark, emoji signature, CLI prompt style |
| `ext/design-rules.md` | draft | Application knowledge: logo placement, grid, spacing, with `appliesTo` routing |

## 13. Scope

In scope: the structure and semantics of the Core; its Markdown renderings; the
DTCG profile; interchange; consumption rules for AI systems.

Out of scope: the full visual identity system (print production, motion,
exhibition, merchandise); component libraries and UI implementation; asset
binaries and DAM behaviour; per-locale content variants; prompt engineering for
any specific model. These are referenced (§5.9), not absorbed.

## 14. Relationship to v0.1

v0.2 is a **breaking change**. v0.1 treated a hand-written Markdown file as the
model, with `snake_case` field labels and a required `brand expressions`
section. In v0.2 the structured Core is the model, Markdown is a rendering, and
text-only expressions moved to an extension (§12).

v0.1 remains readable under `spec/v0.1/` and is no longer normative. Field-level
mapping is in `docs/migration.md`.

## 15. Change policy

- v0.2.x MAY refine wording, tables and extension boundaries without changing
  Core field names.
- Any change to a Core field name, enum value or required status MUST be listed
  as breaking in `CHANGELOG.md`.
- The JSON Schema is generated from the reference implementation's Zod
  definitions and MUST NOT be hand-edited (see `schema/README.md`).
- v1.0 requires: stable field names, a published validator, and at least two
  independent conforming implementations.
