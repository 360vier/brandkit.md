# Migration Map (Legacy -> v0.1)

## Previous state (before restructure)

- `README.md` mixed vision, format explanation, and specification content.
- `brand.md` and `styles.md` had overlapping rules.
- There was no formal normative source, no file-based reference profiles, and no contributor process docs.

## Redundancies in the previous model

- Portability and emoji rules were spread across multiple files.
- Example snippets and normative statements were not clearly separated.
- Profile logic existed conceptually but was not normalized into a clear file format.

## Target mapping

- Normative rules: `SPEC.md`
- Parser-oriented field view: `schema/brandkit-v0.1-fields.md`
- Learning and reference material: `EXAMPLES/*.brandkit.md`
- Context, boundaries, roadmap: `docs/`

## Legacy status

- `brand.md` and `styles.md` are marked as deprecated.
- New implementations should use `brandkit.md` based on `SPEC.md`.
