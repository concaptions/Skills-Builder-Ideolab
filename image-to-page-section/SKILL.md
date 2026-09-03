---
name: image-to-page-section
description: Turn a reference picture of a section into a working page section. Use when the brief carries a screenshot, a design export, a photograph of a printed page or a hand sketch, and the section it shows has to be rebuilt in HTML. Not for using a picture as the section's artwork, and not for cloning a whole page.
---

# Image to Page Section

## What you must return

One `<section>` element, and nothing else.

- No `<!doctype>`, `<html>`, `<head>` or `<body>`. The block is dropped into a page that already exists.
- No markdown code fence around it, and no commentary before or after it. Return the HTML itself. A leftover fence marker renders as visible text on the page, and inside a grid it becomes an extra item that pushes the layout out of shape.
- No `<script>`, no event handlers, no `hidden` attributes waiting to be toggled. Nothing here can run.
- No external files of any kind: no image addresses, no icon library, no web font. The reference picture itself is never shipped inside the block.
- Everything the section needs lives inside that one element, including its own `<style>` block. That stylesheet is expected, not a last resort. Tailwind utilities carry most of the work, but a relationship between elements cannot be written as a utility, and the ones this set depends on all live in the stylesheet: `:checked ~`, `details[open]`, `:target`, `@keyframes`, `animation-timeline` and the `prefers-reduced-motion` rule. Prefix your own class names so they cannot collide with the rest of the page.
- The section carries its own horizontal padding and its own maximum width. Drop that wrapper and the content runs to both edges of the screen.

This skill is self-contained. Do not read, reference or depend on any other skill, and do not assume that a shared stylesheet, a motion system, a component library or a set of helper classes exists somewhere. If something is not described here, it is not available.

## What it is for

Rebuilding one section from a picture of it. The customer has shown what they want rather than described it: a screenshot of a competitor, a design export, a photograph of a brochure page, a sketch on paper. The job is to read the structure out of that picture and build it properly, in the customer's own colors and words.

The picture is a description of a layout. It is not the artwork, and it is not the copy.

## When to use it

- The brief carries an image and asks for that section on the page.
- The customer has said "like this" and attached something.
- A section already exists somewhere in a form nobody can edit, such as a PDF or a flattened export, and it has to become a real page section.
- Part of a larger screenshot needs rebuilding, and the brief has said which part.

## When not to use it

- The image is the content. A photograph that belongs in the section is an image, not a layout to copy. Use the section skill that fits what is being shown.
- A whole page has been sent. This rebuilds one section. A full page screenshot is a list of sections, each one its own job, and the brief has to say which.
- A named section already covers it. If the picture is plainly a three step process, an FAQ or a pricing table, the skill for that section is a better starting point and already carries its options and failure modes. Use this one when the picture does not match anything in the library, or when it matches but with a treatment the library does not offer.
- The picture is unreadable. If the type is too small to read or the layout too compressed to count, say so and ask for a larger copy rather than guessing.

## Where it belongs on the page

Wherever the reference sat, judged from the picture rather than assumed.

- If the picture shows the browser's own chrome, or a menu bar along the top edge, the section is at the top of a page and probably below a hero.
- If it shows a footer beneath it, it is the last section.
- If the picture is a crop with no landmarks, place it by its content: a story goes early, proof and pricing go later, a call to action goes last.

Never place two rebuilt sections next to each other on the strength of one screenshot showing them together. They may have had a third between them that was cropped out.

## What may be changed

- Every word. The copy is rewritten for the customer's business.
- Every color. The reference's palette is replaced by the page's theme.
- The typeface. The page's own font is used.
- Exact spacing, in favour of the ratios the picture shows.
- Anything the picture shows that needs a script, replaced by the nearest thing that works without one.
- The number of repeated items, if the brief gives a different count from the picture.

## What must not be changed

- The structure. The number of columns, what repeats, the reading order and the alignment axis are the whole reason a picture was sent.
- The relative weight of the elements. If the heading dominates the picture, it dominates the rebuild.
- Which element is the action. If the picture has one button, the rebuild has one button, in the same place in the reading order.
- The shape of any media. A portrait photograph stays portrait, a wide banner stays wide, and the placeholder holds that shape.
- The reduced motion rules.

## Settings and Controls

| Setting | Options | Default |
| --- | --- | --- |
| Fidelity | Structure only, structure and proportion, or close visual match | structure and proportion |
| Copy source | Rewrite for the customer, or use the brief's supplied copy | rewrite for the customer |
| Column count | Read from the picture, or overridden by the brief | read from the picture |
| Repeated items | Read from the picture, or a count given by the brief | read from the picture |
| Media placeholders | Inline placeholder, or the customer's supplied image | inline placeholder |
| Alignment | Read from the picture | read from the picture |
| Width | Contained, wide, or full bleed, read from the picture | contained |
| Motion | None, or a standard fade and rise entrance | fade and rise entrance |

**Fidelity is a decision, not a dial to turn up.** *Close visual match* is right when the customer owns the reference, for example their own design export. It is the wrong answer when the reference is somebody else's website, because a close match to a competitor's page is both a legal risk and a bad look. Default to structure and proportion, which gives the customer the layout they asked for in their own brand.

**Reading a count from the picture beats guessing a nicer one.** Four cards shown means four cards, even if three would sit better. If the brief gives a different number, the brief wins.

## Creative Design Options

**How to read the picture, in order**

1. Find the section's edges. What is inside this section and what belongs to the one above or below it.
2. Count what repeats. Cards, rows, steps, logos, list items. A repeated unit is the thing to build once and loop.
3. Name every part. Eyebrow, heading, body, list, media, action, caption, badge, number.
4. Find the axis. Left aligned, centred, or split down the middle.
5. Read the order. What a visitor's eye meets first, second, third. That is the order in the markup, whatever the visual arrangement does on a wide screen.
6. Take the ratios, not the pixels. The heading is roughly three times the body. The gap between cards is roughly half the padding inside them. The media takes about two fifths of the width. Ratios survive a change of screen; pixels do not.

**Layout families you will recognise**

- Copy one side, media the other.
- A centred heading over a row of repeated cards.
- A wide media panel with copy sitting over it.
- A list of rows, each with a small mark and a line of text.
- A split panel, two halves of equal weight with a dividing line.
- A stack of numbered steps.
- A table of values.

Build the one the picture shows. Do not upgrade it.

**Treatments to look for**

Rounded corners and how far. Borders, hairline or heavy. A shadow, and whether it is a soft lift or a hard offset. A panel a shade off the page background. A coloured chip or badge. A rule above the heading. Numbers set large and quiet behind the content.

**What to leave behind**

Gradients you cannot name, glass and blur effects, drop shadows heavy enough to read as a border, and any texture that came from the reference being a photograph of a screen rather than a screenshot of one. When in doubt, build it flat. A flat rebuild looks deliberate. A half copied effect looks broken.

## Motion

- **Entrance:** a fade and rise of roughly 16px over 420 to 460ms, staggered by 60ms across repeated items in reading order, at `standard` intensity.
- Do not add motion the picture does not imply. A still picture is not asking for movement, and a section that animates more than the rest of the page draws the wrong kind of attention.
- If the reference is a frame from a video or an animated export, the movement it shows is very likely scroll-linked or script-driven. Read the section below before promising it.
- With reduced motion asked for, the entrance is dropped and everything renders in its finished position.

## Responsive and Accessibility

| Width | Behavior |
| --- | --- |
| Desktop, 1024px and up | The arrangement the picture shows |
| Tablet, 768px to 1023px | Multi column grids drop to two. A side by side split stacks if either half falls below about 22rem |
| Mobile, under 768px | One column, in the reading order named above. Media above copy unless the picture makes the copy the first thing |

- A screenshot is a single width. It says nothing about narrow screens, so the narrow layout is a judgement, not something to be read off the picture. Stack in reading order and let the type wrap.
- Headings use a real heading level in sequence. A large word in the picture is not automatically an `h2`; decide by what it means on the page, not by how big it is.
- Text in the picture that sits over a photograph needs a scrim behind it in the rebuild, and the contrast is measured with the scrim applied.
- Anything the picture shows as a control is a real control: a link or a form input, reachable by keyboard, with a visible focus ring.
- A decorative mark from the reference takes an empty `alt` attribute, or is drawn in CSS and left out of the accessibility tree.

## Defaults

Structure and proportion fidelity, copy rewritten for the customer, column count and item count read from the picture, alignment read from the picture, contained width, inline placeholders for all media, the page's theme tokens for every color, one action, standard fade and rise entrance staggered across repeated items.

## What breaks this section

1. **Eyedropping the colors out of the picture.** A hex sampled from a screenshot is that business's brand, hardcoded into this customer's page. It looks right on the one page it was copied from and wrong everywhere else, and it cannot follow the theme. Read the roles instead, and use the tokens.

2. **Shipping the reference's own words.** Another company's name, prices, claims and testimonials, live on the customer's site. This is the most common and most damaging mistake, and it survives review because the layout looks correct. Rewrite every line for the customer's business.

3. **Text that is part of a picture, rebuilt as a picture.** A headline baked into a photograph cannot be read by a screen reader, cannot be found by a search engine, cannot be translated and blurs when the screen scales. Rebuild it as real text over the image.

4. **Cropping the reference and using it as the artwork.** The reference is somebody's screenshot. Putting a piece of it into the section ships their content and their image rights along with it. Media positions get the block's own placeholder until the customer's photograph arrives.

5. **One fixed width.** Matching the screenshot's pixel dimensions produces a section that is correct at exactly one screen size and broken at every other. Build in relative units from the ratios.

6. **Rebuilding a whole page as one section.** A full page screenshot contains a hero, several sections and a footer. Built as one block it becomes an unmaintainable slab, and none of it can be reordered or reused. Ask which section is meant.

7. **Recreating a control the picture only implies.** A carousel arrow, a play button, a tab row or a counter in a screenshot look like features. Built as markup with no script behind them they are dead: they render perfectly and do nothing when pressed. Either build the version that works without a script, or leave the control out. Never ship one that only looks alive.

8. **Matching the spacing pixel for pixel.** Screenshots are taken at different zoom levels and pixel ratios, so the numbers in them are not the numbers on the page. Use the ratios and the page's own spacing scale.

## What a static block cannot do

These blocks carry no JavaScript. A reference picture very often shows something that needs one, because the picture was taken from a site that had one. Build the nearest thing that does work and leave the rest out. Never ship a control that looks alive and does nothing when it is pressed.

- **A carousel with arrows that move it.** Build the rail as a real scroll container with snapping, and make the arrows links pointing at each item's id. The browser then does the scrolling. A button with no handler does nothing.
- **Tabs the picture shows as buttons.** Build them as a radio group with labels, and match the panel with the checked input. Buttons cannot switch anything here.
- **A number counting up.** There is no static equivalent. Print the final value as text. It is the number that matters, not the counting.
- **A countdown, a timer, or anything that appears after a delay.** Not available. State the deadline or show the content from the first moment.
- **A modal or a lightbox.** `:target` can open an overlay, but it cannot trap focus, cannot close on Escape and cannot return focus to what opened it, so a keyboard visitor is left with no way back. Leave it out and show the content in place.
- **Mouse dragging.** Not available. A real scroll container gives touch, trackpad and keyboard for free.

**Scroll-linked motion.** If the picture shows something part way through a scroll effect, that effect uses `animation-timeline: view()` inside an `@supports` block. Firefox does not support it, so write the finished state as the default and let the animation be the enhancement. Never use a library or an observer for this.

## Images

Every image belongs to the customer. Use the photographs, logos and artwork supplied in the brief.

The reference picture is not one of them. It is a layout description, and it is very likely somebody else's property. It never appears in the block, whole or cropped.

Never pull a photograph from a stock site. A link to Unsplash, Pexels, Shutterstock or any other outside address is wrong on two counts: it is not the customer's business in the picture, and the block stops working the moment that address changes.

If no photograph has been supplied yet, keep the placeholder that ships inside the block. It is drawn in the page itself, so it needs nothing from outside, it takes the theme colors, and it holds the shape the reference showed, so the layout does not move when the real photograph arrives.

Give every image an `alt` description of what is in it, and a `width` and `height`, so the page does not jump as it loads.

## Color

Use the page's theme tokens. Never hardcode a hex value or a Tailwind palette color, and never sample one out of the reference picture, because this section has to work on a dark editorial page and a light clinical one without being rewritten.

The tokens are `surface`, `muted`, `textdark`, `textmute`, `accent` and `bgdark`. Use them as Tailwind classes: `bg-surface`, `bg-muted`, `bg-bgdark`, `text-textdark`, `text-textmute`, `text-accent`, `border-accent`.

Read the picture for roles, not for colors. The page ground is `bg-surface` if the reference is light and `bg-bgdark` if it is dark. A panel sitting a shade off that ground is `bg-muted`. The one saturated color the reference uses for its button and its small emphases is `accent`. Headings are `textdark`, quieter copy is `textmute`.

Do not invent CSS variable names such as `var(--color-surface)` or `var(--color-border)`. Nothing on the page defines them, so the browser throws the declaration away and that color, border or shape renders as nothing at all. Do not use `text-current/60` either, because an opacity modifier has no effect on `currentColor` and the text comes out at full strength.

A token is not readable just because it is a token. Every piece of text has to reach 4.5 to 1 against the color actually behind it, which is the nearest ancestor that paints a background rather than the page. Large text, meaning 24px, or 18.66px when it is bold, needs 3 to 1.

These four pairs are measured and they fail, so do not reach for them.

| Text | Background | Measured |
| --- | --- | --- |
| `text-textdark` | `bg-bgdark` | 1.05 to 1, near black on near black |
| `text-accent` | `bg-bgdark` | 3.62 to 1, the accent is a dark blue and it sinks into a dark panel |
| `text-textmute` | `bg-bgdark` | 3.93 to 1 |
| `text-textmute` | `bg-muted` | 4.44 to 1, although the same token clears on `bg-surface` at 4.76 to 1 |

On a dark panel, set text in `text-surface` and use a lowered opacity of it for the quieter line. The accent still works there as a button's background, where white sits on it at 6.3 to 1. On a muted panel, use a lowered opacity of `text-textdark` for quiet copy.

A tint such as `bg-accent/10` is mostly the color behind it, so judge contrast against the composited result, not against the accent itself.
