# Extensions

Extensions add optional capability without enlarging the Core. The Core stays
small enough that every brand can fill it honestly; anything that only some
brands need lives here.

| Extension | Version | Status | Purpose |
| --- | --- | --- | --- |
| [text-expressions](text-expressions.md) | `0.1.0-draft` | draft | Text-only brand renderings — fallback wordmark, ASCII mark, emoji signature, CLI prompt. Was required in v0.1. |
| [design-rules](design-rules.md) | `0.1.0-draft` | draft | Application knowledge — logo placement, grid, spacing — with `appliesTo` routing that keeps layout rules out of image prompts. |

## Rules for extensions

An extension:

- lives in its own document here, with its own version and status,
- MUST NOT redefine or contradict a Core field,
- namespaces its data: `x-`prefixed keys in a Core document, an own
  `$extensions` namespace in a DTCG token set,
- states its rendering, if it has one.

Consumers ignore unknown extension data rather than failing on it. See
`SPEC.md` §12.

## Proposing one

Open an issue describing the failure mode first — what breaks today without the
extension — before proposing fields. Extensions that exist only in a
specification and in no implementation are how a standard accumulates dead
surface.
