---
name: section-horizontal-stack-cards
description: A separate card section that moves or stacks cards horizontally rather than vertically. Use for a scroll-controlled or draggable card run, an overlapping fan, or a horizontal stage, carrying a product story, portfolio, benefit sequence, testimonials, a process, or an image gallery. Not for vertical sticky stacking, which is the vertical stack cards section.
---

# Horizontal Stack Cards

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

## Color

Use the page's theme tokens. Never hardcode a hex value or a Tailwind palette color, because this section has to work on a dark editorial page and a light clinical one without being rewritten.
