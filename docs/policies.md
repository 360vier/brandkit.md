# Policies

A policy is a brand rule expressed so that a machine can route, check and
enforce it. Policies are the reason the Core is structured rather than prose.

Normative rules: [`SPEC.md`](../SPEC.md) §8.

## Anatomy

```json
{
  "id": "pol-ppe-complete",
  "title": "Protective equipment shown complete",
  "description": "Where people appear in a working environment that requires protective equipment, that equipment is shown correctly and completely. Partial protection reads as an endorsement of unsafe practice.",
  "category": "safety",
  "severity": "critical",
  "scopes": [{ "type": "global" }],
  "evaluationMethod": "multimodal-model",
  "humanReviewRequired": true,
  "enabled": true
}
```

Each field does one job:

| Field | Job |
| --- | --- |
| `description` | The rule itself, imperative and actionable. This is what a model reads. |
| `category` | Which domain it governs: `visual`, `voice`, `messaging`, `product`, `safety`, `legal`, `channel`, `ai`. |
| `severity` | How bad a violation is: `info`, `warning`, `error`, `critical`. |
| `scopes` | Where it applies: `global`, or scoped to a `channel`, `market`, `product`, `audience`. |
| `evaluationMethod` | *How* a violation is detectable: `human`, `deterministic`, `language-model`, `multimodal-model`, `hybrid`. |
| `humanReviewRequired` | Whether a person must decide before publication. |
| `enabled` | Whether it is active for generation. |

## `severity` decides what a failure means

`critical` is not "very important". It means a violation **invalidates the
artefact** — a hard fail, not a score deduction. An evaluator that averages a
critical violation into a 4.2/5 has produced a number instead of a decision.

Reserve `critical` for the cases where that is true: safety depiction,
unverifiable regulatory claims, identity integrity. Everything else is `error`,
`warning` or `info`.

## `evaluationMethod` is routing, not strength

It declares which evaluator can see a violation, so a pipeline sends the check
to the right place:

- `deterministic` — a length limit, a required label, a forbidden string.
- `language-model` — tone, framing, implied claims.
- `multimodal-model` — anything about what an image actually shows.
- `human` — judgement that no evaluator may claim to have performed.
- `hybrid` — a deterministic pre-filter plus model judgement.

Declaring `human` does not weaken the rule; it forbids automated systems from
reporting it as checked.

## `scopes` must be respected in both directions

A scoped policy applied too widely is as much a defect as one applied too
narrowly. A newsletter length limit that leaks into a poster brief is not
caution — it is a rule firing where the brand never said it should. Filter by
scope before compiling context, and pass only what applies to the task at hand.

## `enabled: false` is a feature

Retired brand language stays in the document, disabled:

```json
{
  "id": "pol-legacy-tagline",
  "title": "Retired tagline \"Trust the Layer\"",
  "description": "Superseded in version 3.0.0. Kept here so tooling recognises and rejects it rather than treating it as valid brand language.",
  "enabled": false
}
```

Deleting it loses the knowledge that the phrase is *wrong*. Keeping it disabled
means a linter can flag it in a draft while a generator never emits it. Disabled
policies are not rendered into `brandkit.md` and never reach a prompt.

## Writing rules that actually work

Four patterns, learned the expensive way.

**1. Name the objects, not the concept.** A model reads a general prohibition as
a general suggestion. "No logo in the image" was obeyed as "no logo overlay" —
and the brand name was still painted onto webbing, carabiners and clothing.
Two runs failed. The rule now names printed labels, embossed codes and standard
markings on objects inside the scene explicitly. After that: no hard fails.

**2. Say what the artefact must contain, not how to feel.** "Be professional" is
not a policy. "No more than three hashtags" is.

**3. Put the negative fact where it cannot be paraphrased.** "Do not claim
certifications that are not verified" belongs in a policy *and* the underlying
fact belongs in structured form (`certifications[].verified`). Prose alone
invites a helpful implication.

**4. State the limit in the same breath as the benefit.** Policies about scope
("distinguish enforced from observed") prevent the single most common brand
failure in technical writing: a true statement read against a scope nobody
claimed.

## Policies at generation time

- Only `enabled` policies reach a prompt, and only those whose scopes match.
- Hard constraints MUST survive prompt truncation. In the reference
  implementation the compiled context reached ~13,000 characters against an
  image model's 4,000-character budget; the policy section began at character
  8,734 and was dropped in full, including three safety-critical rules. Policies
  now render into a compact protected region at the end of the prompt that is
  never shortened. See [`context-compilation.md`](context-compilation.md).
- Messaging policies are deliberately excluded from image prompts: they act on
  text, and the text path has no budget problem. Every character in an image
  prompt spent on an irrelevant rule is one not spent on a relevant one.

## Policies at evaluation time

The same objects feed evaluation:

- `severity: critical` → hard fail criteria.
- `evaluationMethod` → which evaluator runs the check.
- `humanReviewRequired: true` → no publication without a human decision,
  regardless of automated verdicts.

That is what makes the loop closed: the rule that shaped the prompt is the rule
the output is judged against, and it is the same record in both places.
