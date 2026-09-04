---
name: business-location-map
description: A location block placed inside a contact or location section, with the address, the hours and a link to directions. Use for a location card, a map beside details, or a stylised local map. Not for a live interactive map, which is a third party embed.
---

# Business Location Map

## What you must return

One `<section>` element, and nothing else.

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

Put them inside `@layer emp-base { }`. This matters: a layered declaration loses to an unlayered one whatever its specificity or its order, so Tailwind on a real page still wins every one of them, and the layer only shows through where there is no Tailwind. Without the layer, a rule such as `.emp h2` would outrank `.text-4xl` and the block would start overriding the page.

Start with the five reset lines. Tailwind normally supplies these, and without them the list markers show through, the type falls back to a serif, and the content runs wider than the screen. That was measured: a first attempt without them put a block 36px past the right edge of a 1280px viewport.

    @layer emp-base {
      .emp,.emp *,.emp *::before,.emp *::after{box-sizing:border-box}
      .emp{font-family:ui-sans-serif,system-ui,-apple-system,"Segoe UI",Roboto,sans-serif;line-height:1.5}
      .emp h1,.emp h2,.emp h3,.emp h4,.emp p,.emp figure,.emp blockquote{margin:0}
      .emp ul,.emp ol{margin:0;padding:0;list-style:none}
      .emp img,.emp svg,.emp video{display:block;max-width:100%}

Then the block's own shape: the section's background, text color and padding, the wrapper's maximum width, every row or grid the layout depends on, and the heading and body type sizes.

      .emp{background-color:rgb(var(--emp-surface));color:rgb(var(--emp-ink));padding:3rem 1.25rem}
      .emp .emp-wrap{max-width:64rem;margin-inline:auto}
      .emp .emp-row{display:grid;gap:1.5rem}
      .emp h2{font-size:1.875rem;line-height:1.2;font-weight:600}
      .emp h3{font-size:1.125rem;font-weight:600}
      .emp p{font-size:1rem;line-height:1.625}
    }

About twelve declarations in all. Do not try to restate every utility you used. This is the block's skeleton, not a second copy of Tailwind.

**Nothing else is assumed.** The block does not expect the page to define a color, a font, a helper class or a shared stylesheet.

This skill is self-contained. Do not read, reference or depend on any other skill, and do not assume that a shared stylesheet, a motion system, a component library or a set of helper classes exists somewhere. If something is not described here, it is not available.

## What it is for

A map or location block placed inside a contact or location section.

Where the business is, in a form somebody can act on: the address in text, the hours, and a link that opens directions in whatever map app they already use.

## When to use it

In a contact section, and on any page a visitor might be arriving from.

## When not to use it

When there is no address to give. A map of a service area with no premises is decoration, and the coverage is better stated in words.

## Where it belongs on the page

In the contact section, beside the phone number, with the hours next to it.

## What may be changed

The stylised map drawing, the marker position, whether hours are shown, and whether the details sit beside or beneath the map.

## What must not be changed

The address is real text, not a picture and not only a pin. A screen reader, a search engine and anybody copying it into a phone all need the text. The stylised map is decorative and takes `aria-hidden`.

## Settings and Controls

- Set address, coordinates, zoom, map provider, marker, directions link, and interaction level.
- Show or hide: location name, address, hours, phone, directions button, and multiple markers.
- Control width, height, map style, consent behavior, and mobile interaction.

## Creative Design Options

- **Layout.** Full map, map card, map beside details, stylized local map, or multiple-location view.
- **Motion.** Marker pulse, map reveal, location-card slide, route draw, or selected-marker emphasis.
- **Styling.** Brand-color map, minimal map, illustrated map, dark map, or image-backed location card.

## Motion

The marker settles once on entrance. It does not pulse forever, because a marker that never stops moving pulls the eye away from the address beside it.

## Responsive and Accessibility

The map sits above the details below 768px. The directions link is at least 44px and full width on a phone, since it is the thing most likely to be pressed on one.

## Defaults

Stylised map beside the details, marker shown, address, hours and phone in text, directions link.

## What breaks this block

An address that exists only inside the picture. A live embed, which is an external file and is not allowed here. A pin that pulses forever beside the words a reader is trying to read.
- **An icon with no size of its own.** `<svg class="h-4 w-4" viewBox="0 0 20 20">` is 16px only while Tailwind is loaded. Strip the stylesheet away and the SVG has no intrinsic size and stretches to the width of whatever contains it: measured at 900 by 900 inside a 900px section, with the rest of the block pushed off the screen. Fix it with a `width` and a `height` attribute on the tag, and a base layer rule for any icon used more than once.
- **A height attribute on an illustration that fills its column.** The reverse mistake, and it shows up on a page that has Tailwind as well as one that does not. The width follows the column while the height is pinned to the attribute, so the drawing is squashed: measured at 656 by 380 where it should have been 656 by 519. An svg whose width follows its container keeps its height free.
- **A height attribute on an illustration that fills its column.** The reverse mistake, and it shows up on a page that has Tailwind as well as one that does not. The width follows the column while the height is pinned to the attribute, so the drawing is squashed: measured at 656 by 380 where it should have been 656 by 519. An svg whose width follows its container keeps its height free.

## What a static block cannot do

A real interactive map is a third party embed and an external file, so it cannot be included. Panning, zooming, route drawing and multiple selectable markers all need a script. Draw the local streets, mark the door, and link out for directions.

## Images

Images come from the customer's brief: their own photographs, their own logo, the artwork they supplied. If the brief carries none, use the placeholder that ships inside this block, which is inline SVG and needs no request.

Never reach for a stock photo address. Unsplash, Pexels, Pixabay, a CDN, a hotlinked image on someone else's site: all of them are external files, all of them are ruled out here, and a broken address leaves a hole in the layout with no warning.

Give every image an alt description that says what is in it. Decorative artwork takes `alt=""` so a screen reader passes over it rather than reading a filename.

## Color

The block carries its own color. Six variables are declared on the block itself, and every colored part reads one of them.

| Variable | Used for |
| --- | --- |
| `--emp-surface` | card and panel faces |
| `--emp-band` | the quiet band behind the content |
| `--emp-ink` | headings and primary copy |
| `--emp-soft` | supporting copy |
| `--emp-accent` | the one emphasis color |
| `--emp-deep` | the dark ground |

The values are channel triplets, `15 23 42` rather than `#0f172a`, so an opacity suffix still works: `rgb(var(--emp-ink) / .70)`.

Rebrand the block by changing those six values, in one place. Do not spread hex values through the markup, and do not reach for a color name out of the page's Tailwind config. Every page the builder makes writes its own config with its own color names, so a name taken from one page resolves to nothing on the next. Measured on two live sites: `bg-surface` painted no background on either, and `muted` was a pale background on one and a dark text color on the other.

Do not invent CSS variable names such as `var(--color-surface)`. Nothing defines them, the browser throws the declaration away, and that color or border renders as nothing at all. Do not use `text-current/60` either, because an opacity modifier has no effect on `currentColor`.

Every piece of text has to reach 4.5 to 1 against the color actually behind it, which is the nearest ancestor that paints a background rather than the page. Large text, meaning 24px, or 18.66px when bold, needs 3 to 1. With the default values, ink on deep measures 1.05 to 1, accent on deep 3.62 to 1 and soft on deep 3.45 to 1, so none of those three belong on a dark panel. On a dark panel use the surface value for text. Soft measures 5.06 to 1 on the band and 5.42 on surface, and white on the accent measures 5.17 to 1.

A tint such as `rgb(var(--emp-accent) / .10)` is mostly the color behind it, so judge contrast against the composited result.
