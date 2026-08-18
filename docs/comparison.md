# Comparison and Boundaries

What `brandkit.md` covers, and where a different artefact is the right answer.

## Brand guidelines (PDF, site, Figma)

**They cover:** the complete visual identity — print production, motion,
exhibition, merchandise, campaign systems, rationale, history.

**`brandkit.md` covers:** the subset a machine can act on at generation time,
plus the controls that decide whether output may be published.

**Relationship:** complementary, not competing. Guidelines are referenced from
`references[]`. If the guideline changes, the brandkit changes; the brandkit is
never the place where the guideline is *decided*.

## Design systems

**They cover:** components, states, interaction, accessibility of a UI.

**`brandkit.md` covers:** the token layer they consume, and brand context for
content production.

**Relationship:** the DTCG token set is the shared surface. A design system
reads the same tokens a content generator reads — which is the point of using
DTCG rather than a private format.

## Design tokens alone (DTCG, Style Dictionary)

**They cover:** values.

**`brandkit.md` adds:** stance and control. Tokens cannot express "mechanism
before benefit", "this claim is restricted to these conditions" or "this
certification exists but is unverified".

**Relationship:** `brandkit.md` does not replace a token pipeline. It references
one and defines a profile for segments, ordering and grouping.

## `llms.txt`

**It covers:** discovery and navigation — pointing a model at the right pages of
a site.

**`brandkit.md` covers:** how to write and design once the model is already
producing something.

**Relationship:** disjoint. One is a map, the other is a style and control layer.

## `AGENTS.md`, `CLAUDE.md`, skill and rule files

**They cover:** how an agent should behave in a repository — tooling, workflow,
conventions, what to run before committing.

**`brandkit.md` covers:** what the brand sounds and looks like, and what it may
claim.

**Relationship:** an agent file should *reference* the brandkit rather than
restate it. Brand rules pasted into an agent file are a copy that will go stale,
and it will be the copy that reaches the prompt.

## Prompt libraries and system prompts

**They cover:** situational instruction for one task or one model.

**`brandkit.md` covers:** the durable, versioned, reviewable source those
prompts are compiled *from*.

**Relationship:** a prompt is an output of context compilation, not an input to
brand governance. See [`context-compilation.md`](context-compilation.md).

## Content style guides

**They cover:** grammar, spelling, terminology, house rules.

**`brandkit.md` covers:** voice principles as `do`/`dont` data plus a
`vocabulary` of preferred and avoided terms — enough for a model, not a
replacement for an editorial manual.

**Relationship:** a style guide can be referenced; its machine-checkable parts
belong in policies with `evaluationMethod: "deterministic"`.

## Compliance and claims management systems

**They cover:** the legal record — what was approved, by whom, with which
evidence.

**`brandkit.md` covers:** the operational consequence at generation time —
`verified`, `approved`, `restricted`, `forbidden`, with scopes.

**Relationship:** the compliance system stays the source of legal truth. The
brandkit carries the decision into the tools that produce content, and
`references[]` points back to the evidence.

## The boundary, stated once

`brandkit.md` is the layer between brand identity and machine production. It is
deliberately not the identity itself, not the design system, not the asset
store, and not the legal record — it is the artefact that lets those four reach
a model without being retyped by a person under deadline.
