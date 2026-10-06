# Tailwind implementation

## Keep styles in the utility system

Use the project's installed Tailwind version and class-composition helper. Component geometry, responsive layout, typography, borders, shadows, and state styling belong in complete utility strings. Shared variants belong in the existing variant system. Do not add component CSS files, CSS modules, CSS-in-JS, or a second selector-based design system.

The global Tailwind entry may define imports, theme variables, resets, color-scheme, and necessary library configuration. Dynamic coordinates, progress values, and arbitrary user colors may use narrowly scoped inline custom properties. Avoid using inline style objects for static component styling.

## Semantic tokens

Map the [design-system values](design-system.md) into the host's existing roles. Reuse `background`, `foreground`, `card`, `popover`, `primary`, `secondary`, `muted`, `accent`, `border`, `input`, and `ring` where present. In many shadcn projects `accent` means item hover: keep it neutral. Add `surface-hover`, `surface-selected`, `input-surface`, and primary state tokens only when needed.

Keep a single source of truth per role. Do not mix raw HSL channels, complete hex colors, and `oklch()` under variables consumed in incompatible ways. Inspect whether existing utilities expect `hsl(var(--token))` or `var(--token)` before converting values.

This is an illustrative Tailwind 4 mapping, to merge into the application's global entry. The values for all roles and shadow layers live in the design-system guide; do not replace an entire working theme with this partial example.

```css
@import "tailwindcss";
/* Only add this if the host uses a .dark class and has no equivalent. */
@custom-variant dark (&:where(.dark, .dark *));

@theme inline {
  --color-background: var(--background);
  --color-foreground: var(--foreground);
  --color-card: var(--card);
  --color-card-foreground: var(--foreground);
  --color-secondary: var(--secondary);
  --color-secondary-foreground: var(--foreground);
  --color-surface-hover: var(--surface-hover);
  --color-surface-selected: var(--surface-selected);
  --color-input-surface: var(--input-surface);
  --color-primary: var(--primary);
  --color-primary-hover: var(--primary-hover);
  --color-primary-pressed: var(--primary-pressed);
  --color-primary-foreground: var(--primary-foreground);
  --color-ring: var(--ring);
}

:root {
  color-scheme: light;
  --background: #f7f7f7;
  --foreground: #202020;
  --card: #ffffff;
  --secondary: #ffffff;
  --surface-hover: #eeeeee;
  --surface-selected: #e9e9e9;
  --input-surface: #ffffff;
  --primary: #242424;
  --primary-hover: #303030;
  --primary-pressed: #181818;
  --primary-foreground: #ffffff;
  --ring: #777777;
  --shadow-control: 0 0 0 1px rgb(0 0 0 / 8%),
    0 1px 2px rgb(0 0 0 / 8%), inset 0 1px 0 rgb(255 255 255 / 90%);
}

.dark {
  color-scheme: dark;
  --background: #161616;
  --foreground: #fafafa;
  --card: #151515;
  --secondary: #1e1e1e;
  --surface-hover: #2a2a2a;
  --surface-selected: #252525;
  --input-surface: #1e1e1e;
  --primary: #3c3c3c;
  --primary-hover: #484848;
  --primary-pressed: #303030;
  --primary-foreground: #ffffff;
  --ring: #a4a4a4;
  --shadow-control: 0 0 0 1px rgb(0 0 0 / 40%),
    inset 0 1px 0 rgb(255 255 255 / 10%),
    inset 0 0 0 1px rgb(255 255 255 / 6%);
}
```

Add the project's remaining semantic colors and `--shadow-primary`, `--shadow-card`, and `--shadow-overlay` using the complete design-system recipes. Tokens belong in the existing theme layer so cascade order is deliberate. Avoid accidentally overriding a configured brand primary when adding dark defaults.

For Tailwind 3, retain its configuration and map complete-color variables with entries such as `colors: { primary: 'var(--primary)', 'surface-hover': 'var(--surface-hover)' }` and `boxShadow: { control: 'var(--shadow-control)' }` under `theme.extend`. Use `shadow-control` or `shadow-[var(--shadow-control)]` instead of Tailwind 4's `shadow-(--shadow-control)`. Preserve the project's correct dark-mode configuration. If opacity modifiers are required, use the version's supported alpha-aware token format consistently.

## Component utilities

Adapt these class recipes inside existing components; they are not new component implementations.

| Role | Tailwind 4 recipe |
| --- | --- |
| Regular button geometry | `inline-flex h-9 shrink-0 items-center justify-center gap-1.5 whitespace-nowrap rounded-full px-4 text-sm font-medium` |
| Shared focus | `focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-ring focus-visible:ring-offset-2 focus-visible:ring-offset-background` |
| Primary surface | `bg-primary text-primary-foreground shadow-(--shadow-primary) enabled:hover:bg-primary-hover enabled:active:bg-primary-pressed` |
| Secondary surface | `bg-secondary text-secondary-foreground shadow-(--shadow-control) enabled:hover:bg-surface-hover` |
| Native disabled button | `disabled:pointer-events-none disabled:opacity-50` with the actual `disabled` attribute |
| Field geometry | `h-9 min-w-0 rounded-lg border border-input bg-input-surface px-3 text-base sm:text-sm` |
| Card | `rounded-lg bg-card p-4 text-card-foreground shadow-(--shadow-card)` |
| Table row | `h-[42px] border-b border-border hover:bg-surface-hover data-[state=selected]:bg-surface-selected` |
| Numeric cell | `px-3 text-right text-sm tabular-nums` |
| Sidebar row | `flex min-h-8 min-w-0 items-center gap-2 rounded-lg px-2 text-sm hover:bg-surface-hover` |
| Active navigation | `aria-[current=page]:bg-secondary aria-[current=page]:text-foreground aria-[current=page]:shadow-(--shadow-control)` |

These recipes assume the named tokens have been mapped. The example mapping above is deliberately partial: add `input` and `border` from the design-system table before using those utilities. Add focus styling to interactive rows, not static rows. Use the component primitive's supported disabled/highlighted attributes for non-native buttons. A visual 32px control can have a larger target, but its expanded hit area must not overlap neighbors.

Transition only the properties that change. Hover styles should follow pointer capability where the installed Tailwind version needs explicit handling. Keep motion in [the interaction guide](interaction.md). Verify arbitrary utilities and state variants compile in the installed version; do not copy syntax blindly between versions.

## Change the primary color once

The following are complete filled-action presets with white labels. They leave neutral hover, selection, cards, and navigation unchanged.

| Preset | Fill | Hover | Pressed | Foreground |
| --- | --- | --- | --- | --- |
| Neutral | `#3c3c3c` | `#484848` | `#303030` | `#ffffff` |
| Blue | `#2058d4` | `#2863e0` | `#1949b9` | `#ffffff` |
| Green | `#1d6f42` | `#23804c` | `#185e38` | `#ffffff` |
| Purple | `#4124fb` | `#4b30ff` | `#391be7` | `#ffffff` |
| Orange | `#a7481b` | `#ba5120` | `#913e17` | `#ffffff` |
| Red | `#a92e3d` | `#bd3547` | `#922635` | `#ffffff` |

Set all four semantic variables in the application's existing theme configuration. For per-instance colors, use a finite, statically written variant map, for example:

```tsx
const actionColors = {
  green: "[--primary:#1d6f42] [--primary-hover:#23804c] [--primary-pressed:#185e38] [--primary-foreground:#ffffff]",
  blue: "[--primary:#2058d4] [--primary-hover:#2863e0] [--primary-pressed:#1949b9] [--primary-foreground:#ffffff]",
} as const;
```

Compose that map inside the existing Button variant API. Do not generate `bg-${color}-600` strings; production extraction will miss them. A theme wrapper's custom properties are inherited, but a descendant `.dark` rule can override them: put shared brand overrides at the same theme scope or give each theme an explicit primary set.

Check the actual rendered foreground/fill contrast for custom hues and states. Aim for at least 4.5:1 for ordinary text and 3:1 for essential control boundaries/focus cues against their adjacent surfaces. Shadows and white highlights can change local perceived contrast; inspect the rendered result as well as the base color values. Do not use action fills as an automatic palette for status badges or charts.

## Extraction and cascade checks

Ensure Tailwind scans every component package in the monorepo. On Tailwind 4 use the appropriate `@source` paths when automatic detection excludes a shared package; on Tailwind 3 check `content` globs. Keep source paths relative to the correct configuration/stylesheet.

Check utility ordering and class merging after adding arbitrary shadows or theme variables. Remove obsolete conflicting classes in the edited component instead of continually appending overrides. Compile a production build when new scan paths, variants, tokens, or distribution boundaries are involved, and inspect at least one consumer of each changed primitive in both themes.
