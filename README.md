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
- Optional emoji elements (Unicode-first with fallback token)
- Wordmark
- Pixel mark (terminal-style logo)
- Optional SVG mark guidance
- Optional color system

The brand token and pixel mark are the guaranteed portable fallback. Emoji can be used as additional portable accents when defined with clear fallback rules.

### `styles.md`

Defines document layout and formatting rules for authored and generated Markdown.

Typical sections:

- Document start pattern (brand header + title flow)
- Heading hierarchy and maximum depth
- Divider usage
- Callout pattern
- Table conventions
- Spacing rules
- Emoji usage policy
- Reusable style patterns (metadata, links, lists, code blocks, media, templates)

## Rendering Modes

BrandKit.md supports two compatible modes.

### Portable mode (required baseline)

- Pure Markdown only
- Works in any viewer
- Uses token, text, and standard Markdown syntax
- Supports Unicode emoji characters as optional text elements

Example:

```md
◆ ACME

# Strategy Memo
```

### Styled mode (optional enhancement)

- Compatible renderer may apply layout, typography, and color
- May use SVG mark and richer visual styling
- Source Markdown remains unchanged and portable

## Style Patterns

Beyond core structure rules, BrandKit can define reusable style patterns in `styles.md` for:

- metadata conventions
- links and references
- list patterns
- code block formatting
- media and caption usage
- naming and terminology consistency
- template skeletons (memo, guide, release notes)

These patterns make cross-document output more consistent for both humans and AI agents.

## Agent Integration

Before generating project Markdown, an AI agent should:

1. Read `brand.md`
2. Read `styles.md`
3. Apply brand token, structure, and formatting rules
4. Keep output valid, portable Markdown

### Agent Output Contract

- Follow heading limits and structure from `styles.md`
- Use the brand token and header pattern from `brand.md` and `styles.md`
- If emoji are enabled, use only the allowed emoji set from `brand.md`
- Prefer literal Unicode emoji and apply fallback token behavior when needed
- Use only standard Markdown syntax for core document meaning
- Treat styled rendering as optional presentation

## Minimal Starter Files

Use these files at repository root:

- `brand.md` for identity primitives
- `styles.md` for document style rules

Any document generator or AI workflow can then consume both files before writing content.

## V2 Structure (Subbrands and Products)

For larger organizations, BrandKit can be layered to support master brand, subbrands, and product-level overrides while keeping Markdown portability.

### Layer Model

1. `core` defines global defaults (token policy, voice baseline, patterns).
2. `subbrand` inherits core and can override approved fields.
3. `product` inherits subbrand and can override approved fields.
4. `document` follows the resolved ruleset at generation time.

### Suggested Repository Layout

```text
v2/
  brand.core.md
  styles.core.md
  voice.md
  patterns.md
  governance.md
  brands/
    acme-enterprise/
      brand.md
      styles.md
      products/
        nebula/
          brand.md
          styles.md
```

### Inheritance and Override Policy

- Always inherit from `core` first, then subbrand, then product.
- Allow only explicit overrides (`Allowed Overrides` sections in each file).
- Keep portable semantics stable: token fallback, heading limits, callout syntax.
- Never require renderer-specific features for core meaning.

### Agent Resolution Order

Before writing a product document, an AI agent should resolve files in this order:

1. `v2/brand.core.md`
2. `v2/styles.core.md`
3. `v2/voice.md`
4. `v2/patterns.md`
5. matching subbrand `brand.md` and `styles.md`
6. matching product `brand.md` and `styles.md`

If conflicting rules appear, the nearest layer wins only when that field is explicitly overridable.

## Non-Goals (MVP)

- Defining a new Markdown dialect
- Requiring custom parsers for basic usage
- Forcing visual rendering features on non-compatible viewers

## License

See `LICENSE`.
