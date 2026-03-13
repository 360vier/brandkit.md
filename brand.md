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

## Usage Rules

- Portable documents must never depend on SVG or color rendering.
- The brand token should be used in document headers when possible.
- Keep token and wordmark stable across all project documents.
