---
name: section-features-and-details
description: A deeper explanation section for capabilities, specifications, inclusions, or technical details. Use for a bento grid, a checklist beside a visual, a sticky visual with a scrolling feature list, a comparison table, or a labeled hotspot image. Not for customer outcomes, and not for listing what the business sells.
---

# Features and Details

## What it is for

The literal specification. What is included, what is not, and what the differences are between options. This is the section people scan when they are close to deciding.

## When to use it

- The offer has real, listable contents.
- The visitor is comparing tiers, packages or against a competitor.
- There are questions that only a specification answers.

## When not to use it

- The benefit is the point rather than the contents. That is a benefits section.
- There is nothing concrete to list. A features section with vague entries reads as evasive.
- The list would run past about twelve items with no grouping.

## Where it belongs on the page

Lower middle, after benefits and proof. Benefits earn the interest, features close the detail.

## What may be changed

- Number of features, the grid shape and which feature is emphasised.
- Whether a specification table appears at all, and its columns and rows.
- Icons, headings and copy.
- Whether one panel is given the dark emphasis treatment.

## What must not be changed

- The specification staying a real table with real header cells, so it can be read in order rather than as a wall of text.
- The table scrolling inside its own container. The page itself must never scroll sideways.
- The table caption, even when visually hidden.
- The reduced motion rules.

## Settings and Controls

| Setting | Options | Default |
| --- | --- | --- |
| Feature count | 3 to 12 | 6 |
| Content depth | Label only, label with description, or full specification | label with description |
| Columns | 1 to 3 | 2 |
| Image position | Left, right, above, sticky beside the list, or none | right |
| Icon usage | Icon per feature, checkmark per feature, or none | checkmark |
| Section width | Contained or full width | contained |
| Show or hide | Labels, descriptions, screenshots, specifications, tooltips, expandable details | labels, descriptions |
| Visual type | Static image, changing image, product mockup, diagram, or no visual | static image |
| Detail behavior | All details visible, or opened through tabs, accordions, or selection | all details visible |

**Content depth and column count are linked.** Full specifications in three columns produce unreadable lines. Use one column for full specifications, two for label-with-description, and three only for labels alone.

**A changing image needs one image per feature.** If the customer supplies four screenshots for six features, two features will show a stale image. Either require the full set or fall back to a static image.

**Tooltips are for units and jargon, not for hiding content.** Anything the reader needs in order to decide belongs in the visible description.

## Creative Design Options

**Layout**

- Bento grid. Cells of different sizes, so the important features get more room. Good for six to nine features.
- Checklist with visual. A ticked list one side, an image the other. The default and the safest.
- Sticky visual with feature list. The visual pins while the feature list scrolls past it, and the visual changes as each feature becomes active.
- Comparison table. Features down the side, options across the top, for tiers or models.
- Hotspot image. Numbered markers on a product photo or diagram, each opening a label.

**Motion**

Active feature follows scroll, screenshot changes by feature, checkmarks animate, or a progress line fills.

**Interaction**

Hover hotspots, click-to-expand details, selected-feature glow, or linked image and copy transitions.

**Styling**

Gradient feature cards, outlined specifications, glass panels, illuminated callouts, or dimensional device frames.

## Motion

- **Entrance:** the feature list fades and rises, staggered by 60ms at `standard` intensity. With a long list, use `subtle` so the last item is not still arriving when the reader gets there.
- **Active feature follows scroll** and **progress line fills** are scroll-linked, so they are the page's one major showcase effect. A page using the sticky-visual layout must not also carry a sticky card stack.
- **Screenshot changes by feature** crossfades. Both images sit in the same box, and the outgoing one fades out as the incoming one fades in, so the layout never jumps.
- **Checkmarks animate** once on entrance, as a draw or a scale-in. They do not repeat.
- **Hover hotspots** open on hover on a pointer device, and on tap on touch. A hotspot that only opens on hover is unreachable on a phone.

## Responsive and Accessibility

| Width | Behavior |
| --- | --- |
| Desktop, 1024px and up | Full layout as chosen. Sticky visual active |
| Tablet, 768px to 1023px | Two columns. Sticky is switched off and the visual moves above the list. Bento collapses to equal cells |
| Mobile, under 768px | One column. Comparison table becomes one block per option, each repeating the row labels. Hotspots become a numbered list beneath the image |

- Feature labels use a real heading level in sequence, or a description list where they are label-and-value pairs.
- A comparison table is a real `<table>` with `<th scope>` on the header row and column, not a grid of divs.
- Hotspot markers are real buttons, focusable and in order, with the label as accessible text.
- An expandable detail uses a button with `aria-expanded`, and the panel is not hidden from assistive technology while open.
- With reduced motion, the scroll-linked active state stops following scroll and every feature shows as active-neutral, the progress line renders full, checkmark draws are removed, and image crossfades become instant swaps.

## Defaults

Six features, label with description, two columns, checklist-with-visual layout, static image right, checkmark per feature, contained width, all details visible, outlined specification styling, staggered fade-and-rise entrance, checkmarks animating once.

## What breaks this section

1. **Swapping the image by changing `src`.** The new file loads visibly, so the box flashes empty. Stack all images and crossfade opacity.
2. **A comparison table built from divs.** It looks right and is unusable with a screen reader, because no cell knows which column it belongs to.
3. **Hotspots that only respond to hover.** Half the visitors are on a phone and can never open them.

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
