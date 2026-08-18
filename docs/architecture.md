# Architecture

Why `brandkit.md` v0.2 is shaped the way it is, and what belongs in which layer.

## The v0.1 mistake, stated plainly

v0.1 made a single Markdown file the data model. It read beautifully and broke
on three things:

1. **Values.** A model needs `#101B2D`. Prose gives it "a deep navy", and the
   output is nearly right in a way nobody can debug.
2. **State.** A rule is either approved or it is somebody's draft. Markdown has
   no place to put that, so every rule in the file looked equally binding.
3. **Negative facts.** "This certification exists, is not verified, and must
   therefore not be claimed" cannot be expressed in prose without inviting a
   model to helpfully claim it anyway.

All three failures share a shape: the artefact was optimised for reading, and
then asked to do enforcement.

## Four layers

```
1  Brandkit Core        canonical, structured, versioned. One brand, one version.
                        JSON validated against schema/brandkit-v0.2-draft.schema.json
        │
        │  compiled — pure function, deterministic, no I/O
        ↓
2  Renderings           brandkit.md   brand context for humans and models
                        design.md     token values for humans and models
        │
        │  references (never copies)
        ↓
3  External artefacts   design tokens  → DTCG (W3C Design Tokens) JSON
                        assets         → asset manifest / DAM
                        guidelines     → references[] in the Core
        │
        ↓
4  Access layer         UI · file export · HTTP API · MCP
```

### Layer 1 — Core

The canonical model. Structured because the consumers are programs: validators,
compilers, evaluators, agents. It carries brand *stance* (voice, positioning,
messaging), brand *facts* (products, claims, certifications) and brand *controls*
(policies).

One document is one version of one brand. Approved versions are immutable; a
change is a new draft with `supersedesVersion` set. That is not bureaucracy —
it is what makes a generated artefact explainable six months later.

### Layer 2 — Renderings

`brandkit.md` and `design.md` are **generated**. They are the portable form: a
model reads them, a person reviews them, a repository diffs them. Nothing is
authored here, and nothing exists here that is not in the Core or the token set.

Compilation is a pure function. Same input, byte-identical output — because
consumers hash the compiled context to tie a generated artefact to the exact
brand state that produced it. A compiler that iterates object keys, or stamps a
timestamp, destroys that property.

### Layer 3 — External artefacts

Established formats get referenced, not re-invented:

- **Design tokens** are DTCG JSON. The standard adds a profile
  (`$extensions["md.brandkit"]`), never a competing format — a token set stays
  readable by any DTCG-aware tool.
- **Assets** stay in whatever store already holds them; the Core carries ids.
- **Full brand guidelines** stay as PDFs, sites or Figma files, listed in
  `references[]`.

### Layer 4 — Access

The Core is only useful where the work happens. An access layer emits the
artefacts of SPEC §10.2 — compiled Markdown, Core JSON, token JSON, template,
schema — and every export carries brand *and* design values together. A brandkit
export without concrete values is not actionable for an external agent.

## Three kinds of knowledge

The most common structural mistake is putting the same knowledge in two layers.

| Layer | Question | Home | Example |
| --- | --- | --- | --- |
| Design tokens | What **values**? | DTCG set | `color.brand.accent = #1F9E8C` |
| Brandkit Core | What **stance**? | Core | "Name the limits of a control in the same paragraph" |
| Design rules | How **applied**? | `ext/design-rules.md` | "Wordmark lower-left, one cap height clearspace" |

Concretely:

- `visualLanguage.colorPrinciples` says *"one brand colour carries the page; the
  accent marks a single action"*. It MUST NOT contain a hex value — the value
  lives in the token set and would go stale here.
- A token set says `#1F9E8C`. It says nothing about where the accent may appear.
- A design rule says where the logo sits. It is not a token, because it is not a
  value; it is not voice, because it is not language.

A value duplicated across layers will diverge, and the copy that diverges is
always the one in the prompt.

## Segments: one foundation, N expressions

Sub-brands, product lines and tonal variants are handled in the **token layer**,
not by cloning brandkits. A brand declares its segments once, at set level, and
individual tokens carry per-segment values only where the brand genuinely
differs.

In the reference profile, 40 of 43 tokens are shared and 3 differ by segment.
That ratio is the point: a set where everything is segmented is several brands
sharing a filename.

See [`design-tokens.md`](design-tokens.md) §Segments.

## Generation versus composition

Identity elements are never generated. A logo, a wordmark or a headline is
placed from its source file or composed deterministically; a generative model is
never asked to draw it.

This splits an image artefact into two paths:

```
motif   →  generative model  →  full-bleed photograph, no text, no marks
layout  →  deterministic composition  →  crop shape, colour area, logo slot, headline
```

The split is not stylistic caution. Layout rules handed to an image model as
prompt text are executed literally: the model paints a logo into the photograph,
renders the colour panel, draws the grid. See SPEC §11.4 and
[`ext/design-rules.md`](../ext/design-rules.md).

## Data flow, end to end

```
Foundation      Brandkit Core (versioned) · DTCG tokens (versioned)
                design rules · assets · products
       ↓ compile
Context         brandkit.md + design.md + task context
                + protected region: hard constraints & concrete values
                + context hash
       ↓ generate
Production      text models → copy, HTML, documents
                image models → motifs (no text, no marks)
                composition → deterministic headline, logo, layout
       ↓ evaluate
Review          deterministic checks + model evaluation → human approval
       ↓
Distribution    only approved artefacts leave the system
```

Two invariants hold along that path:

1. **Approval gates generation.** Only approved brand material reaches a prompt.
   Machine-extracted candidates are proposals until a person accepts them.
2. **Approval gates distribution.** Only approved output leaves the system.

## Further reading

- [`design-tokens.md`](design-tokens.md) — the DTCG profile in detail
- [`policies.md`](policies.md) — machine-evaluable rules
- [`context-compilation.md`](context-compilation.md) — how Core plus tokens become model context
- [`principles.md`](principles.md) — the trade-offs behind these choices
