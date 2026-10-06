# React integration

## Adapt before replacing

Read the package manifest, lockfile, framework configuration, shared component directory, class helper, variant system, and theme provider. Identify where behavior lives and where appearance lives. Change the smallest shared layer that reaches the requested scope. Use the existing package manager.

| Existing foundation | Integration approach |
| --- | --- |
| Editable shadcn source | Refine local tokens and component variants. Preserve exports, slots, props, and headless behavior. Do not overwrite customized files by reinstalling a registry. |
| Radix or Base UI primitives | Style the installed primitive's actual parts and state attributes. Preserve focus, portals, positioning, keyboard behavior, and accessibility. |
| Custom React components | Keep the public API and behavior; consolidate repetitive classes into existing shared variants. Improve semantics where deficient. |
| Opinionated styled library | Use supported unstyled/slot/class APIs with Tailwind. Inspect style precedence. Do not silently remove behavior or introduce brittle selector overrides. Explain any genuine styling limit. |
| New React interface | Choose a compatible accessible behavior layer and use Tailwind for appearance. A new wrapper should earn its abstraction through repeated use. |

Shadcn is editable source, not a fixed visual theme. A framework migration is unnecessary just to change shadows, radii, colors, or density. Base UI and Radix are not API-compatible replacements; retain whichever the application uses unless the task requires migration.

## Preserve the component contract

- Keep controlled and uncontrolled modes, default values, callbacks, form names, disabled states, and ref access working. Use the ref convention supported by the project's React version.
- Forward consumer props and class overrides deliberately. Preserve composition patterns such as `asChild` or `render` only where the installed primitive supports them; avoid nested buttons and links.
- Use actual primitive state attributes for checked, open, selected, highlighted, disabled, and invalid styling. Do not invent attribute names or reimplement a primitive's state machine for decoration.
- Preserve stable keys and IDs. Do not key an input by its value or remount a component during typing, theme changes, or animation.
- Keep business logic, queries, authorization, mutations, validation, and persistence attached. A visual refinement must not turn a real action into a demo callback.
- Match existing icon and class utilities. Avoid adding dependencies that duplicate installed capabilities.

## Framework boundaries

In server-rendered React, keep static content on the server where supported. Put client directives only at interactive boundaries. Do not move an entire layout to the client merely to change styles. Keep browser-only APIs out of server rendering, and avoid random/time-dependent markup that causes hydration differences.

Use the framework's navigation and image conventions. Keep route loaders, actions, caching, and error boundaries intact. Use local state only for genuine local interaction, not as a replacement for application data.

Theme selection should follow the existing system: persisted preference, optional system preference, and a root class or attribute. Resolve the initial theme with the established provider or framework mechanism to avoid a flash and hydration mismatch. Avoid mounting a separate theme provider inside each component.

Portals need the same tokens as the app. A theme on a nested demo wrapper will not reach a portal attached to `body`; use the primitive's supported container or apply the theme at the appropriate root. Set native `color-scheme` consistently. Charts, date inputs, tooltips, and third-party editors need an explicit theme check.

## Integrate Tailwind without breaking the host

Follow [Tailwind implementation](tailwind.md). Keep the installed major version unless an upgrade is part of the task. In mixed styling codebases, migrate the requested components and necessary shared tokens; preserve unrelated styling until its consumers move. Removing a stylesheet before replacing its layout, animation, or focus rules is a regression.

For virtualized tables, canvas editors, charts, drag positions, or user-selected colors, runtime values can travel through CSS custom properties or the library's required runtime API. Keep visual constants and interaction styling in Tailwind. Avoid inaccessible DOM substitutes for a library-rendered canvas; provide equivalent labels, controls, and summaries through its supported interfaces.

## Beyond a particular screen type

Apply the same roles to any React web product: surface, control, item, track, overlay, and feedback. A storefront product card, document annotation, timeline, tree, media player, or scientific control needs the same geometry, contrast, state, and verification decisions. Product information architecture determines the layout.

For React Native, first verify a Tailwind-compatible adapter already exists or is in scope. Translate tokens into its supported utilities and use native semantics, gestures, focus, and accessibility APIs. CSS shadows, DOM tables, portals, and ARIA recipes require platform equivalents. Never claim web verification establishes native behavior.
