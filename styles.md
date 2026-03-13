# Markdown Style

This file defines layout and formatting rules for branded Markdown documents.

## Default Style Profile

If no profile is selected, use all sections in this file as the default style contract.

## Document Start

Documents begin with a brand header, then the document title.

Example:

```md
◆ ACME

# Document Title
```

Use a horizontal rule after the title block when appropriate:

```md
---
```

## Heading Structure

Maximum heading depth: 3

Allowed pattern:

```md
# Title
## Section
### Subsection
```

Do not skip heading levels.

## Divider

Use standard horizontal rules:

```md
---
```

Avoid decorative divider syntax.

## Callouts

Use blockquote syntax for notes and warnings.

Example:

```md
> NOTE
> Important information
```

Keep callout labels short and uppercase.

## Tables

Prefer Markdown tables for structured data.

- Maximum columns: 6
- Keep headers concise
- Avoid empty columns

## Spacing Rules

- Use one blank line between major blocks
- Avoid trailing spaces
- Keep line length readable for raw Markdown review

## Portability Rules

- All core content must remain valid plain Markdown
- Styled rendering may enhance typography and layout, but must not change document meaning

## Emoji Usage

Emoji are allowed in documents as supporting style elements when used consistently.

### Emoji Policy

- Use literal Unicode emoji characters, not shortcode syntax.
- Use a small, fixed emoji set from `brand.md`.
- Use emoji by semantic meaning, not visual decoration.
- Do not use emoji as the only carrier of critical meaning.

### Placement Rules

- Allowed: document header accent, section titles, callout labels, checklist items.
- Limited use: one emoji per heading or callout label.
- Avoid: emoji-only headings, repeated decorative sequences, dense inline clusters.

### Accessibility Rules

- Pair emoji with text labels (for example `> ⚠️ Warning`).
- Keep documents understandable if emoji rendering differs by platform.
- Prefer stable symbols when alignment-sensitive content is required.

## Metadata (Optional)

Front matter can be used when a toolchain supports it.

Example:

```md
---
title: Strategy Memo
owner: Product Team
status: Draft
---
```

If front matter is unsupported, include metadata in plain headings and lists.

## Link and Reference Style

- Use descriptive link text, avoid raw URLs in prose.
- Prefer relative links for repository-local references.
- Add a short context sentence before critical references.
- Keep one canonical link per source when possible.

## List Style

- Use unordered lists for unordered collections.
- Use ordered lists for sequences and procedures.
- Use task lists for actionable check items.
- Keep bullet wording parallel and concise.

Example:

```md
- Define scope
- Draft outline
- Review with stakeholders
```

## Code Block Style

- Always include language identifiers when known.
- Keep command examples copy-paste friendly.
- Separate command and output when clarity matters.
- Use inline code for short identifiers and file names.

## Media and Caption Pattern

- Add short, explicit alt text for images.
- Include a caption sentence below important visuals.
- Explain why the media is relevant to the section.
- Provide a text fallback summary for non-visual readers.

## Naming and Terminology

- Keep core terms stable across documents (`Brand Token`, `Emoji Element`, `Fallback`).
- Use one preferred term per concept; avoid synonym drift.
- Capitalize formal section labels consistently.

## Document Templates

### Memo Template

```md
◆ ACME

# Memo Title

---

## Context
## Decision
## Next Steps
```

### Guide Template

```md
◆ ACME

# Guide Title

---

## Goal
## Prerequisites
## Steps
## Troubleshooting
```

### Release Notes Template

```md
◆ ACME

# Release Notes

---

## Highlights
## Improvements
## Fixes
## Known Issues
```

## Profile Overrides (Optional)

Use profile overrides for subbrands or products while staying in a single-file setup.

### Profile: acme-enterprise

- Header pattern:

```md
◆ ACME Enterprise

# Document Title
```

- Callout labels: NOTE, WARNING, DECISION

### Profile: nebula

- Header pattern:

```md
◆ Nebula

# Document Title
```

- Preferred section order for product specs: Overview, User Value, Constraints, Implementation Notes, Rollout

## Profile Resolution Rules

- Start with `Default Style Profile`.
- Apply selected profile overrides where explicitly defined.
- Any missing style value falls back to default.
- Profiles must keep portability, heading depth, and core Markdown compatibility unchanged.
