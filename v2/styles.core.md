# Core Markdown Style

Global style defaults used by all subbrands and products.

## Document Header

```md
◆ ACME

# Document Title
```

## Heading Policy

- Maximum depth: 3
- Do not skip heading levels

## Callout Policy

Use blockquotes with labels:

```md
> NOTE
> Message text
```

## Emoji Usage Policy

- Use emoji only from the active brand `Allowed Emoji Set`.
- Keep emoji semantic and sparse.
- Do not make emoji the sole meaning carrier.

## Tables

- Maximum columns: 6
- Keep concise headers and stable units

## Allowed Overrides

Subbrands and products may override:

- `Document Header` token/wordmark rendering
- callout label vocabulary
- template sections

Subbrands and products must not override:

- heading depth limit
- portability rules

## Portability Rules

- Core meaning must be readable in plain Markdown.
- Styled rendering is optional and additive.
