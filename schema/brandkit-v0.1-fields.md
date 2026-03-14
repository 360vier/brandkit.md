# brandkit v0.1 Field Map

This document is a parser-oriented precursor for future JSON Schema or validator implementations.

## Parsing Baseline

- Section titles are used as primary anchors.
- Field names are expected as list keys (`- key: value`).
- Free-form text is allowed as long as required fields stay unambiguous.

## Required Sections and Fields

## brand overview
- display_name: string (required)
- short_name: string (required)

## mission and positioning
- mission: string (required)
- positioning: string (required)

## audience
- primary_segments: string[] (required, min 1)

## voice and tone
- tone_attributes: string[] (required, min 1)
- avoid_patterns: string[] (required, min 1)

## messaging
- core_message: string (required)
- proof_points: string[] (required, min 1)

## brand expressions
- fallback_wordmark: string (required)
- plain_text_signature: string (required)
- text_only_identity_rules: string[] (required, min 1)
- ascii_mark: string (optional)
- terminal_banner: string (optional)
- emoji_signature.value: string (optional)
- emoji_signature.fallback: string (optional, required when emoji_signature.value exists)
- emoji_signature.source: enum(human, tool, hybrid) (optional)
- cli_prompt_style.value: string (optional)
- cli_prompt_style.source: enum(human, tool, hybrid) (optional)

## do and dont
- do: string[] (required, min 1)
- dont: string[] (required, min 1)

## usage notes for ai systems
- allowed_contexts: string[] (required, min 1)
- restricted_contexts: string[] (required, min 1)
- fallback_behavior: string (required)

## Optional Metadata

- spec_version: string (optional but recommended)
- profile_id: string (optional)
- status: string (optional)

## Validation Hints for Future Tooling

- Enforce required section names case-sensitively in v1.
- Enforce heading depth <= 3.
- Warn on unknown top-level sections in strict mode.
- Warn when emoji fields exist without textual fallback.
