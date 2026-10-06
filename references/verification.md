# Verification

Coverage describes observations, not confidence. For complete application/library work, cover every unique component and route/composition. Shared primitives can share behavior tests; every distinct composition still needs rendered inspection.

| Component / route | Light / dark | Widths | Keyboard / states | Breaking cases | Result / evidence |
| --- | --- | --- | --- | --- | --- |
| Actual name/path | Pass / fail / not verified | Widths checked | Actions checked | Input, sequence, failure tested | Test/screenshot/defect |

Use existing tools. Test observable behavior, geometry, state, or useful invariants, not class strings that mirror implementation. Add tests for substantial logic/shared behavior; small reversible polish does not need new infrastructure.

## Source and build

- Component/layout styling is Tailwind. Global CSS contains configuration/tokens/resets/necessary library setup, not hidden component recipes.
- Complete static utilities or controlled CSS variables survive production extraction.
- Themes cover surfaces, text, borders, shadows, statuses, and portals. Filled action labels keep contrast.
- Accessible names, labels, semantics, disabled guards, and error descriptions remain intact.
- No unresolved imports, invalid nesting, missing keys, hydration errors, or unhandled exceptions are introduced.
- Relevant type/lint/tests pass. Build/install checks match scope; report actual limits.

## Rendered checks

Inspect desktop and around 390px or the product's narrow breakpoint, plus intermediate composition breakpoints where relevant. Use long/empty content and zoom when appropriate.

- Surface hierarchy is legible in both themes; generic hover/selection has no blue tint.
- Contours are crisp, lighting is consistent, and no double edges appear.
- Fields/buttons/icons/baselines align; nested radii match padding.
- Tables align headers/cells/numbers; sticky surfaces are opaque.
- Focus stays visible/unclipped; touch targets do not intercept neighbors.
- Page width stays bounded. Intended table/canvas scrolling works locally.
- Long names, translation, validation, missing images, zero/large values fit.
- Reduced motion preserves meaning; theme swaps do not smear through transitions.

## Defining interactions

| Family | Exercise |
| --- | --- |
| Buttons | Pointer/keyboard, disabled/loading, duplicate prevention, submit type, focus |
| Fields/forms | Edit/clear, label focus, validation, preserved input, actual save outcome |
| Checkbox/radio/switch/toggle | Keyboard, state, disabled, controlled updates, announced value |
| Select/combobox/menu | Open/navigate/select, no results, Escape/outside, disabled options, focus return |
| Dialog/popover/sheet/drawer | Focus placement/trap, close/Escape, scroll, nesting, portal themes |
| Tabs/accordion/navigation | Active content, keyboard, collapse, nesting, mobile shell |
| Table | Sort/filter/clear, zero matches, pagination bounds/reset, selection scope, actions |
| Search/command | Query/arrows/Enter/Escape, active-result scrolling, no match, correct action |
| Calendar/date picker | Bounds, selected/disabled/range/today, keyboard, locale, clear, controlled value |
| Slider/resizable/drag | Bounds, scalar/range, steps, keyboard/alternatives, cancellation, value feedback |
| Kanban/workflow | Add/edit/move/remove, filtered data retention, stable IDs, valid edges, alternatives |
| Media/upload/carousel | Load/error, formats, progress/cancel/retry, controls, bounds, fallback |
| Charts/progress | Values/units, zero/empty, tooltip, accessible summary, contrast |
| Feedback | Trigger/action/dismiss, timers, announcements, repeats, reduced motion |
| Static display/layout | Semantics, themes, overflow, content stress, alignment; interactions N/A |

For "every component", each required entry needs a render check and its defining behavior checked when interactive. Record N/A for static components. One tested instance does not cover every possible consumer configuration; state what was exercised.

## Challenge the component

Run the relevant cases from [adversarial testing](adversarial-testing.md) after the normal behavior works. Static components still need long/empty content, constrained layout, theme, and zoom checks. Interactive components also need boundary inputs, interrupted sequences, and applicable failure/recovery checks.

Use deterministic fixtures and existing test tools to reproduce timing or data failures. Assertions should express the user-visible contract: one submission, newest search result, retained draft, correct selection, reachable action, or restored focus. A screenshot alone cannot establish these outcomes.

Record `Component | Breaking case | Expected recovery | Observed result | Fix / retest`. A reproduced failure stays open until the original case passes after the fix. Include important untested combinations in the final limitations rather than implying exhaustive coverage.

## Fix and recheck

Prioritize data loss, duplicate actions, broken/inaccessible interactions, then layout/contrast, then polish. Fix root causes and retest the original failure, normal behavior, and affected consumers. Add focused regression coverage for significant failures where the project's tools support it. Check production when compilation/rendering differs. Do not hide an exception, disable an assertion, or remove a feature to make the check pass.

If rendering is unavailable, finish source/build checks and mark visual states `Not verified`. Physical touch and slow-motion checks must not be claimed without performing them. Never silently change an unverified row to pass.

Reviews use `Severity | Location | Before | After | Why`, grouped by root cause. Implementations fix findings and report outcome/checks. Save useful proof screenshots in the task deliverable location when possible.
