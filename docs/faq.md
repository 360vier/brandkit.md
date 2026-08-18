# FAQ

## Is this a file format, a convention or a standard?

A standard-in-progress: normative prose in `SPEC.md`, a machine-validatable JSON
Schema, one running reference implementation. Version `0.2.0-draft` — the field
names are stable enough to build on, and breaking changes are labelled in
`CHANGELOG.md`.

## Why is the model JSON now? v0.1 was so nice to write.

Because a readable file could not do the job. Three things broke: values (a
model needs `#101B2D`, not "a deep navy"), state (a rule is either approved or
somebody's draft), and negative facts ("this certification exists and must not
be claimed"). All three need structure. The Markdown survives as the rendering —
you still read and review `brandkit.md`, you just no longer author it.

## Do I have to hand-write JSON?

No. Copy `templates/brandkit-template.json` and fill it in, or use an
implementation that gives you a UI. `templates/authoring.brandkit.md` exists for
drafting in Markdown before converting.

## What happened to `brand expressions`?

Moved to [`ext/text-expressions.md`](../ext/text-expressions.md), unchanged in
substance. Most brand programmes do not need an ASCII mark; those that do need
it precisely, which is what an extension is for.

## Why DTCG instead of a token format of your own?

Because a token set should be readable by tools that have never heard of
`brandkit.md`. The profile lives in `$extensions["md.brandkit"]`, so a Style
Dictionary build or a Figma plugin still sees a valid DTCG set.

## Why is there a segment concept in the tokens rather than several brandkits?

Sub-brands usually share almost everything. In the reference profile, 40 of 43
tokens are identical across segments. Cloning a brandkit per variant means every
future CD change has to be made several times, and one copy will be missed.

## Can one file describe several brands or several versions?

No. One document is one version of one brand. Multi-brand containers were tried
and produce ambiguity about which version an artefact was generated from.

## Why must approved versions be immutable?

So that a generated artefact can be explained later. Record the brand version,
the token version and the context hash, and any output can be re-derived. Edit
an approved version in place and that guarantee is gone.

## Is the compiled `brandkit.md` stable enough to diff in a pull request?

Yes — compilation is required to be a pure function, so the same Core produces
byte-identical output. That is also why the hash-based reproducibility works.

## Which language are the renderings in?

Implementation- and locale-defined. What the specification fixes is the
*structure* (§7.2–§7.4), not the language of the surrounding prose. Render one
artefact in one language throughout.

## How do I do multilingual brand content?

Not in one document, in v0.2. `supportedLocales` declares where the brand
operates; per-locale content needs separate documents until a localisation
extension exists. This is an acknowledged gap, tracked in the roadmap.

## Does a validator exist?

The JSON Schema does, and any JSON Schema tool validates against it:

```bash
npx ajv-cli validate -s schema/brandkit-v0.2-draft.schema.json -d my-brand.brandkit.json --spec=draft2020
```

A dedicated CLI that also checks the rules a schema cannot express (§6, §8, §11)
is on the roadmap.

## What can a schema not check?

The rules that matter most: that an unverified certification is never claimed,
that a restricted claim carries its conditions, that hard constraints survive
prompt truncation. Those are consumer obligations, checkable only against
generated output.

## My model still ignores the rules. Is the standard wrong?

Check the order first. Almost every reported case is a truncation or position
problem: the rule was in the context but not in the final prompt, or it sat
before a longer, more specific competing instruction. See
[`context-compilation.md`](context-compilation.md).

## Why so much detail about image models?

Because that is where brand rules fail hardest and most visibly. A text model
that drifts produces a sentence someone edits; an image model that draws its own
logo produces an artefact that has to be thrown away.

## Is my brandkit confidential?

Usually yes — it contains positioning, forbidden claims and unverified
certifications. Nothing in the standard requires publication. The examples here
are a fictional brand for exactly this reason.

## What is *not* in scope?

Print production, motion, merchandise, component libraries, asset binaries,
per-locale content, and prompt engineering for a specific model. These are
referenced, not absorbed. See [`comparison.md`](comparison.md).

## Can I extend it?

Yes — namespaced, documented, versioned, in `ext/`. Consumers ignore unknown
extension data rather than failing. Open an issue describing the failure mode
before proposing fields.
