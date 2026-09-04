---
name: section-products-and-services
description: A flexible showcase for products, services, packages, or solution categories. Use when a page needs to present what the business sells, as a grid of cards, a featured item with supporting cards, a carousel, category tabs, or a comparison. Not for explaining how a single product works in depth, and not for customer outcomes.
---

# Products and Services

## What you must return

One `<section>` element, and nothing else.

- No `<!doctype>`, `<html>`, `<head>` or `<body>`. The block is dropped into a page that already exists.
- No markdown code fence around it, and no commentary before or after it. Return the HTML itself. A leftover fence marker renders as visible text on the page, and inside a grid it becomes an extra item that pushes the layout out of shape.
- No `<script>`, no event handlers, no `hidden` attributes waiting to be toggled. Nothing here can run.
- No external files of any kind: no image addresses, no icon library, no web font.
- Everything the section needs lives inside that one element, including its own `<style>` block. That stylesheet is expected, not a last resort. Tailwind utilities carry most of the work, but a relationship between elements cannot be written as a utility, and the ones this set depends on all live in the stylesheet: `:checked ~`, `details[open]`, `:target`, `@keyframes`, `animation-timeline` and the `prefers-reduced-motion` rule. Prefix your own class names so they cannot collide with the rest of the page.
- The section carries its own horizontal padding and its own maximum width. Drop that wrapper and the content runs to both edges of the screen.

- Color an SVG with a presentation attribute as well as a class. `<rect fill="#f6f7f9" class="fill-muted">` takes the theme color wherever the stylesheet is loaded and falls back to the attribute where it is not. With only the class, an unresolved fill computes to solid black, and the placeholder becomes a black rectangle sitting over the layout. That happens on a page without the stylesheet, and again in the moment before it arrives.
- Write `fill="none"` on any path that is stroke only. A path with no fill is filled solid black. That is the SVG default, not a fault in the browser.

**Write the block's own base layer.** Tailwind utilities carry the detail, but a plain HTML viewer has no Tailwind, and a block built only from utilities arrives there as unstyled text on a white page. So write the few declarations that carry the block's shape into its own stylesheet as well. The block then reads correctly in a viewer and on a real page both.

Put them inside `@layer pns-base { }`. This matters: a layered declaration loses to an unlayered one whatever its specificity or its order, so Tailwind on a real page still wins every one of them, and the layer only shows through where there is no Tailwind. Without the layer, a rule such as `.pns h2` would outrank `.text-4xl` and the block would start overriding the page.

Start with the five reset lines. Tailwind normally supplies these, and without them the list markers show through, the type falls back to a serif, and the content runs wider than the screen. That was measured: a first attempt without them put a block 36px past the right edge of a 1280px viewport.

    @layer pns-base {
      .pns,.pns *,.pns *::before,.pns *::after{box-sizing:border-box}
      .pns{font-family:ui-sans-serif,system-ui,-apple-system,"Segoe UI",Roboto,sans-serif;line-height:1.5}
      .pns h1,.pns h2,.pns h3,.pns h4,.pns p,.pns figure,.pns blockquote{margin:0}
      .pns ul,.pns ol{margin:0;padding:0;list-style:none}
      .pns img,.pns svg,.pns video{display:block;max-width:100%}

Then the block's own shape. Cover these and nothing more.

- The section: background color, text color, horizontal and vertical padding.
- The inner wrapper: maximum width, centered. Name the wrapper class you actually used, not one from this example.
- Every row, column or grid the layout depends on: display, columns, gap.
- The heading and the body copy: font size, weight, line height.
- Shapes a reader would notice at a glance: border radius, border color.

      .pns{background-color:rgb(var(--pns-surface));color:rgb(var(--pns-ink));padding:5rem 1.25rem}
      .pns .pns-wrap{max-width:64rem;margin-inline:auto}
      .pns .pns-row{display:grid;gap:2.5rem}
      .pns h2{font-size:2.25rem;line-height:1.15;font-weight:600}
      .pns h3{font-size:1.25rem;font-weight:600}
      .pns p{font-size:1rem;line-height:1.625}
    }

About twelve declarations in all. Do not try to restate every utility you used. This is the block's skeleton, not a second copy of Tailwind. The exact spacing scale, the small type sizes and the hover states stay in the utilities where they belong.

**Nothing else is assumed.** The block does not expect the page to define a color, a font, a helper class or a shared stylesheet. If the page loads Tailwind the block is complete; if it does not, the base layer keeps it readable and in shape.

This skill is self-contained. Do not read, reference or depend on any other skill, and do not assume that a shared stylesheet, a motion system, a component library or a set of helper classes exists somewhere. If something is not described here, it is not available.

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

## What a static block cannot do

These blocks carry no JavaScript. A few of the options above have no static equivalent, so build the nearest thing that does work and leave the rest out. Never ship a control that looks alive and does nothing when it is pressed.

- **Mouse dragging a carousel.** Not available. Touch and trackpad work. Category tabs are a radio group and need no script.

## Images

Every image belongs to the customer. Use the photographs, logos and artwork supplied in the brief.

Never pull a photograph from a stock site. A link to Unsplash, Pexels, Shutterstock or any other outside address is wrong on two counts: it is not the customer's business in the picture, and the block stops working the moment that address changes.

If no photograph has been supplied yet, keep the placeholder that ships inside the block. It is drawn in the page itself, so it needs nothing from outside, it takes the theme colors, and it holds the right shape so the layout does not move when the real photograph arrives.

Give every image an `alt` description of what is in it, and a `width` and `height`, so the page does not jump as it loads.

## Color

The block carries its own colour. Six variables are declared on the section element itself, and every
coloured part of the block reads one of them.

| Variable | Used for |
| --- | --- |
| `--pns-surface` | card and panel faces |
| `--pns-band` | the quiet band behind the content |
| `--pns-ink` | headings and primary copy |
| `--pns-soft` | supporting copy |
| `--pns-accent` | the one emphasis colour |
| `--pns-deep` | the dark ground |

The values are channel triplets, `15 23 42` rather than `#0f172a`, so an opacity suffix still works:
`rgb(var(--pns-ink) / .70)`.

Rebrand the block by changing those six values, in one place. Do not spread hex values through the
markup, and do not reach for a colour name out of the page's Tailwind config. Every page the builder
makes writes its own config with its own colour names, so a name taken from one page resolves to
nothing on the next. Measured on two live sites: `bg-surface` painted no background on either, and
`muted` was a pale background on one and a dark text colour on the other, which turned a light band
into a dark one with dark text on it.

Do not invent CSS variable names such as `var(--color-surface)` or `var(--color-border)`. Nothing
defines them, the browser throws the declaration away, and that colour, border or shape renders as
nothing at all. Do not use `text-current/60` either, because an opacity modifier has no effect on
`currentColor` and the text comes out at full strength.

A colour is not readable just because it came from a variable. Every piece of text has to reach 4.5 to
1 against the colour actually behind it, which is the nearest ancestor that paints a background rather
than the page. Large text, meaning 24px, or 18.66px when it is bold, needs 3 to 1.

With the default values these four pairs fail, so do not reach for them.

| Text | Background | Measured |
| --- | --- | --- |
| ink | deep | 1.05 to 1, near black on near black |
| accent | deep | 3.62 to 1, the accent is a dark blue and it sinks into a dark panel |
| soft | deep | 3.93 to 1 |
| soft | band | 4.44 to 1, although the same value clears on surface at 4.76 to 1 |

On a dark panel, set text in the surface value and use a lowered opacity of it for the quieter line.
White on the accent measures 5.17 to 1, so the accent still works there as a button background. On the
band, use a lowered opacity of ink for quiet copy.

A tint such as `rgb(var(--pns-accent) / .10)` is mostly the colour behind it, so judge contrast
against the composited result, not against the accent itself.
