---
name: section-products-and-services
description: A flexible showcase for products, services, packages, or solution categories. Use when a page needs to present what the business sells, as a grid of cards, a featured item with supporting cards, a carousel, category tabs, or a comparison. Not for explaining how a single product works in depth, and not for customer outcomes.
---

# Products and Services

## What it is for

What is sold, what it costs, and how to start. Each card carries the four things a visitor needs before acting: what it is, the price, what is included, and the next step.

## When to use it

- There are two to four distinct offers, tiers or packages.
- A price or a starting price can be shown.
- The visitor is choosing between options rather than deciding whether to buy at all.

## When not to use it

- No price can be shown at all. Without a number the cards become a feature list and convert poorly.
- There are more than about five options. Group them first.
- The offers are not really comparable, in which case each deserves its own section.

## Where it belongs on the page

Upper middle, once the visitor knows what the business does. On a pricing page it is the whole page.

## What may be changed

- Number of cards, their copy, prices and inclusions.
- Which card is emphasised, and the wording of that badge.
- Whether prices are exact or starting figures.
- Card padding, borders and the section background.

## What must not be changed

- One call to action per card. Two competing buttons on a card measurably reduces the number of people who press either.
- Only one card carrying the emphasis badge. Two makes the ranking meaningless.
- Cards being equal height with the action pinned to the bottom, so a longer list on one card does not misalign the buttons.
- The reduced motion rules.

## Settings and Controls

| Setting | Options | Default |
| --- | --- | --- |
| Item count | Any number, paged or scrolled past 12 | 6 |
| Columns | 1 to 4 | 3 |
| Rows | Fixed count with the rest paged, or all rows shown | all rows shown |
| Card height | Matched across the row, or content height | matched |
| Image ratio | Square, 4:3, 3:2, 16:9, or portrait | 4:3 |
| Content density | Compact, standard, or detailed | standard |
| Show or hide | Image, icon, description, price, badge, feature list, CTA | image, description, CTA |
| Ordering and source | Manual ordering, featured item, category filtering, or automatic population from product data | manual ordering |
| Card action | Link to details, open a pop-up, expand in place, or perform a purchase action | link to details |

**Content density is what stops the section sprawling.** *Compact* is a title and a price. *Standard* adds a two-line description. *Detailed* adds a feature list. Detailed cards in four columns produce a wall of text, so drop to two or three columns when using it.

**Matched card height is the default for a reason.** Descriptions of different lengths leave a ragged bottom edge across the row, and the CTA buttons stop lining up. Match the height and push the CTA to the bottom of the card.

**Automatic population from product data changes what can be shown.** Only fields the data actually carries can be displayed. A card set to show a price will render an empty gap for any item whose price is missing, so hide the field rather than leaving it to fail item by item.

## Creative Design Options

**Layout**

- Card grid. Equal cards in a grid. The default and the safest.
- Featured item with supporting cards. One large card, the rest smaller beside or beneath it.
- Carousel. A horizontal row the visitor scrolls or drags, for long lists.
- Category tabs. Tabs above the grid filter which items show.
- Comparison layout. Items side by side against the same rows of criteria.
- Stacked cards. Cards layer as the visitor scrolls, for a small number of items.

**Card style** (pick one and hold it across every card)

Minimal, bordered, elevated, glass, gradient, image-led, icon-led, or editorial.

**Motion**

Card lift, image zoom, tilt, animated icon, hover glow, staggered entrance, or expandable detail reveal.

**Emphasis** (for marking one item as the recommended choice)

Featured-card scale, recommended badge, colored edge, spotlight gradient, or animated border.

Use one emphasis treatment, on one card. Two emphasized cards emphasize nothing.

## Motion

- **Entrance:** cards fade and rise, staggered by 60ms at `standard` intensity. Stagger across the whole grid in reading order, not column by column.
- **Hover:** pick **one** of card lift, image zoom, tilt or hover glow. Not all four. The default is card lift with an image zoom of 2 to 4 percent.
- **Animated icon** is an entrance draw or a single gentle movement, not a continuous loop.
- **Expandable detail reveal** animates height, which is expensive. Animate `opacity` and `transform` on the inner content and let the height change without a transition, or use `grid-template-rows` from `0fr` to `1fr`.
- Emphasis animation, such as an animated border, is opt in and applies to the featured card only.

## Responsive and Accessibility

| Width | Behavior |
| --- | --- |
| Desktop, 1024px and up | Full column count |
| Tablet, 768px to 1023px | Two columns. A four-column grid never goes to three here, it goes to two |
| Mobile, under 768px | One column. Carousel keeps a peek of the next card so it reads as scrollable. Comparison layout becomes stacked cards, each repeating the criteria labels |

- The whole card is the click target, with one real link inside it. Do not nest a button inside a linked card, because it produces an invalid, unreachable control.
- Card titles use a real heading level in sequence.
- Prices are readable text, not an image.
- Category filter controls are real buttons with a pressed state, and the grid announces the change.
- A carousel stops its automatic movement on hover and on focus, and can be driven from the keyboard.
- With reduced motion, staggering, tilt and any automatic carousel movement are removed. Cards appear in place.

## Defaults

Six items, three columns, matched card height, 4:3 image, standard density, image with description and CTA shown, manual ordering, cards link to details, card grid layout, elevated card style, staggered fade-and-rise entrance, card lift with image zoom on hover, no emphasis.

## What breaks this section

1. **Ragged card bottoms.** Without `h-full` and `flex-col` on the card, cards size to their own content and the CTAs no longer line up.
2. **Stacking every hover effect.** Lift plus zoom plus tilt plus glow at once reads as a bug. One primary, one supporting, no more.
3. **A button inside a linked card.** It nests an interactive control inside a link, which breaks keyboard use. Make the card a link, or the card a container with one link inside, not both.

## Color

Use the page's theme tokens. Never hardcode a hex value or a Tailwind palette color, because this section has to work on a dark editorial page and a light clinical one without being rewritten.

The tokens are `surface`, `muted`, `textdark`, `textmute`, `accent` and `bgdark`. Use them as Tailwind classes: `bg-surface`, `bg-muted`, `bg-bgdark`, `text-textdark`, `text-textmute`, `text-accent`, `border-accent`.

Do not invent CSS variable names such as `var(--color-surface)` or `var(--color-border)`. Nothing on the page defines them, so the browser throws the declaration away and that color, border or shape renders as nothing at all. Do not use `text-current/60` either, because an opacity modifier has no effect on `currentColor` and the text comes out at full strength.
