---
name: progress-bar
description: A visual indicator for completion, a goal, or a step in a sequence. Use for a horizontal bar, a segmented bar, a goal tracker, or a step tracker, with a value that is known when the page is built. Not for scroll progress or form completion, neither of which can be tracked without a script.
---

# Progress Bar

## What you must return

One `<section>` element, and nothing else.

- No `<!doctype>`, `<html>`, `<head>` or `<body>`. The block is dropped into a page that already exists.
- No markdown code fence around it, and no commentary before or after it. Return the HTML itself. A leftover fence marker renders as visible text on the page, and inside a grid it becomes an extra item that pushes the layout out of shape.
- No `<script>`, no event handlers, no `hidden` attributes waiting to be toggled. Nothing here can run.
- No external files of any kind: no image addresses, no icon library, no web font.
- Everything the block needs lives inside that one element, including its own `<style>`. That stylesheet is expected, not a last resort. Tailwind utilities carry most of the work, but a relationship between elements cannot be written as a utility, and the ones this set depends on all live in the stylesheet: `:checked ~`, `details[open]`, `:target`, `@keyframes` and the `prefers-reduced-motion` rule. Prefix your own class names so they cannot collide with the rest of the page.
- The block carries its own padding and its own maximum width. Drop that wrapper and the content runs to both edges of the screen.
- Color an SVG with a presentation attribute as well as a class. `<rect fill="#f6f7f9" class="fill-muted">` takes the theme color where the stylesheet is loaded and falls back to the attribute where it is not. With only the class, an unresolved fill computes to solid black and the placeholder becomes a black rectangle sitting over the layout.
- Write `fill="none"` on any path that is stroke only. A path with no fill is filled solid black. That is the SVG default, not a fault in the browser.

**Write the block's own base layer.** Tailwind utilities carry the detail, but a plain HTML viewer has no Tailwind, and a block built only from utilities arrives there as unstyled text on a white page. So write the few declarations that carry the block's shape into its own stylesheet as well.

Put them inside `@layer epr-base { }`. This matters: a layered declaration loses to an unlayered one whatever its specificity or its order, so Tailwind on a real page still wins every one of them, and the layer only shows through where there is no Tailwind. Without the layer, a rule such as `.epr h2` would outrank `.text-4xl` and the block would start overriding the page.

Start with the five reset lines. Tailwind normally supplies these, and without them the list markers show through, the type falls back to a serif, and the content runs wider than the screen. That was measured: a first attempt without them put a block 36px past the right edge of a 1280px viewport.

    @layer epr-base {
      .epr,.epr *,.epr *::before,.epr *::after{box-sizing:border-box}
      .epr{font-family:ui-sans-serif,system-ui,-apple-system,"Segoe UI",Roboto,sans-serif;line-height:1.5}
      .epr h1,.epr h2,.epr h3,.epr h4,.epr p,.epr figure,.epr blockquote{margin:0}
      .epr ul,.epr ol{margin:0;padding:0;list-style:none}
      .epr img,.epr svg,.epr video{display:block;max-width:100%}

Then the block's own shape: the section's background, text color and padding, the wrapper's maximum width, every row or grid the layout depends on, and the heading and body type sizes.

      .epr{background-color:rgb(var(--epr-surface));color:rgb(var(--epr-ink));padding:3rem 1.25rem}
      .epr .epr-wrap{max-width:64rem;margin-inline:auto}
      .epr .epr-row{display:grid;gap:1.5rem}
      .epr h2{font-size:1.875rem;line-height:1.2;font-weight:600}
      .epr h3{font-size:1.125rem;font-weight:600}
      .epr p{font-size:1rem;line-height:1.625}
    }

About twelve declarations in all. Do not try to restate every utility you used. This is the block's skeleton, not a second copy of Tailwind.

**Nothing else is assumed.** The block does not expect the page to define a color, a font, a helper class or a shared stylesheet.

This skill is self-contained. Do not read, reference or depend on any other skill, and do not assume that a shared stylesheet, a motion system, a component library or a set of helper classes exists somewhere. If something is not described here, it is not available.

## What it is for

A visual indicator for completion, goals, loading, steps, or scroll progress.

A number the reader can see at a glance: how far along something is, or how close a goal is.

## When to use it

For a funding total, a capacity figure, a stage in a process the reader is being shown, or any figure that is fixed when the page is written.

## When not to use it

For anything that changes while the reader is on the page. A bar that should move and does not is worse than no bar.

## Where it belongs on the page

Beside the figure it illustrates, never on its own, because a bar with no number is a shape.

## What may be changed

The value, the maximum, the label, the unit, the orientation, and whether the bar is solid or segmented.

## What must not be changed

The number is written out as text beside the bar. A bar alone carries nothing for a screen reader, and a `role="progressbar"` with the right `aria` values is what makes the figure available without sight. Do not animate the fill to a value that is not the real one.

## Settings and Controls

- Set value, maximum, label, unit, start value, data source, orientation, and animation trigger.
- Choose manual value, calculated value, scroll progress, form completion, or external data.
- Control width, height, alignment, markers, and accessible progress text.

## Creative Design Options

- **Layout.** Horizontal bar, circular ring, segmented bar, goal tracker, step tracker, or vertical meter.
- **Motion.** Count up, fill, gradient sweep, milestone pop, glow, or spring completion.
- **Styling.** Solid, gradient, striped, illuminated, soft shadow, or brand-color segments.

## Motion

The fill grows from zero to its value once on entrance, over about 900ms. It does not repeat. Under `prefers-reduced-motion` the bar is drawn at its value immediately.

## Responsive and Accessibility

Full width at every size. The label sits above the bar below 640px rather than beside it, so neither is squeezed. The text never falls below 14px.

## Defaults

Horizontal bar, value written beside the label, fill on entrance, accent colour.

## What breaks this block

A bar with no number, which says nothing to anybody who cannot see it. Missing `aria-valuenow`, which leaves a screen reader with a bare div. A fill that animates every time the element scrolls back into view, which is a distraction.

## What a static block cannot do

Counting up the number, tracking scroll position, tracking form completion and reading an external figure all need a script. Write the value into the markup and let CSS draw it.

## Images

Images come from the customer's brief: their own photographs, their own logo, the artwork they supplied. If the brief carries none, use the placeholder that ships inside this block, which is inline SVG and needs no request.

Never reach for a stock photo address. Unsplash, Pexels, Pixabay, a CDN, a hotlinked image on someone else's site: all of them are external files, all of them are ruled out here, and a broken address leaves a hole in the layout with no warning.

Give every image an alt description that says what is in it. Decorative artwork takes `alt=""` so a screen reader passes over it rather than reading a filename.

## Color

The block carries its own color. Six variables are declared on the block itself, and every colored part reads one of them.

| Variable | Used for |
| --- | --- |
| `--epr-surface` | card and panel faces |
| `--epr-band` | the quiet band behind the content |
| `--epr-ink` | headings and primary copy |
| `--epr-soft` | supporting copy |
| `--epr-accent` | the one emphasis color |
| `--epr-deep` | the dark ground |

The values are channel triplets, `15 23 42` rather than `#0f172a`, so an opacity suffix still works: `rgb(var(--epr-ink) / .70)`.

Rebrand the block by changing those six values, in one place. Do not spread hex values through the markup, and do not reach for a color name out of the page's Tailwind config. Every page the builder makes writes its own config with its own color names, so a name taken from one page resolves to nothing on the next. Measured on two live sites: `bg-surface` painted no background on either, and `muted` was a pale background on one and a dark text color on the other.

Do not invent CSS variable names such as `var(--color-surface)`. Nothing defines them, the browser throws the declaration away, and that color or border renders as nothing at all. Do not use `text-current/60` either, because an opacity modifier has no effect on `currentColor`.

Every piece of text has to reach 4.5 to 1 against the color actually behind it, which is the nearest ancestor that paints a background rather than the page. Large text, meaning 24px, or 18.66px when bold, needs 3 to 1. With the default values, ink on deep measures 1.05 to 1, accent on deep 3.62 to 1 and soft on deep 3.45 to 1, so none of those three belong on a dark panel. On a dark panel use the surface value for text. Soft measures 5.06 to 1 on the band and 5.42 on surface, and white on the accent measures 5.17 to 1.

A tint such as `rgb(var(--epr-accent) / .10)` is mostly the color behind it, so judge contrast against the composited result.
