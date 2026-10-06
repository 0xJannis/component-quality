# Shared design system

## Surface hierarchy

Use semantic tokens. Do not tint generic interactions with the primary hue.

| Role | Light | Dark |
| --- | --- | --- |
| Application background | `#f7f7f7` | `#161616` |
| Sidebar | `#f7f7f7` | `#171717` |
| Content card | `#ffffff` | `#151515` |
| Floating surface | `#ffffff` | `#1b1b1b` |
| Secondary control / field | `#ffffff` | `#1e1e1e` |
| Neutral hover | `#eeeeee` | `#2a2a2a` |
| Neutral selected | `#e9e9e9` | `#252525` |
| Raised selected navigation | `#ffffff` | `#2a2a2a` |
| Structural divider | `#e8e8e8` | `#232323` |
| Field boundary | `#d5d5d5` | `#393939` |
| Main text | `#202020` | `#fafafa` |
| Secondary text | `#666666` | `#a4a4a4` |
| Neutral focus ring | `#777777` | `#a4a4a4` |

Use a neutral primary preset when the project has no established accent. Retain the application's deliberate primary hue or the user's requested hue. Primary fill, hover, pressed, and foreground form a coherent set. Blue/green/purple buttons are deliberate variants, never a source of colored row hover. Use one action hue across related screens unless a semantic distinction warrants otherwise.

Theme changes update text, shadows, contours, fields, status tones, charts, portals, native controls, and backgrounds together. A light theme is not a white background under dark components. Use a root theme class or established selector so portals inherit it. Suppress transitions for the theme swap. Avoid per-component theme conditionals.

Map into existing semantic roles. Tailwind 4's `@theme inline` bridges custom properties to utilities; state styling stays in JSX/templates. On Tailwind 3, map the properties in `theme.extend`.

## Depth anatomy

| Layer | Purpose | Implementation |
| --- | --- | --- |
| Structural divider | Separates data/layout regions | Real 1px border |
| Outer contour | Crisp edge against surrounding surface | `0 0 0 1px` shadow |
| Top highlight | Light arriving from above | `inset 0 1px 0`, low-opacity white |
| Inner rim | Defines the interior edge | `inset 0 0 0 1px` |
| Lower shading | Curves a prominent action | Broad dark negative-y inset |
| Contact shadow | Small lift directly underneath | Short low-opacity shadow |
| Ambient shadow | Separates a floating popup | Broader controlled-opacity shadow |

A white inset rim is edge lighting; a dark inset creates recession. Resolve the visual purpose of each layer. Keep lighting consistent across neighbors.

### Dark recipes

```text
Control:
  0 0 0 1px rgb(0 0 0 / 40%),
  inset 0 1px 0 rgb(255 255 255 / 10%),
  inset 0 0 0 1px rgb(255 255 255 / 6%)

Primary action:
  0 4px 4px rgb(0 0 0 / 24%),
  0 0 0 1px #0e0e0e,
  inset 0 4px 6px rgb(255 255 255 / 20%),
  inset 0 0 0 1px rgb(255 255 255 / 15%),
  inset 0 -8px 14px rgb(0 0 0 / 15%)

Card:
  0 4px 4px rgb(0 0 0 / 18%),
  0 0 0 1px #0e0e0e,
  inset 0 1px 0 rgb(255 255 255 / 8%),
  inset 0 0 0 1px rgb(255 255 255 / 8%)

Overlay:
  0 16px 40px rgb(0 0 0 / 50%),
  0 0 0 1px rgb(255 255 255 / 12%)
```

Use these calibrated recipes as the initial quality baseline. Adjust strength for the composition; do not combine border, outline, ring, and shadow into an accidental double edge. Focus rings are separate transient feedback.

### Light recipes

```text
Control:
  0 0 0 1px rgb(0 0 0 / 8%),
  0 1px 2px rgb(0 0 0 / 8%),
  inset 0 1px 0 rgb(255 255 255 / 90%)

Primary action:
  0 2px 3px rgb(0 0 0 / 16%),
  0 0 0 1px rgb(0 0 0 / 18%),
  inset 0 2px 4px rgb(255 255 255 / 18%),
  inset 0 0 0 1px rgb(255 255 255 / 10%),
  inset 0 -4px 8px rgb(0 0 0 / 12%)

Card:
  0 0 0 1px rgb(0 0 0 / 6%),
  0 1px 2px -1px rgb(0 0 0 / 6%),
  0 2px 4px rgb(0 0 0 / 4%)

Overlay:
  0 0 0 1px rgb(0 0 0 / 8%),
  0 12px 32px rgb(0 0 0 / 15%),
  0 3px 8px rgb(0 0 0 / 6%)
```

Shadows provide lift; borders provide structure. Keep solid surfaces, with blur mainly on overlay scrims. Complexity does not imply stronger elevation.

## Geometry and type

- Spacing: 4px base; 4, 8, 12, 16, 20, 24, 32px relationships, plus justified optical corrections.
- Heights: 30px compact action, 36px field/action, 40px spacious action, 42px compact table row, 38px header. Aim for 44px coarse-pointer targets.
- Radii: 8px field/card/nav, 8–12px panel, full pills for action buttons/tags. A checkbox is not a pill.
- Concentric inset: outer radius = inner radius + padding. A 12px panel with 4px inset has 8px items. At ≥24px separation, choose independent radii.
- Type: existing font or Geist/system sans. Start at 14px body/control, 12px supporting text, 16px compact section heading. Page headings and metrics scale by context.
- Prefer normal/medium weights and restrained tracking. Numbers and changing counters use tabular figures. Units and dates follow consistent formatting.
- Align icon/label optically. Prefer 16px/20px native icons; match stroke weight to text. One family per surface, `currentColor` for state coloring.

Long translated labels, multi-line errors, zero/large values, empty lists, and missing avatars must fit. Use logical spacing for RTL. Truncate only when the full value remains practically accessible.

## Icon and image finish

Judge icons at their smallest actual rendered size. Fine detail that looks clear at 48px can disappear at 16px; use an appropriate simplified glyph and the set's native grid rather than shrinking detailed artwork arbitrarily. Compare optical weight to the adjacent label. For an adjustable 24px stroke set, 1.5px beside regular text and 2px beside medium/semibold text are useful starting points; preserve the set's conventions and inspect the result at render size.

An asymmetric glyph may need a small optical adjustment within its fixed box. Correct the glyph or shared wrapper before adding inconsistent margins at every call site. Use logical padding around labels; a play triangle's optical correction belongs to the physical glyph, not automatically to reading direction.

Use `currentColor` so the same icon follows text states. A filled/outline pair can clarify selection when the set supports it; preserve an additional semantic state rather than relying on the swap alone. Do not animate icons merely because their color changes.

Mirror navigation arrows or chevrons when their meaning follows reading direction, using a selective utility such as `rtl:-scale-x-100`. Do not blanket-mirror logos, checkmarks, physical objects, or media controls. Check directional meaning and any overlays on composite glyphs.

Where images or avatars need edge definition, draw the quiet 1px pure-black/light or pure-white/dark outline inside the image edge so dimensions do not change. Match its radius to the crop. A decorative edge is separate from the focus indicator on an interactive image; neither may hide the other.
