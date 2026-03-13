# Markdown Style

This file defines layout and formatting rules for branded Markdown documents.

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
