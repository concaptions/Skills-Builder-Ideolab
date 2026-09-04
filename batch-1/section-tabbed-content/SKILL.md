---
name: section-tabbed-content
description: A complete section that organizes related content without making the page excessively long. Use for two to six panels of comparable content behind horizontal, vertical, pill, image-led, card or full-width tabs, each able to change an image, video, mockup or background. Not for content the visitor needs to read all of, and not for a FAQ, which is the expandable content section.
---

# Tabbed Content

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

Put them inside `@layer tbc-base { }`. This matters: a layered declaration loses to an unlayered one whatever its specificity or its order, so Tailwind on a real page still wins every one of them, and the layer only shows through where there is no Tailwind. Without the layer, a rule such as `.tbc h2` would outrank `.text-4xl` and the block would start overriding the page.

Start with the five reset lines. Tailwind normally supplies these, and without them the list markers show through, the type falls back to a serif, and the content runs wider than the screen. That was measured: a first attempt without them put a block 36px past the right edge of a 1280px viewport.

    @layer tbc-base {
      .tbc,.tbc *,.tbc *::before,.tbc *::after{box-sizing:border-box}
      .tbc{font-family:ui-sans-serif,system-ui,-apple-system,"Segoe UI",Roboto,sans-serif;line-height:1.5}
      .tbc h1,.tbc h2,.tbc h3,.tbc h4,.tbc p,.tbc figure,.tbc blockquote{margin:0}
      .tbc ul,.tbc ol{margin:0;padding:0;list-style:none}
      .tbc img,.tbc svg,.tbc video{display:block;max-width:100%}

Then the block's own shape. Cover these and nothing more.

- The section: background color, text color, horizontal and vertical padding.
- The inner wrapper: maximum width, centered. Name the wrapper class you actually used, not one from this example.
- Every row, column or grid the layout depends on: display, columns, gap.
- The heading and the body copy: font size, weight, line height.
- Shapes a reader would notice at a glance: border radius, border color.

      .tbc{background-color:rgb(var(--tbc-surface));color:rgb(var(--tbc-ink));padding:5rem 1.25rem}
      .tbc .tbc-wrap{max-width:64rem;margin-inline:auto}
      .tbc .tbc-row{display:grid;gap:2.5rem}
      .tbc h2{font-size:2.25rem;line-height:1.15;font-weight:600}
      .tbc h3{font-size:1.25rem;font-weight:600}
      .tbc p{font-size:1rem;line-height:1.625}
    }

About twelve declarations in all. Do not try to restate every utility you used. This is the block's skeleton, not a second copy of Tailwind. The exact spacing scale, the small type sizes and the hover states stay in the utilities where they belong.

**Nothing else is assumed.** The block does not expect the page to define a color, a font, a helper class or a shared stylesheet. If the page loads Tailwind the block is complete; if it does not, the base layer keeps it readable and in shape.

This skill is self-contained. Do not read, reference or depend on any other skill, and do not assume that a shared stylesheet, a motion system, a component library or a set of helper classes exists somewhere. If something is not described here, it is not available.

## What it is for

Several comparable blocks of content in one place, with only one shown at a time. It suits parallel options where the visitor cares about one of them.

## When to use it

- There are three to six comparable things, such as services, audiences or plans.
- The visitor knows which one applies to them, so they will pick rather than read everything.
- Each panel holds a similar amount of content.

## When not to use it

- The visitor needs to compare panels side by side. Use a table.
- There are only two options. Show both.
- The content matters for search. Content in a hidden panel is weaker than content on the page.
- Panels are wildly different lengths, which makes the section jump as tabs change.

## Where it belongs on the page

Middle of the page, wherever the offer splits into parallel options.

## What may be changed

- Number of tabs, their labels and their panel contents.
- Tab position, above the panel or down the side.
- Marker style, underline, pill or plain.
- Which tab is selected on arrival.

## What must not be changed

- The tabs being a radio group. That is what gives arrow key navigation, a single tab stop and an announced selected state with no script.
- The inputs and the panel container staying siblings. The selected panel is matched with the general sibling combinator, so nesting the inputs inside another element silently breaks every panel.
- Panels stacked in one grid cell, so the section does not change height as the visitor moves between tabs.
- A visible focus ring on each label.
- The reduced motion rules.

## Settings and Controls

| Setting | Options | Default |
| --- | --- | --- |
| Tab count | 2 to 6 | 3 |
| Labels | Short noun phrases, one or two words each | supplied |
| Default tab | Which opens on load | first |
| Tab position | Above, left, right, or below the panel | above |
| Content types | Text, list, image, video, mockup, cards, or a mix | text with image |
| Show or hide | Icons, images, descriptions, buttons, arrows, automatic rotation | descriptions, images, buttons |
| Activation | Click, hover, swipe, or controlled automatic progression | click |
| Mobile behavior | Accordion, dropdown, or horizontal scroller | horizontal scroller |

**Labels must be readable at a glance.** Six tabs with four-word labels do not fit on one row and wrap into a mess. Two words is the working limit.

**Hover activation is a trap.** It fires as the pointer crosses tabs on the way somewhere else, so the panel flickers, and it does nothing at all on touch. Click is the default for a reason.

**Automatic rotation changes the panel while the reader is reading it.** If it is used, it must pause on hover and on focus, stop permanently once the reader picks a tab, and stop when the section leaves the viewport.

**Keep the panel height stable.** Panels of different lengths make the page jump every time a tab is pressed. Either set a minimum height from the tallest panel, or transition the height.

## Creative Design Options

**Layout**

- Horizontal tabs. A row above the panel. The default.
- Vertical tabs. A rail down one side, which suits long labels and four or more tabs.
- Pill tabs. Rounded buttons, with the active one filled.
- Image-led tabs. Each tab shows a thumbnail rather than text alone.
- Card tabs. Each tab is a small card with a title and a line.
- Full-width tab stage. The tabs span the container and the panel sits below, edge to edge.

**Motion**

Sliding indicator, crossfade, shared-image transition, content slide, or height transition.

**Styling**

Underline, pill, segmented control, floating tab rail, gradient active state, or icon navigation.

**Media**

Each tab can change an image, video, illustration, mockup, or background treatment.

## Motion

- **Entrance:** the section fades and rises once. Panels do not animate in each time a tab is pressed beyond a short crossfade, because the reader pressed the tab and is waiting.
- **Sliding indicator** moves the underline or pill to the active tab in 200ms. Animate `transform`, not `left` or `width`.
- **Crossfade** is 150 to 200ms. Longer feels unresponsive.
- **Content slide** moves the panel in from the direction of the tab that was pressed, at `subtle` distance.
- **Height transition** uses `grid-template-rows` from `0fr` to `1fr`, or a measured height. Do not animate `height: auto`, which does not transition.
- **Shared-image transition** keeps one image element and swaps the source with a crossfade, rather than replacing the element.

## Responsive and Accessibility

| Width | Behavior |
| --- | --- |
| Desktop, 1024px and up | Full layout as chosen |
| Tablet, 768px to 1023px | Vertical tabs become horizontal. Card tabs shrink to pills |
| Mobile, under 768px | The chosen mobile behavior: a horizontal scroller with the active tab scrolled into view, an accordion, or a dropdown. Automatic rotation is off |

This section has a required keyboard and semantics contract, which is not optional:

- The tab row is `role="tablist"`, each tab is a `<button role="tab">` with `aria-selected` and `aria-controls`, and each panel is `role="tabpanel"` with `aria-labelledby`.
- Only the active tab is in the tab order, `tabindex="0"`; the rest are `tabindex="-1"`.
- Left and right arrow keys move between tabs, Home and End jump to the first and last.
- Hidden panels use the `hidden` attribute, so their content is not reachable by tab or read out.
- The panel is labeled by its tab, so a screen-reader user knows which panel they are in.
- With reduced motion, the indicator jumps rather than slides, crossfades and slides are removed, and automatic rotation does not start.

## Defaults

Three tabs, first open, tabs above, text with image, click activation, descriptions and images and buttons shown, horizontal scroller on mobile, underline styling, sliding indicator with a 180ms crossfade, minimum panel height set from the tallest panel.

## What breaks this section

1. **Buttons, or divs with click handlers, as the tabs.** A `<button role="tab">` does nothing on its own. Something has to listen to the click, and there is no script here, so the section renders looking correct and the tabs are dead: every press leaves the first panel on screen. The tabs are radio inputs and their labels. The browser does the switching.

2. **Hiding panels with the `hidden` attribute.** Same trap. Nothing can remove the attribute without a script, so whichever panel starts hidden is hidden forever. Hide a panel with `visibility: hidden` alongside `opacity: 0`, and reveal it with the checked input: `#tab-2:checked ~ .panels .panel-2 { opacity: 1; visibility: visible }`. `visibility: hidden` is the part that matters, because it also takes the panel's links out of the tab order. `opacity: 0` on its own does not, and tabbing then lands on invisible links.

3. **Nesting the inputs inside a tab bar.** The general sibling combinator only reaches forward across true siblings, so the inputs and the panel container must be children of the same element. Nest the inputs one level deeper and every panel silently renders blank, with the tabs still looking right.

4. **Positioning the panels absolutely.** An absolutely positioned panel is out of the flow, so the section measures as though it had no panels at all. The content then hangs below the section and whatever comes next on the page is drawn straight over it. Stack the panels in the flow instead, in one grid cell (`grid-area: 1 / 1`), which also keeps them the same size.

5. **Panels of different heights with no minimum.** The whole page below the section jumps every time a tab is pressed. Set a minimum height from the tallest panel.
- **An icon with no size of its own.** `<svg class="h-4 w-4" viewBox="0 0 20 20">` is 16px only while Tailwind is loaded. Strip the stylesheet away and the SVG has no intrinsic size and stretches to the width of whatever contains it: measured at 900 by 900 inside a 900px section, with the rest of the block pushed off the screen. Fix it with a `width` and a `height` attribute on the tag, and a base layer rule for any icon used more than once.
- **A height attribute on an illustration that fills its column.** The reverse mistake, and it shows up on a page that has Tailwind as well as one that does not. The width follows the column while the height is pinned to the attribute, so the drawing is squashed: measured at 656 by 380 where it should have been 656 by 519. An svg whose width follows its container keeps its height free.
- **A height attribute on an illustration that fills its column.** The reverse mistake, and it shows up on a page that has Tailwind as well as one that does not. The width follows the column while the height is pinned to the attribute, so the drawing is squashed: measured at 656 by 380 where it should have been 656 by 519. An svg whose width follows its container keeps its height free.

## What a static block cannot do

These blocks carry no JavaScript. A few of the options above have no static equivalent, so build the nearest thing that does work and leave the rest out. Never ship a control that looks alive and does nothing when it is pressed.

- **Automatic rotation.** Not available. The tabs respond to a click and to the arrow keys, which is what the radio group gives. Rotation would also have needed to pause on hover, stop on selection and stop off screen, none of which is possible here.

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
| `--tbc-surface` | card and panel faces |
| `--tbc-band` | the quiet band behind the content |
| `--tbc-ink` | headings and primary copy |
| `--tbc-soft` | supporting copy |
| `--tbc-accent` | the one emphasis colour |
| `--tbc-deep` | the dark ground |

The values are channel triplets, `15 23 42` rather than `#0f172a`, so an opacity suffix still works:
`rgb(var(--tbc-ink) / .70)`.

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

A tint such as `rgb(var(--tbc-accent) / .10)` is mostly the colour behind it, so judge contrast
against the composited result, not against the accent itself.
