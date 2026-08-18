# Templates

Starting points for a Brandkit Core `0.2.0-draft` document and its token set.

| File | Use it when |
| --- | --- |
| `brandkit-template.json` | You are creating a brandkit. Every Core section, placeholder values, importable. |
| `tokens-template.json` | You need a DTCG token set with the `md.brandkit` profile. |
| `authoring.brandkit.md` | You want to draft the prose in Markdown before structuring it. |

## Quick start

```bash
cp templates/brandkit-template.json my-brand.brandkit.json
cp templates/tokens-template.json my-brand.tokens.json
```

Then:

1. **Fill `metadata` first.** `brandId`, `organizationId`, `name`, `version`,
   `status`, `defaultLocale`, `supportedLocales`, `createdAt`, `updatedAt` are all
   required. Start at `status: "draft"`.

2. **Delete what you cannot fill honestly.** Optional fields are better absent
   than filled with a placeholder. Required arrays may stay empty — an empty
   array means "nothing declared", which is a true statement.

3. **Keep values out of the Core.** `visualLanguage.colorPrinciples` says how
   colour is used; the hex value belongs in the token set. Point
   `visualLanguage.designTokenSetRef` at it.

4. **Write policies only where you can answer three questions:** how bad is a
   violation (`severity`), where does it apply (`scopes`), and how would anyone
   detect it (`evaluationMethod`). If you cannot, it is voice guidance, not a
   policy.

5. **Validate:**

   ```bash
   npx ajv-cli validate -s schema/brandkit-v0.2-draft.schema.json -d my-brand.brandkit.json --spec=draft2020
   ```

## Order to fill it in

The Core has 17 sections and filling them front to back stalls. This order gets
you to something usable fastest:

1. `metadata`, `identity`, `positioning` — who you are.
2. `voice`, `messaging` — how you sound.
3. Token set — the concrete values. Do this before `visualLanguage`, so you are
   describing usage of values that already exist.
4. `visualLanguage`, `imagery` — how you look. `imagery.avoid` is the
   highest-leverage array in the document.
5. `aiInstructions` — what a model may and may not do.
6. `products`, `claims` — the facts, with `verified` and `status` set honestly.
7. `policies` — the rules that invalidate an artefact.
8. `audiences`, `channels`, `markets` — scope targets. These can start empty and
   grow as policies need something to point at.
9. `governance`, `references` — who owns this, and what it points back to.

## A note on the template's `designTokens: null`

The template ships `designTokens: null` deliberately. If you fill it with
placeholder values and import it, a worthless token version can displace real CD
values. Ship real values or ship none. See `SPEC.md` §10.1.

## Text-only expressions

If your brand needs an ASCII mark, an emoji signature or a CLI prompt style, add
the `x-textExpressions` block from
[`ext/text-expressions.md`](../ext/text-expressions.md). It is not part of the
Core.

## Looking for a filled-in example?

[`EXAMPLES/helion-systems.brandkit.json`](../EXAMPLES/helion-systems.brandkit.json)
— a fictional brand exercising every section. Read it, don't copy it: deleting
someone else's invented facts is slower than filling a template.

## Coming from v0.1?

The old Markdown templates are archived at
[`spec/v0.1/templates/`](../spec/v0.1/templates/). See
[`docs/migration.md`](../docs/migration.md) for the field mapping.
