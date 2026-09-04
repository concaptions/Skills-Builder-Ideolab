---
name: single-image
description: One image inserted into an existing section. Use for a rounded, framed, shadowed, bordered, masked or full bleed image, with or without a caption. Not for a gallery, and not for an image paired with copy, which is the image with text element.
---

# Single Image

## What you must return

One `<section>` element, and nothing else.

- No `<!doctype>`, `<html>`, `<head>` or `<body>`. The block is dropped into a page that already exists.
- No markdown code fence around it, and no commentary before or after it. Return the HTML itself. A leftover fence marker renders as visible text on the page, and inside a grid it becomes an extra item that pushes the layout out of shape.
- No `<script>`, no event handlers, no `hidden` attributes waiting to be toggled. Nothing here can run.
- No external files of any kind: no image addresses, no icon library, no web font.
- Everything the block needs lives inside that one element, including its own `<style>`. That stylesheet is expected, not a last resort. Tailwind utilities carry most of the work, but a relationship between elements cannot be written as a utility, and the ones this set depends on all live in the stylesheet: `:checked ~`, `details[open]`, `:target`, `@keyframes` and the `prefers-reduced-motion` rule. Prefix your own class names so they cannot collide with the rest of the page.
- The block carries its own padding and its own maximum width. Drop that wrapper and the content runs to both edges of the screen.
- Color an SVG with a presentation attribute as well as a class. `<rect fill="#f6f7f9" class="fill-muted">` takes the theme color where the stylesheet is loaded and falls back to the attribute where it is not. With only the class, an unresolved fill computes to solid black and the placeholder becomes a black rectangle sitting over the layout.
- Write `fill="none"` on any path that is stroke only. A path with no fill is filled solid black. That is the SVG default, not a fault in the browser.

**Write the block's own base layer.** Tailwind utilities carry the detail, but a plain HTML viewer has no Tailwind, and a block built only from utilities arrives there as unstyled text on a white page. So write the few declarations that carry the block's shape into its own stylesheet as well.

Put them inside `@layer eim-base { }`. This matters: a layered declaration loses to an unlayered one whatever its specificity or its order, so Tailwind on a real page still wins every one of them, and the layer only shows through where there is no Tailwind. Without the layer, a rule such as `.eim h2` would outrank `.text-4xl` and the block would start overriding the page.

Start with the five reset lines. Tailwind normally supplies these, and without them the list markers show through, the type falls back to a serif, and the content runs wider than the screen. That was measured: a first attempt without them put a block 36px past the right edge of a 1280px viewport.

    @layer eim-base {
      .eim,.eim *,.eim *::before,.eim *::after{box-sizing:border-box}
      .eim{font-family:ui-sans-serif,system-ui,-apple-system,"Segoe UI",Roboto,sans-serif;line-height:1.5}
      .eim h1,.eim h2,.eim h3,.eim h4,.eim p,.eim figure,.eim blockquote{margin:0}
      .eim ul,.eim ol{margin:0;padding:0;list-style:none}
      .eim img,.eim svg,.eim video{display:block;max-width:100%}

Then the block's own shape: the section's background, text color and padding, the wrapper's maximum width, every row or grid the layout depends on, and the heading and body type sizes.

      .eim{background-color:rgb(var(--eim-surface));color:rgb(var(--eim-ink));padding:3rem 1.25rem}
      .eim .eim-wrap{max-width:64rem;margin-inline:auto}
      .eim .eim-row{display:grid;gap:1.5rem}
      .eim h2{font-size:1.875rem;line-height:1.2;font-weight:600}
      .eim h3{font-size:1.125rem;font-weight:600}
      .eim p{font-size:1rem;line-height:1.625}
    }

About twelve declarations in all. Do not try to restate every utility you used. This is the block's skeleton, not a second copy of Tailwind.

**Nothing else is assumed.** The block does not expect the page to define a color, a font, a helper class or a shared stylesheet.

This skill is self-contained. Do not read, reference or depend on any other skill, and do not assume that a shared stylesheet, a motion system, a component library or a set of helper classes exists somewhere. If something is not described here, it is not available.

## What it is for

One image inserted into an existing section.

One picture that earns its place: the work, the room, the person, the thing being sold.

## When to use it

When the picture tells the reader something the words cannot, and when it is the only image the section needs.

## When not to use it

When the picture is filler. A stock photograph of a handshake says nothing and costs the reader a scroll.

## Where it belongs on the page

Beside or beneath the copy it illustrates, never floated so far from it that the connection is lost.

## What may be changed

The crop, the aspect ratio, the rounding, the frame, the caption, and whether the image runs to the edge of the section.

## What must not be changed

Every image carries a width and a height so the page does not jump while it loads, and an alt description unless it is purely decorative. The aspect ratio is set on the container, not left to whatever the file happens to be, or the layout moves when the image is swapped.

## Settings and Controls

- Choose upload, Brand Gallery, generated image, stock source, or external image.
- Set width, height, crop, focal point, aspect ratio, alignment, caption, alt text, link, and lightbox.
- Control loading quality, mobile crop, and whether the image is decorative or informational.

## Creative Design Options

- **Style.** Rounded, framed, shadowed, bordered, cutout, masked shape, collage layer, or full bleed.
- **Motion.** Default subtle hover zoom, parallax, tilt, slow pan, mask reveal, fade, or floating depth.
- **Hover.** Caption reveal, overlay, color shift, alternate image, hotspot, or lightbox expansion.
- **Background relationship.** Blend into a gradient, overlap a card, break the grid, or sit inside a device frame.

## Motion

A slow zoom on hover inside a fixed frame, over about 500ms. The frame does not move; only the picture inside it does, so nothing around it is pushed. Under `prefers-reduced-motion` the zoom is removed.

## Responsive and Accessibility

The image never exceeds the width of its container. On a phone a wide crop is allowed to become taller so the subject stays visible. A caption is a real `figcaption` inside a `figure`, so it is tied to the picture rather than floating near it.

## Defaults

4 by 3, rounded, no frame, caption shown, slow zoom on hover inside a fixed frame.

## What breaks this block

An image with no width and height, which shifts everything below it as it loads. A crop that cuts the subject in half on a phone. A caption in a plain paragraph, which is not connected to the image for anyone listening.

## What a static block cannot do

Parallax, tilt towards the pointer, a lightbox that traps focus, and hotspots that open on click all need a script. A hover zoom, a hover caption and a mask reveal are all possible in CSS.

## Images

Images come from the customer's brief: their own photographs, their own logo, the artwork they supplied. If the brief carries none, use the placeholder that ships inside this block, which is inline SVG and needs no request.

Never reach for a stock photo address. Unsplash, Pexels, Pixabay, a CDN, a hotlinked image on someone else's site: all of them are external files, all of them are ruled out here, and a broken address leaves a hole in the layout with no warning.

Give every image an alt description that says what is in it. Decorative artwork takes `alt=""` so a screen reader passes over it rather than reading a filename.

## Color

The block carries its own color. Six variables are declared on the block itself, and every colored part reads one of them.

| Variable | Used for |
| --- | --- |
| `--eim-surface` | card and panel faces |
| `--eim-band` | the quiet band behind the content |
| `--eim-ink` | headings and primary copy |
| `--eim-soft` | supporting copy |
| `--eim-accent` | the one emphasis color |
| `--eim-deep` | the dark ground |

The values are channel triplets, `15 23 42` rather than `#0f172a`, so an opacity suffix still works: `rgb(var(--eim-ink) / .70)`.

Rebrand the block by changing those six values, in one place. Do not spread hex values through the markup, and do not reach for a color name out of the page's Tailwind config. Every page the builder makes writes its own config with its own color names, so a name taken from one page resolves to nothing on the next. Measured on two live sites: `bg-surface` painted no background on either, and `muted` was a pale background on one and a dark text color on the other.

Do not invent CSS variable names such as `var(--color-surface)`. Nothing defines them, the browser throws the declaration away, and that color or border renders as nothing at all. Do not use `text-current/60` either, because an opacity modifier has no effect on `currentColor`.

Every piece of text has to reach 4.5 to 1 against the color actually behind it, which is the nearest ancestor that paints a background rather than the page. Large text, meaning 24px, or 18.66px when bold, needs 3 to 1. With the default values, ink on deep measures 1.05 to 1, accent on deep 3.62 to 1 and soft on deep 3.45 to 1, so none of those three belong on a dark panel. On a dark panel use the surface value for text. Soft measures 5.06 to 1 on the band and 5.42 on surface, and white on the accent measures 5.17 to 1.

A tint such as `rgb(var(--eim-accent) / .10)` is mostly the color behind it, so judge contrast against the composited result.
