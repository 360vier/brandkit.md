# Examples

Reference artefacts for Brandkit Core `0.2.0-draft`.

| File | What it is |
| --- | --- |
| `helion-systems.brandkit.json` | Full reference profile — every Core section exercised |
| `helion-systems.brandkit.md` | Its compiled rendering, plus the token summary |
| `helion-systems.tokens.json` | DTCG token set with the `md.brandkit` profile and two segments |
| `helion-systems.design.md` | Compiled token rendering: tables, per-segment values, CSS custom properties |
| `minimal.brandkit.json` | Smallest document that satisfies the Core |
| `minimal.brandkit.md` | Its compiled rendering |

## HELION SYSTEMS is fictional

There is no such company. Every fact, figure, claim and certification is invented
to illustrate a mechanism. Deployment counts, rollout medians and certificate
numbers are examples of the *shape* of a proof point, not data.

The profile is deliberately a security-infrastructure brand, because that is a
domain where the difference between a claim and a verifiable claim has
consequences — which is what the Core is built to carry.

## What to look at, and why

**Proof before assertion.** Product `hel-gateway` carries a SOC 2 certification
with `verified: false` and `isPlaceholder: true`. In the rendering it appears as
`UNVERIFIED, must not be claimed` and the heading is marked
`— _placeholder data_`. This is the sentence prose cannot carry: the fact exists,
is recorded, and may not be used. See SPEC §6.

**Claims with status.** `claims[]` holds approved, restricted (with conditions)
and forbidden entries. `claim-compliant` — *"Makes you compliant with NIS2"* — is
forbidden with the reason attached: compliance is a property of an organisation,
never of a purchased product.

**Policies that route.** Ten policies across `legal`, `product`, `visual`,
`safety`, `messaging`, `channel` and `voice`, each with a severity, a scope and
an evaluation method. `pol-legacy-tagline` is `enabled: false` — a retired
tagline kept so tooling keeps rejecting it rather than forgetting it was wrong.

**Segments in the token layer.** 43 tokens, 2 segments, 3 segmented values. The
ratio is the argument: sub-brands share a foundation rather than cloning one. See
`design.md` for the `[data-segment="labs"]` override block.

**References, not duplicates.** `color.surface.page` is `{color.neutral.0}` — a
statement that the page ground follows the neutral ramp, not a copied hex value.
`visualLanguage.colorPrinciples` describes how colour is used and contains no
values at all.

**Imagery rules that name objects.** `imagery.avoid` names *"text, logos or
wordmarks rendered inside the photograph"* and PPE explicitly, because general
prohibitions are read by image models as general suggestions. See
[`docs/policies.md`](../docs/policies.md).

## Known gap: mixed prose language

`helion-systems.brandkit.md` is rendered in English, but its appended
`## Design Tokens` summary and the whole of `helion-systems.design.md` are in
German. That is not an editorial choice — it is what the reference
implementation currently emits: its brandkit compiler carries English labels
while its token compilers carry German ones.

These files are left exactly as the implementation produces them, because their
value is being verifiably identical to real output. The specification permits it
(§7.5: prose language is implementation-defined, structure is what is normative)
but also says an implementation SHOULD render one artefact in one language
throughout — so this is a gap on the implementation's side, tracked in
[`../docs/roadmap.md`](../docs/roadmap.md). Read past the language; the
structure is the contract.

## The renderings are generated

`helion-systems.brandkit.md`, `minimal.brandkit.md` and
`helion-systems.design.md` are **compiled output**, produced by the reference
implementation's own compilers from the JSON in this directory. Do not edit them
by hand — edit the Core or the token set.

Both Core documents validate against the reference implementation's schema
definition. The examples and the running tool therefore cannot drift apart
quietly, which is the only reason examples in a specification are worth
anything.

## Using one as a starting point

Don't. Copy `templates/brandkit-template.json` instead — it has every section
with placeholder values and no invented facts to delete. Read the reference
profile to see what a filled field looks like, then write your own.
