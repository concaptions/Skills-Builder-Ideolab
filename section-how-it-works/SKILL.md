---
name: section-how-it-works
description: A process section that explains steps, onboarding, delivery, or the customer journey. Use for three to six ordered steps shown horizontally, vertically, as a zigzag, a circular process, a timeline, or a sticky step-by-step story. Not for unordered lists of capabilities, where order carries no meaning.
---

# How It Works

## What it is for

Three to five steps showing what happens after the visitor acts. It removes the fear of the unknown, which is usually the real reason someone does not get in touch.

## When to use it

- The process is unfamiliar, or feels risky to a first time buyer.
- The steps are genuinely simple. If they are not, the honest answer is to simplify the process, not the copy.
- There are three to five steps. Six is already too many.

## When not to use it

- The process is obvious. Nobody needs three steps explaining how to add something to a basket.
- The steps have dates or a history. That is a timeline.
- Each step needs a paragraph of detail. Split it into its own section.

## Where it belongs on the page

Upper middle, straight after the offer is understood and before proof. It converts interest into a next action.

## What may be changed

- Number of steps, their copy and the icons or numbers used.
- Orientation, a horizontal row or a vertical list.
- Connector style, dashed, solid or absent.
- Whether a call to action closes the section.

## What must not be changed

- The list being a real ordered list, so the sequence survives without styling.
- The connector never being drawn after the last step, which otherwise implies a missing sixth step.
- Step numbers being decorative in the markup, since the ordered list already conveys order.
- The reduced motion rules.

## Settings and Controls

| Setting | Options | Default |
| --- | --- | --- |
| Step count | 3 to 6 | 3 |
| Step numbering | Numbers, letters, icons, or none | numbers |
| Direction | Horizontal, vertical, or zigzag | horizontal |
| Icon or image | Icon per step, screenshot per step, illustration per step, or none | icon |
| Content length | Title only, title with a line, or title with a paragraph | title with a line |
| Navigation behavior | Manual, scroll progression, automatic progression, or click progression | scroll progression |
| Show or hide | Step titles, descriptions, icons, screenshots, progress indicator, final CTA | titles, descriptions, icons, final CTA |
| Mobile visibility | One step at a time, or all steps | all steps |

**Three steps is the number that gets read.** Four is fine. Six is the ceiling, and at six the content length must drop to title-only or the section becomes a wall.

**Automatic progression needs a pause control.** A process that advances on its own while the reader is still on step one is taking the page away from them. It must pause on hover and on focus, and stop when the section leaves the viewport.

**Scroll progression is not the same as automatic progression.** Scroll progression is driven by the reader and needs no pause control. Automatic runs on a timer and always does.

## Creative Design Options

**Layout**

- Horizontal steps. Steps run left to right with a connector between them. The default, for three or four steps.
- Vertical steps. Steps run down the page. Better for longer descriptions and for five or six steps.
- Zigzag process. Steps alternate side to side, each with its own visual.
- Circular process. Steps arranged around a circle, for a loop with no end, such as a retainer cycle.
- Timeline. Steps on a dated or lettered spine.
- Sticky step-by-step story. The visual pins while the steps scroll past, and the visual changes with each step.

**Motion**

Connector line draws, step activates, icon pops, supporting image changes, or progress advances.

**Styling**

Numbered gradient circles, alternating color steps, illustrated scenes, card path, or roadmap treatment.

**Interaction**

Clickable steps, hover explanations, rewind, previous/next controls, or synchronized image and copy.

## Motion

- **Entrance:** steps fade and rise in order, staggered by 60ms at `standard` intensity. The stagger follows the process order, which is the point of it.
- **Connector line draws** as the reader reaches it, using a stroke dash on an SVG path. It draws once, in order, and never loops.
- **Step activates** means the current step brightens and the others dim. Change opacity and color only. Do not resize an active step, because the neighboring steps shift when it grows.
- **Icon pops** once on the step becoming active, at `subtle` intensity.
- **Supporting image changes** crossfades between stacked images, so the box never goes empty.
- **Progress advances** fills a bar or ring in step with the active step.

## Responsive and Accessibility

| Width | Behavior |
| --- | --- |
| Desktop, 1024px and up | Full layout as chosen. Connectors and sticky visuals active |
| Tablet, 768px to 1023px | Horizontal becomes two rows, or vertical. Circular becomes vertical. Connectors are dropped where they no longer point anywhere |
| Mobile, under 768px | Vertical, one column, connector running down the left edge. Sticky is switched off. If one-step-at-a-time is set, previous and next controls are shown |

- The steps are an ordered list, `<ol>`, so the order is carried in the markup and not only in the styling.
- Step numbers rendered as decoration are `aria-hidden`, because the list already numbers them.
- Clickable steps are real buttons with a pressed state, and the active step is announced.
- Previous/next controls are buttons with labels, not bare arrow glyphs.
- Connector lines and decorative paths are `aria-hidden`.
- With reduced motion, line draws render complete, icon pops are removed, automatic progression does not start, and every step shows in its resting state.

## Defaults

Three steps, numbered, horizontal, icon per step, title with a line, scroll progression, connector shown, final CTA shown, numbered gradient circles, all steps visible on mobile, staggered fade-and-rise entrance, connector draw once.

## What breaks this section

1. **A connector after the last step.** A line pointing off the edge of the section makes the process look truncated.
2. **Scaling the active step.** Its neighbors shift every time the active step changes, and the whole row jitters as the reader scrolls.
3. **Automatic progression with no pause.** The reader loses their place while reading, and there is no way to go back.

## Color

Use the page's theme tokens. Never hardcode a hex value or a Tailwind palette color, because this section has to work on a dark editorial page and a light clinical one without being rewritten.

The tokens are `surface`, `muted`, `textdark`, `textmute`, `accent` and `bgdark`. Use them as Tailwind classes: `bg-surface`, `bg-muted`, `bg-bgdark`, `text-textdark`, `text-textmute`, `text-accent`, `border-accent`.

Do not invent CSS variable names such as `var(--color-surface)` or `var(--color-border)`. Nothing on the page defines them, so the browser throws the declaration away and that color, border or shape renders as nothing at all. Do not use `text-current/60` either, because an opacity modifier has no effect on `currentColor` and the text comes out at full strength.
