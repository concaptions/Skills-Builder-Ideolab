---
name: section-how-it-works
description: A process section that explains steps, onboarding, delivery, or the customer journey. Use for three to six ordered steps shown horizontally, vertically, as a zigzag, a circular process, a timeline, or a sticky step-by-step story. Not for unordered lists of capabilities, where order carries no meaning.
---

# How It Works

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
- Give every **icon** a `width` and a `height` attribute as well as its size classes. An SVG that carries only a `viewBox` has no size of its own, so `h-4 w-4` is the only thing holding a 16px icon at 16px. Measured in a plain HTML viewer with no Tailwind, that same icon rendered at 900 by 900 and swallowed the section around it. The attribute is the weakest thing on the page, so Tailwind still wins wherever it is loaded, and the attribute only shows through where it is not. Size any icon that repeats in the base layer as well.
- Do the opposite for an **illustration that fills its column**. An SVG set to the full width of its container takes its height from the viewBox ratio, and a `height` attribute overrides that and squashes the drawing: measured, one illustration went from 656 by 519 to 656 by 380. Leave its height free, or pair the attributes with `height:auto` in the base layer. The test is simple: a fixed size gets both attributes, a width that follows the column does not get a height.

**Write the block's own base layer.** Tailwind utilities carry the detail, but a plain HTML viewer has no Tailwind, and a block built only from utilities arrives there as unstyled text on a white page. So write the few declarations that carry the block's shape into its own stylesheet as well. The block then reads correctly in a viewer and on a real page both.

Put them inside `@layer hiw-base { }`. This matters: a layered declaration loses to an unlayered one whatever its specificity or its order, so Tailwind on a real page still wins every one of them, and the layer only shows through where there is no Tailwind. Without the layer, a rule such as `.hiw h2` would outrank `.text-4xl` and the block would start overriding the page.

Start with the five reset lines. Tailwind normally supplies these, and without them the list markers show through, the type falls back to a serif, and the content runs wider than the screen. That was measured: a first attempt without them put a block 36px past the right edge of a 1280px viewport.

    @layer hiw-base {
      .hiw,.hiw *,.hiw *::before,.hiw *::after{box-sizing:border-box}
      .hiw{font-family:ui-sans-serif,system-ui,-apple-system,"Segoe UI",Roboto,sans-serif;line-height:1.5}
      .hiw h1,.hiw h2,.hiw h3,.hiw h4,.hiw p,.hiw figure,.hiw blockquote{margin:0}
      .hiw ul,.hiw ol{margin:0;padding:0;list-style:none}
      .hiw img,.hiw svg,.hiw video{display:block;max-width:100%}

Then the block's own shape. Cover these and nothing more.

- The section: background color, text color, horizontal and vertical padding.
- The inner wrapper: maximum width, centered. Name the wrapper class you actually used, not one from this example.
- Every row, column or grid the layout depends on: display, columns, gap.
- The heading and the body copy: font size, weight, line height.
- Shapes a reader would notice at a glance: border radius, border color.

      .hiw{background-color:rgb(var(--hiw-surface));color:rgb(var(--hiw-ink));padding:5rem 1.25rem}
      .hiw .hiw-wrap{max-width:64rem;margin-inline:auto}
      .hiw .hiw-row{display:grid;gap:2.5rem}
      .hiw h2{font-size:2.25rem;line-height:1.15;font-weight:600}
      .hiw h3{font-size:1.25rem;font-weight:600}
      .hiw p{font-size:1rem;line-height:1.625}
    }

About twelve declarations in all. Do not try to restate every utility you used. This is the block's skeleton, not a second copy of Tailwind. The exact spacing scale, the small type sizes and the hover states stay in the utilities where they belong.

**Nothing else is assumed.** The block does not expect the page to define a color, a font, a helper class or a shared stylesheet. If the page loads Tailwind the block is complete; if it does not, the base layer keeps it readable and in shape.

This skill is self-contained. Do not read, reference or depend on any other skill, and do not assume that a shared stylesheet, a motion system, a component library or a set of helper classes exists somewhere. If something is not described here, it is not available.

## What it is for

Three to five steps showing what happens after the visitor acts. It removes the fear of the unknown, which is usually the real reason someone does not get in touch.

## When to use it

- The process is unfamiliar, or feels risky to a first time buyer.
- The steps are genuinely simple. If they are not, the honest answer is to simplify the process, not the copy.
- There are three to five steps. Six is already too many.

## When not to use it

- The process is obvious. Nobody needs three steps explaining how to add something to a basket.
- The steps have dates or a history. That is a timeline.
- Each step needs a paragraph of detail. Split it into its own section.

## Where it belongs on the page

Upper middle, straight after the offer is understood and before proof. It converts interest into a next action.

## What may be changed

- Number of steps, their copy and the icons or numbers used.
- Orientation, a horizontal row or a vertical list.
- Connector style, dashed, solid or absent.
- Whether a call to action closes the section.

## What must not be changed

- The list being a real ordered list, so the sequence survives without styling.
- The connector never being drawn after the last step, which otherwise implies a missing sixth step.
- Step numbers being decorative in the markup, since the ordered list already conveys order.
- The reduced motion rules.

## Settings and Controls

| Setting | Options | Default |
| --- | --- | --- |
| Step count | 3 to 6 | 3 |
| Step numbering | Numbers, letters, icons, or none | numbers |
| Direction | Horizontal, vertical, or zigzag | horizontal |
| Icon or image | Icon per step, screenshot per step, illustration per step, or none | icon |
| Content length | Title only, title with a line, or title with a paragraph | title with a line |
| Navigation behavior | Manual, scroll progression, automatic progression, or click progression | scroll progression |
| Show or hide | Step titles, descriptions, icons, screenshots, progress indicator, final CTA | titles, descriptions, icons, final CTA |
| Mobile visibility | One step at a time, or all steps | all steps |

**Three steps is the number that gets read.** Four is fine. Six is the ceiling, and at six the content length must drop to title-only or the section becomes a wall.

**Automatic progression needs a pause control.** A process that advances on its own while the reader is still on step one is taking the page away from them. It must pause on hover and on focus, and stop when the section leaves the viewport.

**Scroll progression is not the same as automatic progression.** Scroll progression is driven by the reader and needs no pause control. Automatic runs on a timer and always does.

## Creative Design Options

**Layout**

- Horizontal steps. Steps run left to right with a connector between them. The default, for three or four steps.
- Vertical steps. Steps run down the page. Better for longer descriptions and for five or six steps.
- Zigzag process. Steps alternate side to side, each with its own visual.
- Circular process. Steps arranged around a circle, for a loop with no end, such as a retainer cycle.
- Timeline. Steps on a dated or lettered spine.
- Sticky step-by-step story. The visual pins while the steps scroll past, and the visual changes with each step.

**Motion**

Connector line draws, step activates, icon pops, supporting image changes, or progress advances.

**Styling**

Numbered gradient circles, alternating color steps, illustrated scenes, card path, or roadmap treatment.

**Interaction**

Clickable steps, hover explanations, rewind, previous/next controls, or synchronized image and copy.

## Motion

- **Entrance:** steps fade and rise in order, staggered by 60ms at `standard` intensity. The stagger follows the process order, which is the point of it.
- **Connector line draws** as the reader reaches it, using a stroke dash on an SVG path. It draws once, in order, and never loops.
- **Step activates** means the current step brightens and the others dim. Change opacity and color only. Do not resize an active step, because the neighboring steps shift when it grows.
- **Icon pops** once on the step becoming active, at `subtle` intensity.
- **Supporting image changes** crossfades between stacked images, so the box never goes empty.
- **Progress advances** fills a bar or ring in step with the active step.

## Responsive and Accessibility

| Width | Behavior |
| --- | --- |
| Desktop, 1024px and up | Full layout as chosen. Connectors and sticky visuals active |
| Tablet, 768px to 1023px | Horizontal becomes two rows, or vertical. Circular becomes vertical. Connectors are dropped where they no longer point anywhere |
| Mobile, under 768px | Vertical, one column, connector running down the left edge. Sticky is switched off. If one-step-at-a-time is set, previous and next controls are shown |

- The steps are an ordered list, `<ol>`, so the order is carried in the markup and not only in the styling.
- Step numbers rendered as decoration are `aria-hidden`, because the list already numbers them.
- Clickable steps are real buttons with a pressed state, and the active step is announced.
- Previous/next controls are buttons with labels, not bare arrow glyphs.
- Connector lines and decorative paths are `aria-hidden`.
- With reduced motion, line draws render complete, icon pops are removed, automatic progression does not start, and every step shows in its resting state.

## Defaults

Three steps, numbered, horizontal, icon per step, title with a line, scroll progression, connector shown, final CTA shown, numbered gradient circles, all steps visible on mobile, staggered fade-and-rise entrance, connector draw once.

## What breaks this section

1. **A connector after the last step.** A line pointing off the edge of the section makes the process look truncated.
2. **Scaling the active step.** Its neighbors shift every time the active step changes, and the whole row jitters as the reader scrolls.
3. **Automatic progression with no pause.** The reader loses their place while reading, and there is no way to go back.
- **An icon with no size of its own.** `<svg class="h-4 w-4" viewBox="0 0 20 20">` is 16px only while Tailwind is loaded. Strip the stylesheet away and the SVG has no intrinsic size and stretches to the width of whatever contains it: measured at 900 by 900 inside a 900px section, with the rest of the block pushed off the screen. Fix it with a `width` and a `height` attribute on the tag, and a base layer rule for any icon used more than once.
- **A height attribute on an illustration that fills its column.** The reverse mistake, and it shows up on a page that has Tailwind as well as one that does not. The width follows the column while the height is pinned to the attribute, so the drawing is squashed: measured at 656 by 380 where it should have been 656 by 519. An svg whose width follows its container keeps its height free.

## What a static block cannot do

These blocks carry no JavaScript. A few of the options above have no static equivalent, so build the nearest thing that does work and leave the rest out. Never ship a control that looks alive and does nothing when it is pressed.

- Every option here is buildable with no script. The progress indicator is static markup and the steps need no interaction.

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
| `--hiw-surface` | card and panel faces |
| `--hiw-band` | the quiet band behind the content |
| `--hiw-ink` | headings and primary copy |
| `--hiw-soft` | supporting copy |
| `--hiw-accent` | the one emphasis colour |
| `--hiw-deep` | the dark ground |

The values are channel triplets, `15 23 42` rather than `#0f172a`, so an opacity suffix still works:
`rgb(var(--hiw-ink) / .70)`.

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

A tint such as `rgb(var(--hiw-accent) / .10)` is mostly the colour behind it, so judge contrast
against the composited result, not against the accent itself.
