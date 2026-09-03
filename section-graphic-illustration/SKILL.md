---
name: section-graphic-illustration
description: An AI-generated explanatory graphic that helps visitors understand a concept, process, comparison, or system. Use when an idea is easier to see than to read, as a process, comparison, ecosystem, timeline, diagram, map, exploded view, or illustrative scene, with optional labels, legend and hotspots. Not for decorative imagery, and not for a photograph.
---

# Graphic Illustration

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

## Images

Every image belongs to the customer. Use the photographs, logos and artwork supplied in the brief.

Never pull a photograph from a stock site. A link to Unsplash, Pexels, Shutterstock or any other outside address is wrong on two counts: it is not the customer's business in the picture, and the block stops working the moment that address changes.

If no photograph has been supplied yet, keep the placeholder that ships inside the block. It is drawn in the page itself, so it needs nothing from outside, it takes the theme colors, and it holds the right shape so the layout does not move when the real photograph arrives.

Give every image an `alt` description of what is in it, and a `width` and `height`, so the page does not jump as it loads.

## Color

Use the page's theme tokens. Never hardcode a hex value or a Tailwind palette color, because this section has to work on a dark editorial page and a light clinical one without being rewritten.

The tokens are `surface`, `muted`, `textdark`, `textmute`, `accent` and `bgdark`. Use them as Tailwind classes: `bg-surface`, `bg-muted`, `bg-bgdark`, `text-textdark`, `text-textmute`, `text-accent`, `border-accent`.

Do not invent CSS variable names such as `var(--color-surface)` or `var(--color-border)`. Nothing on the page defines them, so the browser throws the declaration away and that color, border or shape renders as nothing at all. Do not use `text-current/60` either, because an opacity modifier has no effect on `currentColor` and the text comes out at full strength.

A token is not readable just because it is a token. `text-textdark` on `bg-bgdark` is near black on near black, measured at 1.05 to 1, and the words simply are not there. `text-textmute` on `bg-bgdark` reaches 3.93 to 1, which is still short.

On a dark panel, body text is `text-surface`. Use a lowered opacity of `text-surface` for the quieter line rather than reaching for `text-textmute`. Every piece of text has to reach 4.5 to 1 against the color actually behind it, which is the nearest ancestor that paints a background, not the page.
