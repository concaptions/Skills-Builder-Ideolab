---
name: section-animated-timeline
description: A history, roadmap, process, or milestone section that progresses visually. Use for dated milestones running vertically, horizontally, alternating sides, as a roadmap, a chapter timeline, or a sticky timeline. Not for an unordered set of steps with no dates or sequence, and not for a short three-step process, which is the how it works section.
---

# Animated Timeline

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

Put them inside `@layer atl-base { }`. This matters: a layered declaration loses to an unlayered one whatever its specificity or its order, so Tailwind on a real page still wins every one of them, and the layer only shows through where there is no Tailwind. Without the layer, a rule such as `.atl h2` would outrank `.text-4xl` and the block would start overriding the page.

Start with the five reset lines. Tailwind normally supplies these, and without them the list markers show through, the type falls back to a serif, and the content runs wider than the screen. That was measured: the first attempt without them put the block 36px past the right edge of a 1280px viewport.

    @layer atl-base {
      .atl,.atl *,.atl *::before,.atl *::after{box-sizing:border-box}
      .atl{font-family:ui-sans-serif,system-ui,-apple-system,"Segoe UI",Roboto,sans-serif;line-height:1.5}
      .atl h1,.atl h2,.atl h3,.atl h4,.atl p,.atl figure,.atl blockquote{margin:0}
      .atl ul,.atl ol{margin:0;padding:0;list-style:none}
      .atl img,.atl svg,.atl video{display:block;max-width:100%}

Then the block's own shape. Cover these and nothing more.

- The section: background color, text color, horizontal and vertical padding.
- The inner wrapper: maximum width, centered. Name the wrapper class you actually used, not one from this example.
- Every row, column or grid the layout depends on: display, columns, gap.
- The heading and the body copy: font size, weight, line height.
- Shapes a reader would notice at a glance: border radius, border color.

      .atl{background-color:rgb(var(--atl-surface));color:rgb(var(--atl-ink));padding:5rem 1.25rem}
      .atl .atl-wrap{max-width:64rem;margin-inline:auto}
      .atl .atl-track{display:grid;gap:2.5rem}
      .atl h2{font-size:2.25rem;line-height:1.15;font-weight:600}
      .atl h3{font-size:1.25rem;font-weight:600}
      .atl p{font-size:1rem;line-height:1.625}
      .atl .atl-dot{border-radius:9999px;background-color:rgb(var(--atl-accent))}
    }

About twelve declarations in all.

Do not try to restate every utility you used. This is the block's skeleton, not a second copy of Tailwind. Everything else, the exact spacing scale, the small type sizes, the hover states, stays in the utilities where it belongs.

**Nothing else is assumed.** The block does not expect the page to define a color, a font, a helper class or a shared stylesheet. If the page loads Tailwind, the block is complete; if it does not, the base layer keeps it readable and in shape.

This skill is self-contained. Do not read, reference or depend on any other skill, and do not assume that a shared stylesheet, a motion system, a component library or a set of helper classes exists somewhere. If something is not described here, it is not available.

## What it is for

A sequence with dates or order that matters: company history, a project roadmap, a process with stages, or a case study told in order.

## When to use it

- The order of events is part of the meaning.
- There are four to eight milestones. Fewer reads as a list, more becomes a chore.
- Each milestone has a date, a label or a clear position in a sequence.

## When not to use it

- The items have no order. That is a benefits or features section.
- There are only three short steps. That is a how it works section.
- The dates are vague or invented. A timeline invites scrutiny of exactly those dates.

## Where it belongs on the page

Middle of the page for a history or roadmap. In a case study it can carry the whole body of the page.

## What may be changed

- Number of milestones, their dates and their copy.
- Orientation, alternating sides, single side, or horizontal.
- Marker shape and colour, and whether the spine is solid or dashed.
- Whether the reveal happens at all.

## What must not be changed

- Every milestone being fully readable with no scroll effect applied. The reveal is an enhancement layered on top, never the thing that makes the text appear.
- The reveal ending well before the middle of the viewport, otherwise the last milestone can never activate and stays invisible at the bottom of the page.
- The list being a real ordered list, so the sequence survives without styling.
- The reduced motion rules.

## Settings and Controls

| Setting | Options | Default |
| --- | --- | --- |
| Milestone count | 3 to 12 | 5 |
| Dates | Year, month and year, full date, quarter, or a label instead | year |
| Ordering | Oldest first, newest first | oldest first |
| Direction | Vertical, horizontal | vertical |
| Content fields | Date, label, description, image, icon | date, label, description |
| Images | One per milestone, one shared changing image, or none | none |
| Navigation method | Scrolling, clicking, dragging, automatic playback, or static | scrolling |
| Show or hide | Dates, labels, descriptions, images, icons, arrows, progress, CTA | dates, labels, descriptions, progress |
| Active milestone | Which one is highlighted on load | first |
| Starting point | Top, bottom, or a named milestone | top |
| Mobile stacking | All milestones stacked, or one at a time | all stacked |
| Content shown at once | All descriptions, or only the active one | all |

**Only the active description is a risk.** It halves the text on the page and hides content from anyone who does not interact. Use it when descriptions are long. Keep everything visible when they are one line.

**Automatic playback needs a pause control** and must stop when the section leaves the viewport, like every continuous motion.

**A future-dated roadmap is a promise.** Mark unshipped milestones clearly as planned, and put a date on the section so a stale roadmap is obvious rather than misleading.

## Creative Design Options

**Layout**

- Vertical. Milestones down a spine on the left. The default, and the one that survives long descriptions.
- Horizontal. Milestones along a line, for short labels and few items.
- Alternating sides. Milestones left and right of a central spine.
- Roadmap. A path with stages rather than dates, for what is coming.
- Chapter timeline. Grouped into named periods, each with its own heading.
- Sticky timeline. The spine and the active date pin while the milestone content scrolls past.

**Motion**

Line grows, dot activates, milestone glows, number counts up, or image changes.

**Styling**

Gradient path, illuminated nodes, hand-drawn line, brand-color chapters, or dimensional roadmap.

**Interaction**

Jump by year, click milestones, reveal details, or synchronize milestones with supporting media.

## Motion

- **Line grows** is the section's signature: the spine fills as the reader scrolls. It is scroll-linked, so this is the page's one major showcase effect.
- **Dot activates** when its milestone reaches the reading line. Change color, fill and scale of the dot only, never the milestone row, so nothing shifts.
- **Number counts up** applies to a metric inside a milestone, runs once, and has the final value in the markup.
- **Image changes** crossfades between stacked images so the box never empties.
- **Milestone glows** is `subtle` and applies to the active item only.
- **Entrance:** milestones fade and rise in order, staggered by 60ms.

## Responsive and Accessibility

| Width | Behavior |
| --- | --- |
| Desktop, 1024px and up | Full layout as chosen. Line growth and sticky spine active |
| Tablet, 768px to 1023px | Alternating sides becomes single-sided. Horizontal becomes vertical |
| Mobile, under 768px | Vertical, spine on the left, all milestones stacked. Sticky off. Automatic playback off |

- The timeline is an ordered list, `<ol>`, so the sequence is in the markup.
- Dates are in a `<time datetime="...">` element with a machine-readable value.
- The spine, the dots and any decorative path are `aria-hidden`.
- Clickable milestones and year jumps are real buttons, and the active one is marked as current.
- If only the active description is shown, the hidden ones are still reachable, and each is revealed by a real control rather than by hover alone.
- With reduced motion, the line renders full, dot activation and glow are removed, count-ups show the final value, image changes are instant, and automatic playback does not start.

## Defaults

Five milestones, year dates, oldest first, vertical, date and label and description shown, no images, scroll navigation, progress shown, active milestone first, all stacked on mobile, gradient path, staggered fade-and-rise entrance, line grows once.

## What breaks this section

1. **Driving the fill from a raw scroll percentage.** It stops between dots and looks like a rendering error. Drive it from the last passed milestone.
2. **Scaling the whole milestone row when it activates.** Everything below it shifts, and the page jitters all the way down.
3. **A roadmap with no "as of" date.** Six months later it reads as a list of things that never happened.
- **An icon with no size of its own.** `<svg class="h-4 w-4" viewBox="0 0 20 20">` is 16px only while Tailwind is loaded. Strip the stylesheet away and the SVG has no intrinsic size and stretches to the width of whatever contains it: measured at 900 by 900 inside a 900px section, with the rest of the block pushed off the screen. Fix it with a `width` and a `height` attribute on the tag, and a base layer rule for any icon used more than once.
- **A height attribute on an illustration that fills its column.** The reverse mistake, and it shows up on a page that has Tailwind as well as one that does not. The width follows the column while the height is pinned to the attribute, so the drawing is squashed: measured at 656 by 380 where it should have been 656 by 519. An svg whose width follows its container keeps its height free.

## What a static block cannot do

These blocks carry no JavaScript. A few of the options above have no static equivalent, so build the nearest thing that does work and leave the rest out. Never ship a control that looks alive and does nothing when it is pressed.

- **Number counts up.** There is no static equivalent. Print the final value as text.
- **Line grows** and **dot activates** are scroll-linked. See below.

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
| `--atl-surface` | card and panel faces |
| `--atl-band` | the quiet band behind the content |
| `--atl-ink` | headings and primary copy |
| `--atl-soft` | supporting copy |
| `--atl-accent` | the one emphasis colour |
| `--atl-deep` | the dark ground |

The values are channel triplets, `15 23 42` rather than `#0f172a`, so an opacity suffix still works:
`rgb(var(--atl-ink) / .70)`.

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

A tint such as `rgb(var(--atl-accent) / .10)` is mostly the colour behind it, so judge contrast
against the composited result, not against the accent itself.
