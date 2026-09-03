---
name: section-vertical-stack-cards
description: A dedicated section where cards layer vertically as the visitor scrolls through the story. Use for two to four items of equal weight, such as product pillars, service tiers, process phases or differentiators, where each item needs real space and the reader should meet them one at a time. Not for lists of five or more, and not where items must be compared side by side.
---

# Vertical Stack Cards

## What you must return

One `<section>` element, and nothing else.

- No `<!doctype>`, `<html>`, `<head>` or `<body>`. The block is dropped into a page that already exists.
- No markdown code fence around it, and no commentary before or after it. Return the HTML itself. A leftover fence marker renders as visible text on the page, and inside a grid it becomes an extra item that pushes the layout out of shape.
- No `<script>`, no event handlers, no `hidden` attributes waiting to be toggled. Nothing here can run.
- No external files of any kind: no image addresses, no icon library, no web font.
- Everything the section needs lives inside that one element, including its own `<style>` block. That stylesheet is expected, not a last resort. Tailwind utilities carry most of the work, but a relationship between elements cannot be written as a utility, and the ones this set depends on all live in the stylesheet: `:checked ~`, `details[open]`, `:target`, `@keyframes`, `animation-timeline` and the `prefers-reduced-motion` rule. Prefix your own class names so they cannot collide with the rest of the page.
- The section carries its own horizontal padding and its own maximum width. Drop that wrapper and the content runs to both edges of the screen.

This skill is self-contained. Do not read, reference or depend on any other skill, and do not assume that a shared stylesheet, a motion system, a component library or a set of helper classes exists somewhere. If something is not described here, it is not available.

## What it is for

Cards that come to rest on top of each other as the page scrolls, like a pile of paper. It gives a short sequence weight and pace without spending the page height a normal list would.

## When to use it

- There are three to five substantial points that deserve emphasis.
- Each card holds a heading and a short paragraph rather than a long body.
- The order is meaningful and the last card is the one to remember.

## When not to use it

- There are more than about five cards. The pile becomes a shuffle nobody follows.
- The cards carry long content. Anything taller than the viewport breaks the effect.
- The content is reference material people will want to scan. A plain list is better.

## Where it belongs on the page

Middle of the page. It is a pacing device, so it should sit between two calmer sections rather than next to another heavy interaction.

## What may be changed

- Number of cards, their copy and their accent colours.
- The top offset and how far each card steps down from the one before.
- Card padding, radius and border.
- Whether the pile applies on tablet or only on desktop.

## What must not be changed

- Opaque card faces. A translucent card lets the one underneath show through and the pile turns to mud.
- No bottom margin on the sticky wrapper. A margin ends the stick early and the pile falls apart.
- No ancestor with overflow hidden, because position sticky stops working inside one.
- The pile collapsing to a plain list on small screens and under reduced motion.

## Settings and Controls

| Setting | Options | Default |
| --- | --- | --- |
| Card count | 2 to 4 | 3 |
| Card height | Matched across all cards, or content height | matched |
| Stack offset | Vertical distance each card is offset from the one below | 0, cards land flush |
| Scroll distance | How much scroll each card holds before the next arrives | 100vh per card |
| Sticky position | How far from the top of the viewport the cards settle | 18vh |
| Content fields | Eyebrow, headline, description, bullet list, summary band | headline, description, bullets |
| Show or hide, per card | Image, icon, number, headline, description, progress, and CTA per card | image, headline, description on |
| Mobile and reduced-motion fallback | Click, swipe, or static cards | static cards |
| Card persistence | Cards remain visible in the stack, or fully replace one another | remain visible |

**Card persistence changes the build.** *Remain visible* means each card is opaque and simply covers the last, so a sliver of the previous card can be left showing through the stack offset. *Fully replace* means the outgoing card fades out as the incoming one arrives, leaving nothing behind it.

**Matched card height is not cosmetic.** If cards differ in height, a shorter card cannot fully cover the taller one behind it and the reader sees the previous card's edge poking out. Every card must share the same minimum height.

**Set that height from the content, not from a fixed number.** `34rem` suits a card with a heading, a paragraph, three or four bullets and a summary band. Cards carrying only a heading and one paragraph should drop to around `26rem` rather than being padded with filler. The rule is that all cards match each other, not that they all match `34rem`.

## Creative Design Options

**Layout**

- Top-aligned stack. Cards pin near the top of the viewport. The default and the safest.
- Centered stack. Cards pin at the vertical center, which suits shorter cards.
- Alternating offset. Each card sits slightly left or right of the one below it.
- Full-screen stack. Each card fills the viewport.
- Image-and-copy stack. Each card is split, text one side and image the other.

**Motion**

Cards layer, scale, rotate, fade, blur, or compress as the next card arrives.

Pick one and hold it across the whole section. See the Motion notes below for how each is built.

**Story treatment**

Background color or image changes with each card; progress can track the active card. The background change moves the page itself through the story. The progress indicator can be dots, a bar, or a numbered counter.

**Card style** (pick one and hold it across all cards)

Editorial, image-led, glass, bold gradient, browser mockup, testimonial, or product card.

## Motion

- **Scroll-linked:** the sticky stack. This is a *major showcase effect*, so the page must not also carry a scroll-driven story or a parallax hero.
- **Entrance:** each card fades and rises on first appearance, at `standard` intensity.
- **Hover:** card images take the default 2 to 4 percent zoom. The card itself does not lift, because it is already pinned.

How each stacking motion is built:

| Motion | Build |
| --- | --- |
| Layer | Sticky alone. The arriving card simply covers the last. No extra work |
| Scale | The covered card scales down slightly as the next arrives |
| Rotate | The covered card takes a small rotation, one or two degrees |
| Fade | The covered card drops in opacity. This is the *fully replace* persistence mode |
| Blur | The covered card takes a small `filter: blur`. Costly on mobile, keep the radius low |
| Compress | The covered card loses height, so the stack squeezes together |

Scale, rotate, fade, blur and compress all need the covered card's state to follow scroll position, so each is scroll-linked and carries the support caveat below. Plain layering is pure CSS and needs nothing. **Prefer layering** unless the design specifically calls for another.

Do not add tilt, glow or a border draw here. The stacking is the effect.

## Responsive and Accessibility

| Width | Behavior |
| --- | --- |
| Desktop, 1024px and up | Sticky stack active. Cards pin at the offset and cover in order |
| Tablet and mobile, under 1024px | **Sticky is switched off.** The chosen fallback applies: static cards flowing one after another, or a click or swipe carousel |

Turning sticky off below 1024px is required, not optional. A card pinned at 18vh on a phone leaves almost no room to read it, and on iOS a sticky element inside a scrolling container behaves inconsistently.

- Cards are in reading order in the markup, so the effect matches the tab order.
- Buttons and links inside a card remain reachable while that card is pinned.
- With reduced motion, **the stack still works.** The reader is driving it with their own scroll, so nothing plays at them. Remove the entrance fade and any scale, rotate, blur or compress, and keep plain layering.
- Card headings use a real heading level in sequence, not styled text.

## Defaults

Three cards, image-and-copy stack layout, matched height, editorial card style, pinned at 18vh, layer motion, cards remain visible, no progress indicator, sticky above 1024px only with static cards below, standard entrance, image hover zoom.

## What breaks this section

1. **A bottom margin on the sticky wrapper.** It shortens the distance the card stays pinned, so the card slides up past the offset before the next one arrives and the stack visibly breaks. Put spacing on the card itself or on the container, never on the sticky wrapper.
2. **Different card heights.** A short card cannot cover a tall one, and the reader sees the previous card's edge behind it.
3. **Leaving sticky on at mobile widths.** Use the `lg:` prefix so it only applies from 1024px.

## What a static block cannot do

These blocks carry no JavaScript. A few of the options above have no static equivalent, so build the nearest thing that does work and leave the rest out. Never ship a control that looks alive and does nothing when it is pressed.

- **Scale, rotate, fade, blur and compress on the covered card.** Each needs the covered card's state to follow the scroll position, so each is scroll-linked. See below. Plain layering needs nothing at all and is the default for this reason.

**Scroll-linked motion.** Anything whose state has to follow the scroll position uses `animation-timeline: view()` inside an `@supports` block. Firefox does not support it, so write the finished state as the default and let the animation be the enhancement. Never use a library or an observer for this.

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
