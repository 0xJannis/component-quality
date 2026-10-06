# Component Quality

A design engineering skill for refining React interfaces with Tailwind. It teaches your coding agent to improve the components already in your project: tactile buttons, precise tables, neutral navigation, coherent shadows, complete interaction states, and carefully finished light and dark themes.

This repository contains the skill and its reference guides. There is no component library, application, or runtime dependency to install into your product.

## Install

```bash
npx skills add 0xJannis/component-quality --skill component-quality
```

For a global installation:

```bash
npx skills add 0xJannis/component-quality --skill component-quality --global
```

To select Codex explicitly, add `--agent codex`. Other supported agents can be selected through the installer's prompts. Preview the available skill with:

```bash
npx skills add 0xJannis/component-quality --list
```

Installation uses the [skills CLI](https://skills.sh/docs). Agent support and installation locations are managed by that tool.

## Use

In Codex, invoke the skill directly:

```text
Use $component-quality to redesign this application. Keep its functionality,
refine the existing components with Tailwind, and verify light and dark mode.
```

Or give it a focused task:

```text
Use $component-quality to polish our shadcn buttons, sidebar, and data table.
Use neutral gray hover states and a green primary action. Preserve their APIs
and actively try to break them with rapid actions, failed requests, boundary
inputs, and narrow layouts in both themes. Fix the failures and retest.
```

For other agents, ask to use the `component-quality` skill using that agent's supported invocation method. Automatic selection depends on your agent and its configuration; explicit invocation gives the clearest scope.

## What it covers

- **Component detail:** layered contours and inset highlights, optical icon alignment, concentric radii, stable control geometry, legible labels, and purposeful elevation.
- **A coherent system:** black and charcoal dark surfaces, white light surfaces, neutral hover and selection, semantic status colors, and configurable primary action colors.
- **Every component family:** a reusable method for controls, tables, sidebars, search, overlays, forms, charts, editors, boards, and custom components. Examples guide decisions without limiting the catalog.
- **Existing React projects:** Next.js, Vite, React Router, and other React web frameworks; editable shadcn source, headless primitives, and custom components. Preserve the installed framework and behavior layer.
- **Tailwind implementation:** utilities and shared variants for component styles, semantic tokens for themes, version-aware integration, and no component CSS files.
- **Verification:** actual interaction and rendered checks for the requested scope, with explicit reporting of anything that could not be tested.
- **Breaking cases:** deliberately challenge components with rapid actions, stale responses, failed saves, extreme content, nested overlays, and constrained layouts. Fix the cause, check recovery, and add focused regression coverage for significant failures.

The skill adapts the existing application rather than forcing every product into a dashboard. It does not require switching component frameworks. React Native needs a platform-appropriate Tailwind adapter and native behavior; web recipes are not automatically portable to it.

## Quality standard

The agent works from tokens to shared primitives, composites, and screens. Every affected component is checked for its surface, geometry, typography, states, keyboard and touch behavior, responsive layout, and both themes. Shared changes are checked in their actual compositions.

After normal behavior works, the agent actively tries to break the affected components using local fixtures or the project's test environment. It checks that data is retained, stale responses cannot overwrite current results, repeated actions do not duplicate operations, and focus and controls recover correctly. A discovered failure must be fixed and its original sequence retested; a checklist alone is not completion.

The skill guides implementation and verification; it cannot guarantee an identical result from every model or prove every future configuration works. It requires truthful coverage and preserves existing functionality. You remain in control of the task's scope, brand choices, and publication permissions.

## Repository

```text
SKILL.md                          Entry point and quality contract
agents/openai.yaml                Codex display and invocation metadata
references/design-system.md       Theme, geometry, typography, depth
references/component-recipes.md   Buttons, tables, navigation, and more
references/tailwind.md            Tokens and utility implementation
references/react-integration.md   Framework and component integration
references/interaction.md         States, semantics, and motion
references/application-patterns.md Application and library composition
references/verification.md        Coverage and functional checks
references/adversarial-testing.md Breaking cases, recovery, and regression checks
.github/workflows/validate.yml    Repository integrity checks
```

No scripts folder or bundled executable is needed to use this skill. Repository CI checks package structure, metadata, reference links, and the absence of application source. Component testing happens in the target project's own tools; repository CI does not render or evaluate that application.

## License

[MIT](LICENSE).
