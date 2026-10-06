# Interaction and motion

Motion explains change or acknowledges input. Frequent navigation, typing, selection, and command opening should feel immediate.

| Interaction | Default |
| --- | --- |
| Row hover / keyboard selection | Instant or ≤100ms color change; no movement |
| Button hover / press | 150ms, exact properties, optional pointer press scale `.96` |
| Popover / select | 150–200ms enter, 125–150ms exit; opacity and scale `.97` to `1` |
| Dialog / sheet | 200–250ms, restrained transform/opacity |
| Tooltip | Initial delay, immediate adjacent tooltips, no instant-open transition |
| Switch thumb | 150–200ms transform; stable track and label |
| Theme swap | Suppress color/border/shadow transitions during the swap |

Use `cubic-bezier(0.2, 0, 0, 1)` for short transitions. An established drawer may retain `cubic-bezier(0.32, 0.72, 0, 1)`. Prefer interruptible transitions over restarted keyframes. Avoid `transition-all`, `ease-in` entrances, `scale(0)` panels, and layout-heavy animation on frequent actions.

```tsx
// Pointer-only feedback; clear transient state on release/cancel/blur.
className={cn(
  'transition-[background-color,color,box-shadow,transform] duration-150',
  'ease-[cubic-bezier(0.2,0,0,1)] motion-reduce:transition-none',
  !isStatic && pointerPressed && 'motion-safe:scale-[0.96]',
)}

// Base UI-style popup. Verify the installed primitive's actual attributes.
className="origin-[var(--transform-origin)] transition-[opacity,transform] duration-150 ease-[cubic-bezier(0.2,0,0,1)] data-starting-style:scale-[0.97] data-starting-style:opacity-0 data-ending-style:scale-[0.97] data-ending-style:opacity-0 motion-reduce:transition-none motion-reduce:transform-none"
```

Centered dialogs retain a centered origin; anchored popups originate at their trigger. For dense utility controls, use static press feedback instead of adding input-modality state merely to animate them.

Occasional contextual icon swaps use a stable box. When used, use opacity `0 → 1`, scale `.25 → 1`, blur `4px → 0`, 300ms, zero bounce. Frequent keyboard feedback stays immediate; reduced motion uses a static cue. Use the existing animation system or Tailwind transitions, without adding a dependency for one icon. Static navigation icons do not animate.

## Complete states

- Hover: neutral, pointer-appropriate, no layout shift; essential actions remain available without hover.
- Pressed: immediate, no double activation, disabled guard works.
- Focus: neutral visible indicator, unclipped, logical traversal.
- Selected/open: semantic state and readable label; use an extra cue when color alone is insufficient.
- Disabled: non-actionable and legible; styling alone is insufficient.
- Loading: real operation, stable geometry, accessible status, duplicate prevention when needed.
- Empty/no match: accurate explanation and relevant next action.
- Error: specific linked message, preserved input, real retry where appropriate.
- Success: truthful completion and stable feedback; do not invent persistence.

## Keyboard and semantics

Use native elements and existing accessible primitives. Test Tab, Enter/Space, Escape, expected arrow keys, focus restoration, and disabled-item handling. Avoid click-only menus and comboboxes.

Modal focus stays inside and returns appropriately. Nested overlays must not be clipped or trapped behind parents. Respect the product's dismissal/data-loss behavior. Theme tokens must reach portals.

Name icon actions, associate field labels/descriptions, hide decorative icons, use links for navigation and buttons for actions, and avoid nested interactive elements. Keep accessible names stable during loading. Avoid duplicate live-region announcements.

## Gestures and custom controls

Dragging needs a handle, threshold, valid-target feedback, cancellation, and a keyboard or explicit move alternative. Custom drags capture the initiating pointer, ignore additional pointers, and respect intended bounds. Restore focus after moving/removing items.

Workflow editors distinguish pan/select/drag/connect/edit/execute. Validate edges and document actual limits. A visual execution simulation is not an external integration. Kanban preserves stable IDs and data while filtering. Resizing needs a keyboard or explicit size alternative.

## Theme and reduced motion

Use the existing provider's transition suppression, such as `disableTransitionOnChange` where supported. Otherwise suppress transitions only around a theme flip, apply the theme, allow a paint, and clean up. Do not disable transitions permanently.

Remove movement, scale, and continuous shimmer under reduced motion, retaining text/icon/surface feedback. Pause nonessential hidden animation. Test rapid open/close and interruption. Inspect slowly when tools allow; report slow-motion or physical touch testing as unverified if unavailable.
