---
name: section-animated-logo-stream
description: A complete trust section displaying clients, partners, publications, certifications, or supported platforms. Use for a moving logo row, two opposing rows, a vertical stream, a grid that becomes a stream, or a circular logo orbit. Not for a feature list with icons, and not for testimonials.
---

# Animated Logo Stream

## What you must return

One `<section>` element, and nothing else.

- No `<!doctype>`, `<html>`, `<head>` or `<body>`. The block is dropped into a page that already exists.
- No markdown code fence around it, and no commentary before or after it. Return the HTML itself. A stray ``` renders as visible text on the page, and inside a grid it becomes an extra item that pushes the layout out of shape.
- No `<script>`, no event handlers, no `hidden` attributes waiting to be toggled. Nothing here can run.
- No external files of any kind: no image addresses, no icon library, no web font.
- Everything the section needs lives inside that one element, including its own `<style>` block. That stylesheet is expected, not a last resort. Tailwind utilities carry most of the work, but a relationship between elements cannot be written as a utility, and the ones this set depends on all live in the stylesheet: `:checked ~`, `details[open]`, `:target`, `@keyframes`, `animation-timeline` and the `prefers-reduced-motion` rule. Prefix your own class names so they cannot collide with the rest of the page.
- The section carries its own horizontal padding and its own maximum width. Drop that wrapper and the content runs to both edges of the screen.

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

Use the page's theme tokens. Never hardcode a hex value or a Tailwind palette color, because this section has to work on a dark editorial page and a light clinical one without being rewritten.

The tokens are `surface`, `muted`, `textdark`, `textmute`, `accent` and `bgdark`. Use them as Tailwind classes: `bg-surface`, `bg-muted`, `bg-bgdark`, `text-textdark`, `text-textmute`, `text-accent`, `border-accent`.

Do not invent CSS variable names such as `var(--color-surface)` or `var(--color-border)`. Nothing on the page defines them, so the browser throws the declaration away and that color, border or shape renders as nothing at all. Do not use `text-current/60` either, because an opacity modifier has no effect on `currentColor` and the text comes out at full strength.

A token is not readable just because it is a token. Every piece of text has to reach 4.5 to 1 against the color actually behind it, which is the nearest ancestor that paints a background rather than the page. Large text, meaning 24px, or 18.66px when it is bold, needs 3 to 1.

These four pairs are measured and they fail, so do not reach for them.

| Text | Background | Measured |
| --- | --- | --- |
| `text-textdark` | `bg-bgdark` | 1.05 to 1, near black on near black |
| `text-accent` | `bg-bgdark` | 3.62 to 1, the accent is a dark blue and it sinks into a dark panel |
| `text-textmute` | `bg-bgdark` | 3.93 to 1 |
| `text-textmute` | `bg-muted` | 4.44 to 1, although the same token clears on `bg-surface` at 4.76 to 1 |

On a dark panel, set text in `text-surface` and use a lowered opacity of it for the quieter line. The accent still works there as a button's background, where white sits on it at 6.3 to 1. On a muted panel, use a lowered opacity of `text-textdark` for quiet copy.

A tint such as `bg-accent/10` is mostly the color behind it, so judge contrast against the composited result, not against the accent itself.
