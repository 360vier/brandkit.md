# HELION SYSTEMS — Brandkit
> Version 3.1.0 · Status: approved · Schema 0.2.0-draft
**Default locale:** en-US
**Supported locales:** en-US, de-DE
**Owner:** Brand & Communications
**Updated:** 2026-03-02T14:20:00.000Z

---

## Identity

**Mission:** Make secure infrastructure the default, not the upgrade.

**Vision:** Every team ships on infrastructure that is safe by construction, whether or not anyone on that team is a security engineer.

**Purpose:** Security work is unevenly distributed. We move it into the platform, where it only has to be solved once.

**Tagline:** Secure by construction.

**Values:**
- **Verifiable over reassuring** — Every claim we make can be checked by the reader. If it cannot be checked, we do not make it.
- **Boring on purpose** — Infrastructure earns trust by being predictable. We prefer the unremarkable answer that holds under load.
- **Plain language** — We explain what a system does before we explain why it matters. No mystique, no acronym walls.

**Story:**
HELION started as an internal platform team that got tired of writing the same hardening checklist for the fourth product in a row. The checklist became a gateway, the gateway became a product, and the habit of writing things down became the brand.

---

## Positioning

**Statement:** HELION SYSTEMS is the secure infrastructure layer for engineering teams who need to prove their controls, not just describe them.

**Category:** Infrastructure security platform

**Differentiators:**
- Controls ship as code and produce machine-readable evidence, so an audit is a query and not a project.
- Runs in the customer's own network boundary — no data leaves the perimeter to be inspected.
- Defaults are the secure path; the insecure path requires an explicit, logged override.

**Proof points:**
- Median rollout to first enforced policy: 9 days across 40 documented deployments.
- Every control emits a signed evidence record, retained for the customer's full audit window.
- Open control library — the policy definitions are readable before purchase.

**Competitive context:** Adjacent tools either scan after the fact or wrap existing infrastructure in dashboards. HELION sits in the request path and enforces, which is why enforcement latency is a first-class number in our material.

---

## Audiences

### Platform engineers (`platform-eng`)

Own the internal developer platform. Evaluate HELION technically before anyone else sees it, and will read the docs before the landing page.

**Needs:**
- Concrete latency and failure-mode numbers
- A migration path that does not require a freeze window
- Configuration as code, reviewable in a pull request

**Pain points:**
- Security requirements arrive as PDFs, not as interfaces
- Every audit cycle re-derives evidence by hand
- Tools that add a dashboard but no enforcement

**Tone adjustments:** Lead with mechanism. Numbers before adjectives, and never explain what a reverse proxy is.

### Security leads (`security-lead`)

Accountable for controls and for what auditors see. Buy on evidence quality and on how failures behave.

**Needs:**
- Traceability from control to evidence record
- Clear statement of what is enforced and what is only observed
- Named standards, with the scope of each one

**Pain points:**
- Vendor claims that collapse under audit questioning
- Coverage gaps discovered during, not before, review

**Tone adjustments:** Be precise about scope. State the limits of a control in the same breath as its benefit.

### Engineering leadership (`eng-leadership`)

Approves the spend. Cares about delivery speed staying intact while risk goes down.

**Needs:**
- Rollout effort in team-weeks
- What breaks and who gets paged
- A defensible answer for the board

**Pain points:**
- Security programs that quietly become delivery taxes

**Tone adjustments:** Short. Consequence first, mechanism on request.

---

## Messaging

**Core message:** Controls that enforce themselves and prove it.

**Elevator pitch:** HELION SYSTEMS puts your security controls in the request path instead of in a document. Policies ship as code, run inside your own network boundary, and emit signed evidence for every decision — so proving a control works is a query, not a quarter.

**Key messages:**
- A control that is documented but not enforced is a description of intent. HELION enforces in the request path. _(audiences: platform-eng, security-lead)_
- Audit evidence is generated as a side effect of running, not assembled before a review. _(audiences: security-lead, eng-leadership)_
- Secure defaults do not slow delivery down; they remove the decision from every pull request. _(audiences: eng-leadership, platform-eng)_

**Boilerplate (short):** HELION SYSTEMS is the secure infrastructure layer for teams who have to prove their controls.

**Boilerplate (medium):** HELION SYSTEMS provides infrastructure security controls that run in the request path inside the customer's own network boundary. Policies are defined as code, enforced by default, and produce signed evidence records for audit.

**Boilerplate (long):** HELION SYSTEMS provides infrastructure security controls that run in the request path inside the customer's own network boundary. Policies are defined as code and reviewed like code, secure defaults apply without configuration, and every enforcement decision produces a signed evidence record retained for the customer's full audit window. Engineering teams keep their delivery speed; security leads get traceability from control to evidence without a manual collection cycle.

---

## Voice & Tone

**Tone attributes:** precise, calm, concrete, unhurried, respectful of expertise

### Mechanism before benefit

Say what the system does, then what that gets the reader. A benefit without a mechanism reads as marketing to this audience.

**Do:**
- Open with the observable behaviour: what is enforced, where, and at what cost.
- Give the number and the sample size together.

**Don't:**
- Open with an outcome adjective ("effortless", "seamless", "bulletproof").
- Cite a metric without saying what it was measured over.

### Name the limits

Trust in security communication comes from stated boundaries. A control described without its scope will be tested against a scope we never claimed.

**Do:**
- State what a control does not cover, in the same paragraph.
- Distinguish enforced from observed, every time.

**Don't:**
- Use "complete", "total" or "end-to-end" as a coverage claim.
- Let a scoped certification imply the whole product.

### Plain sentences

Our readers are experts in their own domain, not in ours. Expertise is respected by clarity, not by vocabulary.

**Do:**
- One idea per sentence.
- Expand an acronym on first use, then use it.

**Don't:**
- Stack three nouns into a compound ("security posture optimisation layer").
- Write in the passive to avoid naming who acts.

**Preferred vocabulary:** enforce, evidence record, control, network boundary, override, scope

**Avoid:** bulletproof, military-grade, unhackable, AI-powered, next-generation, revolutionise, leverage, synergy

---

## Visual Language

**Logo usage:** The wordmark is the primary mark and is always placed from an original vector file. On dark surfaces use the light cut; never recolour the wordmark to a segment accent.

**Clearspace:** Minimum clearspace on all sides equals the cap height of the wordmark.

**Minimum size:** 24px cap height on screen, 8mm in print.

**Logo misuse:**
- Recolouring, gradients, outlines or drop shadows on the wordmark
- Stretching, rotating or re-spacing the letterforms
- Placing the mark over photographic detail instead of a flat area
- Re-drawing or re-typing the wordmark instead of using the source file

**Color principles:** One brand colour carries the page; the accent marks a single action or state per view. Large areas stay in the neutral ramp — surfaces are calm so that a warning colour still means something.

**Typography principles:** One geometric sans across headings and body, differentiated by weight and size rather than by a second family. Headlines set tight (1.1), body generous (1.6).

**Layout principles:** 12-column grid, wide gutters, generous outer margin. Content sits left-aligned in a measure of 60–75 characters; centred text is reserved for single-line statements.

**Iconography:** Line icons on a 24px grid, 1.5px stroke, square terminals, no fills and no rounded cartoon forms.

**Design token set:** helion-systems.tokens.json

---

## Imagery

**Style:** Documentary photography of real infrastructure and the people who operate it. Available light, no staged smiles, no glossy server-room stock.

**Subjects:**
- Operators at real workstations, mid-task
- Physical infrastructure at honest scale — racks, cable trays, cooling
- Close details that reward a second look: a labelled port, a hand on a console

**Moods:** composed, unhurried, matter-of-fact

**Composition:** Full-bleed photography with a single clear subject and an intentionally quiet area where composed text can sit. Horizon and rack lines stay level.

**Lighting:** Existing light, including mixed colour temperatures. Contrast is allowed to be high; shadow detail may fall away.

**Color grading:** Neutral to slightly cool. No teal-orange grade, no heavy vignette, no synthetic glow.

**Avoid:**
- Glowing blue padlocks, shields, hooded figures or binary rain
- Screens showing invented dashboards or fake log output
- Text, logos or wordmarks rendered inside the photograph
- Depicting people working without the safety equipment their real environment requires

---

## Products

### HELION Mesh (`hel-mesh`)

Service-to-service identity and policy enforcement inside the customer's network boundary.

**Approved claims:**
- Enforces mutual authentication between services without changes to application code.
- Emits a signed evidence record for every allow and deny decision.

**Forbidden claims:**
- Prevents all lateral movement
- Replaces network segmentation

**Safety notes:**
- Fail-closed is the default; describing a fail-open configuration requires an explicit note that it is an override.

**Certifications:**
- ISO/IEC 27001:2022 (Operation of the HELION Mesh control plane) — verified, source: Certificate HEL-27001-2026, valid to 2029-02-28

**Markets:** eu, uk

### HELION Gateway (`hel-gateway`) — _placeholder data_

Edge policy enforcement point for north-south traffic. Early access.

**Approved claims:**
- Available in early access with a documented feature boundary.

**Forbidden claims:**
- Generally available
- Certified to any standard
- Drop-in replacement for an existing edge proxy

**Safety notes:**
- Early access: no production support commitment. Any material must say so on first mention.

**Certifications:**
- SOC 2 Type II (Gateway data plane) — UNVERIFIED, must not be claimed

**Markets:** eu

---

## Claims

**Approved:**
- Every enforcement decision produces a signed evidence record.
- Runs inside your own network boundary; no inspected data leaves the perimeter.

**Restricted:**
- Median 9 days to first enforced policy. — _Only with the sample size named (40 documented deployments) and only for Mesh, not Gateway._

**Forbidden:**
- Makes your infrastructure unbreachable.
- Makes you compliant with NIS2.

---

## Policies

### No unverified standards in content (`pol-no-unverified-norms`)

A standard, norm or certification may only appear in generated content when the corresponding product certification carries verified = true. Unverified entries exist to be excluded, never to be paraphrased into an implication.

**Category:** legal · **Severity:** critical · **Evaluation:** hybrid · human review required

**Scope:** global

### No invented product facts (`pol-no-invented-facts`)

Product capabilities, numbers, deployment counts and latencies come from approvedClaims or proofPoints. If a needed fact is absent, the output says so instead of supplying a plausible value.

**Category:** product · **Severity:** critical · **Evaluation:** language-model · human review required

**Scope:** global

### Logo only from original files (`pol-logo-source-file`)

The wordmark is placed from an original vector asset. It is never drawn, re-typed, generated or reconstructed by a model, and never rendered as part of a generated image.

**Category:** visual · **Severity:** critical · **Evaluation:** deterministic

**Scope:** global

### No text inside generated imagery (`pol-no-text-in-image`)

Generated photography contains no headlines, wordmarks, labels or readable interface text — including lettering printed on objects inside the scene. Headlines and marks are composed deterministically after generation.

**Category:** visual · **Severity:** error · **Evaluation:** multimodal-model

**Scope:** global

### Protective equipment shown complete (`pol-ppe-complete`)

Where people appear in a working environment that requires protective equipment, that equipment is shown correctly and completely. Partial protection reads as an endorsement of unsafe practice.

**Category:** safety · **Severity:** critical · **Evaluation:** multimodal-model · human review required

**Scope:** global

### Early access labelled on first mention (`pol-early-access-labelled`)

HELION Gateway is named as early access at its first mention in any artefact, with no production support commitment implied.

**Category:** product · **Severity:** error · **Evaluation:** deterministic

**Scope:** product:hel-gateway

### No fear appeals (`pol-no-fear-appeals`)

We do not sell through breach imagery, countdown framing or implied blame. The reader's risk is described in operational terms.

**Category:** messaging · **Severity:** warning · **Evaluation:** language-model

**Scope:** global

### LinkedIn posts stay under 1300 characters (`pol-linkedin-length`)

Posts fit without the platform's truncation, and lead with the mechanism inside the first two lines.

**Category:** channel · **Severity:** info · **Evaluation:** deterministic

**Scope:** channel:linkedin

### German-language content uses formal address (`pol-de-formal-address`)

German artefacts address the reader with "Sie". Technical documentation may drop direct address entirely, but never switches to "du".

**Category:** voice · **Severity:** warning · **Evaluation:** language-model

**Scope:** market:dach

---

## Channels

### LinkedIn (`linkedin`)

**Type:** social

**Format notes:** Single image at 1080×1080 with the headline composed, not generated. Post text under 1300 characters, no hashtag stacks.

**Tone adjustments:** One idea per post. The first two lines carry the mechanism; the rest may elaborate.

**Constraints:**
- No more than three hashtags
- No emoji in the first line
- Never imply that a product is generally available when it is in early access

### Product documentation (`docs`)

**Type:** documentation

**Format notes:** Task-oriented headings in the imperative. Every configuration example is runnable as written.

**Tone adjustments:** Instructional and direct. No positioning language, no benefit framing.

**Constraints:**
- No marketing claims inside reference pages
- Every example includes its failure behaviour

### Practitioner newsletter (`newsletter`)

**Type:** email

**Format notes:** One subject, one call to action. Plain-text readable — the layout may not carry meaning the text lacks.

**Tone adjustments:** Write to a peer who has read the last issue. Recap in one sentence, then continue.

**Constraints:**
- Subject line under 60 characters

---

## Markets

### Germany, Austria, Switzerland (`dach`)

**Locales:** de-DE, de-AT, de-CH

**Legal notes:** Comparative advertising is permitted but must be factually verifiable; naming a competitor requires legal review.

**Cultural notes:** Formal address throughout. Certifications and named standards carry more weight than customer logos.

### United Kingdom (`uk`)

**Locales:** en-GB

**Cultural notes:** British spelling. Understatement reads as confidence; superlatives read as noise.

### United States (`us`)

**Locales:** en-US

**Legal notes:** Avoid absolute security claims entirely — they carry liability exposure beyond the brand question.

**Cultural notes:** Outcome-first framing is acceptable, provided the mechanism follows in the same paragraph.

---

## Usage Notes for AI Systems

You are writing as HELION SYSTEMS. Ground every factual statement in this brandkit: approved claims, proof points and verified certifications are the only permitted sources of fact. Prefer mechanism over benefit, name the scope of every control, and state limits explicitly. When a required fact is missing, say that it is missing — never supply a plausible substitute.

**Allowed contexts:**
- Product and platform documentation
- Practitioner-facing blog posts and newsletters
- LinkedIn and conference material
- Sales collateral built from approved claims
- Internal enablement and onboarding material

**Restricted contexts:**
- Regulatory filings and contractual language
- Incident communication and breach notification
- Statements about named competitors
- Pricing, contractual terms and service level commitments
- Any content asserting customer compliance status

**Fallback behavior:** If the brandkit does not cover the case, produce the most conservative version that stays inside approved claims, mark the uncertain passage inline, and route the artefact to human review instead of guessing.

**Image generation rules:**
- Generate the photograph only — no text, no wordmark, no logo, no interface screenshots.
- No lettering on objects inside the scene: no printed labels, embossed codes or standard markings.
- Full bleed: no frames, borders, colour panels or layout areas inside the image.
- Use only the brand palette for any graphic area, tint or accent; photographic subjects keep their natural colours.
- Show protective equipment complete wherever the environment requires it.

**Text generation rules:**
- Lead with the mechanism; the benefit follows in the same paragraph.
- Every number appears with what it was measured over.
- Never name a standard whose certification is not verified.
- Label HELION Gateway as early access on first mention.
- Do not use the avoid-vocabulary, and do not reach for a synonym of it either.

---

## Governance

**Owner:** Brand & Communications

**Review cycle:** Every six months, and on any change to a product certification.

**Approval policy:** Approved versions are immutable. A change starts a new draft version with supersedesVersion set, and requires sign-off from brand and from legal when claims, policies or certifications are touched.

**Change policy:** Policies and claims may only be edited in a draft version. Retired messaging stays in the file with enabled = false so tooling keeps rejecting it.

**Contact:** brand@helion.example

---

## References

- **HELION Brand Guidelines (full visual system)** (guideline) — https://brand.helion.example/guidelines — Print specifications, motion, exhibition and merchandise — everything outside the scope of this brandkit.
- **HELION Design Tokens (DTCG)** (document) — https://brand.helion.example/tokens/helion-systems.tokens.json — Canonical token set referenced by visualLanguage.designTokenSetRef.
- **Claims register and evidence sources** (document) — https://brand.helion.example/claims — Source documents behind every approved claim and verified certification.
- **Wordmark, original vector files** (asset) — The only permitted source for the mark. See policy pol-logo-source-file.

---

---

_Generated from Brandkit Core 0.2.0-draft · brand helion-systems · version 3.1.0_


## Design Tokens
Verbindliche CD-Werte aus Token-Set Version 4.
Segmente: Core Platform, Labs (Early Access) — abweichende Werte sind je Segment ausgewiesen.
Auszug der wichtigsten Werte; vollständiger Satz inklusive CSS-Variablen: `EXAMPLES/helion-systems.design.md`.

### color
| Token | Kurzname | Wert |
| --- | --- | --- |
| `color.brand.primary` | primary | #101B2D |
| `color.brand.accent` | accent | Core Platform: #1F9E8C · Labs (Early Access): #C2571E |
| `color.brand.ink` | ink | #0A1220 |
| `color.neutral.0` | 0 | #FFFFFF |
| `color.neutral.100` | 100 | #F4F6F8 |
| `color.neutral.300` | 300 | #D5DBE2 |
| `color.neutral.500` | 500 | #6B7784 |
| `color.neutral.700` | 700 | #3A4550 |
| `color.neutral.900` | 900 | #18212E |
| `color.surface.page` | page | #FFFFFF _({color.neutral.0})_ |
| `color.surface.muted` | muted | #F4F6F8 _({color.neutral.100})_ |
| `color.surface.inverse` | inverse | #101B2D _({color.brand.primary})_ |
| `color.state.warning` | warning | #B7791F |
| `color.state.danger` | danger | #A32B2B |
| `color.area.post` | post | Core Platform: #101B2D · Labs (Early Access): #18212E |

### typography
| Token | Kurzname | Wert |
| --- | --- | --- |
| `typography.fontFamily.heading` | heading | Inter Tight, Helvetica Neue, sans-serif |
| `typography.fontFamily.body` | body | Inter Tight, Helvetica Neue, sans-serif |
| `typography.fontFamily.mono` | mono | JetBrains Mono, ui-monospace, monospace |
| `typography.fontSize.sm` | sm | 14px |
| `typography.fontSize.base` | base | 17px |
| `typography.fontSize.lg` | lg | 21px |
| `typography.fontSize.xl` | xl | 32px |
| `typography.fontSize.display` | display | Core Platform: 56px · Labs (Early Access): 44px |
| `typography.fontWeight.regular` | regular | 400 |
| `typography.fontWeight.semibold` | semibold | 600 |
| `typography.lineHeight.tight` | tight | 1.1 |
| `typography.lineHeight.body` | body | 1.6 |

### spacing
| Token | Kurzname | Wert |
| --- | --- | --- |
| `spacing.xs` | xs | 4px |
| `spacing.sm` | sm | 8px |
| `spacing.md` | md | 16px |
| `spacing.lg` | lg | 32px |
| `spacing.xl` | xl | 64px |
| `spacing.section` | section | 96px |

### size
| Token | Kurzname | Wert |
| --- | --- | --- |
| `size.container.max` | max | 1200px |
| `size.container.measure` | measure | 68ch |
| `size.logo.minCapHeight` | minCapHeight | 24px |
| `size.icon.grid` | grid | 24px |
| `size.icon.stroke` | stroke | 1.5px |

### border
| Token | Kurzname | Wert |
| --- | --- | --- |
| `border.radius.sm` | sm | 4px |
| `border.radius.md` | md | 8px |
| `border.width.hairline` | hairline | 1px |

### shadow
| Token | Kurzname | Wert |
| --- | --- | --- |
| `shadow.raised` | raised | 2 / 8 / 0 / 12% |

### opacity
| Token | Kurzname | Wert |
| --- | --- | --- |
| `opacity.muted` | muted | 0.64 |
