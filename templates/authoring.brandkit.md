# <Brand Name> — Brandkit (draft)

> Drafting aid, **not** a Brandkit Core document. The canonical model is JSON
> (`brandkit-template.json`); this file exists for writing prose before it is
> structured. Section names mirror the compiled rendering so the mapping is
> obvious. Delete this note and every `→` hint before converting.

## Identity

**Mission:** <one sentence, what you make true>
**Vision:** <optional — where this ends up>
**Purpose:** <optional — why it matters that you do it>
**Tagline:** <optional>

**Values:**
- **<Value name>** — <what it means in practice, not as an aspiration>

**Story:**
<optional, a paragraph>

→ `identity`

## Positioning

**Statement:** <who this is for and what it does better>
**Category:** <the shelf you are on>

**Differentiators:**
- <something a competitor cannot say>

**Proof points:**
- <fact with its measurement basis in the same sentence>

**Competitive context:** <optional>

→ `positioning`

## Audiences

### <Audience name> (`<audience-id>`)

<who they are and how they decide>

**Needs:**
- <what they are looking for>

**Pain points:**
- <what currently fails them>

**Tone adjustments:** <how the voice shifts for them>

→ `audiences[]`

## Messaging

**Core message:** <the one sentence every artefact should leave behind>
**Elevator pitch:** <2–3 sentences>

**Key messages:**
- <message> _(audiences: audience-id)_

**Boilerplate (short):** <one sentence>
**Boilerplate (medium):** <2–3 sentences>
**Boilerplate (long):** <a paragraph>

→ `messaging`

## Voice & Tone

**Tone attributes:** <4–6 adjectives>

### <Principle name>

<why this principle exists>

**Do:**
- <observable behaviour>

**Don't:**
- <observable pattern, not a mood>

**Preferred vocabulary:** <terms>
**Avoid:** <terms — words, not behaviours; behaviours belong in Don't above>

→ `voice`

## Visual Language

**Logo usage:** <how the mark is placed>
**Clearspace:** <…>
**Minimum size:** <…>

**Logo misuse:**
- <what must never happen to the mark>

**Color principles:** <how colour is *used* — no hex values here>
**Typography principles:** <hierarchy through weight and size>
**Layout principles:** <grid, margins, measure>
**Iconography:** <…>
**Design token set:** <path or id of the DTCG set>

→ `visualLanguage`. Concrete values go in the token set, never here.

## Imagery

**Style:** <photographic or illustrative approach>

**Subjects:**
- <what is in frame>

**Moods:** <…>
**Composition:** <…>
**Lighting:** <…>
**Color grading:** <…>

**Avoid:**
- <name the objects and depictions concretely — "no text" is not enough>

→ `imagery`

## Products

### <Product name> (`<product-id>`)

<one-sentence description>

**Approved claims:**
- <what may be said>

**Forbidden claims:**
- <what may never be said>

**Safety notes:**
- <hard constraint, not tone guidance>

**Certifications:**
- <Norm> (<scope>) — verified, source: <evidence>
- <Norm> (<scope>) — UNVERIFIED, must not be claimed

**Markets:** <…>

→ `products[]`. Unproven facts get `verified: false` and `isPlaceholder: true`.

## Claims

**Approved:**
- <claim>

**Restricted:**
- <claim> — _<the conditions under which it may be used>_

**Forbidden:**
- <claim>

→ `claims[]`

## Policies

### <Policy title> (`<policy-id>`)

<the rule, imperative and actionable>

**Category:** visual | voice | messaging | product | safety | legal | channel | ai
**Severity:** info | warning | error | critical
**Evaluation:** human | deterministic | language-model | multimodal-model | hybrid
**Scope:** global | channel:<id> | market:<id> | product:<id> | audience:<id>

→ `policies[]`. If you cannot answer severity, scope and evaluation, it is not a
policy — put it in `voice.principles` instead.

## Channels

### <Channel name> (`<channel-id>`)

**Type:** <social | email | documentation | …>
**Format notes:** <dimensions, length, structure>
**Tone adjustments:** <…>

**Constraints:**
- <checkable limit>

→ `channels[]`

## Markets

### <Market name> (`<market-id>`)

**Locales:** <BCP 47 tags>
**Legal notes:** <…>
**Cultural notes:** <…>

→ `markets[]`

## Usage Notes for AI Systems

<system guidelines — imperative, as a model will read them>

**Allowed contexts:**
- <where output may be produced>

**Restricted contexts:**
- <where it must not>

**Fallback behavior:** <a concrete action for uncovered cases — never "use your
best judgement">

**Image generation rules:**
- <hard rule for the image path>

**Text generation rules:**
- <hard rule for the text path>

→ `aiInstructions`

## Governance

**Owner:** <team or role>
**Review cycle:** <…>
**Approval policy:** <who signs off on what>
**Change policy:** <…>
**Contact:** <…>

→ `governance`

## References

- **<Title>** (guideline | document | url | asset) — <url or asset id> — <what it covers>

→ `references[]`

---

## Converting to a Core document

1. `cp templates/brandkit-template.json my-brand.brandkit.json`
2. Move each section across using the `→` hints.
3. Fill `metadata` — `brandId`, `organizationId`, `version`, `status`,
   `defaultLocale`, `supportedLocales`, timestamps. None of it is optional.
4. Extract every concrete value into a DTCG set
   (`templates/tokens-template.json`) and set `designTokenSetRef`.
5. Validate:

```bash
npx ajv-cli validate -s schema/brandkit-v0.2-draft.schema.json -d my-brand.brandkit.json --spec=draft2020
```
