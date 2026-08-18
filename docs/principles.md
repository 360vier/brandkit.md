# Design Principles

Why `brandkit.md` is built this way, and what each choice costs.

## 1. Structured where it must be, readable where it can be

The canonical model is JSON because its consumers are programs — validators,
compilers, evaluators, agents. The renderings are Markdown because their
consumers are people and language models.

v0.1 tried to be both in one file and could express neither state nor negative
facts. **Cost:** authoring a brandkit now means editing JSON or using a tool,
not typing Markdown. That is the price of enforceability.

## 2. Renderings are generated, never authored

`brandkit.md` and `design.md` are compiled output. Nothing exists in a rendering
that is not in the Core or the token set, and no rendering is a data source.

**Cost:** you cannot fix a brandkit by editing its Markdown. That is deliberate —
a format where both the source and the rendering are editable has two truths.

## 3. Every value exists exactly once

Values live in the token set. Stance lives in the Core. Application knowledge
lives in design rules. A hex value in `colorPrinciples`, a segment colour in a
template, a rule copied into a content type — each is a future divergence, and
the copy that diverges is always the one in the prompt.

## 4. Reference established formats instead of inventing new ones

Design tokens are DTCG. Assets stay in the store that already holds them. Full
brand guidelines stay as PDFs, referenced. The standard adds a namespaced
profile, never a competing format.

**Cost:** conforming to someone else's format means living with its gaps — DTCG
has no concept of segments, which is why they sit in `$extensions`.

## 5. Determinism over convenience

Compilation is a pure function: same input, byte-identical output. No timestamps,
no key-order dependence, no environment reads.

This is not tidiness. Consumers hash the compiled context to tie a generated
artefact to the exact brand state that produced it. Without determinism there is
no hash, and without a hash no output can be explained after the fact.

## 6. Hard rules must survive the budget

Model context is finite and brand context is long, so truncation is a certainty,
not an edge case. A standard that ignores this produces documents that are
correct and ineffective.

Hence: hard constraints and concrete values render into a compact protected
region, placed last, never shortened. Descriptive context is what gives way.

## 7. Approval is a visible act

Only approved material reaches a prompt; only approved output leaves the system.
Machine-extracted rules are proposals carrying their provenance until a person
accepts them.

A model's reading of an artwork is a hypothesis about a brand, not a statement
by it.

## 8. Prefer the negative fact

The most valuable field in a brandkit is often the one saying what must *not*
be said: `verified: false`, `status: forbidden`, `avoid`, `misuse`, `dont`. Free
text cannot express those without inviting a helpful paraphrase, which is
precisely why they are structured.

## 9. Identity is placed, never generated

Logos, wordmarks and headlines come from source files or deterministic
composition. A generated logo is scrap even when it looks good — and a model
told to respect a logo slot will draw a logo in it.

## 10. Small core, explicit extensions

The Core stays small enough that a brand can fill it honestly. Anything only
some brands need becomes a namespaced extension with its own version.

**Cost:** capabilities such as text-only expressions are no longer guaranteed to
be present. Consumers must handle their absence instead of assuming them.

## 11. Rules earn their place by having failed

Every `MUST` in §11 of the specification traces to a measured failure in a
running implementation. A rule with no failure behind it is speculation, and
speculation in a specification is surface area nobody maintains.
