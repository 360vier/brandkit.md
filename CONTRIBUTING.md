# Contributing

Thanks for contributing to `brandkit.md`.

## What is welcome

- Sharpening the specification where it is ambiguous.
- Reporting a rule that failed in practice — with what you observed.
- Extensions, namespaced and versioned under `ext/`.
- Conformance reports from an independent implementation. These are the most
  valuable contribution the project can receive right now (see
  [`docs/roadmap.md`](docs/roadmap.md), v1.0 gate 3).
- Improving examples and documentation.

## Process

1. Open an issue with a problem statement first. For a normative change, describe
   the failure mode the current rule permits.
2. Keep pull requests focused. A spec change plus an unrelated docs tidy-up is
   two pull requests.
3. Reference `SPEC.md` sections in the description.
4. Update `CHANGELOG.md` for anything that changes normative rules or repository
   structure.

## Rules for specification changes

- Normative statements use `MUST`, `MUST NOT`, `SHOULD`, `SHOULD NOT`, `MAY`.
- **A `MUST` needs a failure behind it.** Every `MUST` in §11 traces to a
  measured failure in a running implementation. If a proposed rule has no
  observed failure, propose it as `SHOULD` — speculation in a specification is
  surface area nobody maintains.
- Breaking changes are labelled `BREAKING` in `CHANGELOG.md`, with a migration
  path.
- Changes to Core field names, enum values or required status are breaking. Say so.
- New capability that only some brands need is an extension, not a Core field.

## Do not hand-edit generated files

| File | Source |
| --- | --- |
| `schema/brandkit-v0.2-draft.schema.json` | Generated from the reference implementation's Zod definitions |
| `EXAMPLES/*.brandkit.md` | Compiled from the matching `*.brandkit.json` |
| `EXAMPLES/helion-systems.design.md` | Compiled from `helion-systems.tokens.json` |

Edit the source and regenerate. A hand-edited rendering makes the specification
disagree with the implementation everyone actually runs, and the next
regeneration discards the change silently.

Changing a Core field means: change the reference implementation's definition,
regenerate the schema, update `SPEC.md`, recompile the examples, and note it in
`CHANGELOG.md`. All five, in one pull request.

## Examples must stay fictional

`EXAMPLES/` uses HELION SYSTEMS, a brand that does not exist. Do not contribute
examples containing:

- real customer or client brand data,
- real certification numbers, audit references or evidence sources,
- claims about a real company's products.

A brandkit contains positioning, forbidden claims and unverified certifications.
Yours is probably confidential; treat everyone else's the same way.

## Definition of done

- [ ] `SPEC.md`, `README.md` and `docs/` agree with each other.
- [ ] Normative changes are demonstrated by at least one example.
- [ ] Generated files regenerated from source, not edited.
- [ ] Core documents in `EXAMPLES/` validate against the schema.
- [ ] `CHANGELOG.md` has an entry.
- [ ] Extensions state their version, status and namespace.
