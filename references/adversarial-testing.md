# Try to break the components

Test how a component fails and recovers, not only how it looks with ideal data. Apply the relevant cases to every affected component and important composition. Prioritize stateful controls, shared primitives, data-changing actions, and defects suggested by the actual implementation. Static components need content and layout stress even when interaction checks are N/A.

## Establish the contract

Before testing, identify the observable rules that must hold: valid values, maximum/minimum bounds, latest-result ownership, selection scope, permitted state transitions, retained user input, and focus destination. Preserve the product's existing rules. An intentionally unsupported value should produce a clear, stable response rather than silently changing the API to accept it.

Use the running local application, existing component previews, and the project's tests. Inject slow/failed responses and malformed fixtures through a test boundary. Keep destructive actions, uploads, emails, payments, and other external effects mocked or in an authorized test environment. Do not stress production traffic or modify real user records to prove a visual component works.

## Breaking cases and expected handling

| Challenge | Try | Expected handling |
| --- | --- | --- |
| Content extremes | Empty/whitespace values, a single item, long unbroken text, multi-line labels, missing images, large counts, translated text | No page-wide overflow or overlapped actions; intentional wrapping/truncation, accessible full values, stable fallbacks |
| Field boundaries | Zero, negative and maximum values; intermediate numeric input; paste; Unicode/emoji; invalid dates; composition input where supported | Preserve editable intermediate text; validate at the correct time; associate errors; never submit while an IME composition is still being committed |
| Repeated actions | Double-click submit, repeated Enter, quick toggle/revert, retry while a request is pending | One intended operation; accurate pending/disabled state; predictable final value; no duplicate toast or submission |
| Async ordering | Start search A then B; finish A last; change a filter during loading; navigate away before completion | Latest request owns the visible result; stale responses cannot overwrite current state; obsolete work is ignored or cancelled |
| Failure and recovery | Slow response, rejected request, offline transition, missing asset, failed upload or save | Bounded, truthful loading; useful error and retry; draft/selection retained where appropriate; no false success or permanent disabled state |
| Optimistic changes | Reject a move, edit, or delete; reject one of several pending changes | Reconcile only the affected operation; preserve later valid changes; show the error; allow retry without duplication |
| Data refresh | Remove the selected item, shrink a paginated dataset, replace options while a popup is open | Valid active item and page; explicit empty state; reconciled selection; no orphaned focus or crash |
| Overlay sequences | Open nested popups; press Escape repeatedly; close while loading; remove the trigger; reopen quickly | Correct layer closes; focus returns to an existing sensible target; no leaked scroll lock, invisible blocker, or stale backdrop |
| Input modalities | Keyboard-only use; pointer cancellation; touch-sized targets; no-hover operation; disabled item activation | Essential controls stay available; disabled actions cannot fire; predictable keyboard model; no stuck pressed/drag state |
| Layout pressure | Around 320px and relevant breakpoints; short viewport; 200% zoom; large text; dense content | Actions remain reachable; local table/canvas scrolling; no clipped focus, hidden validation, or unreachable dialog footer |
| Theme and motion | Switch theme with a popup open; test both themes during loading/error/selection; enable reduced motion | Portals inherit tokens; content and focus remain legible; no state reset; meaning survives with motion removed |
| Component lifecycle | Mount/unmount repeatedly; controlled value changes from a parent; development Strict Mode when already enabled | No duplicate listeners, timers, effects, or operations; parent state remains authoritative; no hydration errors |
| Timed feedback | Hide and restore the tab during a toast; focus its action; move across a stack; update and dismiss concurrently | Reading time and focus are respected; one current message per operation; no flicker, swallowed action, or duplicate announcement |
| Animation interruption | Reverse halfway; switch theme while exiting; enable reduced motion; add a realistic rendering workload | Latest state wins; no queued interaction, first-frame jump, unintended movement, clipped focus, or invisible blocker |

Choose realistic boundaries from the component's actual contract. Do not invent arbitrary input limits, infinite requests, or impossible prop combinations. When randomized or property-based checks are useful and supported by existing tools, bound their inputs and preserve the seed and failing sequence.

## Compound scenarios

Failures often live between otherwise working components. Exercise sequences that combine the changed parts:

- **Table:** select rows, sort, move to a later page, then filter to zero results. Clear the filter and remove an item. Page index and select-all scope remain correct; unrelated rows must never receive a bulk action.
- **Search/combobox:** type quickly, return responses out of order, then clear the query while loading. Navigate with arrows and Enter. No stale option is selected; highlighted options stay visible and match the active query.
- **Form/dialog:** enter a draft, cause a save failure, close according to the product's dismissal rules, and reopen. Retry successfully. Draft behavior is intentional, errors are linked to fields, and submission/focus recovery works.
- **Board/editor:** move an item while filtered, cancel a drag, then reject persistence. Items are not lost or duplicated; rollback uses stable IDs; a keyboard or explicit move alternative remains usable.
- **Date/range/slider:** test equal endpoints, min/max, disabled values, clear/reset, and an external controlled update. Values stay valid without jumping, NaN, inverted ranges, or inaccessible handles.
- **Navigation:** collapse the sidebar, open a submenu, change route, and resize to mobile. Current location stays correct; focus, mobile dismissal, and scroll position follow the application's intended behavior.
- **Animated controls:** refresh the page with a selected tab or active toggle, then change it by keyboard and pointer. The initial state is settled, repeated actions remain immediate, and reduced motion suppresses individual scale/translate properties as well as transform animations.

For an unfamiliar component, combine its most consequential state transition with an input boundary, an interruption, and a constrained composition. Test the relevant combinations rather than mechanically multiplying every possible state.

## Handle failures in the implementation

Fix at the layer that owns the broken invariant. A shared button may prevent repeated activation while pending; backend idempotency belongs to the mutation contract and cannot be guaranteed by a disabled button. Preserve existing server protections. If an external prerequisite prevents verification, report it without claiming client styling solved it.

Use stable IDs, functional state updates, explicit pending/error states, and the existing request/cache layer as appropriate. Clean up timers, listeners, and obsolete async work. Reconcile dependent state when data changes. Keep recovery actions reachable and retain meaningful user work.

Normalize or validate uncertain data at its boundary according to the actual contract. Do not scatter optional chaining, blanket `try/catch`, swallowed exceptions, or arbitrary defaults throughout the UI to conceal broken assumptions. Use fallbacks and error boundaries at useful recovery points, with understandable feedback.

For layout failures, fix sizing, wrapping, scroll containment, and stacking in Tailwind. Avoid hiding page overflow, clipping focus rings, removing labels, or shrinking all text to mask the defect. Verify both themes after changes to recovery/error surfaces.

## Reproduce, fix, and retest

1. Record the smallest failing sequence, fixture, viewport, theme, and input method; collect the relevant visible error or console exception.
2. Turn a significant behavior failure into a focused regression test with the project's existing tools when practical. Assert the user-visible invariant, not the exact Tailwind class or internal implementation.
3. Fix the cause while preserving the component's public contract and legitimate interactions.
4. Re-run the original breaking sequence and verify recovery, then the normal path and affected shared consumers. A test should fail on the original defect and pass with the fix where that can be demonstrated.
5. Report attempted cases, actual results, fixes, and remaining unverified conditions. Stop once the relevant checks pass and no demonstrated concern remains; do not run unbounded fuzzing or claim every possible future state is proven safe.

If browser/device tooling is unavailable, complete source checks and executable tests that are possible. Mark rendered, physical touch, IME, or other unobserved behavior as `Not verified`; emulation and unit events do not establish physical-device behavior.
