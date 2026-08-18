# Extension: Text-Only Brand Expressions

- **Extension id:** `text-expressions`
- **Version:** `0.1.0-draft`
- **Status:** draft
- **Requires:** `brandkit.md` Core `0.2.0-draft`
- **Namespace:** `x-textExpressions` in a Core document
- **Predecessor:** the required `## brand expressions` section of v0.1

## What it solves

Brands increasingly appear where no visual asset can render: a terminal, a log
line, a commit message, a plain-text email, a chat client that strips markup, an
agent writing into a CI summary. In those places a brand either has a defined
reduced form, or it degrades into whatever the writer improvises.

This extension defines that reduced form. It was a required part of v0.1 and was
moved out of the Core in v0.2 because most brand programmes do not need it,
while those that do need it precisely and unambiguously.

## Data shape

```ts
type TextExpressions = {
  /** Wordmark in plain uppercase text. Required if the extension is present. */
  fallbackWordmark: string;
  /** Inline signature for running text and log lines. Required. */
  plainTextSignature: string;
  /** Rules that keep the identity recognisable without assets. Required. */
  textOnlyIdentityRules: string[];
  /** Multi-line ASCII rendering of the mark. */
  asciiMark?: string;
  /** Wider banner form, e.g. for CLI startup output. */
  terminalBanner?: string;
  /** Emoji shorthand. A textual fallback is mandatory when present. */
  emojiSignature?: { value: string; fallback: string; source?: Source };
  /** Prompt string for CLI and shell contexts. */
  cliPromptStyle?: { value: string; source?: Source };
  /** Provenance of a non-trivial expression. */
  source?: Source;
};

type Source = "human" | "tool" | "hybrid";
```

## Example

```json
{
  "x-textExpressions": {
    "fallbackWordmark": "HELION SYSTEMS",
    "plainTextSignature": "[HELION]",
    "asciiMark": "|-| E L I O N",
    "cliPromptStyle": { "value": "helion-secure>", "source": "human" },
    "emojiSignature": { "value": "✅", "fallback": "[HELION]", "source": "hybrid" },
    "textOnlyIdentityRules": [
      "Uppercase the wordmark; never title-case it.",
      "Keep the bracketed signature unspaced: [HELION], not [ HELION ].",
      "In monospace contexts, letterspacing is decorative — drop it before breaking a line."
    ],
    "source": "human"
  }
}
```

Rendered in the four contexts it exists for:

```txt
Plain text header      HELION SYSTEMS
Terminal rendering     |-| E L I O N
Emoji + fallback       ✅ [HELION]
CLI prompt             helion-secure>
```

## Normative rules

1. `fallbackWordmark`, `plainTextSignature` and `textOnlyIdentityRules` are
   REQUIRED when this extension is present. An expression set without them has
   no defined degradation path, which is the entire point.
2. Expressions MUST work with no visual assets, no colour and no markup, and
   MUST stay recognisable in a monospace context.
3. `fallbackWordmark` and `plainTextSignature` MUST remain stable across brand
   versions. Downstream systems treat them as identifiers; churn there is a
   broken reference, not a refresh.
4. `emojiSignature` MAY vary and MUST carry a `fallback`. An emoji is not a
   brand asset: rendering differs per platform, some clients strip it, and
   screen readers announce it. Consumers MUST use `fallback` wherever the
   rendering context is unknown.
5. `asciiMark` and `terminalBanner` MUST be reproduced verbatim, preserving
   whitespace. A consumer MUST NOT re-wrap or re-align them.
6. Tool-proposed expressions MUST be marked `source: "tool"` and MUST be treated
   as candidates until a human approves them. Core identity is curated; a
   generated wordmark variant is a suggestion.
7. Expressions MUST NOT be generated at consumption time. If the field is
   absent, the consumer uses `fallbackWordmark` — it does not invent an ASCII
   mark on the spot.

## Mapping from v0.1

| v0.1 field (`## brand expressions`) | This extension |
| --- | --- |
| `fallback_wordmark` | `fallbackWordmark` |
| `plain_text_signature` | `plainTextSignature` |
| `text_only_identity_rules` | `textOnlyIdentityRules` |
| `ascii_mark` | `asciiMark` |
| `terminal_banner` | `terminalBanner` |
| `emoji_signature.value` / `.fallback` / `.source` | `emojiSignature.value` / `.fallback` / `.source` |
| `cli_prompt_style.value` / `.source` | `cliPromptStyle.value` / `.source` |
| `display_name`, `short_name` | Core: `metadata.name`, and `x-textExpressions.plainTextSignature` for the short form |

## Rendering

Where a Renderer supports this extension, it appends a section after
`## Visual Language`:

```md
## Text Expressions

**Fallback wordmark:** HELION SYSTEMS
**Plain text signature:** [HELION]
**CLI prompt:** `helion-secure>`

**ASCII mark:**
```
|-| E L I O N
```

**Text-only identity rules:**
- Uppercase the wordmark; never title-case it.
```

## Open points

- Whether emoji policy belongs here or in a broader accessibility extension.
- Whether `terminalBanner` needs a declared maximum width.
- Whether generated candidates should carry a confidence value alongside
  `source`.
