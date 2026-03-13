# BrandKit.md

BrandKit.md is a lightweight, portable concept for adding brand identity to Markdown documents without breaking Markdown compatibility.

Markdown is the default format for documentation, knowledge bases, and AI-generated content, but it has no native brand layer. BrandKit.md fills that gap with two plain Markdown files:

- `brand.md`
- `styles.md`

Together, these files define a project's BrandKit.

## Why BrandKit.md

BrandKit.md enables:

- consistent, branded Markdown generation by AI agents
- reusable brand rules across repositories and tools
- renderer enhancements without changing source Markdown
- full readability in plain Markdown environments

## Design Principles

- **Portable first:** documents stay valid in any Markdown viewer
- **Simple artifacts:** no custom format; only Markdown files
- **Renderer optionality:** enhanced rendering is additive, not required
- **Agent-friendly:** rules are explicit and machine-readable as text

## Core Files

### `brand.md`

Defines brand identity primitives for portable and styled rendering.

Typical sections:

- Brand token (single symbol for portable Markdown)
- Wordmark
- Pixel mark (terminal-style logo)
- Optional SVG mark guidance
- Optional color system

The brand token and pixel mark are the guaranteed portable fallback.

### `styles.md`

Defines document layout and formatting rules for authored and generated Markdown.

Typical sections:

- Document start pattern (brand header + title flow)
- Heading hierarchy and maximum depth
- Divider usage
- Callout pattern
- Table conventions
- Spacing rules

## Rendering Modes

BrandKit.md supports two compatible modes.

### Portable mode (required baseline)

- Pure Markdown only
- Works in any viewer
- Uses token, text, and standard Markdown syntax

Example:

```md
◆ ACME

# Strategy Memo
```

### Styled mode (optional enhancement)

- Compatible renderer may apply layout, typography, and color
- May use SVG mark and richer visual styling
- Source Markdown remains unchanged and portable

## Agent Integration

Before generating project Markdown, an AI agent should:

1. Read `brand.md`
2. Read `styles.md`
3. Apply brand token, structure, and formatting rules
4. Keep output valid, portable Markdown

### Agent Output Contract

- Follow heading limits and structure from `styles.md`
- Use the brand token and header pattern from `brand.md` and `styles.md`
- Use only standard Markdown syntax for core document meaning
- Treat styled rendering as optional presentation

## Minimal Starter Files

Use these files at repository root:

- `brand.md` for identity primitives
- `styles.md` for document style rules

Any document generator or AI workflow can then consume both files before writing content.

## Non-Goals (MVP)

- Defining a new Markdown dialect
- Requiring custom parsers for basic usage
- Forcing visual rendering features on non-compatible viewers

## License

See `LICENSE`.
