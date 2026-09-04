---
name: section-horizontal-stack-cards
description: A separate card section that moves or stacks cards horizontally rather than vertically. Use for a scroll-controlled or draggable card run, an overlapping fan, or a horizontal stage, carrying a product story, portfolio, benefit sequence, testimonials, a process, or an image gallery. Not for vertical sticky stacking, which is the vertical stack cards section.
---

# Horizontal Stack Cards

## What you must return

One `<section>` element, and nothing else.

- No `<!doctype>`, `<html>`, `<head>` or `<body>`. The block is dropped into a page that already exists.
- No markdown code fence around it, and no commentary before or after it. Return the HTML itself. A leftover fence marker renders as visible text on the page, and inside a grid it becomes an extra item that pushes the layout out of shape.
- No `<script>`, no event handlers, no `hidden` attributes waiting to be toggled. Nothing here can run.
- No external files of any kind: no image addresses, no icon library, no web font.
- **Every icon is an inline `<svg>` written into the block.** Not a character typed in place of a mark, not an emoji, not a letter standing in for a symbol, and not a glyph from an icon font. With the font gone the character falls back to whatever the reader's machine has, which is how a card ends up headed by a stray currency sign or a hash. Draw the mark in a 24 unit box with `stroke="currentColor"`, and give it a `width` and a `height` attribute.
- Everything the section needs lives inside that one element, including its own `<style>` block. That stylesheet is expected, not a last resort. Tailwind utilities carry most of the work, but a relationship between elements cannot be written as a utility, and the ones this set depends on all live in the stylesheet: `:checked ~`, `details[open]`, `:target`, `@keyframes`, `animation-timeline` and the `prefers-reduced-motion` rule. Prefix your own class names so they cannot collide with the rest of the page.
- The section carries its own horizontal padding and its own maximum width. Drop that wrapper and the content runs to both edges of the screen.

- Color an SVG with a presentation attribute as well as a class. `<rect fill="#f6f7f9" class="fill-muted">` takes the theme color wherever the stylesheet is loaded and falls back to the attribute where it is not. With only the class, an unresolved fill computes to solid black, and the placeholder becomes a black rectangle sitting over the layout. That happens on a page without the stylesheet, and again in the moment before it arrives.
- Write `fill="none"` on any path that is stroke only. A path with no fill is filled solid black. That is the SVG default, not a fault in the browser.
- **Any colour that carries readability goes in the block's own CSS, not only in a class.** `text-white` on a dark panel is nothing without Tailwind: the ground stays dark, the words fall back to the inherited ink, and the panel reads as empty. The same goes for a light colour on an accent button. Set `color` and `background-color` in the base layer for anything where losing the stylesheet would make the text disappear, and keep the utility class as well.
- **Text laid over a picture carries its own solid ground.** A caption or a label on an image gets its own `background-color` with an alpha, not a gradient and not a separate overlay behind it. A gradient is a background image and a sibling scrim is not an ancestor, so neither is visible to a contrast check, and neither survives the picture being swapped for a pale one. Two captions measured 1.07 to 1 this way.
- Give every **icon** a `width` and a `height` attribute as well as its size classes. An SVG that carries only a `viewBox` has no size of its own, so `h-4 w-4` is the only thing holding a 16px icon at 16px. Measured in a plain HTML viewer with no Tailwind, that same icon rendered at 900 by 900 and swallowed the section around it. The attribute is the weakest thing on the page, so Tailwind still wins wherever it is loaded, and the attribute only shows through where it is not. Size any icon that repeats in the base layer as well.
- Do the opposite for an **illustration that fills its column**. An SVG set to the full width of its container takes its height from the viewBox ratio, and a `height` attribute overrides that and squashes the drawing: measured, one illustration went from 656 by 519 to 656 by 380. Leave its height free, or pair the attributes with `height:auto` in the base layer. The test is simple: a fixed size gets both attributes, a width that follows the column does not get a height.

**Write the block's own base layer.** Tailwind utilities carry the detail, but a plain HTML viewer has no Tailwind, and a block built only from utilities arrives there as unstyled text on a white page. So write the few declarations that carry the block's shape into its own stylesheet as well. The block then reads correctly in a viewer and on a real page both.

Put them inside `@layer hsc-base { }`. This matters: a layered declaration loses to an unlayered one whatever its specificity or its order, so Tailwind on a real page still wins every one of them, and the layer only shows through where there is no Tailwind. Without the layer, a rule such as `.hsc h2` would outrank `.text-4xl` and the block would start overriding the page.

Start with the five reset lines. Tailwind normally supplies these, and without them the list markers show through, the type falls back to a serif, and the content runs wider than the screen. That was measured: a first attempt without them put a block 36px past the right edge of a 1280px viewport.

    @layer hsc-base {
      .hsc,.hsc *,.hsc *::before,.hsc *::after{box-sizing:border-box}
      .hsc{font-family:ui-sans-serif,system-ui,-apple-system,"Segoe UI",Roboto,sans-serif;line-height:1.5}
      .hsc h1,.hsc h2,.hsc h3,.hsc h4,.hsc p,.hsc figure,.hsc blockquote{margin:0}
      .hsc ul,.hsc ol{margin:0;padding:0;list-style:none}
      .hsc img,.hsc svg,.hsc video{display:block;max-width:100%}

Then the block's own shape. Cover these and nothing more.

- The section: background color, text color, horizontal and vertical padding.
- The inner wrapper: maximum width, centered. Name the wrapper class you actually used, not one from this example.
- Every row, column or grid the layout depends on: display, columns, gap.
- The heading and the body copy: font size, weight, line height.
- Shapes a reader would notice at a glance: border radius, border color.

      .hsc{background-color:rgb(var(--hsc-surface));color:rgb(var(--hsc-ink));padding:5rem 1.25rem}
      .hsc .hsc-wrap{max-width:64rem;margin-inline:auto}
      .hsc .hsc-row{display:grid;gap:2.5rem}
      .hsc h2{font-size:2.25rem;line-height:1.15;font-weight:600}
      .hsc h3{font-size:1.25rem;font-weight:600}
      .hsc p{font-size:1rem;line-height:1.625}
    }

That is the skeleton, and it stays short. The detail, the spacing and the smaller responsive steps still come from the utilities.

**Then, outside the layer, declare the six colour variables.** Nothing else defines them. A block that writes `rgb(var(--hsc-surface))` without declaring `--hsc-surface` has written an invalid declaration and the browser throws the whole line away. Measured with no stylesheet, a block that skipped this had a transparent section, a card with no face, and pure black headings, quiet copy and marks. **The customer's colours go in these six values.** That is where a brief asking for a red and brown clinic actually lands, and it is the only place any colour is decided.

    .hsc{
      --hsc-surface:255 255 255;
      --hsc-band:246 247 249;
      --hsc-ink:15 23 42;
      --hsc-soft:92 107 129;
      --hsc-accent:37 99 235;
      --hsc-deep:11 18 32;
      background-color:rgb(var(--hsc-surface));
      color:rgb(var(--hsc-ink));
    }

This part is not in the layer, on purpose. Tailwind has no rule for a class name prefixed `hsc-`, so there is nothing to conflict with, and a layered rule would lose to a stray utility sitting on the same element.

**Then write out every named class the block cannot be read without.** A card or panel face, its border, its radius and its padding. Any ground that is not the section's own. Any text that is not the default ink. The accent on a mark, a rule or a button. The keyframes, the media queries that build the columns, and the reduced-motion rule. Every one of those is a utility on the markup as well, and a utility is nothing in a viewer with no Tailwind. Measured on a block that left them out: three cards with no face and no border, black marks, and all three at full width down the page instead of three across.

    .hsc .hsc-card{background-color:rgb(var(--hsc-band));border:1px solid rgb(var(--hsc-ink)/.10);
      border-radius:1rem;padding:1.5rem}
    .hsc .hsc-mark{color:rgb(var(--hsc-accent))}
    .hsc .hsc-soft{color:rgb(var(--hsc-soft))}
    @media (min-width:640px){.hsc .hsc-row{grid-template-columns:repeat(2,minmax(0,1fr))}}

Those class names are an example of the shape, not a list to copy. Name the ones this block actually contains.

**The test before you hand it over:** picture every `class` attribute deleted. What is left has to still be this block, in the customer's colours, with its faces and its columns. If it collapses to a column of black text on white, the stylesheet is too short. The exact spacing scale, the small type sizes and the hover states stay in the utilities where they belong.

**Nothing else is assumed.** The block does not expect the page to define a color, a font, a helper class or a shared stylesheet. If the page loads Tailwind the block is complete; if it does not, the base layer keeps it readable and in shape.

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
- **An icon with no size of its own.** `<svg class="h-4 w-4" viewBox="0 0 20 20">` is 16px only while Tailwind is loaded. Strip the stylesheet away and the SVG has no intrinsic size and stretches to the width of whatever contains it: measured at 900 by 900 inside a 900px section, with the rest of the block pushed off the screen. Fix it with a `width` and a `height` attribute on the tag, and a base layer rule for any icon used more than once.
- **A height attribute on an illustration that fills its column.** The reverse mistake, and it shows up on a page that has Tailwind as well as one that does not. The width follows the column while the height is pinned to the attribute, so the drawing is squashed: measured at 656 by 380 where it should have been 656 by 519. An svg whose width follows its container keeps its height free.
- **A colour variable that is used but never declared.** `rgb(var(--hsc-accent))` with no `--hsc-accent` on the block is an invalid declaration, and an invalid declaration is not a fallback to something sensible: the browser throws the line away. Measured on a block that did this, the section had no background, the cards had no face and every piece of text came out pure black, including the parts meant to carry the brand colour.
- **A card that exists only as utility classes.** `class="rounded-2xl border bg-white p-6"` is a card while Tailwind is loaded and nothing at all without it. Measured: no face, no border, no radius, no padding, and the three cards stacked at full width instead of sitting three across. Anything a reader would notice was missing goes in the block's own stylesheet as well as on the markup.

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

The block carries its own colour. Six variables are declared on the section element itself, and every
coloured part of the block reads one of them.

| Variable | Used for |
| --- | --- |
| `--hsc-surface` | card and panel faces |
| `--hsc-band` | the quiet band behind the content |
| `--hsc-ink` | headings and primary copy |
| `--hsc-soft` | supporting copy |
| `--hsc-accent` | the one emphasis colour |
| `--hsc-deep` | the dark ground |

The values are channel triplets, `15 23 42` rather than `#0f172a`, so an opacity suffix still works:


Declare all six on the block itself, and give them real values. They are not defined anywhere else, and a colour read from a variable that was never declared is an invalid declaration: the browser discards the line and that part renders with no colour at all. Where the brief names the customer's colours, those colours go into these six values and nowhere else.

    .hsc{--hsc-surface:255 255 255;--hsc-band:246 247 249;--hsc-ink:15 23 42;
      --hsc-soft:92 107 129;--hsc-accent:37 99 235;--hsc-deep:11 18 32}
`rgb(var(--hsc-ink) / .70)`.

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

A tint such as `rgb(var(--hsc-accent) / .10)` is mostly the colour behind it, so judge contrast
against the composited result, not against the accent itself.
