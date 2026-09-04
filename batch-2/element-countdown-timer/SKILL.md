---
name: countdown-timer
description: A deadline shown inside an offer, hero, form or call to action section. Use for a stated closing date and time in a digital row, stacked units, a minimal line, or a badge. Not for a live ticking clock, which cannot count down without a script.
---

# Countdown Timer

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

Put them inside `@layer ecd-base { }`. This matters: a layered declaration loses to an unlayered one whatever its specificity or its order, so Tailwind on a real page still wins every one of them, and the layer only shows through where there is no Tailwind. Without the layer, a rule such as `.ecd h2` would outrank `.text-4xl` and the block would start overriding the page.

Start with the five reset lines. Tailwind normally supplies these, and without them the list markers show through, the type falls back to a serif, and the content runs wider than the screen. That was measured: a first attempt without them put a block 36px past the right edge of a 1280px viewport.

    @layer ecd-base {
      .ecd,.ecd *,.ecd *::before,.ecd *::after{box-sizing:border-box}
      .ecd{font-family:ui-sans-serif,system-ui,-apple-system,"Segoe UI",Roboto,sans-serif;line-height:1.5}
      .ecd h1,.ecd h2,.ecd h3,.ecd h4,.ecd p,.ecd figure,.ecd blockquote{margin:0}
      .ecd ul,.ecd ol{margin:0;padding:0;list-style:none}
      .ecd img,.ecd svg,.ecd video{display:block;max-width:100%}

Then the block's own shape: the section's background, text color and padding, the wrapper's maximum width, every row or grid the layout depends on, and the heading and body type sizes.

      .ecd{background-color:rgb(var(--ecd-surface));color:rgb(var(--ecd-ink));padding:3rem 1.25rem}
      .ecd .ecd-wrap{max-width:64rem;margin-inline:auto}
      .ecd .ecd-row{display:grid;gap:1.5rem}
      .ecd h2{font-size:1.875rem;line-height:1.2;font-weight:600}
      .ecd h3{font-size:1.125rem;font-weight:600}
      .ecd p{font-size:1rem;line-height:1.625}
    }

About twelve declarations in all. Do not try to restate every utility you used. This is the block's skeleton, not a second copy of Tailwind.

**Nothing else is assumed.** The block does not expect the page to define a color, a font, a helper class or a shared stylesheet.

This skill is self-contained. Do not read, reference or depend on any other skill, and do not assume that a shared stylesheet, a motion system, a component library or a set of helper classes exists somewhere. If something is not described here, it is not available.

## What it is for

A timer element that can be placed inside any offer, hero, form, or CTA section.

Telling the reader when the offer ends, plainly enough that they can act on it.

## When to use it

When there is a real deadline. A date that means something is more persuasive than a clock, because the reader can put it in a diary.

## When not to use it

When the deadline is invented. An evergreen timer that resets for every visitor is a lie the reader eventually notices, and it costs more trust than it wins sales.

## Where it belongs on the page

Beside the offer and above the call to action, never on its own.

## What may be changed

The deadline, the labels, the urgency line, and whether the units are shown as a row or stacked.

## What must not be changed

The date shown is the real date. It is written out as text as well as in units, and it carries a `<time datetime>` so it is machine readable. Never state a number of seconds that does not move, which reads as broken rather than urgent.

## Settings and Controls

- Set fixed date, evergreen duration, timezone, labels, expiration action, reset behavior, and visitor persistence.
- Show or hide: days, hours, minutes, seconds, urgency message, and completion message.
- Control width, alignment, digit size, separators, and mobile layout.

## Creative Design Options

- **Layout.** Digital row, flip clock, circular countdown, stacked units, minimal line, or badge timer.
- **Motion.** Flip digits, count transition, pulse near expiration, progress ring, or color urgency shift.
- **Styling.** Brand gradient, dark display, soft card, illuminated digits, or clean editorial timer.

## Motion

The panel fades and rises on entrance. Nothing pulses. A pulsing deadline beside a call to action makes both harder to read and neither more likely to be pressed.

## Responsive and Accessibility

Units wrap to two rows below 480px rather than shrinking below readable size. The written date stays on one line wherever it can, because that is the part a reader will act on.

## Defaults

Closing date written out, four units shown as labelled boxes, urgency line beneath, no motion beyond the entrance.

## What breaks this block

Seconds shown but never moving, which reads as a bug. A deadline with no year, which is ambiguous once it passes. An urgency colour with no words, which is invisible to anybody who cannot see it.

## What a static block cannot do

Counting down, flipping digits, a progress ring that empties, a colour change near expiry, an expiry action and remembering a visitor across visits all need a script or a server. State the deadline instead, which is what this block does.

## Images

Images come from the customer's brief: their own photographs, their own logo, the artwork they supplied. If the brief carries none, use the placeholder that ships inside this block, which is inline SVG and needs no request.

Never reach for a stock photo address. Unsplash, Pexels, Pixabay, a CDN, a hotlinked image on someone else's site: all of them are external files, all of them are ruled out here, and a broken address leaves a hole in the layout with no warning.

Give every image an alt description that says what is in it. Decorative artwork takes `alt=""` so a screen reader passes over it rather than reading a filename.

## Color

The block carries its own color. Six variables are declared on the block itself, and every colored part reads one of them.

| Variable | Used for |
| --- | --- |
| `--ecd-surface` | card and panel faces |
| `--ecd-band` | the quiet band behind the content |
| `--ecd-ink` | headings and primary copy |
| `--ecd-soft` | supporting copy |
| `--ecd-accent` | the one emphasis color |
| `--ecd-deep` | the dark ground |

The values are channel triplets, `15 23 42` rather than `#0f172a`, so an opacity suffix still works: `rgb(var(--ecd-ink) / .70)`.

Rebrand the block by changing those six values, in one place. Do not spread hex values through the markup, and do not reach for a color name out of the page's Tailwind config. Every page the builder makes writes its own config with its own color names, so a name taken from one page resolves to nothing on the next. Measured on two live sites: `bg-surface` painted no background on either, and `muted` was a pale background on one and a dark text color on the other.

Do not invent CSS variable names such as `var(--color-surface)`. Nothing defines them, the browser throws the declaration away, and that color or border renders as nothing at all. Do not use `text-current/60` either, because an opacity modifier has no effect on `currentColor`.

Every piece of text has to reach 4.5 to 1 against the color actually behind it, which is the nearest ancestor that paints a background rather than the page. Large text, meaning 24px, or 18.66px when bold, needs 3 to 1. With the default values, ink on deep measures 1.05 to 1, accent on deep 3.62 to 1 and soft on deep 3.45 to 1, so none of those three belong on a dark panel. On a dark panel use the surface value for text. Soft measures 5.06 to 1 on the band and 5.42 on surface, and white on the accent measures 5.17 to 1.

A tint such as `rgb(var(--ecd-accent) / .10)` is mostly the color behind it, so judge contrast against the composited result.
