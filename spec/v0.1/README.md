# v0.1 — Archive

**Not normative.** The current specification is [`SPEC.md`](../../SPEC.md)
(`0.2.0-draft`).

This directory preserves v0.1 as it stood, for repositories that still contain
v0.1 files and for anyone tracing why the model changed.

| Path | Contents |
| --- | --- |
| `SPEC.md` | The v0.1 specification (`0.1.0-draft`) |
| `schema/brandkit-v0.1-fields.md` | Parser-oriented field map |
| `EXAMPLES/` | Four v0.1 profiles: startup, enterprise, personal brand, minimal |
| `templates/` | Blank Markdown starters, with and without frontmatter |
| `legacy/` | The even earlier two-file model (`brand.md` + `styles.md`) |
| `migration-from-two-file-model.md` | The earlier migration note, superseded |

## Why v0.1 was replaced

v0.1 treated a single hand-written Markdown file as the data model. It read well
and could not carry three things:

1. **Values.** A model needs `#101B2D`, not "a deep navy". Prose produces output
   that is nearly right in a way nobody can debug.
2. **State.** A rule is either approved or somebody's draft. Markdown has nowhere
   to put that, so every line in the file looked equally binding.
3. **Negative facts.** "This certification exists, is unverified, and must
   therefore not be claimed" cannot be written in prose without inviting a model
   to claim it helpfully.

v0.2 keeps the Markdown as a **generated rendering** and makes a structured Core
document the model. Text-only brand expressions, required in v0.1, moved to
[`ext/text-expressions.md`](../../ext/text-expressions.md) unchanged in
substance.

## Migrating

[`docs/migration.md`](../../docs/migration.md) has the field-by-field mapping and
the conversion steps.
