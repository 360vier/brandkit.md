# brandkit.md

`brandkit.md` is an open, Markdown-native specification in progress for expressing brand context consistently for humans, LLMs, and agents.

The project focuses on a pragmatic standard approach:

- human-readable in raw text,
- AI-readable with explicit structure,
- parser- and validator-friendly over time,
- easy to version in Git.

## Brand rendering showcase

```txt
Plain text header
HELION SYSTEMS
```

```txt
Terminal rendering
|-| E L I O N
```

```txt
Emoji + fallback
✅ [HELION]
```

```txt
CLI prompt style
helion-secure>
```

```md
## brand expressions
- fallback_wordmark: HELION SYSTEMS
- plain_text_signature: [HELION]
- ascii_mark: |
    |-| E L I O N
- emoji_signature:
    value: "✅"
    fallback: "[HELION]"
- cli_prompt_style:
    value: "helion-secure>"
```

See full profiles in `EXAMPLES/enterprise.brandkit.md` and `EXAMPLES/personal-brand.brandkit.md`.

## What `brandkit.md` is

- A convention plus formal specification (`v0.x`) for AI-readable brand context.
- A single-file format (`brandkit.md`) with a defined minimum structure.
- A foundation for future tooling (validators, generators, linters).

## What `brandkit.md` is not

- Not a new Markdown dialect.
- Not a replacement for complete brand guidelines or a design system.
- Not a claim of an already mature tooling ecosystem.

## Project status

- **Status:** Standard-in-progress
- **Spec version:** `0.1.0-draft`
- **Maturity:** Early, but intentionally structured and normative

## Try in 60 seconds

```bash
cp templates/blank.brandkit.md brandkit.md
```

Then fill the required sections in order:

1. `# brand overview`
2. `## mission and positioning`
3. `## audience`
4. `## voice and tone`
5. `## messaging`
6. `## brand expressions`
7. `## do and dont`
8. `## usage notes for ai systems`

For validation, use:

- `SPEC.md`
- `schema/brandkit-v0.1-fields.md`

## Quick start

1. Read the normative specification in `SPEC.md`.
2. Start from one profile in `EXAMPLES/`.
3. Create your own `brandkit.md` using the required sections.
4. Check consistency against `schema/brandkit-v0.1-fields.md`.
5. Use a blank starter file from `templates/`.

## Repository structure

- `SPEC.md` - normative rules (`MUST`, `SHOULD`, `MAY`)
- `EXAMPLES/` - reference profiles
- `docs/principles.md` - design principles and trade-offs
- `docs/faq.md` - common questions and non-goals
- `docs/comparison.md` - boundaries against adjacent artifacts
- `docs/roadmap.md` - v0.x to v2 path
- `docs/migration.md` - migration from the legacy two-file model
- `schema/brandkit-v0.1-fields.md` - parser-oriented field mapping
- `templates/blank.brandkit.md` - blank starter without frontmatter
- `templates/blank.brandkit.frontmatter.md` - blank starter with frontmatter
- `templates/README.md` - 30-second copy/paste quick start
- `CONTRIBUTING.md` - contribution process
- `CHANGELOG.md` - tracked changes

## Legacy note

The earlier files `brand.md` and `styles.md` remain as legacy references but are no longer the canonical model. The normative source is `SPEC.md`.

## Writing convention

Standardized section names in `brandkit.md` use lowercase to simplify parsing and consistency.

## License

See `LICENSE`.
