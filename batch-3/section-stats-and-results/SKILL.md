---
name: stats-results
description: A section of measured figures with labels and a note saying where they come from. Use for a number row, results cards, one central statistic, or before and after numbers. Not for a chart, and not for figures that count up, which needs a script.
---

# Stats & Results

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

Put them inside `@layer sre-base { }`. This matters: a layered declaration loses to an unlayered one whatever its specificity or its order, so Tailwind on a real page still wins every one of them, and the layer only shows through where there is no Tailwind. Without the layer, a rule such as `.sre h2` would outrank `.text-4xl` and the block would start overriding the page.

Start with the five reset lines. Tailwind normally supplies these, and without them the list markers show through, the type falls back to a serif, and the content runs wider than the screen. That was measured: a first attempt without them put a block 36px past the right edge of a 1280px viewport.

    @layer sre-base {
      .sre,.sre *,.sre *::before,.sre *::after{box-sizing:border-box}
      .sre{font-family:ui-sans-serif,system-ui,-apple-system,"Segoe UI",Roboto,sans-serif;line-height:1.5}
      .sre h1,.sre h2,.sre h3,.sre h4,.sre p,.sre figure,.sre blockquote{margin:0}
      .sre ul,.sre ol{margin:0;padding:0;list-style:none}
      .sre img,.sre svg,.sre video{display:block;max-width:100%}

Then the block's own shape: the section's background, text color and padding, the wrapper's maximum width, every row or grid the layout depends on, and the heading and body type sizes.

      .sre{background-color:rgb(var(--sre-surface));color:rgb(var(--sre-ink));padding:3rem 1.25rem}
      .sre .sre-wrap{max-width:64rem;margin-inline:auto}
      .sre .sre-row{display:grid;gap:1.5rem}
      .sre h2{font-size:1.875rem;line-height:1.2;font-weight:600}
      .sre h3{font-size:1.125rem;font-weight:600}
      .sre p{font-size:1rem;line-height:1.625}
    }

About twelve declarations in all. Do not try to restate every utility you used. This is the block's skeleton, not a second copy of Tailwind.

**Nothing else is assumed.** The block does not expect the page to define a color, a font, a helper class or a shared stylesheet.

This skill is self-contained. Do not read, reference or depend on any other skill, and do not assume that a shared stylesheet, a motion system, a component library or a set of helper classes exists somewhere. If something is not described here, it is not available.

## What it is for

Settings - Set metrics, labels, units, data source, starting value, precision, and proof note.

A few numbers that are true and checkable, each with a label and a source.

## When to use it

When there are real figures: jobs completed, years trading, average response time, a measured saving.

## When not to use it

When the figures are invented or rounded until they mean nothing. A made up statistic is the fastest way to lose a reader who checks.

## Where it belongs on the page

After the claim they support, and before the call to action.

## What may be changed

The figures, the labels, how many are shown, and whether they sit in a row or as cards.

## What must not be changed

Every figure carries a unit and a label, and the section carries a note saying where the numbers come from and when they were counted. Remove an invented figure rather than softening it. Four figures is the most a reader takes in at once.

## Settings and Controls



## Creative Design Options

- **Animation.** Count-up values, progress rings, growing bars, gradient numbers, or staggered results.

## Motion

The figures fade and rise together on entrance. They do not count up, which needs a script, and a number that animates on every scroll back is a distraction rather than a flourish.

## Responsive and Accessibility

Two columns below 640px, four above. The figure never falls below 28px, since it is the thing the reader is meant to take away.

## Defaults

Four figures in a row, label under each, source note beneath the row, fade and rise.

## What breaks this block

Figures with no source. A number with no unit, so 94 could be anything. More than four, which stops being a headline and becomes a table.

## What a static block cannot do

Counting up, progress rings that fill to a value read from elsewhere, and growing bars driven by data all need a script. A fade in and a real number do the job.

## Images

Images come from the customer's brief: their own photographs, their own logo, the artwork they supplied. If the brief carries none, use the placeholder that ships inside this block, which is inline SVG and needs no request.

Never reach for a stock photo address. Unsplash, Pexels, Pixabay, a CDN, a hotlinked image on someone else's site: all of them are external files, all of them are ruled out here, and a broken address leaves a hole in the layout with no warning.

Give every image an alt description that says what is in it. Decorative artwork takes `alt=""` so a screen reader passes over it rather than reading a filename.

## Color

The block carries its own color. Six variables are declared on the block itself, and every colored part reads one of them.

| Variable | Used for |
| --- | --- |
| `--sre-surface` | card and panel faces |
| `--sre-band` | the quiet band behind the content |
| `--sre-ink` | headings and primary copy |
| `--sre-soft` | supporting copy |
| `--sre-accent` | the one emphasis color |
| `--sre-deep` | the dark ground |

The values are channel triplets, `15 23 42` rather than `#0f172a`, so an opacity suffix still works: `rgb(var(--sre-ink) / .70)`.

Rebrand the block by changing those six values, in one place. Do not spread hex values through the markup, and do not reach for a color name out of the page's Tailwind config. Every page the builder makes writes its own config with its own color names, so a name taken from one page resolves to nothing on the next. Measured on two live sites: `bg-surface` painted no background on either, and `muted` was a pale background on one and a dark text color on the other.

Do not invent CSS variable names such as `var(--color-surface)`. Nothing defines them, the browser throws the declaration away, and that color or border renders as nothing at all. Do not use `text-current/60` either, because an opacity modifier has no effect on `currentColor`.

Every piece of text has to reach 4.5 to 1 against the color actually behind it, which is the nearest ancestor that paints a background rather than the page. Large text, meaning 24px, or 18.66px when bold, needs 3 to 1. With the default values, ink on deep measures 1.05 to 1, accent on deep 3.62 to 1 and soft on deep 3.45 to 1, so none of those three belong on a dark panel. On a dark panel use the surface value for text. Soft measures 5.06 to 1 on the band and 5.42 on surface, and white on the accent measures 5.17 to 1.

A tint such as `rgb(var(--sre-accent) / .10)` is mostly the color behind it, so judge contrast against the composited result.
