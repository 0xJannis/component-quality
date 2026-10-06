---
name: component-quality
description: Redesign and polish React applications, shadcn components, and custom interfaces with precise Tailwind styling. Use for frontend redesigns, component improvements, new screens, and visual quality reviews. Covers tactile buttons, aligned tables, neutral sidebars, layered shadows, light and dark themes, and adversarial interaction checks that find and fix breaking cases. Adapt to the existing React framework and component system while preserving behavior. Excludes backend-only and content-only work.
license: MIT
metadata:
  version: "1.2.0"
---

# Component Quality — application redesign

Turn the requested application or component into a coherent, carefully finished interface. Quality must reach ordinary controls, dense tables, empty states, dialogs, and secondary screens as well as the main page. This is one self-contained skill; it does not require loading other design skills.

## Design contract

1. **Use Tailwind for all component and layout styling.** Put state styles, responsive behavior, geometry, typography, and shadows in utilities and shared variant recipes. A global Tailwind entry may contain imports, theme tokens, base resets, and necessary library configuration. Do not create component CSS files, CSS modules, styled-components, or parallel handwritten selector systems. Runtime coordinates and user-selected values may use inline CSS custom properties; static styling belongs in Tailwind.
2. **Use true neutral surfaces.** Dark mode has black and charcoal cards, black/gray navigation, and gray hover/selection surfaces. Light mode has white cards and neutral gray structure. Do not introduce blue, slate, or purple tint into generic hover, focus, selected rows, or cards. Color is intentional: a configurable primary action, a meaningful status, a chart series, or existing brand content.
3. **Make depth deliberate.** Distinguish a structural divider, an outer contour, a top inset highlight, a recessed track, and a floating shadow. Do not give every container a bevel. Use the calibrated recipes instead of substituting a generic shadow everywhere.
4. **Finish states and behavior together.** Default, hover, pressed, focus, selected/open, disabled, loading, empty, error, and success states belong to component design. Use the states relevant to its actual behavior.
5. **Preserve the application.** Retain its framework, routes, data, permissions, integrations, content, accessible behaviors, and established APIs. Restyle shared primitives before changing every call site. Do not replace working interactions with static mockups, simulate persistence without disclosure, or change product semantics for visual convenience.
6. **Support both themes.** For a complete system, ship light and dark tokens and verify both. Respect an explicit single-theme request. Preserve a supplied brand palette for intentional actions while keeping generic surfaces neutral.
7. **Inspect the rendered result.** Types and builds do not prove menus, tables, focus, responsive layouts, or theme portals work. Track actual coverage; never label an untested state as verified or promise that no future use can break.
8. **Actively try to break affected components.** Go beyond the expected path: challenge relevant input boundaries, rapid actions, async failures, state combinations, and constrained layouts. Reproduce failures, fix their cause, and re-run the breaking case. A component must recover predictably without lost input, stale results, duplicate actions, or trapped focus.

## Start with the real application

Inspect project instructions, installed dependency versions, styling entry point, semantic tokens, shared components, page shell, representative routes, and available tests. Open the running interface before changing the visual system when possible.

Determine the authorized scope:

- **Application redesign:** inventory all routes, component families, major composites, menus/dialogs, and responsive shells in scope. A polished landing page alone is incomplete.
- **Component library:** inventory the requested catalog, exports, variants, examples, docs, and installation dependencies. If asked for all shadcn components, compare against the current official catalog and record the date. A familiar subset or old hardcoded count is insufficient.
- **Focused improvement:** apply the same standard to the requested component and affected compositions. Do not turn a button fix into a redesign of unrelated screens.

Keep a compact working inventory: component/route, shared primitive, states, themes, widths, and verification status. Reuse project tracking conventions; create a separate checklist only when broad coverage needs one. Resolve routine implementation decisions without pausing for approval.

## Read the right details

For implementation, read [the design system](references/design-system.md) and relevant sections of [component recipes](references/component-recipes.md).

For an application or library, also read [application composition](references/application-patterns.md). Read [interaction and motion](references/interaction.md) before changing behavior or animation. Finish with [verification](references/verification.md).

For animated components, also read [motion performance](references/motion-performance.md): first-render behavior, property selection, interruption, rendering cost, and reduced-motion checks are part of component quality.

Read [adversarial component testing](references/adversarial-testing.md) before verifying interactive changes or a complete redesign. Use its failure cases and recovery expectations to test the requested scope, including static components' content and layout boundaries.

Read [React integration](references/react-integration.md) to adapt the workflow to the installed framework, shadcn, headless primitives, or a custom design system. Read [Tailwind implementation](references/tailwind.md) when editing tokens and component variants. This package contains instructions only; modify the target application's existing source rather than installing a replacement component library.

## Implement from the inside out

Fix shared tokens, then primitives, then composed controls, then page layouts:

| Layer | Responsibilities |
| --- | --- |
| Tokens | Neutral themes, accents, boundaries, typography, radii, shadows, timing |
| Primitives | Buttons, fields, badges, selection controls, surfaces, menus, dialogs |
| Composites | Search tables, filter bars, forms, navigation, calendars, uploads |
| Application blocks | Tables with toolbars, settings, dashboards, boards, workflows |
| Screens | Hierarchy, spacing, responsive composition, real data and actions |

Use existing accessible primitives. In a new React project, Base UI is a suitable unstyled behavior layer; shadcn source is also fully editable. Neither dictates the visual style. Avoid replacing working Radix/shadcn behavior simply to gain styling freedom. Align API names and state attributes with the installed version.

Use explicit variants instead of repeated local exceptions. Primary color should be changeable through semantic tokens application-wide and an optional per-instance variant. Changing primary hue must not change neutral row hover or sidebar selection.

Make the default variant complete before adding customization. Reuse the host's providers, tokens, and motion system; do not require new setup or dependencies merely to obtain polished defaults. Match related controls' timing and density. Animation must have a purpose and must never delay input, conceal a state, or substitute for correct behavior.

Avoid dynamically assembled Tailwind names such as `bg-${color}-600`. Use complete static class maps or CSS custom properties for runtime values. Decorative layers inherit radii, ignore pointer input, and do not clip focus rings.

## Apply the standard to every component

For an unfamiliar component, decompose it into structural surface, control, selected item, track, content card, overlay, and status. Resolve each part's surface, contour, radius, padding, type hierarchy, icon alignment, elevation, states, keyboard model, touch target, and responsive behavior.

The catalog is illustrative, not a whitelist. Media, commerce, editors, trees, maps, charts, command interfaces, uploads, and future components all use this method. Do not force every product into a dashboard layout or every element into a pill.

When examples or a library are requested, use actual exported components, demonstrate meaningful variants and edge states, provide copyable code with complete imports, and give larger blocks working local behavior or real application integration. See application composition for distribution and attribution.

## Quality gate

Before finishing, every affected component must have a deliberate surface/contour, coherent geometry, readable type, aligned icons, complete relevant states, usable keyboard/touch behavior, and correct light/dark rendering. Shared components must compose correctly in their real screens and withstand the relevant breaking cases. If one of these fails, fix it and recheck affected consumers. Report any case that could not be exercised instead of treating it as a pass.

## Complete the work

Exercise each affected component family and every route in a whole-application redesign. Inspect both themes, desktop and narrow layouts, keyboard use, state changes, long content, and overlay stacking. Revisit every distinct composition affected by shared token changes; one passing button example does not validate an application.

After the baseline works, deliberately stress the affected components using local fixtures or the project's test environment. Check the expected recovery as well as the initial failure: errors must be actionable, entered data must remain available, and the component must become usable again. Add focused regression coverage for significant reproduced failures using the project's existing tools.

Fix defects, run relevant project checks, and check a production build when styling order, server rendering, or generated routes could differ from development. Record the breaking cases attempted, fixes, retest results, and what could not be verified. Once applicable checks pass, broaden testing only for a new change or unresolved concern.

Report the outcome, checks, and material limits concisely. For an audit/review, use `Severity | Location | Before | After | Why`, one row per root cause. Use `Not verified` for unobserved states. A failed functional check remains a blocker; a style preference does not justify an approval gate. For implementation work, fix findings instead of stopping at a review table.

Apply these instructions to the requested source files and finish with working, verified changes. Do not treat this skill as authorization to publish, deploy, change access, or expand the task beyond the user’s request.
