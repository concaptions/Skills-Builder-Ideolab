---
name: section-photo-gallery
description: A complete visual gallery section for brand photography, portfolio work, locations, products, or events. Use for a grid, masonry, collage, carousel, filmstrip, full-screen gallery, or an alternating-size editorial layout, with optional category filters and a lightbox. Not for a single hero image, and not for product cards that need a price and a call to action.
---

# Photo Gallery

## What it is for

Proof by showing rather than telling. Finished work, the premises, the team, an event. The pictures carry the persuasion and the copy stays out of the way.

## When to use it

- The business sells on visible craft or transformation: trades, interiors, salons, restaurants, venues, photography, events.
- There are at least six real photographs available. Below six a gallery looks thin, and an image and text section serves better.
- The visitor's real question is "what does their work actually look like".

## When not to use it

- Only one strong image exists. That is a hero or an image and text section.
- The items need a price, a specification or a buy action. That is a products and services section.
- The photographs are generic stock. A gallery of stock images actively reduces trust.
- Twenty or more images with no filter and no expansion control. Add a filter, a carousel or a show more control first.

## Where it belongs on the page

Middle of the page, after the offer is understood and before the closing call to action. It works well directly after a services or benefits section, because it evidences the claim those sections just made. It should not be the first thing under the hero, since a visitor who does not yet know what the business does has no reason to study photographs.

## What may be changed

- Image count, column count, gap, crop ratio and the order of items.
- Category names and how many categories exist, including removing filtering entirely.
- Headline, introduction copy and captions.
- Layout, swapping the grid for masonry, collage, carousel, filmstrip, full-screen or an alternating editorial rhythm.
- Corner radius, border treatment and shadow, through the page's own styling.
- Colors, which come from the page theme rather than from this section.

## What must not be changed

- The fixed frame around each image. The image scales inside the frame on hover, the frame itself never resizes, otherwise the grid reflows and every other tile jumps.
- The `width` and `height` attributes on each image. Removing them makes the page shift as images load.
- Alt text. Every image needs a description of what it shows, and a purely decorative image takes an empty alt attribute.
- The identifier prefix, unless it is changed consistently. The filters are driven by ids, so two copies of this block on one page need different prefixes.
- The reduced motion rules.

## Settings and Controls

| Setting | Options | Default |
| --- | --- | --- |
| Image count | Any number, with an expansion control past the visible limit | 9 |
| Columns | 2 to 5 | 3 |
| Gap | None, tight, standard, or generous | standard |
| Crop ratio | Square, 4:3, 3:2, 16:9, portrait, or original per image | square |
| Captions | Always visible, on hover, in the lightbox only, or hidden | hidden |
| Ordering | Manual, newest first, or as supplied | manual |
| Image source | Manual images, brand gallery collection, project collection, or filtered category | manual images |
| Show or hide | Headline, introduction, captions, category filters, arrows, dots, lightbox | headline, filters |
| Loading order | All at once, or lazy below the fold | lazy below the fold |
| Image quality | Standard, or high for detail work | standard |
| Maximum visible | How many show before a show more control | 9 |
| Mobile behavior | Grid, carousel, or single column | two column grid |

**Crop ratio set to original per image is what breaks grids.** A grid of mixed ratios leaves ragged rows. Either fix one ratio for the grid, or use masonry, which is built for mixed heights.

**Lazy loading below the fold is the default and should stay on.** A twenty image gallery loading eagerly is the single heaviest thing on most pages. The first row loads eagerly so the section is never empty on arrival, everything after it loads lazily.

**Captions on hover are invisible on touch.** If the caption carries information, set it to always visible.

## Creative Design Options

**Layout**

- Grid. Equal cells, one ratio. The default and the safest.
- Masonry. Columns of varying heights, for mixed ratios.
- Collage. A deliberate arrangement of different sizes, for a small curated set.
- Carousel. A horizontal row the visitor drags or scrolls.
- Filmstrip. A single low row of thumbnails, often above a larger active image.
- Full-screen gallery. Each image takes the viewport.
- Alternating-size editorial layout. Large and small images alternating down the page.

**Motion**

Staggered reveal, hover zoom, directional pan, tilt, auto-scroll, or scroll-speed variation.

**Interaction**

Lightbox, drag, swipe, category filters, image expansion, or caption reveal.

**Styling**

Rounded cards, edge-to-edge photography, layered frames, film border, gradient caption, or soft image shadows.

## Motion

- **Entrance:** a staggered rise at 40ms per item. A gallery has many items, and a 60ms stagger across twelve images means the last one arrives most of a second late.
- **Hover:** a zoom of 2 to 4 percent inside a fixed frame, so the layout does not move. This is the default.
- **Tilt** stays small and applies to the image only, never to the caption.
- **Auto-scroll and scroll-speed variation** are continuous, so they must pause on hover, pause on focus, and stop when the section leaves the viewport. Without a script none of that is possible, so avoid them in a static build.
- **Directional pan** moves the image slowly inside its frame. Bound it so it does not run indefinitely.

## Responsive and Accessibility

| Width | Behavior |
| --- | --- |
| Desktop, 1024px and up | Full column count, filters shown as a row |
| Tablet, 768px to 1023px | Two or three columns. Collage becomes a grid |
| Mobile, under 768px | Two columns or a single column. Full-screen gallery becomes one image per screen with swipe |

- Every image has alt text describing what it shows. A purely decorative image takes an empty alt attribute.
- Category filters are real form controls with a visible selected state and a visible focus ring, so they work by keyboard.
- Images carry `width` and `height` so the page does not shift as they load.
- With reduced motion the entrance, the hover zoom and any pan are all removed.
- A lightbox needs focus trapping, an escape key handler and focus return. None of that can be done without a script, so a static build should leave the lightbox out rather than ship a version that traps keyboard users.

## Defaults

Nine images, three columns, standard gap, square crop, captions hidden, manual order, headline and introduction shown, category filters on, lightbox off, lazy loading below the first row, grid layout, rounded cards, two columns on mobile, staggered rise at 40ms, hover zoom of 5 percent.

## What breaks this section

1. **Zooming the frame instead of the image.** The cell grows, the grid reflows, and every other image jumps.
2. **Mixed ratios in a fixed grid.** Rows end ragged. Use masonry for mixed ratios, or crop to one ratio.
3. **Duplicate identifiers.** The filters are driven by ids. Two copies of this block on one page with the same prefix means clicking a filter in one gallery silently filters the other.
4. **A lightbox without focus handling.** Tab moves behind the overlay onto the page underneath and there is no way back.

## What a static block cannot do

These blocks carry no JavaScript. A few of the options above have no static equivalent, so build the nearest thing that does work and leave the rest out. Never ship a control that looks alive and does nothing when it is pressed.

- **Mouse dragging a carousel.** Not available, for the same reason as any scroll rail. Touch and trackpad work. The lightbox uses `:target` and needs no script.

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
