---
name: section-expandable-content
description: A complete accordion or expandable-card section for FAQs, details, specifications, or supporting information. Use when many short answers must be scannable without filling the page, as a standard accordion, expandable cards, a split view, an image-changing accordion, or an FAQ list. Not for alternatives the visitor picks between, which is the tabbed content section.
---

# Expandable Content

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

Questions and answers, or any long content that most visitors do not need but some must have. It keeps the page short without hiding anything.

## When to use it

- There are real, recurring questions from actual customers.
- The page must carry detail such as terms, coverage or process without becoming unreadable.
- Five to ten items. Beyond about twelve, split into groups with headings.

## When not to use it

- Something everybody needs to read. Do not hide the price or the guarantee behind a click.
- There are only two items. Just write them out.
- The questions are invented marketing copy rather than things people ask.

## Where it belongs on the page

Low on the page, above the final call to action. It is the last objection handler before the visitor decides.

## What may be changed

- Question wording, answer copy and the number of items.
- Whether one item starts open.
- Whether the group allows several open at once, or only one.
- Marker style, borders and spacing.

## What must not be changed

- The native details and summary elements. They already give keyboard operation, screen reader announcement and open state with no script, and every hand built replacement loses at least one of those.
- The grid rows technique for the slide open. Height auto cannot be transitioned.
- A visible focus ring on each summary.
- The reduced motion rules.

## Settings and Controls

| Setting | Options | Default |
| --- | --- | --- |
| Item count | 3 to 20 | 6 |
| Default open item | None, the first, or a named item | none |
| Open behavior | Single open, or multiple open | multiple open |
| Content types | Text, list, image, video, link, or a mix | text |
| Show or hide | Icon, number, image, supporting text, link, CTA | icon, CTA below the list |
| Click target | The whole row, or the title only | the whole row |
| Open direction | Downward, or as an overlay panel | downward |
| Content height | Animated, or instant | animated |
| Mobile spacing | Compact, standard, or generous | standard |
| Supporting media | Opening an item changes a shared image or the background, or nothing | nothing |

**Single open is the wrong default for a FAQ.** A reader comparing two answers has to keep reopening them. Use single open only when the panels are long enough that two open at once is unreadable.

**Write the questions in the customer's words.** "How much does it cost" beats "Pricing information". The closed row is what gets scanned, so it carries the whole burden of being found.

**Do not hide anything essential in an accordion.** Price, availability and what is not included belong in the open page. An accordion is for the second question, not the first.

**A FAQ accordion should carry FAQ structured data** so the answers can appear in search results, and the markup must match the visible text exactly.

## Creative Design Options

**Layout**

- Standard accordion. Rows separated by lines. The default and the most scannable.
- Expandable cards. Each item is a card that grows when opened.
- Split-view accordion. Questions one side, the open answer the other.
- Image-changing accordion. Opening an item changes a shared image beside the list.
- FAQ list. Question and answer pairs with a lighter frame.

**Motion**

Smooth expansion, icon rotation, content fade, card lift, or supporting-image transition.

**Styling**

Minimal lines, bordered cards, colored questions, glass panels, or alternating backgrounds.

**Interaction**

Search, category filters, open all, deep-link to an item, or animated emphasis on a selected answer.

## Motion

- **Smooth expansion** uses `grid-template-rows` from `0fr` to `1fr` on a wrapper, which transitions cleanly without measuring anything. Do not animate `height: auto`, which does not transition at all.
- **Icon rotation** is 180 degrees for a chevron, or a plus turning into a minus, over 200ms.
- **Content fade** runs with the expansion, not after it, so nothing appears to lag.
- **Card lift** on hover is `subtle` and applies to the closed state only. An open card does not lift.
- **Supporting-image transition** crossfades stacked images so the box never empties.
- Expansion is 200 to 250ms. The reader has just clicked and is waiting, so this is one of the few places where faster is better.

## Responsive and Accessibility

| Width | Behavior |
| --- | --- |
| Desktop, 1024px and up | Full layout as chosen. Split view and image-changing active |
| Tablet, 768px to 1023px | Split view becomes a standard accordion with the image above |
| Mobile, under 768px | Standard accordion, full width, larger tap targets, generous spacing between rows |

- Each row is a `<button aria-expanded>` inside a heading of the right level, and the button controls the panel with `aria-controls`.
- The question text is inside the button, so it is announced with the state.
- The chevron or plus icon is `aria-hidden`, because `aria-expanded` already carries the state.
- The tap target is at least 44 by 44 pixels on touch.
- Deep-linking to an item opens it, scrolls to it, and moves focus to its button.
- Search and category filters are real form controls, and the result count is announced.
- With reduced motion, panels open and close instantly, icon rotation is removed, and card lift is removed.

## Defaults

Six items, none open on load, multiple open, text content, icon shown, whole row clickable, downward, animated height, standard spacing, no supporting media, standard accordion layout, minimal lines styling, 220ms expansion with chevron rotation.

## What breaks this section

1. **Animating `height: auto`.** It does not transition, so the panel snaps open with no animation at all.
2. **Leaving out `overflow: hidden` on the inner wrapper.** The answer is visible through the closed row, which looks like a rendering fault.
3. **A div with a click handler instead of a button.** The row cannot be opened by keyboard, and nothing announces that it expands.

## What a static block cannot do

These blocks carry no JavaScript. A few of the options above have no static equivalent, so build the nearest thing that does work and leave the rest out. Never ship a control that looks alive and does nothing when it is pressed.

- **The image-changing accordion** only works if the panels are a radio group rather than `details` elements, because a sibling selector can then reach the image. With `details` the image cannot react to which panel is open.

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
