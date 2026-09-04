---
name: section-customer-benefits
description: A results-focused section that explains what the customer gains rather than only what the product does. Use for outcome statements, three pillars, benefit cards, or measurable results with proof. Not for listing product capabilities or specifications, which belong in a features section.
---

# Customer Benefits

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
- Give every **icon** a `width` and a `height` attribute as well as its size classes. An SVG that carries only a `viewBox` has no size of its own, so `h-4 w-4` is the only thing holding a 16px icon at 16px. Measured in a plain HTML viewer with no Tailwind, that same icon rendered at 900 by 900 and swallowed the section around it. The attribute is the weakest thing on the page, so Tailwind still wins wherever it is loaded, and the attribute only shows through where it is not. Size any icon that repeats in the base layer as well.
- Do the opposite for an **illustration that fills its column**. An SVG set to the full width of its container takes its height from the viewBox ratio, and a `height` attribute overrides that and squashes the drawing: measured, one illustration went from 656 by 519 to 656 by 380. Leave its height free, or pair the attributes with `height:auto` in the base layer. The test is simple: a fixed size gets both attributes, a width that follows the column does not get a height.

**Write the block's own base layer.** Tailwind utilities carry the detail, but a plain HTML viewer has no Tailwind, and a block built only from utilities arrives there as unstyled text on a white page. So write the few declarations that carry the block's shape into its own stylesheet as well. The block then reads correctly in a viewer and on a real page both.

Put them inside `@layer cbn-base { }`. This matters: a layered declaration loses to an unlayered one whatever its specificity or its order, so Tailwind on a real page still wins every one of them, and the layer only shows through where there is no Tailwind. Without the layer, a rule such as `.cbn h2` would outrank `.text-4xl` and the block would start overriding the page.

Start with the five reset lines. Tailwind normally supplies these, and without them the list markers show through, the type falls back to a serif, and the content runs wider than the screen. That was measured: a first attempt without them put a block 36px past the right edge of a 1280px viewport.

    @layer cbn-base {
      .cbn,.cbn *,.cbn *::before,.cbn *::after{box-sizing:border-box}
      .cbn{font-family:ui-sans-serif,system-ui,-apple-system,"Segoe UI",Roboto,sans-serif;line-height:1.5}
      .cbn h1,.cbn h2,.cbn h3,.cbn h4,.cbn p,.cbn figure,.cbn blockquote{margin:0}
      .cbn ul,.cbn ol{margin:0;padding:0;list-style:none}
      .cbn img,.cbn svg,.cbn video{display:block;max-width:100%}

Then the block's own shape. Cover these and nothing more.

- The section: background color, text color, horizontal and vertical padding.
- The inner wrapper: maximum width, centered. Name the wrapper class you actually used, not one from this example.
- Every row, column or grid the layout depends on: display, columns, gap.
- The heading and the body copy: font size, weight, line height.
- Shapes a reader would notice at a glance: border radius, border color.

      .cbn{background-color:rgb(var(--cbn-surface));color:rgb(var(--cbn-ink));padding:5rem 1.25rem}
      .cbn .cbn-wrap{max-width:64rem;margin-inline:auto}
      .cbn .cbn-row{display:grid;gap:2.5rem}
      .cbn h2{font-size:2.25rem;line-height:1.15;font-weight:600}
      .cbn h3{font-size:1.25rem;font-weight:600}
      .cbn p{font-size:1rem;line-height:1.625}
    }

About twelve declarations in all. Do not try to restate every utility you used. This is the block's skeleton, not a second copy of Tailwind. The exact spacing scale, the small type sizes and the hover states stay in the utilities where they belong.

**Nothing else is assumed.** The block does not expect the page to define a color, a font, a helper class or a shared stylesheet. If the page loads Tailwind the block is complete; if it does not, the base layer keeps it readable and in shape.

This skill is self-contained. Do not read, reference or depend on any other skill, and do not assume that a shared stylesheet, a motion system, a component library or a set of helper classes exists somewhere. If something is not described here, it is not available.

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
- **Count-up results** run once, when the metric first enters view, over roughly 1 to 1.5 seconds. There is no script here, so the counting cannot happen. Print the final value as text.
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
2. **Leaving a metric empty for a count-up to fill in.** Nothing will ever fill it. Put the final value in the markup and leave it there.
3. **A metric with no proof.** An unsupported percentage reads as marketing and lowers trust in the rest of the page.
- **An icon with no size of its own.** `<svg class="h-4 w-4" viewBox="0 0 20 20">` is 16px only while Tailwind is loaded. Strip the stylesheet away and the SVG has no intrinsic size and stretches to the width of whatever contains it: measured at 900 by 900 inside a 900px section, with the rest of the block pushed off the screen. Fix it with a `width` and a `height` attribute on the tag, and a base layer rule for any icon used more than once.
- **A height attribute on an illustration that fills its column.** The reverse mistake, and it shows up on a page that has Tailwind as well as one that does not. The width follows the column while the height is pinned to the attribute, so the drawing is squashed: measured at 656 by 380 where it should have been 656 by 519. An svg whose width follows its container keeps its height free.

## What a static block cannot do

These blocks carry no JavaScript. A few of the options above have no static equivalent, so build the nearest thing that does work and leave the rest out. Never ship a control that looks alive and does nothing when it is pressed.

- **Count-up results.** There is no static equivalent. Print the final number as text. It is the number that matters, not the counting.
- **Connecting-line draw** is scroll-linked. See below.

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
| `--cbn-surface` | card and panel faces |
| `--cbn-band` | the quiet band behind the content |
| `--cbn-ink` | headings and primary copy |
| `--cbn-soft` | supporting copy |
| `--cbn-accent` | the one emphasis colour |
| `--cbn-deep` | the dark ground |

The values are channel triplets, `15 23 42` rather than `#0f172a`, so an opacity suffix still works:
`rgb(var(--cbn-ink) / .70)`.

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

A tint such as `rgb(var(--cbn-accent) / .10)` is mostly the colour behind it, so judge contrast
against the composited result, not against the accent itself.
