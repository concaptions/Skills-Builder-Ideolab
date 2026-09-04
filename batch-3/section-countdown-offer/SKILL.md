---
name: countdown-offer
description: An offer section built around a stated deadline, with the offer, the saving and the call to action together. Use for a centred deadline, an offer split, or a deadline with a form. Not for a live ticking countdown, which needs a script.
---

# Countdown Offer

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

Put them inside `@layer sco-base { }`. This matters: a layered declaration loses to an unlayered one whatever its specificity or its order, so Tailwind on a real page still wins every one of them, and the layer only shows through where there is no Tailwind. Without the layer, a rule such as `.sco h2` would outrank `.text-4xl` and the block would start overriding the page.

Start with the five reset lines. Tailwind normally supplies these, and without them the list markers show through, the type falls back to a serif, and the content runs wider than the screen. That was measured: a first attempt without them put a block 36px past the right edge of a 1280px viewport.

    @layer sco-base {
      .sco,.sco *,.sco *::before,.sco *::after{box-sizing:border-box}
      .sco{font-family:ui-sans-serif,system-ui,-apple-system,"Segoe UI",Roboto,sans-serif;line-height:1.5}
      .sco h1,.sco h2,.sco h3,.sco h4,.sco p,.sco figure,.sco blockquote{margin:0}
      .sco ul,.sco ol{margin:0;padding:0;list-style:none}
      .sco img,.sco svg,.sco video{display:block;max-width:100%}

Then the block's own shape: the section's background, text color and padding, the wrapper's maximum width, every row or grid the layout depends on, and the heading and body type sizes.

      .sco{background-color:rgb(var(--sco-surface));color:rgb(var(--sco-ink));padding:3rem 1.25rem}
      .sco .sco-wrap{max-width:64rem;margin-inline:auto}
      .sco .sco-row{display:grid;gap:1.5rem}
      .sco h2{font-size:1.875rem;line-height:1.2;font-weight:600}
      .sco h3{font-size:1.125rem;font-weight:600}
      .sco p{font-size:1rem;line-height:1.625}
    }

About twelve declarations in all. Do not try to restate every utility you used. This is the block's skeleton, not a second copy of Tailwind.

**Nothing else is assumed.** The block does not expect the page to define a color, a font, a helper class or a shared stylesheet.

This skill is self-contained. Do not read, reference or depend on any other skill, and do not assume that a shared stylesheet, a motion system, a component library or a set of helper classes exists somewhere. If something is not described here, it is not available.

## What it is for

Settings - Set deadline, evergreen rules, expiration action, offer content, CTA, form, and visitor persistence.

One offer with a real closing date, and everything the reader needs to take it before then.

## When to use it

When there is a genuine deadline: a seasonal price, a limited number of slots, a date after which the terms change.

## When not to use it

When the deadline is invented or resets for each visitor. Readers check, and an evergreen timer costs more than it earns.

## Where it belongs on the page

Near the end of a sales page, after the proof.

## What may be changed

The deadline, the offer wording, the saving, the call to action, and whether the reason for the deadline is given.

## What must not be changed

The date is real, written in words, and carries a `<time datetime>`. No seconds are shown, because seconds that do not move read as broken. The saving is the difference between two prices that were both actually charged.

## Settings and Controls



## Creative Design Options

- **Animation.** Flip digits, urgency color change, progress ring, offer reveal, or CTA emphasis near expiration.

## Motion

The panel fades and rises on entrance. Nothing pulses, and no colour shifts towards urgency, because both need a clock.

## Responsive and Accessibility

One column below 768px with the deadline above the call to action. The button is full width on a phone.

## Defaults

Centred panel, deadline written out with three units, saving shown, one call to action, reason for the deadline stated.

## What breaks this block

Frozen seconds. A deadline with no year. A saving with no original price. An urgency colour with no words.

## What a static block cannot do

Counting down, flip digits, a colour change near expiry, an expiry action and remembering a visitor across visits all need a script or a server. State the date and the reason for it.

## Images

Images come from the customer's brief: their own photographs, their own logo, the artwork they supplied. If the brief carries none, use the placeholder that ships inside this block, which is inline SVG and needs no request.

Never reach for a stock photo address. Unsplash, Pexels, Pixabay, a CDN, a hotlinked image on someone else's site: all of them are external files, all of them are ruled out here, and a broken address leaves a hole in the layout with no warning.

Give every image an alt description that says what is in it. Decorative artwork takes `alt=""` so a screen reader passes over it rather than reading a filename.

## Color

The block carries its own color. Six variables are declared on the block itself, and every colored part reads one of them.

| Variable | Used for |
| --- | --- |
| `--sco-surface` | card and panel faces |
| `--sco-band` | the quiet band behind the content |
| `--sco-ink` | headings and primary copy |
| `--sco-soft` | supporting copy |
| `--sco-accent` | the one emphasis color |
| `--sco-deep` | the dark ground |

The values are channel triplets, `15 23 42` rather than `#0f172a`, so an opacity suffix still works: `rgb(var(--sco-ink) / .70)`.

Rebrand the block by changing those six values, in one place. Do not spread hex values through the markup, and do not reach for a color name out of the page's Tailwind config. Every page the builder makes writes its own config with its own color names, so a name taken from one page resolves to nothing on the next. Measured on two live sites: `bg-surface` painted no background on either, and `muted` was a pale background on one and a dark text color on the other.

Do not invent CSS variable names such as `var(--color-surface)`. Nothing defines them, the browser throws the declaration away, and that color or border renders as nothing at all. Do not use `text-current/60` either, because an opacity modifier has no effect on `currentColor`.

Every piece of text has to reach 4.5 to 1 against the color actually behind it, which is the nearest ancestor that paints a background rather than the page. Large text, meaning 24px, or 18.66px when bold, needs 3 to 1. With the default values, ink on deep measures 1.05 to 1, accent on deep 3.62 to 1 and soft on deep 3.45 to 1, so none of those three belong on a dark panel. On a dark panel use the surface value for text. Soft measures 5.06 to 1 on the band and 5.42 on surface, and white on the accent measures 5.17 to 1.

A tint such as `rgb(var(--sco-accent) / .10)` is mostly the color behind it, so judge contrast against the composited result.
