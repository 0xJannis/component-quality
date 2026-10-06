# Component recipes

Read the sections that match the work. Use the numeric recipes in the design-system guide and map them into the target project’s existing Tailwind tokens. Retain actual behavior and accessible primitives.

## Buttons and action hierarchy

| Variant | Fill and edge | Use |
| --- | --- | --- |
| Primary | Configurable fill, white label, `shadow-(--shadow-primary)` | Main action in a local decision group |
| Secondary | `bg-secondary`, `shadow-(--shadow-control)`; gray hover | Export, Filter, Back, supporting actions |
| Outline | Transparent/surface fill, one inset contour | Actions on already raised surfaces |
| Ghost | Transparent, muted label; gray hover/stronger text | Toolbar utilities and low-emphasis actions |
| Destructive | Restrained red fill, readable label | Destructive operation, not ordinary error text |
| Link | Text with underline on hover/focus | Navigation or inline text action |

Use 30px compact, 36px regular, 40px spacious desktop heights. Action buttons default to pills; compact square utilities have 8px corners. Labels: 12px compact, 14px regular, medium weight. Use 6px icon gap, 12–16px inline padding, 14–16px icons. A leading icon may reduce its side's padding by 2px. Expand touch targets to 44px without crowding neighbors.

Primary depth uses a contact shadow, dark outer contour, broad white upper inset, faint inner rim, and darker lower shading. This creates curvature without a gradient/glow. Secondary controls have a narrower top highlight and rim. Do not stack an extra border over a contour shadow.

Hover changes fill/text subtly. Press changes fill and optionally uses pointer-only `scale(.96)` for standalone actions. Provide a `static` escape hatch for dense tables, toolbars, calendar cells, and keyboard-heavy interfaces. Focus uses a neutral 2px ring with 2px offset. Disabled/loading controls cannot activate; `aria-disabled` alone needs an activation guard. Labels remain legible.

Loading retains label and width, adds a 14px spinner in reserved space, exposes `aria-busy`, and prevents duplicate submission. Feedback reflects a real operation. Success remains understandable without motion. Set `type="button"` on non-submit buttons in forms.

Primary customization has two levels:

```tsx
// Global: --primary, --primary-hover,
// --primary-pressed, --primary-foreground.

// Optional per-button API implemented with presets or CSS custom properties:
<Button color="green">Save changes</Button>
<Button color="blue">Publish</Button>
<Button variant="secondary">Cancel</Button>
```

This API is a recipe, not a claim about an existing Button. Add it to the shared wrapper as appropriate. The Tailwind guide supplies complete color presets. Filled-action foreground is a separate token; it must not turn dark when body text changes for light mode.

## Pills, badges, tags, counters

Distinguish action pills, static status badges, and removable filters. Only actionable elements get pointer/hover/pressed treatment. Static status is a span; a removable chip has a named remove control without nested buttons.

- Badges: 22px minimum height, full radius, 12px label, 8px inline padding, 4px icon gap, faint inset contour. Dense tables may use 14px labels to match the row.
- Neutral tone: gray fill/edge and clear text. Semantic tones: pale fill/dark text in light mode, dark fill/pale text in dark mode. Saturation stays local.
- Counters: 16–20px high, minimum 24px width, tabular figures, stable geometry.
- Filter pill: 30px high, muted label/stronger value separated by a quiet rule, 12px chevron, gray open state. Show selected value and provide a clear action where useful.
- A `+2` badge reveals omitted meaningful content through an accessible control.

## Inputs, textareas, field groups

36px fields, 8px radius, 12px inline padding, a real 1px input boundary, `bg-input-surface`. Keep fields flatter than buttons. Textareas retain these edges with readable line height and useful minimum height.

Use persistent labels; placeholders are hints. Match icon boxes and baselines. Prefix units are muted; editable values remain prominent. Reserve end padding for clear, reveal, validation, and shortcut controls.

Focus strengthens the boundary and adds a neutral ring. Invalid fields have a semantic boundary plus an `aria-describedby` message. Required/optional status is textual. Disabled differs from read-only. Password reveal preserves value/focus. Numeric fields allow empty/invalid intermediate editing without jumps.

Prefer 16px mobile text entry where necessary to avoid automatic zoom. Check long labels, wrapping errors, autofill, autocomplete, and groups with mixed control heights. Never scale text-entry fields on press.

## Sidebar and navigation

Start around 254px wide on desktop. Use a solid sidebar and quiet divider. Separate header, groups, flexible content, footer. Scroll navigation independently when the footer must remain visible.

- Rows: 30–34px desktop height, 8px radius, 12px outer inset, 8px icon/label gap, 14px labels, aligned 16px icons.
- Default: flat/muted. Hover: neutral gray. Active: raised neutral fill, stronger label, `shadow-(--shadow-control)`. Use `aria-current="page"` for active links. Default active navigation is not a colored block.
- Group labels: 11–12px muted type, restrained tracking, clear space above. Group dividers stay flat.
- Counts align at the end. Long labels do not push counts, disclosures, or focus rings out of view.
- Nested rows have consistent indentation and distinct disclosure/navigation actions.
- Footer/profile controls reuse row primitives and shared overlay styling.
- Collapsed icons have names, adequate targets, and tooltips where useful. Collapse control reflects state.
- Narrow layouts use a proper sheet with focus containment, close/Escape, and focus restoration. Do not squeeze a desktop sidebar beside mobile content.

Fumadocs or other docs navigation follows these roles while preserving search, current-page indication, mobile behavior, and grouping. Avoid blanket anchor selectors that erase hierarchy.

## Tables

A table is a structured information surface, not a grid of cards.

| Part | Recipe |
| --- | --- |
| Header | 38px high, 12px muted labels, quiet/table fill |
| Body | 42px compact row, 14px values; taller for two-line content |
| Cell inset | 12px horizontally; aligned header/body |
| Dividers | One low-contrast horizontal rule; vertical only for real groups |
| Hover | Neutral gray, immediate or ≤100ms color change |
| Selected | Neutral fill plus checked/explicit selection cue |
| Numbers | Right aligned, tabular, consistent units and precision |
| Identity | Small icon/avatar, strong name, optional secondary line |
| Actions | Stable trailing column, named controls, keyboard/touch access |

Use semantic `table`, `thead`, `tbody`, `th scope`, and `td`. Do not use ARIA grid without its keyboard interaction. Sorting headers contain buttons; `aria-sort` identifies sorted columns. Icons do not shift labels. Only meaningfully sortable columns offer sorting.

Use one bounded horizontal scroll container on small screens, without page-wide overflow. Keep identity/actions accessible. If converting to mobile lists, retain essential values with labels. Sticky headers/pinned columns need solid theme-aware fills and correct stacking.

Selection uses labeled checkboxes and an actual indeterminate select-all. Communicate whether all means page or filtered dataset. Bulk actions operate on the indicated scope. Search/filter changes reconcile page index and selection. Use stable row IDs, not array position.

Loading rows match geometry. Empty data offers a relevant action; no-match results offer clear/reset; errors offer real retry. Pagination disables at boundaries. Counts/totals explicitly describe their dataset. Test sorting after filtering, zero matches from a later page, column visibility/resizing, and selection where implemented.

## Search and command results

Record search repeats the table hierarchy: identity, category/status, owner, numeric summary, recent activity. Headers/results share column templates. Reduce secondary columns responsively while retaining identity and distinguishing context.

A command list uses its accessible combobox/listbox primitive. Its options can be visually columnar without pretending to be table rows. Do not put independently tabbable actions inside options. A true search-results table instead uses table semantics and its own appropriate navigation model.

Search field: full-width top row, leading icon, useful label/hint, optional Escape keycap. Results: 40–44px rows, 8px radius, gray highlighted state, no per-row shadow. Pointer hover and keyboard selection are coherent. Footer: quiet divider and truthful keyboard hints.

Use the shared overlay surface/shadow. Large record search can be 880–960px wide, constrained to viewport width and height. Support clearing, loading, no results, arrows, Enter, Escape, focus return, and active-result scrolling. Frequent command opening/typing stays immediate without staged entrances.

## Cards, panels, metrics

Dark cards are neutral `#151515`; light cards are white. Default 8px radius, 12px for larger panels, 16px padding, shared card shadow. Avoid a double edge from border plus contour.

Static cards do not need hover effects. Clickable cards use real link/button behavior and focus without nested interactive conflicts. Header/body/footer align to the same inset. Dividers provide structure rather than extra shadows.

Metrics have clear labels, tabular values, supporting descriptions, and explicit comparison periods. Small colored icon containers may have restrained inset highlights. Charts use card geometry without heavy inner cards; do not invent decorative data.

## Selection, tabs, range controls

- Checkbox/radio: 16–18px visual control, associated label, clear checked/indeterminate states, neutral unchecked boundary, visible focus, larger hit area. Preserve the correct single/multiple choice model.
- Switch: recessed 28–32px track, 12–16px thumb, precise travel/contact shadow, stable label/layout, semantic checked state.
- Slider: recessed 4px track, 12–14px thumb with larger hit target, label/value. One thumb for a scalar, one per range endpoint. Test bounds, steps, keyboard, disabled, controlled value.
- Progress/meter: actual value or explicit indeterminate state and readable text. Segmented bars can be 14px tall with 10px segments/2px gaps; retain numeric semantics.
- Underline tabs: flat row, 1px active underline, muted inactive text. Segmented controls: neutral tray with raised selection. Keep the geometries distinct and avoid label-width shifts.
- Tabs implement panel/keyboard semantics. Filters use selection semantics suited to filtering. Nested tab styles must not leak into children.
- Pagination has current/boundary states, meaningful labels, stable geometry, and appropriate link/button semantics.

## Overlays and disclosure

Menus/selects/comboboxes/popovers/hover cards use solid themed surfaces, `shadow-(--shadow-overlay)`, 8–12px radius, and restrained contours. Inner radius follows inset: 12px panel, 4px padding, 8px items. Highlighted rows are gray. Shortcuts are muted; destructive/disabled states are explicit.

Dialogs have a title and optional description inside accessible content. Scrims are 45–60% black with optional 6px blur. Centered dialogs keep a centered origin; anchored popups originate at their trigger. Constrain height and allow scrolling so actions remain reachable.

Side sheets use a flat frame, edge divider, and roughly 480–560px constrained width. Bottom drawers can round upper corners. Preserve focus, portal themes, scroll lock, outside-click rules, and nested stacking.

Tooltips provide concise nonessential help with a short initial delay and immediate adjacent activation. Required instructions remain accessible outside tooltips. Accordions use flat disclosure rows, expanded state, and restrained nested styling.

## Remaining families

| Family | Required decisions |
| --- | --- |
| Calendar/date/time | Tabular grid, today/selection/range distinction, neutral hover, disabled dates, locale/week start, keyboard, clear/reset |
| Avatar/image/aspect ratio | Stable geometry, fallback, crop, 1px pure black/white low-opacity outline by theme, useful alt text |
| Breadcrumb/navigation menu/menubar | Current location, keyboard menus, correct links, stable targets, compact overflow |
| Carousel/scroll area/resizable | Bounds, keyboard controls, handle targets, unclipped focus, alternative to dragging |
| Charts | Neutral axes, deliberate series colors, themed tooltip, summary, real units/data, zero/empty states |
| Upload/attachment/OTP | Entry/paste, progress/error/retry/removal, long names, supported formats, truthful success |
| Alert/toast/banner | Local semantic color, clear action, correct announcement urgency, dismissibility, repeat events |
| Message/bubble/questionnaire | Sender/role hierarchy, actual pending/streaming state, choices, validation, long text, stable scrolling |
| Skeleton/spinner/empty | Matching final geometry, reserved space, honest progress, reduced motion, useful next action |
| Typography/label/separator/kbd/marker | Hierarchy, reading order, quiet boundaries, correct shortcuts, semantic purpose |
| Kanban/workflow/timeline/tree | Shared surfaces, drag alternatives, selection, editing, empty states, stable IDs, valid transitions |

For other components, resolve visual roles and the same state checklist. A recipe is a design method, not permission to skip implementation testing.
