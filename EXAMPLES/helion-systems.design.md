# HELION SYSTEMS — Design Tokens

> design.md · Token-Set v4 · DTCG-kompatibel

Diese Werte sind bindend. Verwende sie exakt — keine eigenen Farben,
Schriften oder Abstände erfinden.

**Segmente:** Core Platform · Labs (Early Access). 40 von 43 Werten gelten für alle Segmente; nur 3 Werte sind segmentabhängig (unten je Segment ausgewiesen). Ohne Segment-Angabe gilt der gemeinsame Wert.

## color

| Token | Wert | Typ | Beschreibung |
| --- | --- | --- | --- |
| `color.brand.primary` | `#101B2D` | color | Carries the page. Large surfaces, inverse sections, wordmark on light ground. |
| `color.brand.accent` | `#1F9E8C (Core Platform) · #C2571E (Labs (Early Access))` | color | One action or state per view. Segment-dependent: Labs material is visually distinguishable from the core platform. |
| `color.brand.ink` | `#0A1220` | color | Body copy on light surfaces. |
| `color.neutral.0` | `#FFFFFF` | color |  |
| `color.neutral.100` | `#F4F6F8` | color |  |
| `color.neutral.300` | `#D5DBE2` | color |  |
| `color.neutral.500` | `#6B7784` | color |  |
| `color.neutral.700` | `#3A4550` | color |  |
| `color.neutral.900` | `#18212E` | color |  |
| `color.surface.page` | `{color.neutral.0}` | color | Reference, not a duplicate value — the page ground follows the neutral ramp. |
| `color.surface.muted` | `{color.neutral.100}` | color |  |
| `color.surface.inverse` | `{color.brand.primary}` | color |  |
| `color.state.warning` | `#B7791F` | color | Reserved for warnings. Never used decoratively — a warning colour must keep its meaning. |
| `color.state.danger` | `#A32B2B` | color |  |
| `color.area.post` | `{color.brand.primary} (Core Platform) · {color.neutral.900} (Labs (Early Access))` | color | Colour area behind composed headlines. Consumed by the deterministic composition layer, not by prose. |

## typography

| Token | Wert | Typ | Beschreibung |
| --- | --- | --- | --- |
| `typography.fontFamily.heading` | `Inter Tight, Helvetica Neue, sans-serif` | fontFamily | First entry is the brand typeface. A static cut must exist for the composition layer. |
| `typography.fontFamily.body` | `Inter Tight, Helvetica Neue, sans-serif` | fontFamily |  |
| `typography.fontFamily.mono` | `JetBrains Mono, ui-monospace, monospace` | fontFamily | Configuration, evidence records, CLI output. |
| `typography.fontSize.sm` | `14px` | dimension |  |
| `typography.fontSize.base` | `17px` | dimension |  |
| `typography.fontSize.lg` | `21px` | dimension |  |
| `typography.fontSize.xl` | `32px` | dimension |  |
| `typography.fontSize.display` | `56px (Core Platform) · 44px (Labs (Early Access))` | dimension | Segment-dependent: Labs headlines run one step smaller to keep early-access material calmer. |
| `typography.fontWeight.regular` | `400` | fontWeight |  |
| `typography.fontWeight.semibold` | `600` | fontWeight | Headline weight. The composition layer loads exactly this cut of the heading family. |
| `typography.lineHeight.tight` | `1.1` | number | Headlines. |
| `typography.lineHeight.body` | `1.6` | number |  |

## spacing

| Token | Wert | Typ | Beschreibung |
| --- | --- | --- | --- |
| `spacing.xs` | `4px` | dimension |  |
| `spacing.sm` | `8px` | dimension |  |
| `spacing.md` | `16px` | dimension |  |
| `spacing.lg` | `32px` | dimension |  |
| `spacing.xl` | `64px` | dimension |  |
| `spacing.section` | `96px` | dimension | Vertical rhythm between page sections. |

## size

| Token | Wert | Typ | Beschreibung |
| --- | --- | --- | --- |
| `size.container.max` | `1200px` | dimension |  |
| `size.container.measure` | `68ch` | dimension | Reading measure, 60–75 characters. |
| `size.logo.minCapHeight` | `24px` | dimension | Minimum on screen. Enforced by the composition layer, not left to judgement. |
| `size.icon.grid` | `24px` | dimension |  |
| `size.icon.stroke` | `1.5px` | dimension |  |

## border

| Token | Wert | Typ | Beschreibung |
| --- | --- | --- | --- |
| `border.radius.sm` | `4px` | dimension |  |
| `border.radius.md` | `8px` | dimension |  |
| `border.width.hairline` | `1px` | dimension |  |

## shadow

| Token | Wert | Typ | Beschreibung |
| --- | --- | --- | --- |
| `shadow.raised` | `{"offsetX":0,"offsetY":2,"blur":8,"spread":0,"alpha":12}` | shadow | The only elevation. Depth is not a brand signal here. |

## opacity

| Token | Wert | Typ | Beschreibung |
| --- | --- | --- | --- |
| `opacity.muted` | `0.64` | number | Secondary copy on inverse surfaces. |

## CSS Custom Properties

```css
:root {
  --color-brand-primary: #101B2D;
  --color-brand-accent: #1F9E8C;
  --color-brand-ink: #0A1220;
  --color-neutral-0: #FFFFFF;
  --color-neutral-100: #F4F6F8;
  --color-neutral-300: #D5DBE2;
  --color-neutral-500: #6B7784;
  --color-neutral-700: #3A4550;
  --color-neutral-900: #18212E;
  --color-surface-page: {color.neutral.0};
  --color-surface-muted: {color.neutral.100};
  --color-surface-inverse: {color.brand.primary};
  --color-state-warning: #B7791F;
  --color-state-danger: #A32B2B;
  --color-area-post: {color.brand.primary};
  --typography-fontFamily-heading: "Inter Tight", "Helvetica Neue", sans-serif;
  --typography-fontFamily-body: "Inter Tight", "Helvetica Neue", sans-serif;
  --typography-fontFamily-mono: "JetBrains Mono", ui-monospace, monospace;
  --typography-fontSize-sm: 14px;
  --typography-fontSize-base: 17px;
  --typography-fontSize-lg: 21px;
  --typography-fontSize-xl: 32px;
  --typography-fontSize-display: 56px;
  --typography-fontWeight-regular: 400;
  --typography-fontWeight-semibold: 600;
  --typography-lineHeight-tight: 1.1;
  --typography-lineHeight-body: 1.6;
  --spacing-xs: 4px;
  --spacing-sm: 8px;
  --spacing-md: 16px;
  --spacing-lg: 32px;
  --spacing-xl: 64px;
  --spacing-section: 96px;
  --size-container-max: 1200px;
  --size-container-measure: 68ch;
  --size-logo-minCapHeight: 24px;
  --size-icon-grid: 24px;
  --size-icon-stroke: 1.5px;
  --border-radius-sm: 4px;
  --border-radius-md: 8px;
  --border-width-hairline: 1px;
  --shadow-raised: 0 2px 8px 0px rgba(0,0,0,0.12);
  --opacity-muted: 0.64;
}

[data-segment="core"] {
  --color-brand-accent: #1F9E8C;
  --color-area-post: {color.brand.primary};
  --typography-fontSize-display: 56px;
}

[data-segment="labs"] {
  --color-brand-accent: #C2571E;
  --color-area-post: {color.neutral.900};
  --typography-fontSize-display: 44px;
}
```
