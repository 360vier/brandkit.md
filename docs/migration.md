# Migration: v0.1 → v0.2

v0.2 is a breaking change. v0.1 treated a hand-written Markdown file as the data
model; v0.2 makes a structured Core document the model and Markdown a generated
rendering.

The v0.1 specification remains readable under [`spec/v0.1/`](../spec/v0.1/) and
is no longer normative.

## What changed, and why

| | v0.1 | v0.2 |
| --- | --- | --- |
| Data model | `brandkit.md` (Markdown) | Core document (JSON, schema-validated) |
| Markdown | the source | generated rendering |
| Field naming | `snake_case` labels in lists | `camelCase` JSON keys |
| Section names | lowercase (`## voice and tone`) | title case in the rendering (`## Voice & Tone`) |
| Design values | prose, or absent | DTCG token set, referenced |
| Rules | prose in `## do and dont` | `policies[]` with severity, scope, evaluation method |
| Product facts | prose | `products[]`, `claims[]`, `certifications[].verified` |
| Text expressions | required section | [extension](../ext/text-expressions.md) |
| Frontmatter | optional `spec_version` | `schemaVersion` field (required) |

The driver was enforceability. Prose cannot express *state* ("this rule is a
draft") or a *negative fact* ("this certification exists and must not be
claimed") without inviting a model to helpfully paraphrase it into a claim.

## Field mapping

### brand overview

| v0.1 | v0.2 |
| --- | --- |
| `display_name` | `metadata.name` |
| `short_name` | `x-textExpressions.plainTextSignature` (extension), or an internal short form |

`metadata` additionally requires `brandId`, `organizationId`, `version`,
`status`, `defaultLocale`, `supportedLocales`, `createdAt`, `updatedAt` — none of
which v0.1 had, and all of which versioning depends on.

### mission and positioning

| v0.1 | v0.2 |
| --- | --- |
| `mission` | `identity.mission` |
| `positioning` | `positioning.statement` |

New and worth filling: `identity.values[]`, `identity.tagline`,
`positioning.differentiators[]`, `positioning.proofPoints[]`.

### audience

| v0.1 | v0.2 |
| --- | --- |
| `primary_segments[]` | `audiences[]` as `{id, name, description}` |

Each audience gains an `id`, which is what `keyMessages[].audienceIds` and
audience-scoped policies target.

### voice and tone

| v0.1 | v0.2 |
| --- | --- |
| `tone_attributes[]` | `voice.toneAttributes[]` |
| `avoid_patterns[]` | `voice.principles[].dont[]`, and/or `voice.vocabulary.avoid[]` |

Split them deliberately: `vocabulary.avoid` is a word list, `principles[].dont`
is a behaviour ("open with an outcome adjective").

### messaging

| v0.1 | v0.2 |
| --- | --- |
| `core_message` | `messaging.coreMessage` |
| `proof_points[]` | `positioning.proofPoints[]` |

`messaging.keyMessages[]` is new and required (may be empty). A key message can
carry its own `proofPoints` and target `audienceIds`.

### brand expressions

Entire section moves to [`ext/text-expressions.md`](../ext/text-expressions.md);
the field-by-field table is in that document. Nothing was renamed beyond
`snake_case` → `camelCase`.

### do and dont

v0.1's single prose section splits by intent:

| v0.1 content | v0.2 home |
| --- | --- |
| Language and behaviour rules | `voice.principles[].do` / `.dont` |
| Rules with consequences (must never happen) | `policies[]` with `severity` and `scopes` |
| Logo handling | `visualLanguage.logo.misuse[]`, plus a `visual` policy |
| Imagery prohibitions | `imagery.avoid[]` |

This is the largest piece of real work in a migration. A `dont` that invalidates
an artefact is a policy with `severity: "critical"`, not a bullet in a list.

### usage notes for ai systems

| v0.1 | v0.2 |
| --- | --- |
| `allowed_contexts[]` | `aiInstructions.allowedContexts[]` |
| `restricted_contexts[]` | `aiInstructions.restrictedContexts[]` |
| `fallback_behavior` | `aiInstructions.fallbackBehavior` |

New: `aiInstructions.systemGuidelines` (required),
`imageGenerationRules[]`, `textGenerationRules[]`.

### visual identity (optional in v0.1)

Prose splits by layer:

| v0.1 content | v0.2 home |
| --- | --- |
| "Primary colour is deep navy" | token `color.brand.primary` = `#101B2D` |
| "One accent, used sparingly" | `visualLanguage.colorPrinciples` |
| "Logo bottom-left, one cap height clear" | [`ext/design-rules.md`](../ext/design-rules.md) |

Do not carry values into `visualLanguage`. A hex value there is a copy that will
go stale, and it will be the copy that reaches the prompt.

## Steps

1. **Start from the template.** `cp templates/brandkit-template.json
   my-brand.brandkit.json`. Do not convert your Markdown by hand into a shape
   you guess at — the template has every section in the right form.

2. **Move the prose across** using the tables above. Delete optional fields you
   cannot fill honestly; an empty string is worse than an absent field.

3. **Extract a token set.** Every concrete value from the old file becomes a DTCG
   token. Start from `templates/tokens-template.json`, and set
   `visualLanguage.designTokenSetRef` to point at it.

4. **Turn hard rules into policies.** For each one: which category, how bad is a
   violation, where does it apply, and how would anyone detect it? A rule that
   cannot be answered for is prose, and belongs in `voice.principles` instead.

5. **Structure the product facts.** Claims become `claims[]` with a status;
   certifications become `certifications[]` with `verified`. Anything unproven
   gets `verified: false` and `isPlaceholder: true` — that is what keeps it out
   of generated content.

6. **Validate.**

   ```bash
   npx ajv-cli validate -s schema/brandkit-v0.2-draft.schema.json -d my-brand.brandkit.json --spec=draft2020
   ```

7. **Compile and read the rendering.** The generated `brandkit.md` is what a
   model will see. Anything that reads as vague there will be interpreted as
   vague.

8. **Keep the old file** until the new one is approved. `spec/v0.1/` exists for
   the same reason.

## What you gain

- Schema validation instead of hoping a parser agrees with you.
- Rules with severity and scope instead of a flat bullet list.
- Concrete values that survive prompt truncation.
- Versioning with immutable approved states and explainable outputs.
- A place to put the sentence prose could never carry: *"this exists and must
  not be claimed."*

## What you lose

- Editing brand context in a text editor without a tool or a template.
- Text expressions as a guaranteed feature — they are now an extension a
  consumer may not implement.
