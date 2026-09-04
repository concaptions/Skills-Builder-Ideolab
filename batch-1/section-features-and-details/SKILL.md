---
name: section-features-and-details
description: A deeper explanation section for capabilities, specifications, inclusions, or technical details. Use for a bento grid, a checklist beside a visual, a sticky visual with a scrolling feature list, a comparison table, or a labeled hotspot image. Not for customer outcomes, and not for listing what the business sells.
---

# Features and Details

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

Put them inside `@layer fad-base { }`. This matters: a layered declaration loses to an unlayered one whatever its specificity or its order, so Tailwind on a real page still wins every one of them, and the layer only shows through where there is no Tailwind. Without the layer, a rule such as `.fad h2` would outrank `.text-4xl` and the block would start overriding the page.

Start with the five reset lines. Tailwind normally supplies these, and without them the list markers show through, the type falls back to a serif, and the content runs wider than the screen. That was measured: a first attempt without them put a block 36px past the right edge of a 1280px viewport.

    @layer fad-base {
      .fad,.fad *,.fad *::before,.fad *::after{box-sizing:border-box}
      .fad{font-family:ui-sans-serif,system-ui,-apple-system,"Segoe UI",Roboto,sans-serif;line-height:1.5}
      .fad h1,.fad h2,.fad h3,.fad h4,.fad p,.fad figure,.fad blockquote{margin:0}
      .fad ul,.fad ol{margin:0;padding:0;list-style:none}
      .fad img,.fad svg,.fad video{display:block;max-width:100%}

Then the block's own shape. Cover these and nothing more.

- The section: background color, text color, horizontal and vertical padding.
- The inner wrapper: maximum width, centered. Name the wrapper class you actually used, not one from this example.
- Every row, column or grid the layout depends on: display, columns, gap.
- The heading and the body copy: font size, weight, line height.
- Shapes a reader would notice at a glance: border radius, border color.

      .fad{background-color:rgb(var(--fad-surface));color:rgb(var(--fad-ink));padding:5rem 1.25rem}
      .fad .fad-wrap{max-width:64rem;margin-inline:auto}
      .fad .fad-row{display:grid;gap:2.5rem}
      .fad h2{font-size:2.25rem;line-height:1.15;font-weight:600}
      .fad h3{font-size:1.25rem;font-weight:600}
      .fad p{font-size:1rem;line-height:1.625}
    }

That is the skeleton, and it stays short. The detail, the spacing and the smaller responsive steps still come from the utilities.

**Then, outside the layer, declare the six colour variables.** Nothing else defines them. A block that writes `rgb(var(--fad-surface))` without declaring `--fad-surface` has written an invalid declaration and the browser throws the whole line away. Measured with no stylesheet, a block that skipped this had a transparent section, a card with no face, and pure black headings, quiet copy and marks. **The customer's colours go in these six values.** That is where a brief asking for a red and brown clinic actually lands, and it is the only place any colour is decided.

    .fad{
      --fad-surface:255 255 255;
      --fad-band:246 247 249;
      --fad-ink:15 23 42;
      --fad-soft:92 107 129;
      --fad-accent:37 99 235;
      --fad-deep:11 18 32;
      background-color:rgb(var(--fad-surface));
      color:rgb(var(--fad-ink));
    }

This part is not in the layer, on purpose. Tailwind has no rule for a class name prefixed `fad-`, so there is nothing to conflict with, and a layered rule would lose to a stray utility sitting on the same element.

**Then write out every named class the block cannot be read without.** A card or panel face, its border, its radius and its padding. Any ground that is not the section's own. Any text that is not the default ink. The accent on a mark, a rule or a button. The keyframes, the media queries that build the columns, and the reduced-motion rule. Every one of those is a utility on the markup as well, and a utility is nothing in a viewer with no Tailwind. Measured on a block that left them out: three cards with no face and no border, black marks, and all three at full width down the page instead of three across.

    .fad .fad-card{background-color:rgb(var(--fad-band));border:1px solid rgb(var(--fad-ink)/.10);
      border-radius:1rem;padding:1.5rem}
    .fad .fad-mark{color:rgb(var(--fad-accent))}
    .fad .fad-soft{color:rgb(var(--fad-soft))}
    @media (min-width:640px){.fad .fad-row{grid-template-columns:repeat(2,minmax(0,1fr))}}

Those class names are an example of the shape, not a list to copy. Name the ones this block actually contains.

**The test before you hand it over:** picture every `class` attribute deleted. What is left has to still be this block, in the customer's colours, with its faces and its columns. If it collapses to a column of black text on white, the stylesheet is too short. The exact spacing scale, the small type sizes and the hover states stay in the utilities where they belong.

**Nothing else is assumed.** The block does not expect the page to define a color, a font, a helper class or a shared stylesheet. If the page loads Tailwind the block is complete; if it does not, the base layer keeps it readable and in shape.

This skill is self-contained. Do not read, reference or depend on any other skill, and do not assume that a shared stylesheet, a motion system, a component library or a set of helper classes exists somewhere. If something is not described here, it is not available.

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
- **An icon with no size of its own.** `<svg class="h-4 w-4" viewBox="0 0 20 20">` is 16px only while Tailwind is loaded. Strip the stylesheet away and the SVG has no intrinsic size and stretches to the width of whatever contains it: measured at 900 by 900 inside a 900px section, with the rest of the block pushed off the screen. Fix it with a `width` and a `height` attribute on the tag, and a base layer rule for any icon used more than once.
- **A height attribute on an illustration that fills its column.** The reverse mistake, and it shows up on a page that has Tailwind as well as one that does not. The width follows the column while the height is pinned to the attribute, so the drawing is squashed: measured at 656 by 380 where it should have been 656 by 519. An svg whose width follows its container keeps its height free.
- **A colour variable that is used but never declared.** `rgb(var(--fad-accent))` with no `--fad-accent` on the block is an invalid declaration, and an invalid declaration is not a fallback to something sensible: the browser throws the line away. Measured on a block that did this, the section had no background, the cards had no face and every piece of text came out pure black, including the parts meant to carry the brand colour.
- **A card that exists only as utility classes.** `class="rounded-2xl border bg-white p-6"` is a card while Tailwind is loaded and nothing at all without it. Measured: no face, no border, no radius, no padding, and the three cards stacked at full width instead of sitting three across. Anything a reader would notice was missing goes in the block's own stylesheet as well as on the markup.

## What a static block cannot do

These blocks carry no JavaScript. A few of the options above have no static equivalent, so build the nearest thing that does work and leave the rest out. Never ship a control that looks alive and does nothing when it is pressed.

- **Highlighting the feature that matches the sticky visual as you scroll** is scroll-linked. See below. The sticky visual itself is `position: sticky` and needs nothing.

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
| `--fad-surface` | card and panel faces |
| `--fad-band` | the quiet band behind the content |
| `--fad-ink` | headings and primary copy |
| `--fad-soft` | supporting copy |
| `--fad-accent` | the one emphasis colour |
| `--fad-deep` | the dark ground |

The values are channel triplets, `15 23 42` rather than `#0f172a`, so an opacity suffix still works:


Declare all six on the block itself, and give them real values. They are not defined anywhere else, and a colour read from a variable that was never declared is an invalid declaration: the browser discards the line and that part renders with no colour at all. Where the brief names the customer's colours, those colours go into these six values and nowhere else.

    .fad{--fad-surface:255 255 255;--fad-band:246 247 249;--fad-ink:15 23 42;
      --fad-soft:92 107 129;--fad-accent:37 99 235;--fad-deep:11 18 32}
`rgb(var(--fad-ink) / .70)`.

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

A tint such as `rgb(var(--fad-accent) / .10)` is mostly the colour behind it, so judge contrast
against the composited result, not against the accent itself.
