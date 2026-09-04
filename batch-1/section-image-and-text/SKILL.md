---
name: section-image-and-text
description: A general-purpose storytelling section pairing written content with a strong visual. Use for image-left or image-right rows, an overlapping image and card, a full-bleed visual, text over an image, or alternating rows down a page. The workhorse section for any point that needs one picture and a paragraph.
---

# Image and Text

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

Put them inside `@layer iat-base { }`. This matters: a layered declaration loses to an unlayered one whatever its specificity or its order, so Tailwind on a real page still wins every one of them, and the layer only shows through where there is no Tailwind. Without the layer, a rule such as `.iat h2` would outrank `.text-4xl` and the block would start overriding the page.

Start with the five reset lines. Tailwind normally supplies these, and without them the list markers show through, the type falls back to a serif, and the content runs wider than the screen. That was measured: a first attempt without them put a block 36px past the right edge of a 1280px viewport.

    @layer iat-base {
      .iat,.iat *,.iat *::before,.iat *::after{box-sizing:border-box}
      .iat{font-family:ui-sans-serif,system-ui,-apple-system,"Segoe UI",Roboto,sans-serif;line-height:1.5}
      .iat h1,.iat h2,.iat h3,.iat h4,.iat p,.iat figure,.iat blockquote{margin:0}
      .iat ul,.iat ol{margin:0;padding:0;list-style:none}
      .iat img,.iat svg,.iat video{display:block;max-width:100%}

Then the block's own shape. Cover these and nothing more.

- The section: background color, text color, horizontal and vertical padding.
- The inner wrapper: maximum width, centered. Name the wrapper class you actually used, not one from this example.
- Every row, column or grid the layout depends on: display, columns, gap.
- The heading and the body copy: font size, weight, line height.
- Shapes a reader would notice at a glance: border radius, border color.

      .iat{background-color:rgb(var(--iat-surface));color:rgb(var(--iat-ink));padding:5rem 1.25rem}
      .iat .iat-wrap{max-width:64rem;margin-inline:auto}
      .iat .iat-row{display:grid;gap:2.5rem}
      .iat h2{font-size:2.25rem;line-height:1.15;font-weight:600}
      .iat h3{font-size:1.25rem;font-weight:600}
      .iat p{font-size:1rem;line-height:1.625}
    }

About twelve declarations in all. Do not try to restate every utility you used. This is the block's skeleton, not a second copy of Tailwind. The exact spacing scale, the small type sizes and the hover states stay in the utilities where they belong.

**Nothing else is assumed.** The block does not expect the page to define a color, a font, a helper class or a shared stylesheet. If the page loads Tailwind the block is complete; if it does not, the base layer keeps it readable and in shape.

This skill is self-contained. Do not read, reference or depend on any other skill, and do not assume that a shared stylesheet, a motion system, a component library or a set of helper classes exists somewhere. If something is not described here, it is not available.

## What it is for

A picture beside a paragraph, usually repeated two or three times with the picture alternating sides. It is the workhorse for explaining several things at a pace the visitor controls.

## When to use it

- There are two to four points that each need a sentence or two plus a visual.
- The points are unrelated enough that a grid would flatten them.
- Real photographs or meaningful visuals exist for each row.

## When not to use it

- There is only one point. That is a single feature panel.
- There are six or more rows. The page becomes a corridor and people stop reading.
- The images are decorative stock. An empty picture beside every paragraph adds length and nothing else.

## Where it belongs on the page

Middle of the page, after the offer and before or around the proof. It is often the main body of a services page.

## What may be changed

- Number of rows, and which side the first image sits on.
- Image aspect and corner treatment.
- Copy, eyebrow, headings and the link at the end of each row.
- Vertical spacing between rows.

## What must not be changed

- The image coming first on mobile in every row. A stack of paragraphs with the image below reads as a wall of text.
- The alternating rule being driven by position rather than marked by hand, so adding or removing a row keeps the rhythm.
- Alt text on every meaningful image.
- The reduced motion rules.

## Settings and Controls

| Setting | Options | Default |
| --- | --- | --- |
| Image position | Left, right, above, behind the text, or alternating per row | right |
| Content width | Narrow, contained, or full width | contained |
| Section height | Auto, fixed, or full viewport | auto |
| Column ratio | Equal, text-led (60:40), or image-led (40:60) | equal |
| Crop | Square, 4:3, 3:2, 16:9, portrait, or original | 3:2 |
| Focal point | Where the crop centers, set as a point on the image | center |
| Padding | Compact, standard, or generous | standard |
| Alignment | Text top, center, or bottom relative to the image | center |
| Show or hide | Eyebrow, headline, paragraph, list, image caption, button, secondary link | headline, paragraph, button |
| Media source | Uploaded image, Brand Gallery image, generated image, background image, or video | uploaded image |
| Mobile stacking order | Image first, or text first | image first |
| Image width on mobile | Full width, or contained | contained |

**Focal point is the setting that stops faces being cropped.** Any crop other than the original ratio will cut something. Set the focal point on the subject, not on the middle of the frame.

**Mobile stacking order is a real decision.** Image first gives the reader something to look at immediately. Text first is right when the image only makes sense after the copy explains it, such as a chart or a diagram.

**Text over an image is a different job from image-behind-text.** Text over an image needs a scrim, a focal point set to empty space, and a fallback color behind it in case the image fails to load.

## Creative Design Options

**Layout**

- Image left. Text right.
- Image right. Text left. The default.
- Overlapping image and card. The copy sits in a card that overlaps the edge of the image.
- Full-bleed visual. The image runs to the edges of the viewport, with the copy contained above or below.
- Text over image. The copy sits on the image behind a scrim.
- Alternating rows. Several of these stacked, with the image swapping side each row.

**Image treatment**

Rounded frame, editorial crop, gradient overlay, cutout subject, collage, shadow, or decorative border.

**Motion**

Hover zoom, parallax, mask reveal, slow pan, floating depth, or image swap.

**Typography**

Word or line reveal, gradient phrase, gold-to-soft-gold line progression, or rolling emphasized text.

The gold-to-soft-gold treatment generalizes: it is a bright-to-soft progression of one brand color across successive lines, so it works on any palette.

## Motion

- **Entrance:** the row fades and rises as one unit at `standard` intensity. The image and text do not arrive separately, because a split entrance draws attention to the seam.
- **Hover zoom** is the default, at 2 to 4 percent.
- **Parallax, slow pan and floating depth** are scroll-linked, so they count as the page's one major showcase effect. A page with three alternating rows must not give all three parallax.
- **Mask reveal** wipes the image in on entrance, once. Use it on one row per page, not every row.
- **Rolling emphasized text** cycles a single word inside the headline. Bound it: either a set number of cycles, or stop when the section leaves the viewport.
- **Word or line reveal** applies to the headline only. Body paragraphs appear immediately.

## Responsive and Accessibility

| Width | Behavior |
| --- | --- |
| Desktop, 1024px and up | Side-by-side at the chosen ratio. Parallax and overlap active |
| Tablet, 768px to 1023px | Side by side at equal ratio, or stacked if the copy is long. Overlap is removed |
| Mobile, under 768px | Stacked in the chosen order. Parallax off. Text over image keeps the scrim and gains extra padding |

- The image needs alt text describing what it shows. A decorative image takes `alt=""`.
- An image caption is a `<figcaption>` inside a `<figure>`, not a loose paragraph.
- Text over an image must meet contrast against the darkest and the lightest part of the image it covers, measured with the scrim applied.
- A background video is muted, carries `playsinline`, has a poster image, and never carries information that is not also in the text.
- With reduced motion, parallax, pan, floating depth and mask reveals are removed, rolling text settles on its first word, and the image appears in place.

## Defaults

Image right, equal ratio, 3:2 crop, center focal point, contained width, auto height, standard padding, center aligned, headline and paragraph and button shown, rounded frame, image first on mobile, standard fade-and-rise entrance, hover zoom.

## What breaks this section

1. **Reordering in the markup instead of with `order`.** It flips the reading order for keyboard and screen-reader users, and it changes what appears first on mobile.
2. **Cropping without a focal point.** Faces lose their tops on the ratio that was not designed for.
3. **Giving every row parallax.** Three parallax rows on one page is the "one major showcase effect" rule broken twice.
- **An icon with no size of its own.** `<svg class="h-4 w-4" viewBox="0 0 20 20">` is 16px only while Tailwind is loaded. Strip the stylesheet away and the SVG has no intrinsic size and stretches to the width of whatever contains it: measured at 900 by 900 inside a 900px section, with the rest of the block pushed off the screen. Fix it with a `width` and a `height` attribute on the tag, and a base layer rule for any icon used more than once.
- **A height attribute on an illustration that fills its column.** The reverse mistake, and it shows up on a page that has Tailwind as well as one that does not. The width follows the column while the height is pinned to the attribute, so the drawing is squashed: measured at 656 by 380 where it should have been 656 by 519. An svg whose width follows its container keeps its height free.
- **A height attribute on an illustration that fills its column.** The reverse mistake, and it shows up on a page that has Tailwind as well as one that does not. The width follows the column while the height is pinned to the attribute, so the drawing is squashed: measured at 656 by 380 where it should have been 656 by 519. An svg whose width follows its container keeps its height free.

## What a static block cannot do

These blocks carry no JavaScript. A few of the options above have no static equivalent, so build the nearest thing that does work and leave the rest out. Never ship a control that looks alive and does nothing when it is pressed.

- **Image parallax** is scroll-linked. See below.

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
| `--iat-surface` | card and panel faces |
| `--iat-band` | the quiet band behind the content |
| `--iat-ink` | headings and primary copy |
| `--iat-soft` | supporting copy |
| `--iat-accent` | the one emphasis colour |
| `--iat-deep` | the dark ground |

The values are channel triplets, `15 23 42` rather than `#0f172a`, so an opacity suffix still works:
`rgb(var(--iat-ink) / .70)`.

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

A tint such as `rgb(var(--iat-accent) / .10)` is mostly the colour behind it, so judge contrast
against the composited result, not against the accent itself.
