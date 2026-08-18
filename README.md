# brandkit.md

An open standard for expressing brand context to humans, language models and
agents — structured enough to enforce, readable enough to review in a pull
request.

- **Spec version:** `0.2.0-draft`
- **Status:** standard-in-progress, with a running reference implementation
- **Normative source:** [`SPEC.md`](SPEC.md)

## The problem

Ask a model to write in your brand voice and it will oblige. Ask it twice and
you get two brands. The usual fixes fail in specific ways:

- **A PDF brand guideline** cannot be read at generation time.
- **A prompt** is situational, unversioned and unreviewable.
- **Prose about the brand** gives a model "a deep navy" when what it needs is
  `#101B2D`.
- **None of them** can express *"this certification exists but must not be
  claimed"* — the sentence that keeps a marketing team out of trouble.

`brandkit.md` closes that gap: one canonical, versioned model of a brand, plus
renderings that a person can read and a model can obey.

## Four layers

```
1  Brandkit Core        canonical model of one brand version — JSON, schema-validated
                        ↓ compiled deterministically
2  Renderings           brandkit.md · design.md — for humans and models
3  External artefacts   design tokens as DTCG JSON · assets as manifest (referenced, not reinvented)
4  Access layer         UI · file export · API · MCP
```

Three kinds of knowledge, kept strictly apart so no value exists twice:

| Layer | Answers | Example |
| --- | --- | --- |
| Design tokens | What **values**? | `color.brand.primary = #101B2D` |
| Brandkit Core | What **stance**? | "Mechanism before benefit" |
| Design rules | How **applied**? | "Wordmark in the lower-left slot" |

## What that buys you

A brandkit is enforceable, not decorative:

```json
{ "norm": "SOC 2 Type II", "scope": "Gateway data plane",
  "source": "Audit scheduled, report not yet issued", "verified": false }
```

`verified: false` means no generated artefact may name that standard — not as a
claim, not as an implication, not as a paraphrase. The same structure carries
forbidden claims, restricted claims with their conditions, safety notes, and
policies with a severity and an evaluation method:

```json
{ "id": "pol-no-text-in-image", "category": "visual", "severity": "error",
  "evaluationMethod": "multimodal-model", "humanReviewRequired": false,
  "enabled": true }
```

That is the difference between a brand guideline and a control.

## Try it in 60 seconds

```bash
cp templates/brandkit-template.json my-brand.brandkit.json
```

Fill it in, then validate against the schema with any JSON Schema tool:

```bash
npx ajv-cli validate -s schema/brandkit-v0.2-draft.schema.json -d my-brand.brandkit.json --spec=draft2020
```

Prefer to start from something filled in? Read
[`EXAMPLES/helion-systems.brandkit.json`](EXAMPLES/helion-systems.brandkit.json)
— a fictional brand exercising every feature — and its compiled rendering,
[`EXAMPLES/helion-systems.brandkit.md`](EXAMPLES/helion-systems.brandkit.md).

## Brand rendering showcase

Brands also have to survive where nothing renders — a terminal, a log line, a
plain-text mail. That is the
[text-expressions extension](ext/text-expressions.md):

```txt
Plain text header      HELION SYSTEMS
Terminal rendering     |-| E L I O N
Emoji + fallback       ✅ [HELION]
CLI prompt             helion-secure>
```

## Reference implementation

The **brandkit.md Portal** ([portal.brandkit.md](https://portal.brandkit.md))
is the first conforming implementation and the source of most of this
specification's hard rules. It holds brandkits and DTCG token sets as versioned
records, compiles `brandkit.md` and `design.md`, assembles task-specific model
context, and runs generation through evaluation and human approval.

Every rule in `SPEC.md` §11 (*Consumption by AI systems*) exists because its
absence produced a measurable failure there. Two examples:

- A compiled brand context ran to ~13,000 characters; the image model accepted
  4,000. The policy section started at character 8,734 and was dropped in full —
  including three safety-critical rules. Hard constraints now render into a
  compact protected region that is never truncated (§11.2).
- Layout rules given to an image model as prompt text were obeyed literally: the
  model painted a logo, the colour panel and the grid lines into the photograph.
  Layout now routes to a deterministic composition step and never reaches a
  prompt (§11.4, `ext/design-rules.md`).

The examples in this repository are compiled by that implementation's own
compilers, and validate against its schema — the standard and the tool cannot
drift apart quietly.

## Repository structure

| Path | Contents |
| --- | --- |
| [`SPEC.md`](SPEC.md) | Normative specification (`MUST`/`SHOULD`/`MAY`) |
| [`schema/`](schema/) | `brandkit-v0.2-draft.schema.json` — generated, machine-validatable |
| [`EXAMPLES/`](EXAMPLES/) | Reference profile, minimal document, token set, compiled renderings |
| [`templates/`](templates/) | Importable starting points for Core and tokens |
| [`ext/`](ext/) | Optional extensions: text expressions, design rules |
| [`docs/`](docs/) | Architecture, design tokens, policies, context compilation, principles, FAQ, comparison, migration, roadmap |
| [`spec/v0.1/`](spec/v0.1/) | Archived v0.1 — historical, not normative |

## Where to start

- **I want to author a brandkit** → [`templates/README.md`](templates/README.md)
- **I want to implement this** → [`SPEC.md`](SPEC.md), then [`docs/architecture.md`](docs/architecture.md)
- **I want to consume brandkits in an agent** → [`SPEC.md`](SPEC.md) §11 and [`docs/context-compilation.md`](docs/context-compilation.md)
- **I came from v0.1** → [`docs/migration.md`](docs/migration.md)
- **I want to know what this is *not*** → [`docs/comparison.md`](docs/comparison.md)

## What `brandkit.md` is not

- Not a replacement for a full brand guideline — print production, motion and
  merchandise stay where they are, referenced from the Core.
- Not a design system: no components, no UI implementation.
- Not a new Markdown dialect. The Markdown is *output*.
- Not a mature tooling ecosystem yet. One reference implementation, an early
  standard, an open change process.

## Contributing

See [`CONTRIBUTING.md`](CONTRIBUTING.md). Changes to Core field names, enums or
required status are breaking and are tracked in
[`CHANGELOG.md`](CHANGELOG.md).

## License

See [`LICENSE`](LICENSE).
