---
name: photo-gallery
description: A gallery element placed inside an existing section rather than occupying a full section. Use for a small grid, a mini filmstrip, or a compact scrolling rail of images. Not for a full photo gallery section with filters, which is its own section.
---

# Photo Gallery

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

Put them inside `@layer egl-base { }`. This matters: a layered declaration loses to an unlayered one whatever its specificity or its order, so Tailwind on a real page still wins every one of them, and the layer only shows through where there is no Tailwind. Without the layer, a rule such as `.egl h2` would outrank `.text-4xl` and the block would start overriding the page.

Start with the five reset lines. Tailwind normally supplies these, and without them the list markers show through, the type falls back to a serif, and the content runs wider than the screen. That was measured: a first attempt without them put a block 36px past the right edge of a 1280px viewport.

    @layer egl-base {
      .egl,.egl *,.egl *::before,.egl *::after{box-sizing:border-box}
      .egl{font-family:ui-sans-serif,system-ui,-apple-system,"Segoe UI",Roboto,sans-serif;line-height:1.5}
      .egl h1,.egl h2,.egl h3,.egl h4,.egl p,.egl figure,.egl blockquote{margin:0}
      .egl ul,.egl ol{margin:0;padding:0;list-style:none}
      .egl img,.egl svg,.egl video{display:block;max-width:100%}

Then the block's own shape: the section's background, text color and padding, the wrapper's maximum width, every row or grid the layout depends on, and the heading and body type sizes.

      .egl{background-color:rgb(var(--egl-surface));color:rgb(var(--egl-ink));padding:3rem 1.25rem}
      .egl .egl-wrap{max-width:64rem;margin-inline:auto}
      .egl .egl-row{display:grid;gap:1.5rem}
      .egl h2{font-size:1.875rem;line-height:1.2;font-weight:600}
      .egl h3{font-size:1.125rem;font-weight:600}
      .egl p{font-size:1rem;line-height:1.625}
    }

About twelve declarations in all. Do not try to restate every utility you used. This is the block's skeleton, not a second copy of Tailwind.

**Nothing else is assumed.** The block does not expect the page to define a color, a font, a helper class or a shared stylesheet.

This skill is self-contained. Do not read, reference or depend on any other skill, and do not assume that a shared stylesheet, a motion system, a component library or a set of helper classes exists somewhere. If something is not described here, it is not available.

## What it is for

A gallery element placed inside an existing section rather than occupying a full section.

A handful of pictures inside a larger section, shown together so the reader can take them in at a glance.

## When to use it

Inside an about or a services section, where four to eight pictures support what is being said without becoming the section themselves.

## When not to use it

When there are more than about eight pictures, which is a gallery section. Not for one picture either.

## Where it belongs on the page

Beneath the copy it supports, inside the same section.

## What may be changed

The number of columns, the gap, the crop, whether captions show, and whether the rail scrolls instead of wrapping.

## What must not be changed

Every tile keeps the same aspect ratio, set on the tile rather than on the picture, so the grid does not go ragged when one photograph is a different shape. Every picture carries an alt description.

## Settings and Controls

- Set image count, columns, gaps, aspect ratios, ordering, captions, and lightbox behavior.
- Choose image source and control maximum visible images, loading, and mobile columns.

## Creative Design Options

- **Layout.** Grid, masonry, collage, mini filmstrip, or compact carousel.
- **Motion.** Staggered reveal, hover zoom, tilt, caption reveal, or lightbox expansion.
- **Styling.** Rounded tiles, editorial crops, layered cards, film border, or edge-to-edge images.

## Motion

Tiles stagger in at about 50ms apart, and the picture zooms slightly inside its fixed tile on hover. The tile itself does not move, so the grid never shifts. Under `prefers-reduced-motion` both are removed.

## Responsive and Accessibility

Two columns below 640px, three above. A scrolling rail uses real scroll with `scroll-snap`, so it works with a finger, a trackpad and a keyboard without a script.

## Defaults

Three columns, square crop, no captions, hover zoom inside a fixed tile, staggered entrance.

## What breaks this block

Tiles with no fixed ratio, which makes the grid ragged as soon as one image differs. A lightbox, which needs focus trapping and a script and must be left out. Pictures with no alt text, which are silence.

## What a static block cannot do

A lightbox, drag to scroll and a caption that follows the pointer all need a script. A real scroll container with `scroll-snap` gives a carousel that works everywhere, which is what to build instead.

## Images

Images come from the customer's brief: their own photographs, their own logo, the artwork they supplied. If the brief carries none, use the placeholder that ships inside this block, which is inline SVG and needs no request.

Never reach for a stock photo address. Unsplash, Pexels, Pixabay, a CDN, a hotlinked image on someone else's site: all of them are external files, all of them are ruled out here, and a broken address leaves a hole in the layout with no warning.

Give every image an alt description that says what is in it. Decorative artwork takes `alt=""` so a screen reader passes over it rather than reading a filename.

## Color

The block carries its own color. Six variables are declared on the block itself, and every colored part reads one of them.

| Variable | Used for |
| --- | --- |
| `--egl-surface` | card and panel faces |
| `--egl-band` | the quiet band behind the content |
| `--egl-ink` | headings and primary copy |
| `--egl-soft` | supporting copy |
| `--egl-accent` | the one emphasis color |
| `--egl-deep` | the dark ground |

The values are channel triplets, `15 23 42` rather than `#0f172a`, so an opacity suffix still works: `rgb(var(--egl-ink) / .70)`.

Rebrand the block by changing those six values, in one place. Do not spread hex values through the markup, and do not reach for a color name out of the page's Tailwind config. Every page the builder makes writes its own config with its own color names, so a name taken from one page resolves to nothing on the next. Measured on two live sites: `bg-surface` painted no background on either, and `muted` was a pale background on one and a dark text color on the other.

Do not invent CSS variable names such as `var(--color-surface)`. Nothing defines them, the browser throws the declaration away, and that color or border renders as nothing at all. Do not use `text-current/60` either, because an opacity modifier has no effect on `currentColor`.

Every piece of text has to reach 4.5 to 1 against the color actually behind it, which is the nearest ancestor that paints a background rather than the page. Large text, meaning 24px, or 18.66px when bold, needs 3 to 1. With the default values, ink on deep measures 1.05 to 1, accent on deep 3.62 to 1 and soft on deep 3.45 to 1, so none of those three belong on a dark panel. On a dark panel use the surface value for text. Soft measures 5.06 to 1 on the band and 5.42 on surface, and white on the accent measures 5.17 to 1.

A tint such as `rgb(var(--egl-accent) / .10)` is mostly the color behind it, so judge contrast against the composited result.
