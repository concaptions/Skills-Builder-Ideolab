---
name: section-animated-logo-stream
description: A complete trust section displaying clients, partners, publications, certifications, or supported platforms. Use for a moving logo row, two opposing rows, a vertical stream, a grid that becomes a stream, or a circular logo orbit. Not for a feature list with icons, and not for testimonials.
---

# Animated Logo Stream

## What you must return

One `<section>` element, and nothing else.

- No `<!doctype>`, `<html>`, `<head>` or `<body>`. The block is dropped into a page that already exists.
- No markdown code fence around it, and no commentary before or after it. Return the HTML itself. A leftover fence marker renders as visible text on the page, and inside a grid it becomes an extra item that pushes the layout out of shape.
- No `<script>`, no event handlers, no `hidden` attributes waiting to be toggled. Nothing here can run.
- No external files of any kind: no image addresses, no icon library, no web font.
- Everything the section needs lives inside that one element, including its own `<style>` block. That stylesheet is expected, not a last resort. Tailwind utilities carry most of the work, but a relationship between elements cannot be written as a utility, and the ones this set depends on all live in the stylesheet: `:checked ~`, `details[open]`, `:target`, `@keyframes`, `animation-timeline` and the `prefers-reduced-motion` rule. Prefix your own class names so they cannot collide with the rest of the page.
- The section carries its own horizontal padding and its own maximum width. Drop that wrapper and the content runs to both edges of the screen.

- Color an SVG with a presentation attribute as well as a class. `<rect fill="#f6f7f9" class="fill-muted">` takes the theme color wherever the stylesheet is loaded and falls back to the attribute where it is not. With only the class, an unresolved fill computes to solid black, and the placeholder becomes a black rectangle sitting over the layout. That happens on a page without the stylesheet, and again in the moment before it arrives.
- Write `fill="none"` on any path that is stroke only. A path with no fill is filled solid black. That is the SVG default, not a fault in the browser.

**Write the block's own base layer.** Tailwind utilities carry the detail, but a plain HTML viewer has no Tailwind, and a block built only from utilities arrives there as unstyled text on a white page. So write the few declarations that carry the block's shape into its own stylesheet as well. The block then reads correctly in a viewer and on a real page both.

Put them inside `@layer als-base { }`. This matters: a layered declaration loses to an unlayered one whatever its specificity or its order, so Tailwind on a real page still wins every one of them, and the layer only shows through where there is no Tailwind. Without the layer, a rule such as `.als h2` would outrank `.text-4xl` and the block would start overriding the page.

Start with the five reset lines. Tailwind normally supplies these, and without them the list markers show through, the type falls back to a serif, and the content runs wider than the screen. That was measured: a first attempt without them put a block 36px past the right edge of a 1280px viewport.

    @layer als-base {
      .als,.als *,.als *::before,.als *::after{box-sizing:border-box}
      .als{font-family:ui-sans-serif,system-ui,-apple-system,"Segoe UI",Roboto,sans-serif;line-height:1.5}
      .als h1,.als h2,.als h3,.als h4,.als p,.als figure,.als blockquote{margin:0}
      .als ul,.als ol{margin:0;padding:0;list-style:none}
      .als img,.als svg,.als video{display:block;max-width:100%}

Then the block's own shape. Cover these and nothing more.

- The section: background color, text color, horizontal and vertical padding.
- The inner wrapper: maximum width, centered. Name the wrapper class you actually used, not one from this example.
- Every row, column or grid the layout depends on: display, columns, gap.
- The heading and the body copy: font size, weight, line height.
- Shapes a reader would notice at a glance: border radius, border color.

      .als{background-color:rgb(var(--als-surface));color:rgb(var(--als-ink));padding:5rem 1.25rem}
      .als .als-wrap{max-width:64rem;margin-inline:auto}
      .als .als-row{display:grid;gap:2.5rem}
      .als h2{font-size:2.25rem;line-height:1.15;font-weight:600}
      .als h3{font-size:1.25rem;font-weight:600}
      .als p{font-size:1rem;line-height:1.625}
    }

About twelve declarations in all. Do not try to restate every utility you used. This is the block's skeleton, not a second copy of Tailwind. The exact spacing scale, the small type sizes and the hover states stay in the utilities where they belong.

**Nothing else is assumed.** The block does not expect the page to define a color, a font, a helper class or a shared stylesheet. If the page loads Tailwind the block is complete; if it does not, the base layer keeps it readable and in shape.

This skill is self-contained. Do not read, reference or depend on any other skill, and do not assume that a shared stylesheet, a motion system, a component library or a set of helper classes exists somewhere. If something is not described here, it is not available.

## What it is for

A moving row of client, partner or accreditation logos. It is a credibility signal read in about one second, not something anyone studies.

## When to use it

- There are at least six recognisable names or accreditations.
- The names carry weight in the visitor's world, such as trade bodies, insurers or known brands.
- The page needs proof early without spending vertical space on it.

## When not to use it

- Fewer than six logos. A short row that loops looks like padding. Use a static row instead.
- The logos are unknown to the audience. An unrecognised name proves nothing and costs trust.
- Permission to display a client's mark has not been given.

## Where it belongs on the page

Directly under the hero, or immediately after the first proof section. It is a thin band, not a destination, so it should never sit between two heavy sections.

## What may be changed

- The logos, their order and how many appear before the set repeats.
- Speed, direction and the width of the edge fade.
- Whether logos are greyscale at rest or shown in full colour.
- The label above the row.

## What must not be changed

- The duplicate copy of the set. Removing it breaks the seamless loop.
- The aria-hidden attribute on that duplicate, otherwise every name is announced twice.
- Pausing on hover and on focus. A row that never stops cannot be read by someone who needs a moment.
- The reduced motion rules, which stop the drift completely.

## Settings and Controls

| Setting | Options | Default |
| --- | --- | --- |
| Logos | The set of images, in order | supplied set |
| Logo height | Fixed height in pixels, with widths free | 28px |
| Spacing | Tight, standard, or generous | standard |
| Rows | 1 to 3 | 1 |
| Direction | Left, right, up, down, or alternating per row | left |
| Speed | Slow, standard, or fast | slow |
| Maximum visible logos | How many are in the loop before it repeats | all |
| Show or hide | Headline, supporting statement, captions, arrows, drag behavior, pause control | headline |
| Behavior | Automatic looping, scroll-linked movement, user dragging, or static | automatic looping |
| Source assets | Monochrome or brand color | monochrome |
| Mobile speed | Same, slower, or static | slower |

**Fix the height, never the width.** Logos come in wildly different proportions. Setting a common width squashes a wordmark and blows up a monogram. Set one height and let widths fall where they may, with consistent spacing between.

**Monochrome is the safe default.** Twelve logos in twelve brand palettes turn a trust signal into visual noise. Monochrome with an optional color-on-hover keeps the row calm and still rewards a closer look.

**The loop needs the set duplicated.** A seamless stream is the same list rendered twice, translated by exactly half its width, so the reset is invisible. With one copy, the row runs out and jumps.

**Use the logos you are allowed to use.** A client logo is that client's trademark. This section only carries logos the business has permission to display.

## Creative Design Options

**Layout**

- Single row. The default.
- Two opposing rows, the second moving the other way.
- Vertical stream, moving up or down a column.
- Logo grid becoming a stream, static on entry then moving as the visitor scrolls.
- Circular logo orbit, arranged around a center point.

**Motion**

Continuous movement, alternating directions, scroll-speed response, draggable momentum, or pause on hover.

**Logo treatment**

Grayscale-to-color hover, glow, glass tile, soft shadow, or no container.

**Section treatment**

Gradient edge fades, subtle background mesh, trust statement reveal, or layered depth.

## Motion

- **Continuous movement** is the section's defining behavior and it is bounded by these rules: it **pauses on hover, pauses on focus, and stops when the section leaves the viewport**. A logo row still animating off screen is wasted work on every frame.
- **Speed:** slow means the row takes 40 to 60 seconds to complete one pass. Anything faster reads as urgent, which is the wrong tone for a trust signal.
- **Scroll-speed response** ties movement to scroll, so it counts as the page's one major showcase effect.
- **Draggable momentum** needs the pointer events to release cleanly. A drag that carries on after the pointer leaves the window leaves the row spinning.
- **Grayscale-to-color hover** is a `filter` transition at `subtle` intensity.
- **Gradient edge fades** are static masks, not motion. They stop the row appearing to be cut off at the section edge.

## Responsive and Accessibility

| Width | Behavior |
| --- | --- |
| Desktop, 1024px and up | Full layout as chosen. Hover treatments and drag active |
| Tablet, 768px to 1023px | Rows reduce to one or two. Circular orbit becomes a single row |
| Mobile, under 768px | One row, slower or static. No hover treatments. Drag remains if it was on |

- Each logo has alt text naming the company. A logo with `alt=""` tells a screen-reader user nothing, and the names are the entire content of this section.
- The row is a list, so the count is available without seeing it.
- Duplicated logos in the loop are `aria-hidden`, otherwise every name is read twice.
- A pause control is provided whenever movement is automatic, and it is a real button with a label that changes between Pause and Play.
- Arrows are labeled controls.
- With reduced motion, **the stream does not move at all.** It renders as a static row or grid showing as many logos as fit, with the rest reachable by scrolling the container. This is the required fallback, not an optional one.

## Defaults

One row, 28px logo height, standard spacing, moving left, slow speed, monochrome assets, headline shown, automatic looping, pause on hover, grayscale to color on hover, gradient edge fades, slower on mobile, static under reduced motion.

## What breaks this section

1. **One copy of the list.** The row empties, then snaps back, and the jump is the only thing anyone notices.
2. **Setting a common width.** Wide wordmarks squash and square monograms tower over everything else. Fix the height instead.
3. **No pause and no reduced-motion stop.** Permanent movement in the corner of the eye makes the rest of the page harder to read.

## What a static block cannot do

These blocks carry no JavaScript. A few of the options above have no static equivalent, so build the nearest thing that does work and leave the rest out. Never ship a control that looks alive and does nothing when it is pressed.

- **Drag to scrub the row.** The stream is a CSS animation and cannot be dragged. If the row is also a real scroll container, a touch swipe and a trackpad still work, but mouse dragging does not. Pause on hover and on focus is pure CSS and stays.

## Images

Every image belongs to the customer. Use the photographs, logos and artwork supplied in the brief.

Never pull a photograph from a stock site. A link to Unsplash, Pexels, Shutterstock or any other outside address is wrong on two counts: it is not the customer's business in the picture, and the block stops working the moment that address changes.

If no photograph has been supplied yet, keep the placeholder that ships inside the block. It is drawn in the page itself, so it needs nothing from outside, it takes the theme colors, and it holds the right shape so the layout does not move when the real photograph arrives.

Give every image an `alt` description of what is in it, and a `width` and `height`, so the page does not jump as it loads.

## Color

The block carries its own colour. Six variables are declared on the section element itself, and every
coloured part of the block reads one of them.

| Variable | Used for |
| --- | --- |
| `--als-surface` | card and panel faces |
| `--als-band` | the quiet band behind the content |
| `--als-ink` | headings and primary copy |
| `--als-soft` | supporting copy |
| `--als-accent` | the one emphasis colour |
| `--als-deep` | the dark ground |

The values are channel triplets, `15 23 42` rather than `#0f172a`, so an opacity suffix still works:
`rgb(var(--als-ink) / .70)`.

Rebrand the block by changing those six values, in one place. Do not spread hex values through the
markup, and do not reach for a colour name out of the page's Tailwind config. Every page the builder
makes writes its own config with its own colour names, so a name taken from one page resolves to
nothing on the next. Measured on two live sites: `bg-surface` painted no background on either, and
`muted` was a pale background on one and a dark text colour on the other, which turned a light band
into a dark one with dark text on it.

Do not invent CSS variable names such as `var(--color-surface)` or `var(--color-border)`. Nothing
defines them, the browser throws the declaration away, and that colour, border or shape renders as
nothing at all. Do not use `text-current/60` either, because an opacity modifier has no effect on
`currentColor` and the text comes out at full strength.

A colour is not readable just because it came from a variable. Every piece of text has to reach 4.5 to
1 against the colour actually behind it, which is the nearest ancestor that paints a background rather
than the page. Large text, meaning 24px, or 18.66px when it is bold, needs 3 to 1.

With the default values these four pairs fail, so do not reach for them.

| Text | Background | Measured |
| --- | --- | --- |
| ink | deep | 1.05 to 1, near black on near black |
| accent | deep | 3.62 to 1, the accent is a dark blue and it sinks into a dark panel |
| soft | deep | 3.93 to 1 |
| soft | band | 4.44 to 1, although the same value clears on surface at 4.76 to 1 |

On a dark panel, set text in the surface value and use a lowered opacity of it for the quieter line.
White on the accent measures 6.3 to 1, so the accent still works there as a button background. On the
band, use a lowered opacity of ink for quiet copy.

A tint such as `rgb(var(--als-accent) / .10)` is mostly the colour behind it, so judge contrast
against the composited result, not against the accent itself.
