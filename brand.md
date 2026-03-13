# Brand

This file defines the portable brand identity for Markdown documents.

## Brand Token

Use exactly one symbol that can appear in plain text and Markdown.

◆

## Wordmark

ACME Corporation

## Pixel Mark

Use a terminal-friendly logo that remains readable in monospaced text.

```text
   ██
  █  █
 █    █
 █    █
  █  █
   ██
```

## SVG Mark (Optional)

Use a monochrome SVG when supported by the renderer.
If SVG is not supported, fall back to token + wordmark and/or pixel mark.

## Colors (Optional)

Define optional semantic colors for compatible styled renderers.

- Primary: neutral
- Accent: optional

## Emoji Elements (Optional)

Emoji can be used as portable brand elements when they are written as literal Unicode characters.

### Primary Emoji

✨

### Allowed Emoji Set

Use a small, stable set only.

- ✨ Primary brand accent
- ✅ Confirmation and completion
- ⚠️ Caution and risk
- 💡 Insight and recommendation

### Fallback Token

If emoji rendering is unavailable, inconsistent, or visually noisy, use the brand token:

◆

### Portability Rules

- Prefer literal emoji characters over renderer-specific shortcodes (for example `:sparkles:`).
- Do not rely on emoji color, platform style, or font-specific appearance.
- Emoji must support text meaning, never replace required words by themselves.

### Placement Rules

- Allowed: document header accent, callout labels, section markers.
- Avoid: dense paragraph text, table cells where alignment is critical, repeated decorative chains.

## Usage Rules

- Portable documents must never depend on SVG or color rendering.
- The brand token should be used in document headers when possible.
- Keep token and wordmark stable across all project documents.
- If emoji are used, keep them consistent with the allowed emoji set and fallback rules.
