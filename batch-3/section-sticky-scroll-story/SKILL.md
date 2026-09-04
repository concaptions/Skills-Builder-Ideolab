---
name: sticky-scroll-story
description: A section where one visual stays put while the chapters beside it scroll past. Use for a sticky image with changing copy, or a sticky diagram with feature callouts. Not for a story where the visual must change per chapter, which needs a script.
---

# Sticky Scroll Story

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

Put them inside `@layer sst-base { }`. This matters: a layered declaration loses to an unlayered one whatever its specificity or its order, so Tailwind on a real page still wins every one of them, and the layer only shows through where there is no Tailwind. Without the layer, a rule such as `.sst h2` would outrank `.text-4xl` and the block would start overriding the page.

Start with the five reset lines. Tailwind normally supplies these, and without them the list markers show through, the type falls back to a serif, and the content runs wider than the screen. That was measured: a first attempt without them put a block 36px past the right edge of a 1280px viewport.

    @layer sst-base {
      .sst,.sst *,.sst *::before,.sst *::after{box-sizing:border-box}
      .sst{font-family:ui-sans-serif,system-ui,-apple-system,"Segoe UI",Roboto,sans-serif;line-height:1.5}
      .sst h1,.sst h2,.sst h3,.sst h4,.sst p,.sst figure,.sst blockquote{margin:0}
      .sst ul,.sst ol{margin:0;padding:0;list-style:none}
      .sst img,.sst svg,.sst video{display:block;max-width:100%}

Then the block's own shape: the section's background, text color and padding, the wrapper's maximum width, every row or grid the layout depends on, and the heading and body type sizes.

      .sst{background-color:rgb(var(--sst-surface));color:rgb(var(--sst-ink));padding:3rem 1.25rem}
      .sst .sst-wrap{max-width:64rem;margin-inline:auto}
      .sst .sst-row{display:grid;gap:1.5rem}
      .sst h2{font-size:1.875rem;line-height:1.2;font-weight:600}
      .sst h3{font-size:1.125rem;font-weight:600}
      .sst p{font-size:1rem;line-height:1.625}
    }

About twelve declarations in all. Do not try to restate every utility you used. This is the block's skeleton, not a second copy of Tailwind.

**Nothing else is assumed.** The block does not expect the page to define a color, a font, a helper class or a shared stylesheet.

This skill is self-contained. Do not read, reference or depend on any other skill, and do not assume that a shared stylesheet, a motion system, a component library or a set of helper classes exists somewhere. If something is not described here, it is not available.

## What it is for

Settings - Set chapters, content fields, sticky position, scroll distance, supporting media, and mobile fallback.

A sequence explained against one picture, so the reader keeps their bearings while the explanation moves.

## When to use it

When the chapters are all about the same thing: one product, one property, one process seen from one angle.

## When not to use it

When each chapter needs its own picture, because swapping the sticky visual as the reader scrolls needs a script. Not for two or three chapters either, which is a list.

## Where it belongs on the page

In the middle of a page, where a reader is already scrolling and will not be surprised by a pinned column.

## What may be changed

The number of chapters, the copy, the sticky visual, and which side the visual sits on.

## What must not be changed

The sticky column must have somewhere to stick. `position: sticky` does nothing if a parent has `overflow: hidden`, and it does nothing if the sticky element is taller than the viewport. Both are silent failures, which is why they are worth checking rather than assuming.

## Settings and Controls



## Creative Design Options

- **Animation.** Crossfade, mask transition, feature highlight, progressive diagram, or scroll-controlled transformation.

## Motion

Each chapter fades and rises as it arrives, using `animation-timeline: view()` inside `@supports`, with the finished state as the default so a browser without it shows everything. The sticky visual itself does not animate.

## Responsive and Accessibility

Sticky is switched off below 1024px and the visual sits above the chapters, because pinning half a phone screen leaves almost nothing for the copy. That is a real fallback, not a compromise.

## Defaults

Visual left and sticky, four chapters right, sticky off below 1024px, chapter fade on arrival.

## What breaks this block

An `overflow: hidden` on an ancestor, which stops sticky working with no error. A sticky element taller than the viewport, which never sticks. Sticky left on at phone width, which fills the screen with the picture.

## What a static block cannot do

Crossfading the visual between chapters, a diagram that builds up, and anything driven by exact scroll position need a script. `position: sticky` and `animation-timeline: view()` cover the rest, and the second degrades to a static section in Firefox.

## Images

Images come from the customer's brief: their own photographs, their own logo, the artwork they supplied. If the brief carries none, use the placeholder that ships inside this block, which is inline SVG and needs no request.

Never reach for a stock photo address. Unsplash, Pexels, Pixabay, a CDN, a hotlinked image on someone else's site: all of them are external files, all of them are ruled out here, and a broken address leaves a hole in the layout with no warning.

Give every image an alt description that says what is in it. Decorative artwork takes `alt=""` so a screen reader passes over it rather than reading a filename.

## Color

The block carries its own color. Six variables are declared on the block itself, and every colored part reads one of them.

| Variable | Used for |
| --- | --- |
| `--sst-surface` | card and panel faces |
| `--sst-band` | the quiet band behind the content |
| `--sst-ink` | headings and primary copy |
| `--sst-soft` | supporting copy |
| `--sst-accent` | the one emphasis color |
| `--sst-deep` | the dark ground |

The values are channel triplets, `15 23 42` rather than `#0f172a`, so an opacity suffix still works: `rgb(var(--sst-ink) / .70)`.

Rebrand the block by changing those six values, in one place. Do not spread hex values through the markup, and do not reach for a color name out of the page's Tailwind config. Every page the builder makes writes its own config with its own color names, so a name taken from one page resolves to nothing on the next. Measured on two live sites: `bg-surface` painted no background on either, and `muted` was a pale background on one and a dark text color on the other.

Do not invent CSS variable names such as `var(--color-surface)`. Nothing defines them, the browser throws the declaration away, and that color or border renders as nothing at all. Do not use `text-current/60` either, because an opacity modifier has no effect on `currentColor`.

Every piece of text has to reach 4.5 to 1 against the color actually behind it, which is the nearest ancestor that paints a background rather than the page. Large text, meaning 24px, or 18.66px when bold, needs 3 to 1. With the default values, ink on deep measures 1.05 to 1, accent on deep 3.62 to 1 and soft on deep 3.45 to 1, so none of those three belong on a dark panel. On a dark panel use the surface value for text. Soft measures 5.06 to 1 on the band and 5.42 on surface, and white on the accent measures 5.17 to 1.

A tint such as `rgb(var(--sst-accent) / .10)` is mostly the color behind it, so judge contrast against the composited result.
