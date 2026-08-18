# Schema

`brandkit-v0.2-draft.schema.json` is the machine-validatable definition of a
Brandkit Core document.

- **`$id`:** `https://brandkit.md/schema/brandkit-v0.2-draft.schema.json`
- **Dialect:** JSON Schema 2020-12
- **Covers:** `SPEC.md` §5–§9 structure

## Validate a document

```bash
npx ajv-cli validate -s schema/brandkit-v0.2-draft.schema.json -d my-brand.brandkit.json --spec=draft2020
```

Any JSON Schema 2020-12 tool works — `ajv`, `check-jsonschema`,
`jsonschema` (Python), or your editor if it honours the `$schema` key in the
document itself.

## Generated, not hand-written

This file is **generated** from the reference implementation's Zod definitions,
which are the single source of truth for field names and types. Do not edit it by
hand: the next regeneration would silently discard the change, and the standard
would then disagree with the implementation everyone actually runs.

To change the schema, change the Core definition and regenerate. To propose a
change, open an issue against `SPEC.md` — the prose and the schema move together.

## What the schema cannot check

A schema validates shape. The rules that matter most are semantic and are
consumer obligations:

| Rule | Where |
| --- | --- |
| An unverified certification is never claimed | SPEC §6.1 |
| A restricted claim always carries its conditions | SPEC §6.2 |
| Missing facts are reported, never substituted | SPEC §6.4 |
| Disabled policies are not applied as rules | SPEC §8.1 |
| Scoped policies do not leak outside their scope | SPEC §8.2 |
| `critical` violations are hard fails, not deductions | SPEC §8.3 |
| Hard constraints survive prompt truncation | SPEC §11.2 |
| Values are not duplicated between `visualLanguage` and the token set | SPEC §5.7 |

A document can be schema-valid and still be a bad brandkit. A validator covering
these rules is on the [roadmap](../docs/roadmap.md).

## Design token sets

Token sets are **DTCG** documents and are not validated by this schema. The
profile `brandkit.md` adds is specified in `SPEC.md` §9 and explained in
[`docs/design-tokens.md`](../docs/design-tokens.md).
