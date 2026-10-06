# Motion performance and rendering

Read alongside [interaction](interaction.md) when changing animations, gestures, transient feedback, or dense animated compositions. Static styling remains Tailwind; this guide does not authorize adding component CSS files or replacing the application's animation framework.

## Match the property being changed

Inspect generated CSS for the installed Tailwind major version. Tailwind 4 utilities can set the individual `scale`, `translate`, and `rotate` properties. A transition limited to `transform` will not interpolate a separate `scale` declaration. Use `transition-transform` when all transform-related properties are intended, or a precise list such as `transition-[opacity,scale]`. Tailwind 3 utilities may instead compose a `transform` value; choose the list for the actual output.

Reduced motion must suppress the same property that creates movement. `transform-none` does not reset a separately declared `scale` or `translate`. Prefer putting movement utilities behind `motion-safe:` so no moving state is introduced under reduced motion. Ensure a motion library respects the same preference.

## Choose the smallest mechanism

| Need | Approach |
| --- | --- |
| Hover, press, ordinary open/close | Tailwind transitions that can reverse toward the latest state |
| Presence and stateful gestures | Existing accessible primitive and installed motion system |
| Infrequent predetermined sequence | Existing animation utilities or motion system, with cancellation and reduced-motion behavior |
| Programmatic timeline without a library | Web Animations API only when it reduces complexity and is supported by the target browsers |
| No useful spatial or state information | Immediate update |

A CSS keyframe or timeline can be paused or reversed, but changing classes or remounting it often restarts the sequence. Test its actual interruption behavior. Do not build a second scheduler for an ordinary toggle. A spring is useful for an existing continuous gesture when preserving velocity helps; precise work controls should not bounce merely because a spring is available.

## Control rendering cost

Prefer transform and opacity for movement and fading. Layout-affecting properties need more work; keep any necessary height expansion bounded and inspect it with realistic content. Neither CSS nor a motion-library API guarantees that an animation runs off the main thread. Browser, property, rendering context, and library version all matter.

Avoid broad blur and backdrop effects on large moving regions. Small icon crossfades can use the calibrated blur recipe; preserve crisp text and use a static alternative under reduced motion. Do not assume `filter` or `clip-path` is inexpensive on every device. Profile the actual target when performance is uncertain.

Set rapidly updated coordinates on the smallest relevant element. Updating an inherited custom property on a large ancestor can trigger work throughout its descendants. Prefer leaf-scoped variables or the existing gesture library's supported runtime transform API; visual constants and state styling stay in Tailwind. Do not update React state for every pointer frame when the installed interaction layer already handles the motion value.

Batch geometry reads and writes instead of repeatedly interleaving measurement and mutation. Avoid a synchronous layout measurement on every pointer movement. Clean up observers, animation frames, timers, and listeners when the component unmounts or the gesture ends.

Add a narrow `will-change` hint only after observing a relevant rendering problem and remove temporary hints when the interaction finishes. Do not promote every row or card: additional layers consume memory and can alter stacking or containing blocks. Test transformed ancestors with fixed and portalled descendants.

## Decorative clipping and shared indicators

An active-tab highlight, reveal, or progress mask can use a visual overlay when it improves continuity. Keep one semantic, interactive control per action. Any duplicated visual label must be `aria-hidden`, nonfocusable, noninteractive, and free of duplicate IDs. Keyboard and screen-reader behavior stays with the real controls.

Never make completion of a decorative fill authorize a destructive action. Preserve the product's actual confirmation model. If a hold interaction already exists, interruption and an accessible alternative must be covered by its behavior contract. Clip-path styling must not clip the focus indicator or make content permanently unavailable without motion.

## Inspect under realistic conditions

Check initial render, warm repeat use, rapid reversal, long content, and reduced motion. Reproduce the component's realistic workload, such as a route update or dense list, while animating. Look for delayed input, jumps, disappearing labels, and excessive rendering work; measure before claiming a performance improvement.

When available, inspect at normal speed and around 10% playback speed or frame by frame. Check origin, synchronized properties, overlapping states, and the first and final frames. Restore normal playback before finishing. Use browser performance tools where a visible problem warrants them, and include the actual browsers/devices tested in the result. Emulation does not establish physical touch behavior.
