---
name: section-vertical-stack-cards
description: A dedicated section where cards layer vertically as the visitor scrolls through the story. Use for two to four items of equal weight, such as product pillars, service tiers, process phases or differentiators, where each item needs real space and the reader should meet them one at a time. Not for lists of five or more, and not where items must be compared side by side.
---

# Vertical Stack Cards

## What you must return

One `<section>` element, and nothing else.

**Five things to check before you hand the block back.** Each one has been the entire reason a delivered block was broken.

1. The six colour variables are declared on the block, with values. Nothing else in the world defines them, and a colour read from a variable that does not exist is an invalid declaration the browser throws away.
2. Every face, ground, border and text colour is written into the block's own stylesheet, not left to a utility class alone.
3. Every icon carries a `width` and a `height` attribute as well as its size classes.
4. No `<script>`, no external file, no markdown fence, no page wrapper.
5. Read it back once with every `class` attribute imagined away. What is left still has to be this block, in the customer's colours.

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

Put them inside `@layer vsc-base { }`. This matters: a layered declaration loses to an unlayered one whatever its specificity or its order, so Tailwind on a real page still wins every one of them, and the layer only shows through where there is no Tailwind. Without the layer, a rule such as `.vsc h2` would outrank `.text-4xl` and the block would start overriding the page.

Start with the five reset lines. Tailwind normally supplies these, and without them the list markers show through, the type falls back to a serif, and the content runs wider than the screen. That was measured: a first attempt without them put a block 36px past the right edge of a 1280px viewport.

    @layer vsc-base {
      .vsc,.vsc *,.vsc *::before,.vsc *::after{box-sizing:border-box}
      .vsc{font-family:ui-sans-serif,system-ui,-apple-system,"Segoe UI",Roboto,sans-serif;line-height:1.5}
      .vsc h1,.vsc h2,.vsc h3,.vsc h4,.vsc p,.vsc figure,.vsc blockquote{margin:0}
      .vsc ul,.vsc ol{margin:0;padding:0;list-style:none}
      .vsc img,.vsc svg,.vsc video{display:block;max-width:100%}

Then the block's own shape. Cover these and nothing more.

- The section: background color, text color, horizontal and vertical padding.
- The inner wrapper: maximum width, centered. Name the wrapper class you actually used, not one from this example.
- Every row, column or grid the layout depends on: display, columns, gap.
- The heading and the body copy: font size, weight, line height.
- Shapes a reader would notice at a glance: border radius, border color.

      .vsc{background-color:rgb(var(--vsc-surface));color:rgb(var(--vsc-ink));padding:5rem 1.25rem}
      .vsc .vsc-wrap{max-width:64rem;margin-inline:auto}
      .vsc .vsc-row{display:grid;gap:2.5rem}
      .vsc h2{font-size:2.25rem;line-height:1.15;font-weight:600}
      .vsc h3{font-size:1.25rem;font-weight:600}
      .vsc p{font-size:1rem;line-height:1.625}
    }

That is the skeleton, and it stays short. The detail, the spacing and the smaller responsive steps still come from the utilities.

**Then, outside the layer, declare the six colour variables.** Nothing else defines them. A block that writes `rgb(var(--vsc-surface))` without declaring `--vsc-surface` has written an invalid declaration and the browser throws the whole line away. Measured with no stylesheet, a block that skipped this had a transparent section, a card with no face, and pure black headings, quiet copy and marks. **The customer's colours go in these six values.** That is where a brief asking for a red and brown clinic actually lands, and it is the only place any colour is decided.

    .vsc{
      --vsc-surface:255 255 255;
      --vsc-band:246 247 249;
      --vsc-ink:15 23 42;
      --vsc-soft:92 107 129;
      --vsc-accent:37 99 235;
      --vsc-deep:11 18 32;
      background-color:rgb(var(--vsc-surface));
      color:rgb(var(--vsc-ink));
    }

This part is not in the layer, on purpose. Tailwind has no rule for a class name prefixed `vsc-`, so there is nothing to conflict with, and a layered rule would lose to a stray utility sitting on the same element.

**Then write out every named class the block cannot be read without.** A card or panel face, its border, its radius and its padding. Any ground that is not the section's own. Any text that is not the default ink. The accent on a mark, a rule or a button. The keyframes, the media queries that build the columns, and the reduced-motion rule. Every one of those is a utility on the markup as well, and a utility is nothing in a viewer with no Tailwind. Measured on a block that left them out: three cards with no face and no border, black marks, and all three at full width down the page instead of three across.

    .vsc .vsc-card{background-color:rgb(var(--vsc-band));border:1px solid rgb(var(--vsc-ink)/.10);
      border-radius:1rem;padding:1.5rem}
    .vsc .vsc-mark{color:rgb(var(--vsc-accent))}
    .vsc .vsc-soft{color:rgb(var(--vsc-soft))}
    @media (min-width:640px){.vsc .vsc-row{grid-template-columns:repeat(2,minmax(0,1fr))}}

Those class names are an example of the shape, not a list to copy. Name the ones this block actually contains.

**The test before you hand it over:** picture every `class` attribute deleted. What is left has to still be this block, in the customer's colours, with its faces and its columns. If it collapses to a column of black text on white, the stylesheet is too short. The exact spacing scale, the small type sizes and the hover states stay in the utilities where they belong.

**Nothing else is assumed.** The block does not expect the page to define a color, a font, a helper class or a shared stylesheet. If the page loads Tailwind the block is complete; if it does not, the base layer keeps it readable and in shape.

This skill is self-contained. Do not read, reference or depend on any other skill, and do not assume that a shared stylesheet, a motion system, a component library or a set of helper classes exists somewhere. If something is not described here, it is not available.

## What it is for

Cards that come to rest on top of each other as the page scrolls, like a pile of paper. It gives a short sequence weight and pace without spending the page height a normal list would.

## When to use it

- There are three to five substantial points that deserve emphasis.
- Each card holds a heading and a short paragraph rather than a long body.
- The order is meaningful and the last card is the one to remember.

## When not to use it

- There are more than about five cards. The pile becomes a shuffle nobody follows.
- The cards carry long content. Anything taller than the viewport breaks the effect.
- The content is reference material people will want to scan. A plain list is better.

## Where it belongs on the page

Middle of the page. It is a pacing device, so it should sit between two calmer sections rather than next to another heavy interaction.

## What may be changed

- Number of cards, their copy and their accent colours.
- The top offset and how far each card steps down from the one before.
- Card padding, radius and border.
- Whether the pile applies on tablet or only on desktop.

## What must not be changed

- Opaque card faces. A translucent card lets the one underneath show through and the pile turns to mud.
- No bottom margin on the sticky wrapper. A margin ends the stick early and the pile falls apart.
- No ancestor with overflow hidden, because position sticky stops working inside one.
- The pile collapsing to a plain list on small screens and under reduced motion.

## Settings and Controls

| Setting | Options | Default |
| --- | --- | --- |
| Card count | 2 to 4 | 3 |
| Card height | Matched across all cards, or content height | matched |
| Stack offset | Vertical distance each card is offset from the one below | 0, cards land flush |
| Scroll distance | How much scroll each card holds before the next arrives | 100vh per card |
| Sticky position | How far from the top of the viewport the cards settle | 18vh |
| Content fields | Eyebrow, headline, description, bullet list, summary band | headline, description, bullets |
| Show or hide, per card | Image, icon, number, headline, description, progress, and CTA per card | image, headline, description on |
| Mobile and reduced-motion fallback | Click, swipe, or static cards | static cards |
| Card persistence | Cards remain visible in the stack, or fully replace one another | remain visible |

**Card persistence changes the build.** *Remain visible* means each card is opaque and simply covers the last, so a sliver of the previous card can be left showing through the stack offset. *Fully replace* means the outgoing card fades out as the incoming one arrives, leaving nothing behind it.

**Matched card height is not cosmetic.** If cards differ in height, a shorter card cannot fully cover the taller one behind it and the reader sees the previous card's edge poking out. Every card must share the same minimum height.

**Set that height from the content, not from a fixed number.** `34rem` suits a card with a heading, a paragraph, three or four bullets and a summary band. Cards carrying only a heading and one paragraph should drop to around `26rem` rather than being padded with filler. The rule is that all cards match each other, not that they all match `34rem`.

## Creative Design Options

**Layout**

- Top-aligned stack. Cards pin near the top of the viewport. The default and the safest.
- Centered stack. Cards pin at the vertical center, which suits shorter cards.
- Alternating offset. Each card sits slightly left or right of the one below it.
- Full-screen stack. Each card fills the viewport.
- Image-and-copy stack. Each card is split, text one side and image the other.

**Motion**

Cards layer, scale, rotate, fade, blur, or compress as the next card arrives.

Pick one and hold it across the whole section. See the Motion notes below for how each is built.

**Story treatment**

Background color or image changes with each card; progress can track the active card. The background change moves the page itself through the story. The progress indicator can be dots, a bar, or a numbered counter.

**Card style** (pick one and hold it across all cards)

Editorial, image-led, glass, bold gradient, browser mockup, testimonial, or product card.

## Motion

- **Scroll-linked:** the sticky stack. This is a *major showcase effect*, so the page must not also carry a scroll-driven story or a parallax hero.
- **Entrance:** each card fades and rises on first appearance, at `standard` intensity.
- **Hover:** card images take the default 2 to 4 percent zoom. The card itself does not lift, because it is already pinned.

How each stacking motion is built:

| Motion | Build |
| --- | --- |
| Layer | Sticky alone. The arriving card simply covers the last. No extra work |
| Scale | The covered card scales down slightly as the next arrives |
| Rotate | The covered card takes a small rotation, one or two degrees |
| Fade | The covered card drops in opacity. This is the *fully replace* persistence mode |
| Blur | The covered card takes a small `filter: blur`. Costly on mobile, keep the radius low |
| Compress | The covered card loses height, so the stack squeezes together |

Scale, rotate, fade, blur and compress all need the covered card's state to follow scroll position, so each is scroll-linked and carries the support caveat below. Plain layering is pure CSS and needs nothing. **Prefer layering** unless the design specifically calls for another.

Do not add tilt, glow or a border draw here. The stacking is the effect.

## Responsive and Accessibility

| Width | Behavior |
| --- | --- |
| Desktop, 1024px and up | Sticky stack active. Cards pin at the offset and cover in order |
| Tablet and mobile, under 1024px | **Sticky is switched off.** The chosen fallback applies: static cards flowing one after another, or a click or swipe carousel |

Turning sticky off below 1024px is required, not optional. A card pinned at 18vh on a phone leaves almost no room to read it, and on iOS a sticky element inside a scrolling container behaves inconsistently.

- Cards are in reading order in the markup, so the effect matches the tab order.
- Buttons and links inside a card remain reachable while that card is pinned.
- With reduced motion, **the stack still works.** The reader is driving it with their own scroll, so nothing plays at them. Remove the entrance fade and any scale, rotate, blur or compress, and keep plain layering.
- Card headings use a real heading level in sequence, not styled text.

## Defaults

Three cards, image-and-copy stack layout, matched height, editorial card style, pinned at 18vh, layer motion, cards remain visible, no progress indicator, sticky above 1024px only with static cards below, standard entrance, image hover zoom.

## What breaks this section

1. **A bottom margin on the sticky wrapper.** It shortens the distance the card stays pinned, so the card slides up past the offset before the next one arrives and the stack visibly breaks. Put spacing on the card itself or on the container, never on the sticky wrapper.
2. **Different card heights.** A short card cannot cover a tall one, and the reader sees the previous card's edge behind it.
3. **Leaving sticky on at mobile widths.** Use the `lg:` prefix so it only applies from 1024px.
- **An icon with no size of its own.** `<svg class="h-4 w-4" viewBox="0 0 20 20">` is 16px only while Tailwind is loaded. Strip the stylesheet away and the SVG has no intrinsic size and stretches to the width of whatever contains it: measured at 900 by 900 inside a 900px section, with the rest of the block pushed off the screen. Fix it with a `width` and a `height` attribute on the tag, and a base layer rule for any icon used more than once.
- **A height attribute on an illustration that fills its column.** The reverse mistake, and it shows up on a page that has Tailwind as well as one that does not. The width follows the column while the height is pinned to the attribute, so the drawing is squashed: measured at 656 by 380 where it should have been 656 by 519. An svg whose width follows its container keeps its height free.
- **A colour variable that is used but never declared.** `rgb(var(--vsc-accent))` with no `--vsc-accent` on the block is an invalid declaration, and an invalid declaration is not a fallback to something sensible: the browser throws the line away. Measured on a block that did this, the section had no background, the cards had no face and every piece of text came out pure black, including the parts meant to carry the brand colour.
- **A card that exists only as utility classes.** `class="rounded-2xl border bg-white p-6"` is a card while Tailwind is loaded and nothing at all without it. Measured: no face, no border, no radius, no padding, and the three cards stacked at full width instead of sitting three across. Anything a reader would notice was missing goes in the block's own stylesheet as well as on the markup.

## What a static block cannot do

These blocks carry no JavaScript. A few of the options above have no static equivalent, so build the nearest thing that does work and leave the rest out. Never ship a control that looks alive and does nothing when it is pressed.

- **Scale, rotate, fade, blur and compress on the covered card.** Each needs the covered card's state to follow the scroll position, so each is scroll-linked. See below. Plain layering needs nothing at all and is the default for this reason.

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
| `--vsc-surface` | card and panel faces |
| `--vsc-band` | the quiet band behind the content |
| `--vsc-ink` | headings and primary copy |
| `--vsc-soft` | supporting copy |
| `--vsc-accent` | the one emphasis colour |
| `--vsc-deep` | the dark ground |

The values are channel triplets, `15 23 42` rather than `#0f172a`, so an opacity suffix still works:


Declare all six on the block itself, and give them real values. They are not defined anywhere else, and a colour read from a variable that was never declared is an invalid declaration: the browser discards the line and that part renders with no colour at all. Where the brief names the customer's colours, those colours go into these six values and nowhere else.

    .vsc{--vsc-surface:255 255 255;--vsc-band:246 247 249;--vsc-ink:15 23 42;
      --vsc-soft:92 107 129;--vsc-accent:37 99 235;--vsc-deep:11 18 32}
`rgb(var(--vsc-ink) / .70)`.

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

With the default values these three pairs fail, so do not reach for them.

| Text | Background | Measured |
| --- | --- | --- |
| ink | deep | 1.05 to 1, near black on near black |
| accent | deep | 3.62 to 1, the accent is a dark blue and it sinks into a dark panel |
| soft | deep | 3.45 to 1 |

Soft clears everywhere else: 5.06 to 1 on the band and 5.42 on surface.

On a dark panel, set text in the surface value and use a lowered opacity of it for the quieter line.
White on the accent measures 5.17 to 1, so the accent still works there as a button background. On the
band, use a lowered opacity of ink for quiet copy.

A tint such as `rgb(var(--vsc-accent) / .10)` is mostly the colour behind it, so judge contrast
against the composited result, not against the accent itself.
