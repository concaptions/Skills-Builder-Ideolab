---
name: section-graphic-illustration
description: An AI-generated explanatory graphic that helps visitors understand a concept, process, comparison, or system. Use when an idea is easier to see than to read, as a process, comparison, ecosystem, timeline, diagram, map, exploded view, or illustrative scene, with optional labels, legend and hotspots. Not for decorative imagery, and not for a photograph.
---

# Graphic Illustration

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
- Give every inline `<svg>` a `width` and a `height` attribute as well as its size classes. An SVG that carries only a `viewBox` has no size of its own, so `h-4 w-4` is the only thing holding a 16px icon at 16px. Measured in a plain HTML viewer with no Tailwind, that same icon rendered at 900 by 900 and swallowed the section around it. The attribute is the weakest thing on the page, so Tailwind still wins wherever it is loaded, and the attribute only shows through where it is not. Size any icon that repeats in the base layer as well.

**Write the block's own base layer.** Tailwind utilities carry the detail, but a plain HTML viewer has no Tailwind, and a block built only from utilities arrives there as unstyled text on a white page. So write the few declarations that carry the block's shape into its own stylesheet as well. The block then reads correctly in a viewer and on a real page both.

Put them inside `@layer gil-base { }`. This matters: a layered declaration loses to an unlayered one whatever its specificity or its order, so Tailwind on a real page still wins every one of them, and the layer only shows through where there is no Tailwind. Without the layer, a rule such as `.gil h2` would outrank `.text-4xl` and the block would start overriding the page.

Start with the five reset lines. Tailwind normally supplies these, and without them the list markers show through, the type falls back to a serif, and the content runs wider than the screen. That was measured: a first attempt without them put a block 36px past the right edge of a 1280px viewport.

    @layer gil-base {
      .gil,.gil *,.gil *::before,.gil *::after{box-sizing:border-box}
      .gil{font-family:ui-sans-serif,system-ui,-apple-system,"Segoe UI",Roboto,sans-serif;line-height:1.5}
      .gil h1,.gil h2,.gil h3,.gil h4,.gil p,.gil figure,.gil blockquote{margin:0}
      .gil ul,.gil ol{margin:0;padding:0;list-style:none}
      .gil img,.gil svg,.gil video{display:block;max-width:100%}

Then the block's own shape. Cover these and nothing more.

- The section: background color, text color, horizontal and vertical padding.
- The inner wrapper: maximum width, centered. Name the wrapper class you actually used, not one from this example.
- Every row, column or grid the layout depends on: display, columns, gap.
- The heading and the body copy: font size, weight, line height.
- Shapes a reader would notice at a glance: border radius, border color.

      .gil{background-color:rgb(var(--gil-surface));color:rgb(var(--gil-ink));padding:5rem 1.25rem}
      .gil .gil-wrap{max-width:64rem;margin-inline:auto}
      .gil .gil-row{display:grid;gap:2.5rem}
      .gil h2{font-size:2.25rem;line-height:1.15;font-weight:600}
      .gil h3{font-size:1.25rem;font-weight:600}
      .gil p{font-size:1rem;line-height:1.625}
    }

About twelve declarations in all. Do not try to restate every utility you used. This is the block's skeleton, not a second copy of Tailwind. The exact spacing scale, the small type sizes and the hover states stay in the utilities where they belong.

**Nothing else is assumed.** The block does not expect the page to define a color, a font, a helper class or a shared stylesheet. If the page loads Tailwind the block is complete; if it does not, the base layer keeps it readable and in shape.

This skill is self-contained. Do not read, reference or depend on any other skill, and do not assume that a shared stylesheet, a motion system, a component library or a set of helper classes exists somewhere. If something is not described here, it is not available.

## What it is for

A labelled diagram that explains something a photograph cannot show: how a system fits together, what is inside a product, or what happens at each stage.

## When to use it

- The thing being explained is spatial or structural.
- A photograph would show the outside but hide the point.
- There are three to six parts worth naming.

## When not to use it

- A photograph would do the job better. A diagram of something real and visible is a step backwards.
- There is nothing to label. A decorative illustration is not this section.
- More than about eight call outs. The drawing stops being readable.

## Where it belongs on the page

Middle of the page, wherever the explanation is needed. In a technical or considered purchase it can be the centrepiece.

## What may be changed

- The drawing itself, the number of call outs and their copy.
- Call out placement, around the drawing or in a list beside it.
- Colours, which should come from the page theme.
- Whether the call outs are numbered.

## What must not be changed

- The labels living in the HTML as real text rather than inside the drawing. Text baked into an image cannot be read, translated, searched or resized.
- The title and description on the drawing, which is what makes it meaningful to a screen reader.
- Call outs being real focusable controls, so the highlight works by keyboard as well as by pointer.
- The reduced motion rules.

## Settings and Controls

The input is a brief, so these are the fields the brief is written into:

| Field | What goes in it |
| --- | --- |
| Primary prompt | What would you like this graphic to help visitors understand? |
| Subject | What is being illustrated? |
| Key points | What must be included? |
| Relationship | How do the ideas or objects connect? |
| Format | Process, comparison, ecosystem, timeline, diagram, map, exploded view, or illustrative scene |
| Style | Brand illustration, flat vector, isometric, dimensional, hand-drawn, infographic, or realistic composite |
| Animation | None, build on scroll, gentle loop, interactive hotspots, or step by step |
| Reference | Upload an image, or choose a brand asset |

And these are the ordinary settings:

| Setting | Options | Default |
| --- | --- | --- |
| Aspect ratio | 16:9, 4:3, 1:1, or 3:4 | 16:9 |
| Illustrated stages | How many steps, parts or items the graphic shows | 4 |
| Show or hide | Headline, explanatory copy, labels, legend, hotspots, supporting CTA, source note | headline, explanatory copy, labels |
| Desired outcome | What the visitor should do or believe after seeing it | supplied |

**Keep the stage count low.** Four is comfortable, six is the ceiling. Beyond that the labels shrink below readable size at mobile widths, which is where most of the traffic is.

**Text inside a generated image is unreliable and unsearchable.** Generate the graphic without words wherever the layout allows, and place labels as real HTML positioned over it. Labels then stay sharp, translate, and can be read by assistive technology.

**A source note is required for any graphic showing data.** A chart with no source is an assertion.

## Creative Design Options

**Layout**

- Centered graphic. The illustration alone with a headline above. The default.
- Text and illustration split. Copy one side, graphic the other.
- Full-width explainer. Edge to edge, for a wide process or timeline.
- Sticky illustration. The graphic pins while explanatory copy scrolls past it.
- Staged diagram. The graphic is built from parts that appear in sequence.

**Style**

Brand illustration, flat vector, isometric, dimensional, hand-drawn, infographic, or realistic composite.

**Motion**

Build on scroll, animated connectors, gentle loop, step-by-step reveal, hover hotspots, or interactive selection.

**Brand treatment**

Inherit brand palette, illustration style, icon language, typography, texture, and visual references.

## Motion

- **Build on scroll and animated connectors** are scroll-linked, so this becomes the page's one major showcase effect.
- **Step-by-step reveal** shows parts in order, staggered by 90ms, once. It does not replay every time the section re-enters view.
- **Gentle loop** is idle motion, so it is opt in, `subtle`, and it stops when the section leaves the viewport.
- **Hover hotspots** must also open on tap, otherwise they are unreachable on a phone.
- **Interactive selection** highlights one part and dims the others. Change opacity and color only, never position, so the diagram does not rearrange itself.
- **Entrance:** the graphic fades and rises as one at `standard` intensity.

## Responsive and Accessibility

| Width | Behavior |
| --- | --- |
| Desktop, 1024px and up | Full layout as chosen. Sticky, hotspots and build-on-scroll active |
| Tablet, 768px to 1023px | Split becomes stacked. Sticky is switched off |
| Mobile, under 768px | Full width, one column. Hotspots become a numbered list beneath the graphic. A wide diagram scrolls inside its own container rather than shrinking below legibility |

- The graphic needs alt text that carries the same meaning, not a description of the picture. For anything complex, a short text explanation next to it serves everyone, including search engines.
- Hotspot markers are real buttons, in a sensible order, with the label as accessible text.
- Labels are real text over the image, not baked into it.
- Meaning is never carried by color alone. Add a shape, a label or a pattern.
- Text over the graphic must meet contrast against the part of the graphic it sits on.
- A decorative background layer is `aria-hidden`.
- With reduced motion, the build renders complete, connector animation and loops are removed, and every stage is visible at once.

## Defaults

16:9, four stages, process format, brand illustration style, centered graphic layout, headline and explanatory copy and labels shown, labels as real HTML over the image, no animation beyond a standard fade-and-rise entrance, brand palette inherited.

## What breaks this section

1. **Letting the generator write the labels.** Generated text renders misspelled, cannot be translated, cannot be selected, and cannot be read by a screen reader.
2. **Prompting for a look instead of a meaning.** The output is decoration, and the reader learns nothing.
3. **Hotspots on hover only.** They are unreachable for every visitor on a phone.
- **An icon with no size of its own.** `<svg class="h-4 w-4" viewBox="0 0 20 20">` is 16px only while Tailwind is loaded. Strip the stylesheet away and the SVG has no intrinsic size and stretches to the width of whatever contains it: measured at 900 by 900 inside a 900px section, with the rest of the block pushed off the screen. Fix it with a `width` and a `height` attribute on the tag, and a base layer rule for any icon used more than once.

## What a static block cannot do

These blocks carry no JavaScript. A few of the options above have no static equivalent, so build the nearest thing that does work and leave the rest out. Never ship a control that looks alive and does nothing when it is pressed.

- **Revealing hotspots as the graphic enters view** is scroll-linked. See below. Hotspots themselves work on hover and on focus with no script.

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
| `--gil-surface` | card and panel faces |
| `--gil-band` | the quiet band behind the content |
| `--gil-ink` | headings and primary copy |
| `--gil-soft` | supporting copy |
| `--gil-accent` | the one emphasis colour |
| `--gil-deep` | the dark ground |

The values are channel triplets, `15 23 42` rather than `#0f172a`, so an opacity suffix still works:
`rgb(var(--gil-ink) / .70)`.

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

A tint such as `rgb(var(--gil-accent) / .10)` is mostly the colour behind it, so judge contrast
against the composited result, not against the accent itself.
