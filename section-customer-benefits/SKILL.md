---
name: section-customer-benefits
description: A results-focused section that explains what the customer gains rather than only what the product does. Use for outcome statements, three pillars, benefit cards, or measurable results with proof. Not for listing product capabilities or specifications, which belong in a features section.
---

# Customer Benefits

## What it is for

What the customer gets, written as outcomes rather than features. It answers "why does this matter to me" before the visitor has to work it out themselves.

## When to use it

- The offer is easy to describe but the value is not obvious.
- The business competes against cheaper alternatives and needs to explain the difference.
- There are four to eight genuine outcomes that a customer would actually name.

## When not to use it

- The items are features, not outcomes. "Twelve engineers" is a feature. "Someone can be with you today" is a benefit.
- There are fewer than three. A row of two looks unfinished.
- The same points already appear in a features section on the same page.

## Where it belongs on the page

Upper middle, immediately after the hero or the offer. It is the first place a visitor decides whether to keep reading.

## What may be changed

- The number of benefits and the column count.
- Icons, and whether an icon appears at all.
- Alignment, centred or left, and whether each item sits in a card.
- Headline, eyebrow and introduction.

## What must not be changed

- Outcome first phrasing. The moment these become feature statements the section stops working.
- Icons being decorative and hidden from assistive technology, since the heading already carries the meaning.
- The reduced motion rules.

## Settings and Controls

| Setting | Options | Default |
| --- | --- | --- |
| Benefit count | 2 to 6 | 3 |
| Columns | 1 to 4 | 3 |
| Card sizing | Equal-height cards, content-sized cards, or one featured benefit | equal-height cards |
| Icon or image | Icon, image, outcome number, or none | icon |
| Text length | Short, standard, or detailed | standard |
| Alignment | Left, centered | left |
| Card height | Matched, or content height | matched |
| Show or hide | Eyebrow, headline, supporting copy, outcome metric, icon, proof statement, CTA | eyebrow, headline, supporting copy, icon |
| Item order | Manual, or as supplied | manual |
| Content source | Manually entered, or generated from brand data | manually entered |

**The outcome metric and the proof statement work as a pair.** The metric is the claim, such as "42 percent fewer missed calls". The proof statement is why it can be believed, such as "measured across 180 installs in 2025". Showing a metric with no proof invites the reader to discount it.

**One featured benefit changes the grid.** The featured item takes a larger cell and the others fill around it, so the count must suit the shape. Three or five items work. Four leaves a hole.

## Creative Design Options

**Layout**

- Three pillars. Three equal columns, each with an icon, a title and a line. The default.
- Icon grid. A denser grid for five or six shorter benefits.
- Outcome cards. Each card leads with the number rather than the icon.
- Before-and-after outcomes. Each benefit is stated as a pair, the situation now and the situation after.
- Central visual with surrounding benefits. One image or diagram in the middle, benefits arranged around it.
- Scrolling cards. Benefits move past horizontally, for longer lists.

**Visual treatment**

Individual card gradients, icon containers, connecting lines, background glow, or alternating color bands.

**Motion**

Sequential benefit reveal, animated checkmarks, count-up results, connecting-line draw, or hover spotlight.

**Typography**

Gradient result words, large outcome statements, color-stepped lines, or highlighted proof phrases.

## Motion

- **Entrance:** sequential benefit reveal, which is a fade and rise staggered by 60ms in reading order, at `standard` intensity.
- **Count-up results** run once, when the metric first enters view, over roughly 1 to 1.5 seconds. The final value must be present in the markup so it is readable if the script never runs, and the element carries `aria-live="off"` so a screen reader is not read a stream of changing digits.
- **Animated checkmarks** draw once on entrance. They do not repeat.
- **Connecting-line draw** is scroll-linked, so it counts as the page's one major showcase effect.
- **Hover spotlight** is a background tint or glow on the hovered card, at `subtle` intensity. It never moves the card and the metric at the same time.

## Responsive and Accessibility

| Width | Behavior |
| --- | --- |
| Desktop, 1024px and up | Full column count. Connecting lines and central-visual layouts render as designed |
| Tablet, 768px to 1023px | Two columns. Connecting lines are dropped, since they only read horizontally |
| Mobile, under 768px | One column. Central visual moves above the benefits. Scrolling cards keep a peek of the next card |

- Benefit titles use a real heading level in sequence.
- A count-up number is real text. Never render a metric as an image.
- Proof statements sit next to the claim they support, not in a footnote at the bottom of the section.
- Connecting lines and decorative glows are `aria-hidden`.
- With reduced motion, count-ups show the final value immediately, checkmark draws and line draws are removed, and the stagger is dropped.

## Defaults

Three benefits, three columns, equal-height cards, icon shown, standard text length, left aligned, eyebrow and headline and supporting copy on, three-pillars layout, icon containers, sequential reveal at standard intensity, hover spotlight off.

## What breaks this section

1. **Writing features as benefits.** "Automated SMS replies" is a feature. "Nobody waits for a callback" is the benefit. If the line does not say what the reader gets, it is in the wrong section.
2. **Starting a count-up from an empty element.** The number is missing until the script runs, and missing forever for anyone whose script fails. Put the final value in the markup.
3. **A metric with no proof.** An unsupported percentage reads as marketing and lowers trust in the rest of the page.

## Images

Every image belongs to the customer. Use the photographs, logos and artwork supplied in the brief.

Never pull a photograph from a stock site. A link to Unsplash, Pexels, Shutterstock or any other outside address is wrong on two counts: it is not the customer's business in the picture, and the block stops working the moment that address changes.

If no photograph has been supplied yet, keep the placeholder that ships inside the block. It is drawn in the page itself, so it needs nothing from outside, it takes the theme colors, and it holds the right shape so the layout does not move when the real photograph arrives.

Give every image an `alt` description of what is in it, and a `width` and `height`, so the page does not jump as it loads.

## Color

Use the page's theme tokens. Never hardcode a hex value or a Tailwind palette color, because this section has to work on a dark editorial page and a light clinical one without being rewritten.

The tokens are `surface`, `muted`, `textdark`, `textmute`, `accent` and `bgdark`. Use them as Tailwind classes: `bg-surface`, `bg-muted`, `bg-bgdark`, `text-textdark`, `text-textmute`, `text-accent`, `border-accent`.

Do not invent CSS variable names such as `var(--color-surface)` or `var(--color-border)`. Nothing on the page defines them, so the browser throws the declaration away and that color, border or shape renders as nothing at all. Do not use `text-current/60` either, because an opacity modifier has no effect on `currentColor` and the text comes out at full strength.

A token is not readable just because it is a token. `text-textdark` on `bg-bgdark` is near black on near black, measured at 1.05 to 1, and the words simply are not there. `text-textmute` on `bg-bgdark` reaches 3.93 to 1, which is still short.

On a dark panel, body text is `text-surface`. Use a lowered opacity of `text-surface` for the quieter line rather than reaching for `text-textmute`. Every piece of text has to reach 4.5 to 1 against the color actually behind it, which is the nearest ancestor that paints a background, not the page.
