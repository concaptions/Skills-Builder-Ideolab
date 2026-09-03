---
name: section-expandable-content
description: A complete accordion or expandable-card section for FAQs, details, specifications, or supporting information. Use when many short answers must be scannable without filling the page, as a standard accordion, expandable cards, a split view, an image-changing accordion, or an FAQ list. Not for alternatives the visitor picks between, which is the tabbed content section.
---

# Expandable Content

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

## Color

Use the page's theme tokens. Never hardcode a hex value or a Tailwind palette color, because this section has to work on a dark editorial page and a light clinical one without being rewritten.

The tokens are `surface`, `muted`, `textdark`, `textmute`, `accent` and `bgdark`. Use them as Tailwind classes: `bg-surface`, `bg-muted`, `bg-bgdark`, `text-textdark`, `text-textmute`, `text-accent`, `border-accent`.

Do not invent CSS variable names such as `var(--color-surface)` or `var(--color-border)`. Nothing on the page defines them, so the browser throws the declaration away and that color, border or shape renders as nothing at all. Do not use `text-current/60` either, because an opacity modifier has no effect on `currentColor` and the text comes out at full strength.
