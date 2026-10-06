# Application composition and library coverage

## Redesign without losing the application

Inventory routes/shared components first. Preserve data, authorization, integrations, and information hierarchy. Derive layout from the product: storefronts, editors, admin tools, and content sites do not share one universal dashboard template.

Work through tokens → primitives → composites → blocks → routes. After shared changes, revisit consumers for spacing, overflow, hidden states, and behavior. Remove unused styling only after migration. Use Tailwind in the affected scope; focused work does not authorize unrelated framework migration.

If Tailwind is absent, add the version-appropriate integration for the existing framework within authorized frontend work. Preserve framework/package manager. Read installed guides/current official docs when APIs are uncertain.

## Compositions

**Application shell:** compact navigation, clear title/location, restrained toolbar, predictable inset. Theme/profile/search use shared utilities. Mobile navigation and toolbars reflow without hiding essential actions.

**Table workspace:** title/count, search, filter pills, primary creation action, table, pagination/summary. Controls align with cell insets. Filter/sort/selection/bulk actions agree about record scope.

**Settings:** coherent field groups, persistent labels, supporting copy, validation, dirty/saving/saved states, truthful persistence, predictable action placement. Avoid a separate card per field.

**Dashboard:** meaningful metrics and comparison periods, charts with units/summaries, useful detailed list. Space/dividers separate sections. Avoid shadows on every nested mark/container.

**Kanban:** neutral columns, black/white themed task cards, concise metadata, local status color, stable handles, explicit move alternative, edit/add/remove, loading/empty. Preserve tasks while filtering.

**Workflow:** neutral canvas/nodes, shared toolbar/fields, readable ports, selected node/edge feedback, inspector, viewport controls, connection validation, keyboard alternatives. Coordinates/zoom are runtime values; other styling is Tailwind. Execution cues represent actual state.

Use the target's real content/actions. Label demonstration data and local-only behavior in examples.

## Complete library coverage

When every component from a catalog is requested, fetch its current official list, record date/source, and compare against actual exported entries. Mark implemented, user-excluded, or missing. Counts alone are not proof.

Each required entry needs:

- Working implementation with shared theme/Tailwind.
- Companion exports, hooks, dependencies, and accessible behavior.
- Representative interactive example with complete imports; useful edge/variant examples.
- Accurate usage/prop documentation matching its actual API.
- Theme/responsive checks and defining interaction coverage.
- Installable source and dependency/license/provenance data when distributing.

Use established accessible primitives. Shadcn-style distribution is editable source, not a design restriction. Base UI suits new unstyled behavior; existing Radix/shadcn can be restyled freely. Avoid incompatible primitives or falsely claiming identical wrapper APIs.

## Add components consistently

For a library, prefer one catalog manifest for name/category/source/demo/dependencies/docs/provenance. Drive gallery, sidebar, and registry from it where useful. Adding an entry should not require unrelated routing edits.

Validate names, files, local dependencies, cycles, and requested coverage. Fail a registry build when a helper is missing. Use public primitives in previews so fixes reach consumers and the gallery together. Blocks are functioning compositions, not screenshots.

For shadcn-compatible distribution, test the actual CLI in a disposable consumer project. Check alias rewriting, tokens, hooks, dependency versions, portal behavior, and production extraction. Schema validity alone does not prove installation works.

## Original work and project ownership

Adapt the target application's own components. Preserve required license notices on any existing third-party code. This skill does not require cloning a reference application or importing another creator's implementation. Keep project ownership, framework, APIs, and product identity intact.
