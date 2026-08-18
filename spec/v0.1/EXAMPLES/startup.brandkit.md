---
spec_version: 0.1.0-draft
profile_id: startup
status: example
---

# brand overview
- display_name: Fluxbyte
- short_name: Flux

## mission and positioning
- mission: Help product teams ship faster without losing reliability.
- positioning: Developer-first release automation for growing SaaS teams.

## audience
- primary_segments:
  - Startup CTOs
  - Product engineers
  - Technical founders

## voice and tone
- tone_attributes:
  - energetic
  - practical
  - transparent
- avoid_patterns:
  - inflated enterprise claims
  - fear-based urgency

## messaging
- core_message: Faster releases with predictable quality.
- proof_points:
  - Rollback automation with audit history
  - Release dashboards tuned for small teams
  - Integrates in less than one sprint

## visual identity
- primary_color: electric_blue
- accent_color: neon_lime
- usage_note: Visual colors are optional and never required for meaning.

## brand expressions
- fallback_wordmark: FLUXBYTE
- plain_text_signature: [FLUX]
- emoji_signature:
    value: "⚡"
    source: human
    fallback: "[FLUX]"
- terminal_banner:
    source: tool
    value: |
      ============
      == FLUX  ==
      ============
- text_only_identity_rules:
  - Use [FLUX] in command examples and logs.
  - Keep release announcements in present tense.
  - Limit symbols to one per heading.

## do and dont
- do:
  - Lead with outcome and implementation detail.
  - Keep examples runnable and concise.
- dont:
  - Overpromise with unsupported metrics.
  - Mix marketing taglines into troubleshooting docs.

## example outputs
- release_note_stub: "Flux 2.2 shortens rollback execution by 35 percent in default mode."

## usage notes for ai systems
- allowed_contexts:
  - changelogs
  - release notes
  - setup guides
- restricted_contexts:
  - investment documents
  - legal guarantees
- fallback_behavior: If evidence is missing, remove numeric claims and keep only verified facts.
