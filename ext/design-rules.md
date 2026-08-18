# Extension: Design Rules

- **Extension id:** `design-rules`
- **Version:** `0.1.0-draft`
- **Status:** draft
- **Requires:** `brandkit.md` Core `0.2.0-draft`
- **Namespace:** `x-designRules` in a Core document

## Why this is not in the Core

The Core separates three kinds of knowledge (SPEC §3):

- **Design tokens** hold values — `#101B2D`, `16px`, `Inter Tight`.
- **Brandkit Core** holds stance — voice, positioning, claims, policies.
- **Design rules** hold *application knowledge* — how the logo sits on a poster,
  which grid a landing page follows, how much air a headline needs.

Application knowledge is the layer that decides whether output looks like the
brand rather than merely uses its colours. It is kept as an extension because it
is highly implementation-shaped: rules are authored per brand, often derived
from existing artwork, and they are routed differently depending on what is
being produced.

## Data shape

```ts
type DesignRule = {
  id: string;
  category: "logo" | "spacing" | "grid" | "typography"
          | "composition" | "imagery" | "color";
  title: string;
  /** Imperative rule text. Goes into a prompt as written. */
  rule: string;
  /** Where the rule applies — see routing below. */
  appliesTo: "image" | "document" | "all" | "composition";
  /** Provenance: authored by a person, or proposed by a model from artwork. */
  source: "human" | "ai-extracted";
  /** Only `approved` rules reach a prompt. */
  status: "draft" | "approved";
  /** Author order — presentation and truncation priority. */
  position: number;
  /** Assets that demonstrate the rule geometrically (not style references). */
  exampleAssetIds?: string[];
};
```

In a Core document:

```json
{
  "x-designRules": [
    {
      "id": "rule-logo-corner",
      "category": "logo",
      "title": "Logo sits in the lower left",
      "rule": "Place the wordmark in the lower-left slot with one cap height of clearspace on both edges.",
      "appliesTo": "composition",
      "source": "human",
      "status": "approved",
      "position": 0
    },
    {
      "id": "rule-backlight",
      "category": "imagery",
      "title": "Backlight the subject",
      "rule": "Light the subject from behind so that the silhouette separates from the background; let shadow detail fall away.",
      "appliesTo": "image",
      "source": "human",
      "status": "approved",
      "position": 1
    }
  ]
}
```

## Routing — `appliesTo`

`appliesTo` is the load-bearing field. It is a routing instruction, not a label.

| Value | Reaches an image prompt | Reaches a document prompt | Implemented by |
| --- | --- | --- | --- |
| `image` | yes | no | generative model |
| `document` | no | yes | language model |
| `all` | yes | yes | both |
| `composition` | **never** | **never** | deterministic composition step |

`composition` MUST NOT reach any prompt. This is the extension's most important
rule, and it is empirical: given layout rules as prompt text, image models
execute them literally — they paint a logo into the photograph, draw the colour
panel, and render the grid lines as visible lines. The rule was obeyed and the
output was unusable.

Note that `all` means image **and** document, not "everywhere". Layout is not a
third prompt target; it is a different mechanism.

## Rules for consumers

1. Only `status: "approved"` rules MAY reach a prompt. `ai-extracted` proposals
   are candidates until a person approves them — a model's reading of an artwork
   is not a brand statement.
2. Image prompts have a hard budget. A consumer MUST cap the rendered rule block,
   MUST prioritise by `position`, and MUST report how many rules were omitted
   rather than truncating silently. (The reference implementation caps at ~1500
   characters; at 900 only 4 of 11 rules survived and two essential lighting
   rules were lost without any signal.)
3. Document prompts SHOULD receive the full set, grouped by category — for a
   landing page, grid and spacing are exactly the details that otherwise get
   invented.
4. Rules MUST live in one place. Copying a rule into a content type or template
   means every future change has to be chased through each copy.
5. `exampleAssetIds` demonstrate geometry, not style. They MUST NOT be passed to
   an image model as style references: a grid diagram used as a style reference
   drags the whole image toward diagram.

## Rendering

For document prompts and `design.md`, rules render grouped by category in this
order: `logo`, `grid`, `spacing`, `typography`, `composition`, `color`,
`imagery`.

```md
## Design System Rules

### logo
- **Logo sits in the lower left:** Place the wordmark in the lower-left slot …
```

For image prompts, rules render as a compact imperative block inside the
protected prompt region (SPEC §11.2):

```
=== DESIGN SYSTEM RULES ===
- Backlight the subject: Light the subject from behind so that …
```

## Open points

- Whether `composition` rules should carry structured geometry (slots, zones)
  instead of prose, given that no model ever reads them.
- Whether `category` should be extensible per brand.
- Promotion path into the Core once the routing model has held across more
  implementations.
