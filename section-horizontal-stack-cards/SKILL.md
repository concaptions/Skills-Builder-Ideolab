---
name: section-horizontal-stack-cards
description: A separate card section that moves or stacks cards horizontally rather than vertically. Use for a scroll-controlled or draggable card run, an overlapping fan, or a horizontal stage, carrying a product story, portfolio, benefit sequence, testimonials, a process, or an image gallery. Not for vertical sticky stacking, which is the vertical stack cards section.
---

# Horizontal Stack Cards

## What you must return

One `<section>` element, and nothing else.

- No `<!doctype>`, `<html>`, `<head>` or `<body>`. The block is dropped into a page that already exists.
- No markdown code fence around it, and no commentary before or after it. Return the HTML itself. A stray ``` renders as visible text on the page, and inside a grid it becomes an extra item that pushes the layout out of shape.
- No `<script>`, no event handlers, no `hidden` attributes waiting to be toggled. Nothing here can run.
- No external files of any kind: no image addresses, no icon library, no web font.
- Everything the section needs lives inside that one element, including any `<style>` it uses. Prefix your own class names so they cannot collide with the rest of the page.
- The section carries its own horizontal padding and its own maximum width. Drop that wrapper and the content runs to both edges of the screen.

This skill is self-contained. Do not read, reference or depend on any other skill, and do not assume that a shared stylesheet, a motion system, a component library or a set of helper classes exists somewhere. If something is not described here, it is not available.

## What it is for

A sideways row of cards the visitor moves through: case studies, products, team members or locations. It shows there is more without spending the vertical space.

## When to use it

- There are five or more comparable items.
- Each item is a small card rather than something that needs a full section.
- Vertical space is tight and the items are browsable rather than essential.

## When not to use it

- Every item must be seen. Anything off screen will be missed by most visitors.
- There are three or fewer items. A grid is better and needs no interaction.
- The content is critical to the decision. Put that in a grid where nothing hides.

## Where it belongs on the page

Middle of the page. It works well after a services section, showing proof of the services just described.

## What may be changed

- Card count, card width, gap and the snap position.
- Whether arrows or a scroll hint appear.
- Card contents, image ratio and corner treatment.
- Whether the first card is inset or flush with the page edge.

## What must not be changed

- The real scroll container. It is what gives drag, swipe, trackpad and keyboard support with no script at all.
- Scroll snapping, so cards never come to rest half off the edge.
- The container staying focusable, otherwise keyboard users cannot reach the later cards.
- Never converting vertical scroll into horizontal movement. It traps the visitor on the section and fights the trackpad.
- The reduced motion rules.

## Settings and Controls

| Setting | Options | Default |
| --- | --- | --- |
| Card count | 3 to 12 | 5 |
| Card width | Narrow, standard, wide, or full stage | standard |
| Overlap | None, slight, or heavy | none |
| Direction | Left to right, right to left, or alternating | left to right |
| Scroll distance | How much page scroll drives one full pass | 1.5 screens |
| Snapping | Snap to each card, snap to groups, or free | snap to each card |
| Controls | Arrows, dots, progress, or none | arrows |
| Content fields | Eyebrow, number, headline, description, image, CTA | number, headline, description, image |
| Show or hide | Arrows, dots, progress, captions, numbers, images, CTAs | arrows, numbers, images |
| Behavior | Scroll-controlled, draggable, arrow-controlled, automatic, or static | draggable with arrows |
| Mobile conversion | Becomes a carousel, or stays as it is | carousel |
| Visible peek on mobile | How much of the next card shows | 15 percent |

**Scroll-controlled behavior hijacks the page.** Turning vertical scroll into horizontal movement means the visitor cannot scroll past the section normally, and on a trackpad it fights them. Hijacking scroll is not acceptable, so prefer draggable with arrows. If scroll-controlled is genuinely required, keep the scroll distance short, no more than about 1.5 screens, so the section cannot trap anyone.

**The peek is what tells the visitor there is more.** A row that ends exactly at the container edge looks finished. Always leave part of the next card showing.

**Snapping and free scrolling suit different content.** Snap to each card when each card is a step or a chapter. Use free when the cards are a gallery the visitor is browsing.

## Creative Design Options

**Layout**

- Left to right. The default.
- Right to left, for a run that reads as counting down or unwinding.
- Alternating directions across two rows.
- Overlapping fan, where cards splay from a common point.
- Horizontal stage, where one card is centered and active and the neighbors are pushed back.

**Motion**

Snap, overlap, rotate, fan, slide under, slide over, or accelerate with scrolling.

**Depth**

Perspective, shadow progression, background transition, active-card scale, or edge blur.

**Card style**

Product story, portfolio, benefit sequence, testimonial, process, or image gallery card.

## Motion

- **Scroll-controlled and accelerate-with-scrolling** are scroll-linked, so this becomes the page's one major showcase effect. Do not put it on a page that already has a vertical card stack or a sticky scroll story.
- **Entrance:** the section fades and rises once. The cards do not stagger, because they arrive already in a row.
- **Snap** is CSS `scroll-snap-type: x mandatory` on the track with `scroll-snap-align: center` on the cards. It needs no JavaScript.
- **Active-card scale** at `subtle` only, and the track keeps a fixed height so a scaling card does not resize the section.
- **Rotate and fan** apply small angles, two to six degrees. Anything larger makes text hard to read.
- **Edge blur** is a mask on the track edges, not a `filter` on the cards, because blurring several cards every frame is expensive.
- **Automatic behavior** pauses on hover and on focus, and stops when the section leaves the viewport.

## Responsive and Accessibility

| Width | Behavior |
| --- | --- |
| Desktop, 1024px and up | Full layout as chosen. Drag, arrows and scroll control active |
| Tablet, 768px to 1023px | Fan and stage become a plain snapping row. Overlap reduces to slight |
| Mobile, under 768px | A swipeable carousel with a peek of the next card. Scroll-controlled behavior is switched off, because taking over vertical scroll on a phone traps the visitor |

- The track is keyboard scrollable, and arrows move it by exactly one card.
- Arrows and dots are labeled buttons, and the arrow at the end of the run is disabled rather than removed, so the row does not reflow.
- Cards are in reading order in the markup, so tab order matches the visual order.
- Focusing a card off screen scrolls it into view, which `scroll-snap` handles natively when the track is a real scroll container.
- Automatic movement has a pause control.
- With reduced motion, automatic movement does not start, scroll-controlled movement is replaced by a plain scroll container, rotation and fan angles are removed, and snapping becomes `proximity` so the page is never pulled around.

## Defaults

Five cards, standard width, no overlap, left to right, snap to each card, arrows shown, numbers and headline and description and image shown, draggable with arrows, carousel on mobile with a 15 percent peek, shadow progression for depth, process card style, standard fade-and-rise entrance.

## What breaks this section

1. **Turning vertical scroll into horizontal movement over a long distance.** The visitor cannot get past the section, which is the scroll hijacking the shared motion rules forbids.
2. **No peek of the next card.** Nothing signals that the row moves, so most visitors never scroll it.
3. **Rebuilding drag in JavaScript.** A native `overflow-x-auto` container already handles drag, swipe, momentum, keyboard and focus scrolling. A custom implementation loses most of that.

## What a static block cannot do

These blocks carry no JavaScript. A few of the options above have no static equivalent, so build the nearest thing that does work and leave the rest out. Never ship a control that looks alive and does nothing when it is pressed.

- **Mouse dragging.** Not available. The rail is a real scroll container, so a touch swipe, a trackpad and the keyboard all work; a mouse can use the arrows and the scrollbar.
- **Scroll-controlled movement**, where the page's vertical scroll drives the cards sideways, is scroll-linked. See below.
- **Arrows** must be links pointing at each card's id. The browser then scrolls the rail itself, with no script. A button with no handler does nothing.

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
