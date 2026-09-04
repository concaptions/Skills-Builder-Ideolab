---
name: product-or-service
description: A single commerce or service offer block inserted inside another section. Use for a product card, a featured offer, a compact service card, or an image and price split. Not for a row of pricing tiers, which is the price and package element.
---

# Product or Service

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

Put them inside `@layer epd-base { }`. This matters: a layered declaration loses to an unlayered one whatever its specificity or its order, so Tailwind on a real page still wins every one of them, and the layer only shows through where there is no Tailwind. Without the layer, a rule such as `.epd h2` would outrank `.text-4xl` and the block would start overriding the page.

Start with the five reset lines. Tailwind normally supplies these, and without them the list markers show through, the type falls back to a serif, and the content runs wider than the screen. That was measured: a first attempt without them put a block 36px past the right edge of a 1280px viewport.

    @layer epd-base {
      .epd,.epd *,.epd *::before,.epd *::after{box-sizing:border-box}
      .epd{font-family:ui-sans-serif,system-ui,-apple-system,"Segoe UI",Roboto,sans-serif;line-height:1.5}
      .epd h1,.epd h2,.epd h3,.epd h4,.epd p,.epd figure,.epd blockquote{margin:0}
      .epd ul,.epd ol{margin:0;padding:0;list-style:none}
      .epd img,.epd svg,.epd video{display:block;max-width:100%}

Then the block's own shape: the section's background, text color and padding, the wrapper's maximum width, every row or grid the layout depends on, and the heading and body type sizes.

      .epd{background-color:rgb(var(--epd-surface));color:rgb(var(--epd-ink));padding:3rem 1.25rem}
      .epd .epd-wrap{max-width:64rem;margin-inline:auto}
      .epd .epd-row{display:grid;gap:1.5rem}
      .epd h2{font-size:1.875rem;line-height:1.2;font-weight:600}
      .epd h3{font-size:1.125rem;font-weight:600}
      .epd p{font-size:1rem;line-height:1.625}
    }

About twelve declarations in all. Do not try to restate every utility you used. This is the block's skeleton, not a second copy of Tailwind.

**Nothing else is assumed.** The block does not expect the page to define a color, a font, a helper class or a shared stylesheet.

This skill is self-contained. Do not read, reference or depend on any other skill, and do not assume that a shared stylesheet, a motion system, a component library or a set of helper classes exists somewhere. If something is not described here, it is not available.

## What it is for

A single commerce or service-offer block inserted inside another section.

One thing that is for sale, with the four things a buyer needs: what it is, what it costs, what is included, and how to get it.

## When to use it

Inside a services or shop section, as one card among several of the same shape.

## When not to use it

When the price is genuinely on application. A card with no price is a card most readers skip. Say from what figure, or say what a survey costs.

## Where it belongs on the page

In a grid with its siblings, all the same height, with the call to action at the foot of each card.

## What may be changed

The image, the badge, the price wording, how many features are listed, and the call to action.

## What must not be changed

Cards in a row must be the same height with the call to action aligned, or the row reads as broken. A sale price shows what it was and what it is, and the old figure is real. A badge like Most booked appears on one card only, or it means nothing.

## Settings and Controls

- Set title, image, description, price, badge, options, inventory, CTA, purchase behavior, and destination.
- Show or hide: ratings, features, sale price, subscription term, quantity, and add-to-cart.
- Control width, image ratio, alignment, mobile stacking, and data source.

## Creative Design Options

- **Layout.** Product card, featured offer, compact service card, image-and-price split, or comparison card.
- **Motion.** Image zoom, card lift, option transition, add-to-cart feedback, badge reveal, or hover spotlight.
- **Styling.** Premium card, minimal commerce, gradient offer, image-led product, or outlined service card.

## Motion

The card lifts a few pixels on hover and the image zooms slightly inside its fixed frame. The card itself does not resize, so the grid never reflows.

## Responsive and Accessibility

One column below 640px. The image keeps its ratio at every width. The call to action is full width on a phone.

## Defaults

Product card, 4 by 3 image, badge hidden, price from figure, three features, one call to action, lift on hover.

## What breaks this block

Cards of unequal height in a row. A struck through price that was never charged. A badge on every card. A card with no price at all.

## What a static block cannot do

Add to cart, quantity, inventory, option switching and any price that changes when an option is chosen all need a script and a shop behind it. Link to the checkout or the enquiry instead.

## Images

Images come from the customer's brief: their own photographs, their own logo, the artwork they supplied. If the brief carries none, use the placeholder that ships inside this block, which is inline SVG and needs no request.

Never reach for a stock photo address. Unsplash, Pexels, Pixabay, a CDN, a hotlinked image on someone else's site: all of them are external files, all of them are ruled out here, and a broken address leaves a hole in the layout with no warning.

Give every image an alt description that says what is in it. Decorative artwork takes `alt=""` so a screen reader passes over it rather than reading a filename.

## Color

The block carries its own color. Six variables are declared on the block itself, and every colored part reads one of them.

| Variable | Used for |
| --- | --- |
| `--epd-surface` | card and panel faces |
| `--epd-band` | the quiet band behind the content |
| `--epd-ink` | headings and primary copy |
| `--epd-soft` | supporting copy |
| `--epd-accent` | the one emphasis color |
| `--epd-deep` | the dark ground |

The values are channel triplets, `15 23 42` rather than `#0f172a`, so an opacity suffix still works: `rgb(var(--epd-ink) / .70)`.

Rebrand the block by changing those six values, in one place. Do not spread hex values through the markup, and do not reach for a color name out of the page's Tailwind config. Every page the builder makes writes its own config with its own color names, so a name taken from one page resolves to nothing on the next. Measured on two live sites: `bg-surface` painted no background on either, and `muted` was a pale background on one and a dark text color on the other.

Do not invent CSS variable names such as `var(--color-surface)`. Nothing defines them, the browser throws the declaration away, and that color or border renders as nothing at all. Do not use `text-current/60` either, because an opacity modifier has no effect on `currentColor`.

Every piece of text has to reach 4.5 to 1 against the color actually behind it, which is the nearest ancestor that paints a background rather than the page. Large text, meaning 24px, or 18.66px when bold, needs 3 to 1. With the default values, ink on deep measures 1.05 to 1, accent on deep 3.62 to 1 and soft on deep 3.45 to 1, so none of those three belong on a dark panel. On a dark panel use the surface value for text. Soft measures 5.06 to 1 on the band and 5.42 on surface, and white on the accent measures 5.17 to 1.

A tint such as `rgb(var(--epd-accent) / .10)` is mostly the color behind it, so judge contrast against the composited result.
