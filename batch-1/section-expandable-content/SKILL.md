---
name: section-expandable-content
description: A complete accordion or expandable-card section for FAQs, details, specifications, or supporting information. Use when many short answers must be scannable without filling the page, as a standard accordion, expandable cards, a split view, an image-changing accordion, or an FAQ list. Not for alternatives the visitor picks between, which is the tabbed content section.
---

# Expandable Content

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

Put them inside `@layer exp-base { }`. This matters: a layered declaration loses to an unlayered one whatever its specificity or its order, so Tailwind on a real page still wins every one of them, and the layer only shows through where there is no Tailwind. Without the layer, a rule such as `.exp h2` would outrank `.text-4xl` and the block would start overriding the page.

Start with the five reset lines. Tailwind normally supplies these, and without them the list markers show through, the type falls back to a serif, and the content runs wider than the screen. That was measured: a first attempt without them put a block 36px past the right edge of a 1280px viewport.

    @layer exp-base {
      .exp,.exp *,.exp *::before,.exp *::after{box-sizing:border-box}
      .exp{font-family:ui-sans-serif,system-ui,-apple-system,"Segoe UI",Roboto,sans-serif;line-height:1.5}
      .exp h1,.exp h2,.exp h3,.exp h4,.exp p,.exp figure,.exp blockquote{margin:0}
      .exp ul,.exp ol{margin:0;padding:0;list-style:none}
      .exp img,.exp svg,.exp video{display:block;max-width:100%}

Then the block's own shape. Cover these and nothing more.

- The section: background color, text color, horizontal and vertical padding.
- The inner wrapper: maximum width, centered. Name the wrapper class you actually used, not one from this example.
- Every row, column or grid the layout depends on: display, columns, gap.
- The heading and the body copy: font size, weight, line height.
- Shapes a reader would notice at a glance: border radius, border color.

      .exp{background-color:rgb(var(--exp-surface));color:rgb(var(--exp-ink));padding:5rem 1.25rem}
      .exp .exp-wrap{max-width:64rem;margin-inline:auto}
      .exp .exp-row{display:grid;gap:2.5rem}
      .exp h2{font-size:2.25rem;line-height:1.15;font-weight:600}
      .exp h3{font-size:1.25rem;font-weight:600}
      .exp p{font-size:1rem;line-height:1.625}
    }

About twelve declarations in all. Do not try to restate every utility you used. This is the block's skeleton, not a second copy of Tailwind. The exact spacing scale, the small type sizes and the hover states stay in the utilities where they belong.

**Nothing else is assumed.** The block does not expect the page to define a color, a font, a helper class or a shared stylesheet. If the page loads Tailwind the block is complete; if it does not, the base layer keeps it readable and in shape.

This skill is self-contained. Do not read, reference or depend on any other skill, and do not assume that a shared stylesheet, a motion system, a component library or a set of helper classes exists somewhere. If something is not described here, it is not available.

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
- **An icon with no size of its own.** `<svg class="h-4 w-4" viewBox="0 0 20 20">` is 16px only while Tailwind is loaded. Strip the stylesheet away and the SVG has no intrinsic size and stretches to the width of whatever contains it: measured at 900 by 900 inside a 900px section, with the rest of the block pushed off the screen. Fix it with a `width` and a `height` attribute on the tag, and a base layer rule for any icon used more than once.
- **A height attribute on an illustration that fills its column.** The reverse mistake, and it shows up on a page that has Tailwind as well as one that does not. The width follows the column while the height is pinned to the attribute, so the drawing is squashed: measured at 656 by 380 where it should have been 656 by 519. An svg whose width follows its container keeps its height free.
- **A height attribute on an illustration that fills its column.** The reverse mistake, and it shows up on a page that has Tailwind as well as one that does not. The width follows the column while the height is pinned to the attribute, so the drawing is squashed: measured at 656 by 380 where it should have been 656 by 519. An svg whose width follows its container keeps its height free.

## What a static block cannot do

These blocks carry no JavaScript. A few of the options above have no static equivalent, so build the nearest thing that does work and leave the rest out. Never ship a control that looks alive and does nothing when it is pressed.

- **The image-changing accordion** only works if the panels are a radio group rather than `details` elements, because a sibling selector can then reach the image. With `details` the image cannot react to which panel is open.

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
| `--exp-surface` | card and panel faces |
| `--exp-band` | the quiet band behind the content |
| `--exp-ink` | headings and primary copy |
| `--exp-soft` | supporting copy |
| `--exp-accent` | the one emphasis colour |
| `--exp-deep` | the dark ground |

The values are channel triplets, `15 23 42` rather than `#0f172a`, so an opacity suffix still works:
`rgb(var(--exp-ink) / .70)`.

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

A tint such as `rgb(var(--exp-accent) / .10)` is mostly the colour behind it, so judge contrast
against the composited result, not against the accent itself.
