# brandkit.md Specification v0.1.0-draft

## 1. Purpose

`brandkit.md` defines portable, AI-readable brand context in a single Markdown file.

This specification is intended for:

- content and documentation teams using AI workflows,
- tooling developers building parsers, validators, or generators,
- open-source contributors who need interoperable brand rules.

## 2. Normative Language

The keywords `MUST`, `SHOULD`, and `MAY` are normative as described in RFC 2119.

## 3. Design Principles

- Human-readable first
- AI-readable by design
- Markdown-native
- Progressively structured
- Portable across tools
- Versionable in Git
- Easy to author, easy to extend

## 4. Scope and Non-goals

### In Scope

- structure and semantics of `brandkit.md`,
- required vs optional sections,
- required core fields for AI usage,
- rules for reduced brand renderings (`brand expressions`).

### Out of Scope

- full replacement of brand guidelines,
- renderer-specific specs for colors, fonts, or UI behavior,
- mandatory JSON/YAML-only authoring.

## 5. File Identity and Versioning

- The file name SHOULD be `brandkit.md`.
- A file MAY include YAML frontmatter.
- If frontmatter is present, `spec_version` MUST be set.
- The canonical normative source is this file (`SPEC.md`).
- If local `spec_version` conflicts with this spec, `SPEC.md` wins.

### 5.1 Optional Frontmatter Example

```md
---
spec_version: 0.1.0-draft
profile_id: startup
status: draft
---
```

## 6. Document Structure

### 6.1 Required Sections

A valid `brandkit.md` MUST include at least these sections (lowercase):

1. `# brand overview`
2. `## mission and positioning`
3. `## audience`
4. `## voice and tone`
5. `## messaging`
6. `## brand expressions`
7. `## do and dont`
8. `## usage notes for ai systems`

### 6.2 Optional Sections

- `## visual identity`
- `## example outputs`
- `## governance metadata`

### 6.3 Heading Conventions

- Heading depth MUST remain between `#` and `###`.
- Heading levels MUST NOT be skipped.
- Section names SHOULD match this specification exactly for robust parsing.
- Section names SHOULD be lowercase.

## 7. Required Core Fields

These fields MUST be present in the listed sections.

### 7.1 brand overview

- `display_name`
- `short_name`

### 7.2 voice and tone

- `tone_attributes` (list)
- `avoid_patterns` (list)

### 7.3 messaging

- `core_message`
- `proof_points` (list)

### 7.4 brand expressions

- `fallback_wordmark`
- `plain_text_signature`
- `text_only_identity_rules` (list)

### 7.5 usage notes for ai systems

- `allowed_contexts` (list)
- `restricted_contexts` (list)
- `fallback_behavior`

## 8. Brand Expressions (Normative)

The section `## brand expressions` defines reduced brand renderings for text-first and terminal-like environments.

### 8.1 Goal

Brand expressions MUST work without visual assets and preserve recognizability.

### 8.2 Expression Schema v0.1

The following fields are defined:

- `display_name` (human curated)
- `short_name` (human curated)
- `fallback_wordmark` (required, human curated)
- `plain_text_signature` (required)
- `ascii_mark` (optional)
- `terminal_banner` (optional)
- `emoji_signature` (optional, only with fallback behavior)
- `cli_prompt_style` (optional)
- `text_only_identity_rules` (required)

### 8.3 Source Attribution

Each non-trivial expression SHOULD include a source marker:

- `source: human`
- `source: tool`
- `source: hybrid`

### 8.4 Stability Rules

- `fallback_wordmark` and `plain_text_signature` MUST remain stable.
- `emoji_signature` MAY vary but MUST include a textual fallback.
- Tool-generated proposals MUST be marked as candidates until human-approved.

## 9. Parsability Model (v0.x)

`brandkit.md` is semi-structured in v0.x.

- Humans author regular Markdown text.
- Parsers rely on stable section names and field labels.
- YAML frontmatter is optional, not required.

This model SHOULD evolve into stricter validator rules in later versions.

## 10. Profiles and Overrides

v0.1 primarily describes one profile per file.

- Multiple profiles MAY be organized as separate files in `EXAMPLES/` or project-specific paths.
- In-file multi-profile syntax is intentionally left non-normative in v0.x to keep the standard lightweight.

## 11. Minimal Valid Skeleton

```md
# brand overview
- display_name: Example Brand
- short_name: Example

## mission and positioning
- mission: ...
- positioning: ...

## audience
- primary_segments:
  - ...

## voice and tone
- tone_attributes:
  - ...
- avoid_patterns:
  - ...

## messaging
- core_message: ...
- proof_points:
  - ...

## brand expressions
- fallback_wordmark: EXAMPLE
- plain_text_signature: [EXAMPLE]
- text_only_identity_rules:
  - ...

## do and dont
- do:
  - ...
- dont:
  - ...

## usage notes for ai systems
- allowed_contexts:
  - ...
- restricted_contexts:
  - ...
- fallback_behavior: ...
```

## 12. Compatibility Notes

- Legacy artifacts (`brand.md`, `styles.md`) MAY remain as references.
- New implementations SHOULD use the single-file `brandkit.md` model.

## 13. Change Policy

- v0.x allows targeted structural refinement.
- Breaking changes MUST be labeled as breaking in `CHANGELOG.md`.
- v1.0 requires stable required sections and field names.
