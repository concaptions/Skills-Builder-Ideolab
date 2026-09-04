---
name: sliding-images
description: A carousel or slider inserted inside a section. Use for a standard carousel, a filmstrip, a logo slider, or a full width rail. Not for a static grid of pictures, which is the photo gallery element.
---

# Sliding Images

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

Put them inside `@layer esl-base { }`. This matters: a layered declaration loses to an unlayered one whatever its specificity or its order, so Tailwind on a real page still wins every one of them, and the layer only shows through where there is no Tailwind. Without the layer, a rule such as `.esl h2` would outrank `.text-4xl` and the block would start overriding the page.

Start with the five reset lines. Tailwind normally supplies these, and without them the list markers show through, the type falls back to a serif, and the content runs wider than the screen. That was measured: a first attempt without them put a block 36px past the right edge of a 1280px viewport.

    @layer esl-base {
      .esl,.esl *,.esl *::before,.esl *::after{box-sizing:border-box}
      .esl{font-family:ui-sans-serif,system-ui,-apple-system,"Segoe UI",Roboto,sans-serif;line-height:1.5}
      .esl h1,.esl h2,.esl h3,.esl h4,.esl p,.esl figure,.esl blockquote{margin:0}
      .esl ul,.esl ol{margin:0;padding:0;list-style:none}
      .esl img,.esl svg,.esl video{display:block;max-width:100%}

Then the block's own shape: the section's background, text color and padding, the wrapper's maximum width, every row or grid the layout depends on, and the heading and body type sizes.

      .esl{background-color:rgb(var(--esl-surface));color:rgb(var(--esl-ink));padding:3rem 1.25rem}
      .esl .esl-wrap{max-width:64rem;margin-inline:auto}
      .esl .esl-row{display:grid;gap:1.5rem}
      .esl h2{font-size:1.875rem;line-height:1.2;font-weight:600}
      .esl h3{font-size:1.125rem;font-weight:600}
      .esl p{font-size:1rem;line-height:1.625}
    }

About twelve declarations in all. Do not try to restate every utility you used. This is the block's skeleton, not a second copy of Tailwind.

**Nothing else is assumed.** The block does not expect the page to define a color, a font, a helper class or a shared stylesheet.

This skill is self-contained. Do not read, reference or depend on any other skill, and do not assume that a shared stylesheet, a motion system, a component library or a set of helper classes exists somewhere. If something is not described here, it is not available.

## What it is for

A carousel or slider inserted inside a section.

More pictures than fit across the section, kept reachable without making the page taller.

## When to use it

When the pictures are equally worth seeing and none has to be seen, so a reader who never scrolls the rail has lost nothing.

## When not to use it

For anything the reader must see. Content past the first slide is content most readers never reach, so never put the price or the call to action inside one.

## Where it belongs on the page

Beneath the copy it supports, running the width of the section so the next slide peeks in and invites a scroll.

## What may be changed

The number of visible slides, the gap, the peek amount, the crop, and whether captions show.

## What must not be changed

It is a real scroll container with `scroll-snap`, not a transform driven track. That gives a finger, a trackpad, a keyboard and a screen reader a working control for nothing. Never hide the overflow and animate a track by hand.

## Settings and Controls

- Set images, slide count, visible items, speed, direction, autoplay, loop, arrows, dots, and drag behavior.
- Control image ratio, peek amount, pause on hover, captions, links, and mobile swipe.

## Creative Design Options

- **Layout.** Standard carousel, coverflow, filmstrip, overlapping images, logo slider, or full-width slider.
- **Motion.** Slide, fade, scale, rotate, depth, spring, momentum, or scroll-linked movement.
- **Styling.** Gradient edge fade, floating cards, image frames, active-slide glow, or layered shadows.

## Motion

Scrolling is the motion, and `scroll-snap` settles each slide into place. Tiles fade in on entrance. Nothing moves on its own, because a rail that advances by itself takes the slide away as the reader starts to read it.

## Responsive and Accessibility

One and a bit slides visible below 640px, two and a bit at 640px, three above 1024px. The peek is deliberate: a partly visible next slide is what tells the reader there is more.

## Defaults

Three visible above 1024px, one and a bit on a phone, 16 by 10 crop, snap to each slide, no autoplay.

## What breaks this block

A transform driven track with hidden overflow, which is unreachable without a script. Autoplay, which fights the reader. A rail with no peek, which looks like a finished row and is never scrolled.

## What a static block cannot do

Arrows, dots, drag with momentum, loop, autoplay and pause on hover all need a script. A real scroll container gives keyboard access, touch, trackpad and a scrollbar with no script at all.

## Images

Images come from the customer's brief: their own photographs, their own logo, the artwork they supplied. If the brief carries none, use the placeholder that ships inside this block, which is inline SVG and needs no request.

Never reach for a stock photo address. Unsplash, Pexels, Pixabay, a CDN, a hotlinked image on someone else's site: all of them are external files, all of them are ruled out here, and a broken address leaves a hole in the layout with no warning.

Give every image an alt description that says what is in it. Decorative artwork takes `alt=""` so a screen reader passes over it rather than reading a filename.

## Color

The block carries its own color. Six variables are declared on the block itself, and every colored part reads one of them.

| Variable | Used for |
| --- | --- |
| `--esl-surface` | card and panel faces |
| `--esl-band` | the quiet band behind the content |
| `--esl-ink` | headings and primary copy |
| `--esl-soft` | supporting copy |
| `--esl-accent` | the one emphasis color |
| `--esl-deep` | the dark ground |

The values are channel triplets, `15 23 42` rather than `#0f172a`, so an opacity suffix still works: `rgb(var(--esl-ink) / .70)`.

Rebrand the block by changing those six values, in one place. Do not spread hex values through the markup, and do not reach for a color name out of the page's Tailwind config. Every page the builder makes writes its own config with its own color names, so a name taken from one page resolves to nothing on the next. Measured on two live sites: `bg-surface` painted no background on either, and `muted` was a pale background on one and a dark text color on the other.

Do not invent CSS variable names such as `var(--color-surface)`. Nothing defines them, the browser throws the declaration away, and that color or border renders as nothing at all. Do not use `text-current/60` either, because an opacity modifier has no effect on `currentColor`.

Every piece of text has to reach 4.5 to 1 against the color actually behind it, which is the nearest ancestor that paints a background rather than the page. Large text, meaning 24px, or 18.66px when bold, needs 3 to 1. With the default values, ink on deep measures 1.05 to 1, accent on deep 3.62 to 1 and soft on deep 3.45 to 1, so none of those three belong on a dark panel. On a dark panel use the surface value for text. Soft measures 5.06 to 1 on the band and 5.42 on surface, and white on the accent measures 5.17 to 1.

A tint such as `rgb(var(--esl-accent) / .10)` is mostly the color behind it, so judge contrast against the composited result.
