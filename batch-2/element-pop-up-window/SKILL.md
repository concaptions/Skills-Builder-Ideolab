---
name: pop-up-window
description: A panel of offer content that opens when the visitor asks for it. Use for a centre panel, a side panel, or a bottom drawer, opened from a link. Not for a modal that opens on a timer, on exit intent, or on scroll depth, none of which can be built without a script.
---

# Pop-Up Window

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

Put them inside `@layer epu-base { }`. This matters: a layered declaration loses to an unlayered one whatever its specificity or its order, so Tailwind on a real page still wins every one of them, and the layer only shows through where there is no Tailwind. Without the layer, a rule such as `.epu h2` would outrank `.text-4xl` and the block would start overriding the page.

Start with the five reset lines. Tailwind normally supplies these, and without them the list markers show through, the type falls back to a serif, and the content runs wider than the screen. That was measured: a first attempt without them put a block 36px past the right edge of a 1280px viewport.

    @layer epu-base {
      .epu,.epu *,.epu *::before,.epu *::after{box-sizing:border-box}
      .epu{font-family:ui-sans-serif,system-ui,-apple-system,"Segoe UI",Roboto,sans-serif;line-height:1.5}
      .epu h1,.epu h2,.epu h3,.epu h4,.epu p,.epu figure,.epu blockquote{margin:0}
      .epu ul,.epu ol{margin:0;padding:0;list-style:none}
      .epu img,.epu svg,.epu video{display:block;max-width:100%}

Then the block's own shape: the section's background, text color and padding, the wrapper's maximum width, every row or grid the layout depends on, and the heading and body type sizes.

      .epu{background-color:rgb(var(--epu-surface));color:rgb(var(--epu-ink));padding:3rem 1.25rem}
      .epu .epu-wrap{max-width:64rem;margin-inline:auto}
      .epu .epu-row{display:grid;gap:1.5rem}
      .epu h2{font-size:1.875rem;line-height:1.2;font-weight:600}
      .epu h3{font-size:1.125rem;font-weight:600}
      .epu p{font-size:1rem;line-height:1.625}
    }

That is the skeleton, and it stays short. The detail, the spacing and the smaller responsive steps still come from the utilities.

**Then, outside the layer, declare the six colour variables.** Nothing else defines them. A block that writes `rgb(var(--epu-surface))` without declaring `--epu-surface` has written an invalid declaration and the browser throws the whole line away. Measured with no stylesheet, a block that skipped this had a transparent section, a card with no face, and pure black headings, quiet copy and marks. **The customer's colours go in these six values.** That is where a brief asking for a red and brown clinic actually lands, and it is the only place any colour is decided.

    .epu{
      --epu-surface:255 255 255;
      --epu-band:246 247 249;
      --epu-ink:15 23 42;
      --epu-soft:92 107 129;
      --epu-accent:37 99 235;
      --epu-deep:11 18 32;
      background-color:rgb(var(--epu-surface));
      color:rgb(var(--epu-ink));
    }

This part is not in the layer, on purpose. Tailwind has no rule for a class name prefixed `epu-`, so there is nothing to conflict with, and a layered rule would lose to a stray utility sitting on the same element.

**Then write out every named class the block cannot be read without.** A card or panel face, its border, its radius and its padding. Any ground that is not the section's own. Any text that is not the default ink. The accent on a mark, a rule or a button. The keyframes, the media queries that build the columns, and the reduced-motion rule. Every one of those is a utility on the markup as well, and a utility is nothing in a viewer with no Tailwind. Measured on a block that left them out: three cards with no face and no border, black marks, and all three at full width down the page instead of three across.

    .epu .epu-card{background-color:rgb(var(--epu-band));border:1px solid rgb(var(--epu-ink)/.10);
      border-radius:1rem;padding:1.5rem}
    .epu .epu-mark{color:rgb(var(--epu-accent))}
    .epu .epu-soft{color:rgb(var(--epu-soft))}
    @media (min-width:640px){.epu .epu-row{grid-template-columns:repeat(2,minmax(0,1fr))}}

Those class names are an example of the shape, not a list to copy. Name the ones this block actually contains.

**The test before you hand it over:** picture every `class` attribute deleted. What is left has to still be this block, in the customer's colours, with its faces and its columns. If it collapses to a column of black text on white, the stylesheet is too short.

**Nothing else is assumed.** The block does not expect the page to define a color, a font, a helper class or a shared stylesheet.

This skill is self-contained. Do not read, reference or depend on any other skill, and do not assume that a shared stylesheet, a motion system, a component library or a set of helper classes exists somewhere. If something is not described here, it is not available.

## What it is for

A modal or triggered content window that can contain other elements.

An offer or a short form kept out of the way until the reader asks to see it, so the page is not interrupted.

## When to use it

When something genuinely secondary needs to be available without taking up room: a discount code, a short enquiry form, a note about delivery.

## When not to use it

Anything the reader must see. Content behind a control is content most readers never open, so the price and the call to action stay on the page.

## Where it belongs on the page

Near the link that opens it, so opening it does not throw the reader somewhere else on the page.

## What may be changed

The wording on the opening link, the panel position, the content inside it, and whether it is a panel or a drawer.

## What must not be changed

It must not open on its own. A panel that appears on a timer or as the pointer leaves the window needs a script, and a panel a keyboard user cannot close is a trap. This one opens from a real link and closes from a real link, both reachable by keyboard.

## Settings and Controls

- Set trigger: button, time, scroll depth, exit intent, page entry, or custom event.
- Set frequency, delay, close behavior, size, position, overlay, and mobile presentation.
- Choose included content: text, image, video, form, offer, countdown, or custom elements.

## Creative Design Options

- **Layout.** Center modal, side panel, bottom drawer, corner pop-up, full-screen takeover, or image-and-form split.
- **Motion.** Fade, scale, slide, flip, spring, blur transition, or background dimming.
- **Styling.** Clean card, image-led offer, gradient panel, dark spotlight, or floating glass window.

## Motion

The panel fades and scales in over about 240ms, and the dimmed backdrop fades with it. Under `prefers-reduced-motion` it simply appears.

## Responsive and Accessibility

Full width with a small margin below 640px, and a bottom drawer shape rather than a centre panel, because a centre panel on a phone leaves nothing visible around it. The close control is at least 44px.

## Defaults

Centre panel, opened from a link, dimmed backdrop, close link top right, drawer shape on a phone.

## What breaks this block

Opening on a timer or on exit intent, which cannot be done here and should not be attempted. A close control smaller than a thumb. A panel with no way out except the browser back button.
- **An icon with no size of its own.** `<svg class="h-4 w-4" viewBox="0 0 20 20">` is 16px only while Tailwind is loaded. Strip the stylesheet away and the SVG has no intrinsic size and stretches to the width of whatever contains it: measured at 900 by 900 inside a 900px section, with the rest of the block pushed off the screen. Fix it with a `width` and a `height` attribute on the tag, and a base layer rule for any icon used more than once.
- **A height attribute on an illustration that fills its column.** The reverse mistake, and it shows up on a page that has Tailwind as well as one that does not. The width follows the column while the height is pinned to the attribute, so the drawing is squashed: measured at 656 by 380 where it should have been 656 by 519. An svg whose width follows its container keeps its height free.
- **A colour variable that is used but never declared.** `rgb(var(--epu-accent))` with no `--epu-accent` on the block is an invalid declaration, and an invalid declaration is not a fallback to something sensible: the browser throws the line away. Measured on a block that did this, the section had no background, the cards had no face and every piece of text came out pure black, including the parts meant to carry the brand colour.
- **A card that exists only as utility classes.** `class="rounded-2xl border bg-white p-6"` is a card while Tailwind is loaded and nothing at all without it. Measured: no face, no border, no radius, no padding, and the three cards stacked at full width instead of sitting three across. Anything a reader would notice was missing goes in the block's own stylesheet as well as on the markup.

## What a static block cannot do

Timed, scroll depth, exit intent and page entry triggers all need a script, as do frequency rules and remembering that a visitor has already seen it. Focus is not trapped inside the panel either, which is why the content behind it must never be hidden from the reader in a way that matters. Build it as an opt in panel, or put the content on the page.

## Images

Images come from the customer's brief: their own photographs, their own logo, the artwork they supplied. If the brief carries none, use the placeholder that ships inside this block, which is inline SVG and needs no request.

Never reach for a stock photo address. Unsplash, Pexels, Pixabay, a CDN, a hotlinked image on someone else's site: all of them are external files, all of them are ruled out here, and a broken address leaves a hole in the layout with no warning.

Give every image an alt description that says what is in it. Decorative artwork takes `alt=""` so a screen reader passes over it rather than reading a filename.

## Color

The block carries its own color. Six variables are declared on the block itself, and every colored part reads one of them.

| Variable | Used for |
| --- | --- |
| `--epu-surface` | card and panel faces |
| `--epu-band` | the quiet band behind the content |
| `--epu-ink` | headings and primary copy |
| `--epu-soft` | supporting copy |
| `--epu-accent` | the one emphasis color |
| `--epu-deep` | the dark ground |

The values are channel triplets, `15 23 42` rather than `#0f172a`, so an opacity suffix still works: `rgb(var(--epu-ink) / .70)`.


Declare all six on the block itself, and give them real values. They are not defined anywhere else, and a colour read from a variable that was never declared is an invalid declaration: the browser discards the line and that part renders with no colour at all. Where the brief names the customer's colours, those colours go into these six values and nowhere else.

    .epu{--epu-surface:255 255 255;--epu-band:246 247 249;--epu-ink:15 23 42;
      --epu-soft:92 107 129;--epu-accent:37 99 235;--epu-deep:11 18 32}

Rebrand the block by changing those six values, in one place. Do not spread hex values through the markup, and do not reach for a color name out of the page's Tailwind config. Every page the builder makes writes its own config with its own color names, so a name taken from one page resolves to nothing on the next. Measured on two live sites: `bg-surface` painted no background on either, and `muted` was a pale background on one and a dark text color on the other.

Do not invent CSS variable names such as `var(--color-surface)`. Nothing defines them, the browser throws the declaration away, and that color or border renders as nothing at all. Do not use `text-current/60` either, because an opacity modifier has no effect on `currentColor`.

Every piece of text has to reach 4.5 to 1 against the color actually behind it, which is the nearest ancestor that paints a background rather than the page. Large text, meaning 24px, or 18.66px when bold, needs 3 to 1. With the default values, ink on deep measures 1.05 to 1, accent on deep 3.62 to 1 and soft on deep 3.45 to 1, so none of those three belong on a dark panel. On a dark panel use the surface value for text. Soft measures 5.06 to 1 on the band and 5.42 on surface, and white on the accent measures 5.17 to 1.

A tint such as `rgb(var(--epu-accent) / .10)` is mostly the color behind it, so judge contrast against the composited result.
