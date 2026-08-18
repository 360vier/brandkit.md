# Context Compilation

How a Core document plus a token set becomes the context a model actually
receives — and why the naive version fails.

Normative rules: [`SPEC.md`](../SPEC.md) §11.

## The naive version, and why it fails

The obvious approach is: compile the whole brandkit, prepend it to the prompt,
done. Three things break.

**1. Everything does not fit.** A complete brand context runs to five figures in
characters. An image model's usable prompt budget is a few thousand. Something
will be dropped, and if you did not decide what, the model decided for you.

**2. What gets dropped is the important part.** Measured in the reference
implementation: the compiled context was ~13,000 characters against a
4,000-character budget. The section `## Applicable Policies` began at character
8,734 — so *every* policy fell away, including three safety-critical rules and
the rule that a logo may only come from original files. The control layer never
reached the model at all. The output looked fine and was unusable.

**3. Prose is not a value.** Concrete colour and typeface values sat inside that
same long context and disappeared the same way. Measured: present in the stored
context, absent from the final prompt. The model was given prose about colour
but never a colour — which is exactly what "the tone is slightly off, I can't
say why" looks like from the outside.

## The shape that works

```
┌─ Task context ───────────────────────────────────────┐
│  brand identity · positioning · voice · imagery       │  ← may be truncated
│  audience · channel · market · product facts          │
│  policies in full, with rationale                     │
├─ PROTECTED REGION (never truncated, always last) ─────┤
│  === HARD RULES ===                                   │
│  === BRAND ANCHORS (non-negotiable) ===               │
│  === DESIGN SYSTEM RULES ===                          │
│  user revision request                                │
└───────────────────────────────────────────────────────┘
```

Two properties do the work:

- **Position.** Where instructions compete, the later and more specific one
  wins. The protected region sits at the end.
- **Brevity.** A short block beats a long context. The anchors are a handful of
  lines on purpose.

## Task context: compile for the task, not the brand

Do not send the whole brand. Send what this task needs: brand core, the one
product, the one audience, the market, the channel, the content type's
requirements, and the policies whose scopes match.

Filtering by scope is not an optimisation — an out-of-scope rule is a rule the
brand never asked for in this situation, and every character it consumes is one
the applicable rules do not get.

## The protected region

### Hard rules

Policies with `severity` of `critical` or `error`, in categories relevant to the
modality, rendered as one imperative line each, `critical` first so that
truncation cannot reach safety:

```
=== HARD RULES (violating any of these makes the result unusable) ===
- Do NOT render any logo, wordmark, brand name or headline text into the image.
- This includes lettering ON objects: show equipment, webbing, helmets, gloves and
  clothing WITHOUT any printed brand names, model names, labels, embossed codes or
  certification markings.
- The photograph must fill the entire frame edge to edge. Do NOT paint layout
  scaffolding: no frames, borders, colour panels or bands, grid or guide lines …
- [CRITICAL] Protective equipment shown complete: Where people appear …
```

Note the second line. "No logo in the image" was read as "no logo overlay", and
the model kept printing the brand name onto webbing, carabiners and clothing;
two runs failed evaluation. Naming the objects fixed it. General prohibitions
are read as general suggestions.

`messaging` policies are deliberately excluded from image prompts: they act on
text, and the text path has no budget problem.

### Brand anchors

The concrete values, in the region that never gets cut:

```
=== BRAND ANCHORS (non-negotiable) ===
Brand palette — use ONLY these colours for any graphic area, tint or accent:
#101B2D (brand primary), #1F9E8C (brand accent), #0A1220 (brand ink), …
Do not introduce colours outside this palette. Photographic content keeps its natural colours.
Brand typeface is Inter Tight — a clean geometric sans. Do not render headlines,
logos or wordmarks in the image; they are composed separately.
```

A hex value cannot be misread. "Warm orange" can.

### Design system rules

Approved rules with `appliesTo` of `image` or `all`, capped by priority, with
the number of omitted rules reported rather than silently truncated. Layout
rules (`appliesTo: "composition"`) never appear here — see
[`../ext/design-rules.md`](../ext/design-rules.md).

## Two modalities, two budgets

| | Image models | Language models |
| --- | --- | --- |
| Context | Truncated; protected region always survives | Full context, no truncation needed |
| Design values | ~5 lines of anchors | Full `design.md`, grid and scale included |
| Design rules | Capped, prioritised | Complete, grouped by category |
| Layout | Never — deterministic composition | N/A |
| Reference images | Only rasters, never placeholders or SVG | N/A |

The asymmetry is the point. Rules written for one budget silently fail in the
other.

## Reproducibility

Record three things with every generated artefact:

1. the brand version,
2. the token set version,
3. a hash of the compiled context.

With them, any output can be explained after the fact and re-derived. Without
them, brand governance cannot answer "why did it say that?" — which is the only
question that matters after something goes wrong.

This is why compilation must be a **pure function**: same input, byte-identical
output. A compiler that iterates object keys, stamps a timestamp or resolves a
value non-deterministically breaks the hash and takes reproducibility with it.

## Truncation etiquette

When a consumer must drop material:

- drop descriptive context before constraints, never the reverse,
- keep `critical` above `error`,
- report what was dropped — a silent cut reads downstream as "everything was
  included", which is worse than the cut itself.

## Checklist for implementers

- [ ] Only approved material reaches a prompt; machine-extracted candidates do not.
- [ ] Policies filtered by scope, then by modality relevance.
- [ ] Hard constraints and concrete values in a protected region at the end.
- [ ] Truncation touches context first, constraints never.
- [ ] Omissions reported, not silent.
- [ ] Layout rules routed to composition, never into a prompt.
- [ ] Brand version, token version and context hash recorded per artefact.
