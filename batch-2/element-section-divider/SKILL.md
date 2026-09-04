---
name: section-divider
description: A visual separator placed between content inside a section. Use for a full width rule, a short centered rule, a left accent, a gradient line, a dotted line, a labelled divider, or an icon divider. Not for the boundary between two sections, which is spacing and background, not a line.
---

# Section Divider

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
- Everything the block needs lives inside that one element, including its own `<style>`. That stylesheet is expected, not a last resort. Tailwind utilities carry most of the work, but a relationship between elements cannot be written as a utility, and the ones this set depends on all live in the stylesheet: `:checked ~`, `details[open]`, `:target`, `@keyframes` and the `prefers-reduced-motion` rule. Prefix your own class names so they cannot collide with the rest of the page.
- The block carries its own padding and its own maximum width. Drop that wrapper and the content runs to both edges of the screen.
- Color an SVG with a presentation attribute as well as a class. `<rect fill="#f6f7f9" class="fill-muted">` takes the theme color where the stylesheet is loaded and falls back to the attribute where it is not. With only the class, an unresolved fill computes to solid black and the placeholder becomes a black rectangle sitting over the layout.
- Write `fill="none"` on any path that is stroke only. A path with no fill is filled solid black. That is the SVG default, not a fault in the browser.
- **Any colour that carries readability goes in the block's own CSS, not only in a class.** `text-white` on a dark panel is nothing without Tailwind: the ground stays dark, the words fall back to the inherited ink, and the panel reads as empty. The same goes for a light colour on an accent button. Set `color` and `background-color` in the base layer for anything where losing the stylesheet would make the text disappear, and keep the utility class as well.
- **Text laid over a picture carries its own solid ground.** A caption or a label on an image gets its own `background-color` with an alpha, not a gradient and not a separate overlay behind it. A gradient is a background image and a sibling scrim is not an ancestor, so neither is visible to a contrast check, and neither survives the picture being swapped for a pale one. Two captions measured 1.07 to 1 this way.
- Give every **icon** a `width` and a `height` attribute as well as its size classes. An SVG that carries only a `viewBox` has no size of its own, so `h-4 w-4` is the only thing holding a 16px icon at 16px. Measured in a plain HTML viewer with no Tailwind, that same icon rendered at 900 by 900 and swallowed the section around it. The attribute is the weakest thing on the page, so Tailwind still wins wherever it is loaded, and the attribute only shows through where it is not. Size any icon that repeats in the base layer as well.
- Do the opposite for an **illustration that fills its column**. An SVG set to the full width of its container takes its height from the viewBox ratio, and a `height` attribute overrides that and squashes the drawing: measured, one illustration went from 656 by 519 to 656 by 380. Leave its height free, or pair the attributes with `height:auto` in the base layer. The test is simple: a fixed size gets both attributes, a width that follows the column does not get a height.

**Write the block's own base layer.** Tailwind utilities carry the detail, but a plain HTML viewer has no Tailwind, and a block built only from utilities arrives there as unstyled text on a white page. So write the few declarations that carry the block's shape into its own stylesheet as well.

Put them inside `@layer edv-base { }`. This matters: a layered declaration loses to an unlayered one whatever its specificity or its order, so Tailwind on a real page still wins every one of them, and the layer only shows through where there is no Tailwind. Without the layer, a rule such as `.edv h2` would outrank `.text-4xl` and the block would start overriding the page.

Start with the five reset lines. Tailwind normally supplies these, and without them the list markers show through, the type falls back to a serif, and the content runs wider than the screen. That was measured: a first attempt without them put a block 36px past the right edge of a 1280px viewport.

    @layer edv-base {
      .edv,.edv *,.edv *::before,.edv *::after{box-sizing:border-box}
      .edv{font-family:ui-sans-serif,system-ui,-apple-system,"Segoe UI",Roboto,sans-serif;line-height:1.5}
      .edv h1,.edv h2,.edv h3,.edv h4,.edv p,.edv figure,.edv blockquote{margin:0}
      .edv ul,.edv ol{margin:0;padding:0;list-style:none}
      .edv img,.edv svg,.edv video{display:block;max-width:100%}

Then the block's own shape: the section's background, text color and padding, the wrapper's maximum width, every row or grid the layout depends on, and the heading and body type sizes.

      .edv{background-color:rgb(var(--edv-surface));color:rgb(var(--edv-ink));padding:3rem 1.25rem}
      .edv .edv-wrap{max-width:64rem;margin-inline:auto}
      .edv .edv-row{display:grid;gap:1.5rem}
      .edv h2{font-size:1.875rem;line-height:1.2;font-weight:600}
      .edv h3{font-size:1.125rem;font-weight:600}
      .edv p{font-size:1rem;line-height:1.625}
    }

That is the skeleton, and it stays short. The detail, the spacing and the smaller responsive steps still come from the utilities.

**Then, outside the layer, declare the six colour variables.** Nothing else defines them. A block that writes `rgb(var(--edv-surface))` without declaring `--edv-surface` has written an invalid declaration and the browser throws the whole line away. Measured with no stylesheet, a block that skipped this had a transparent section, a card with no face, and pure black headings, quiet copy and marks. **The customer's colours go in these six values.** That is where a brief asking for a red and brown clinic actually lands, and it is the only place any colour is decided.

    .edv{
      --edv-surface:255 255 255;
      --edv-band:246 247 249;
      --edv-ink:15 23 42;
      --edv-soft:92 107 129;
      --edv-accent:37 99 235;
      --edv-deep:11 18 32;
      background-color:rgb(var(--edv-surface));
      color:rgb(var(--edv-ink));
    }

This part is not in the layer, on purpose. Tailwind has no rule for a class name prefixed `edv-`, so there is nothing to conflict with, and a layered rule would lose to a stray utility sitting on the same element.

**Then write out every named class the block cannot be read without.** A card or panel face, its border, its radius and its padding. Any ground that is not the section's own. Any text that is not the default ink. The accent on a mark, a rule or a button. The keyframes, the media queries that build the columns, and the reduced-motion rule. Every one of those is a utility on the markup as well, and a utility is nothing in a viewer with no Tailwind. Measured on a block that left them out: three cards with no face and no border, black marks, and all three at full width down the page instead of three across.

    .edv .edv-card{background-color:rgb(var(--edv-band));border:1px solid rgb(var(--edv-ink)/.10);
      border-radius:1rem;padding:1.5rem}
    .edv .edv-mark{color:rgb(var(--edv-accent))}
    .edv .edv-soft{color:rgb(var(--edv-soft))}
    @media (min-width:640px){.edv .edv-row{grid-template-columns:repeat(2,minmax(0,1fr))}}

Those class names are an example of the shape, not a list to copy. Name the ones this block actually contains.

**The test before you hand it over:** picture every `class` attribute deleted. What is left has to still be this block, in the customer's colours, with its faces and its columns. If it collapses to a column of black text on white, the stylesheet is too short.

**Nothing else is assumed.** The block does not expect the page to define a color, a font, a helper class or a shared stylesheet.

This skill is self-contained. Do not read, reference or depend on any other skill, and do not assume that a shared stylesheet, a motion system, a component library or a set of helper classes exists somewhere. If something is not described here, it is not available.

## What it is for

A visual separator placed between content inside a section.

A pause between two parts of the same section, when spacing alone is not enough to say that one idea has ended.

## When to use it

Between two groups inside one section that would otherwise run together, and above a label that introduces a new group.

## When not to use it

Between sections, where a change of background and generous spacing does the job better. Not between every pair of items either, which turns a page into a grid of boxes.

## Where it belongs on the page

Inside the section, with equal space above and below it, so it reads as a pause rather than as a lid on what came before.

## What may be changed

The width, the thickness, the style, the alignment, and whether a label or an icon sits in the line.

## What must not be changed

A purely decorative divider must not be announced. A plain `<hr>` is announced as a separator, which is correct when it really divides content; a decorative rule made of a div takes `aria-hidden`. Do not use a divider to carry meaning that the words should carry.

## Settings and Controls

- Set width, thickness, alignment, spacing, orientation, and whether a label or icon is included.
- Choose solid, dashed, dotted, or custom separator source.

## Creative Design Options

- **Style.** Solid line, gradient line, dots, wave, hand-drawn stroke, illuminated line, or icon divider.
- **Motion.** Draw on scroll, expand from center, color sweep, glow pass, or icon arrival.
- **Layout.** Full width, short centered divider, left accent, vertical divider, or labeled divider.

## Motion

The line draws from the centre outwards over about 500ms on entrance. A label in the line fades in slightly after the line reaches it. Under `prefers-reduced-motion` the line is full width immediately.

## Responsive and Accessibility

Full width below 768px whatever the setting, because a short centered rule at phone width is barely a dash. A labelled divider keeps at least 1rem of space either side of the label so the words never touch the line.

## Defaults

Short centered rule, one pixel, hairline color, draw from centre.

## What breaks this block

A divider built from a border on the element above, which then moves when that element is reordered. A label sitting on the line with no background behind it, so the line runs through the text. A decorative rule left announced, which makes a screen reader say separator between every paragraph.
- **An icon with no size of its own.** `<svg class="h-4 w-4" viewBox="0 0 20 20">` is 16px only while Tailwind is loaded. Strip the stylesheet away and the SVG has no intrinsic size and stretches to the width of whatever contains it: measured at 900 by 900 inside a 900px section, with the rest of the block pushed off the screen. Fix it with a `width` and a `height` attribute on the tag, and a base layer rule for any icon used more than once.
- **A height attribute on an illustration that fills its column.** The reverse mistake, and it shows up on a page that has Tailwind as well as one that does not. The width follows the column while the height is pinned to the attribute, so the drawing is squashed: measured at 656 by 380 where it should have been 656 by 519. An svg whose width follows its container keeps its height free.
- **A colour variable that is used but never declared.** `rgb(var(--edv-accent))` with no `--edv-accent` on the block is an invalid declaration, and an invalid declaration is not a fallback to something sensible: the browser throws the line away. Measured on a block that did this, the section had no background, the cards had no face and every piece of text came out pure black, including the parts meant to carry the brand colour.
- **A card that exists only as utility classes.** `class="rounded-2xl border bg-white p-6"` is a card while Tailwind is loaded and nothing at all without it. Measured: no face, no border, no radius, no padding, and the three cards stacked at full width instead of sitting three across. Anything a reader would notice was missing goes in the block's own stylesheet as well as on the markup.

## What a static block cannot do

A colour sweep along the line and a glow pass repeat forever unless they are entrance only, which is what to build. A hand drawn stroke is an inline SVG path, not a script.

## Images

Images come from the customer's brief: their own photographs, their own logo, the artwork they supplied. If the brief carries none, use the placeholder that ships inside this block, which is inline SVG and needs no request.

Never reach for a stock photo address. Unsplash, Pexels, Pixabay, a CDN, a hotlinked image on someone else's site: all of them are external files, all of them are ruled out here, and a broken address leaves a hole in the layout with no warning.

Give every image an alt description that says what is in it. Decorative artwork takes `alt=""` so a screen reader passes over it rather than reading a filename.

## Color

The block carries its own color. Six variables are declared on the block itself, and every colored part reads one of them.

| Variable | Used for |
| --- | --- |
| `--edv-surface` | card and panel faces |
| `--edv-band` | the quiet band behind the content |
| `--edv-ink` | headings and primary copy |
| `--edv-soft` | supporting copy |
| `--edv-accent` | the one emphasis color |
| `--edv-deep` | the dark ground |

The values are channel triplets, `15 23 42` rather than `#0f172a`, so an opacity suffix still works: `rgb(var(--edv-ink) / .70)`.


Declare all six on the block itself, and give them real values. They are not defined anywhere else, and a colour read from a variable that was never declared is an invalid declaration: the browser discards the line and that part renders with no colour at all. Where the brief names the customer's colours, those colours go into these six values and nowhere else.

    .edv{--edv-surface:255 255 255;--edv-band:246 247 249;--edv-ink:15 23 42;
      --edv-soft:92 107 129;--edv-accent:37 99 235;--edv-deep:11 18 32}

Rebrand the block by changing those six values, in one place. Do not spread hex values through the markup, and do not reach for a color name out of the page's Tailwind config. Every page the builder makes writes its own config with its own color names, so a name taken from one page resolves to nothing on the next. Measured on two live sites: `bg-surface` painted no background on either, and `muted` was a pale background on one and a dark text color on the other.

Do not invent CSS variable names such as `var(--color-surface)`. Nothing defines them, the browser throws the declaration away, and that color or border renders as nothing at all. Do not use `text-current/60` either, because an opacity modifier has no effect on `currentColor`.

Every piece of text has to reach 4.5 to 1 against the color actually behind it, which is the nearest ancestor that paints a background rather than the page. Large text, meaning 24px, or 18.66px when bold, needs 3 to 1. With the default values, ink on deep measures 1.05 to 1, accent on deep 3.62 to 1 and soft on deep 3.45 to 1, so none of those three belong on a dark panel. On a dark panel use the surface value for text. Soft measures 5.06 to 1 on the band and 5.42 on surface, and white on the accent measures 5.17 to 1.

A tint such as `rgb(var(--edv-accent) / .10)` is mostly the color behind it, so judge contrast against the composited result.
