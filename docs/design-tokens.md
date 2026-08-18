# Design Tokens

The token layer answers exactly one question: **what values?** Colour, typeface,
spacing step, radius, elevation. Nothing about where a value may be used — that
is stance (Core) or application knowledge (design rules).

Normative rules are in [`SPEC.md`](../SPEC.md) §9. This document explains the
reasoning and shows the shapes.

## DTCG, with a profile

The canonical format is **DTCG (W3C Design Tokens)** JSON: `$value`, `$type`,
`$description`. `brandkit.md` does not define a token format — it defines a
profile on top of DTCG, kept in `$extensions["md.brandkit"]`.

The consequence matters more than the syntax: a token set stays readable by any
DTCG-aware tool. Style Dictionary, a Figma plugin or a design tool sees a valid
set; the profile data is namespaced extra.

```json
{
  "color": {
    "brand": {
      "primary": {
        "$type": "color",
        "$value": "#101B2D",
        "$description": "Carries the page.",
        "$extensions": {
          "md.brandkit": { "order": 0, "group": "brand", "inputType": "color" }
        }
      }
    }
  }
}
```

## Category roots

Top-level keys carry the category, processed in this order:

`color` → `typography` → `spacing` → `size` → `border` → `shadow` → `opacity`

A legacy top-level `radius` root is treated as `border`.

## The profile fields

| Field | Why it exists |
| --- | --- |
| `order` | Author order. **Not cosmetic:** JSON object key order is not preserved by common storage — Postgres `jsonb` normalises keys shortest-first, then alphabetically. Without an explicit index, the order a designer authored is lost and the UI shows a random arrangement. |
| `group` | Sub-grouping inside a category (`brand`, `neutral`, `surface`, `state`). |
| `inputType` | The value's semantics and its authoring control: `color`, `dim`, `weight`, `ratio`, `pct`, `select`, `font`, `shadow`. |
| `unit` | Unit for `dim` values (`px`, `rem`, `ch`). |
| `tags` | Subset of the set-level vocabulary, e.g. marking which tokens the composition layer consumes. |
| `options` | Permitted values for `inputType: select`. |
| `segments` | Per-segment values (below). |

`inputType` maps onto DTCG `$type` as follows:

| `inputType` | `$type` |
| --- | --- |
| `color` | `color` |
| `dim` | `dimension` |
| `weight` | `fontWeight` |
| `font` | `fontFamily` |
| `shadow` | `shadow` |
| `ratio`, `pct` | `number` |
| `select` | `string` |

## Value forms

**Dimensions** serialise as number plus unit in one string: `"16px"`, `"68ch"`.

**Font families** are a CSS stack. The first non-generic entry is the brand
typeface; generics (`sans-serif`, `serif`, `monospace`, `system-ui`) carry no
brand information:

```json
{ "$type": "fontFamily", "$value": ["Inter Tight", "Helvetica Neue", "sans-serif"] }
```

**Shadows** serialise structurally, with alpha in percent:

```json
{ "$type": "shadow", "$value": { "offsetX": 0, "offsetY": 2, "blur": 8, "spread": 0, "alpha": 12 } }
```

**References** use `{token.path}` and are resolved recursively:

```json
{ "color": { "surface": { "page": { "$type": "color", "$value": "{color.neutral.0}" } } } }
```

A reference is not a convenience — it is a statement that the page ground
*follows* the neutral ramp. Resolution is segment-aware (a reference can point at
a token that differs by segment), bounded against cycles, and an unresolvable
reference stays visible rather than being replaced by a plausible default.

## Segments — one foundation, N expressions

Many brands have sub-brands, product lines or tonal variants that share almost
everything. Cloning a whole token set per variant means every future CD change
has to be made several times, and one of the copies will be missed.

Instead, the brand declares its segments **once**, at set level:

```json
{
  "$extensions": {
    "md.brandkit": {
      "segments": [
        { "id": "core", "label": "Core Platform" },
        { "id": "labs", "label": "Labs (Early Access)" }
      ],
      "tags": ["screen", "print", "composition"]
    }
  }
}
```

and individual tokens deviate only where the brand genuinely does:

```json
{
  "color": { "brand": { "accent": {
    "$type": "color",
    "$value": "#1F9E8C",
    "$extensions": { "md.brandkit": {
      "order": 1, "group": "brand", "inputType": "color",
      "segments": { "core": "#1F9E8C", "labs": "#C2571E" }
    } }
  } } }
}
```

Rules that keep this honest:

- **Segment values live in exactly one place** — the token's own `segments` map.
  Never in a channel profile, a content type or a template.
- **`$value` keeps a valid value** (the first segment value) so plain DTCG
  consumers still work. The authoritative truth is the `segments` map.
- **Resolution substitutes, then falls back**: use the segment value if present,
  otherwise the shared one.
- **Keep deviation rare.** In the reference profile, 40 of 43 tokens are shared.
  The ratio *is* the argument for the model.

## design.md

Token sets get a Markdown rendering for humans and models. It contains a table
per category, per-segment values shown explicitly, and CSS custom properties —
shared values in `:root`, segment deviations as override blocks:

```css
:root {
  --color-brand-accent: #1F9E8C;
  --typography-fontSize-display: 56px;
}

[data-segment="labs"] {
  --color-brand-accent: #C2571E;
  --typography-fontSize-display: 44px;
}
```

Property names derive from the token path, `.` → `-`.

See [`../EXAMPLES/helion-systems.design.md`](../EXAMPLES/helion-systems.design.md)
for a full rendering, generated by the reference implementation.

## What consumers actually need

Two shapes, for two different budgets:

**Language models** get the full rendering. There is no space problem: grid,
spacing and the whole scale are exactly the details a landing page otherwise
invents.

**Image models** get a handful of lines, placed in the protected prompt region:

```
=== BRAND ANCHORS (non-negotiable) ===
Brand palette — use ONLY these colours for any graphic area, tint or accent:
#101B2D (brand primary), #1F9E8C (brand accent), …
Do not introduce colours outside this palette. Photographic content keeps its natural colours.
Brand typeface is Inter Tight — a clean geometric sans. Do not render headlines,
logos or wordmarks in the image; they are composed separately.
```

Why so short: the concrete values used to sit inside the long compiled context
and were the first thing dropped when the prompt exceeded the model's budget —
measured as present in the context and absent from the final prompt. The model
received prose about colour but never a colour. A hex value cannot be
misunderstood; "warm orange" can. See
[`context-compilation.md`](context-compilation.md).

## Typography has a second consumer

A token value like `"Inter Tight, Helvetica Neue, sans-serif"` also has to match
an actual font **file** when a headline is composed deterministically. Two
practical constraints from the reference implementation:

- Match font families **normalised** and family-wise, not by filename. A CSS
  stack never matches an asset called `MonaSansVF[wght] - TTF` on an exact
  comparison, and the fallback silently burns the headline in a system font.
- The composition renderer needs a **static cut** (TTF/OTF/WOFF). Variable fonts
  and WOFF2 are not usable there. A brand typeface without a static cut cannot
  be composed, so the token set SHOULD note which cut exists.

A brand font is closer to the brand than a system font even when the weight is
imperfect — but the failure must be visible, not silent.
