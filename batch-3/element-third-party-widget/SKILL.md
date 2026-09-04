---
name: third-party-widget
description: A framed slot for a tool or feed supplied by another service, with a described fallback and a link out. Use for a booking tool, a review feed, or a calculator. Not for the embed code itself, which is an external file and cannot be included here.
---

# Third-Party Widget

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

Put them inside `@layer ewd-base { }`. This matters: a layered declaration loses to an unlayered one whatever its specificity or its order, so Tailwind on a real page still wins every one of them, and the layer only shows through where there is no Tailwind. Without the layer, a rule such as `.ewd h2` would outrank `.text-4xl` and the block would start overriding the page.

Start with the five reset lines. Tailwind normally supplies these, and without them the list markers show through, the type falls back to a serif, and the content runs wider than the screen. That was measured: a first attempt without them put a block 36px past the right edge of a 1280px viewport.

    @layer ewd-base {
      .ewd,.ewd *,.ewd *::before,.ewd *::after{box-sizing:border-box}
      .ewd{font-family:ui-sans-serif,system-ui,-apple-system,"Segoe UI",Roboto,sans-serif;line-height:1.5}
      .ewd h1,.ewd h2,.ewd h3,.ewd h4,.ewd p,.ewd figure,.ewd blockquote{margin:0}
      .ewd ul,.ewd ol{margin:0;padding:0;list-style:none}
      .ewd img,.ewd svg,.ewd video{display:block;max-width:100%}

Then the block's own shape: the section's background, text color and padding, the wrapper's maximum width, every row or grid the layout depends on, and the heading and body type sizes.

      .ewd{background-color:rgb(var(--ewd-surface));color:rgb(var(--ewd-ink));padding:3rem 1.25rem}
      .ewd .ewd-wrap{max-width:64rem;margin-inline:auto}
      .ewd .ewd-row{display:grid;gap:1.5rem}
      .ewd h2{font-size:1.875rem;line-height:1.2;font-weight:600}
      .ewd h3{font-size:1.125rem;font-weight:600}
      .ewd p{font-size:1rem;line-height:1.625}
    }

About twelve declarations in all. Do not try to restate every utility you used. This is the block's skeleton, not a second copy of Tailwind.

**Nothing else is assumed.** The block does not expect the page to define a color, a font, a helper class or a shared stylesheet.

This skill is self-contained. Do not read, reference or depend on any other skill, and do not assume that a shared stylesheet, a motion system, a component library or a set of helper classes exists somewhere. If something is not described here, it is not available.

## What it is for

A controlled embed for tools or content supplied by another service.

The place a third party tool will sit, prepared properly: the right shape, a clear label, and something useful in the slot until the tool is added.

## When to use it

When the customer really does use an outside tool, and the page has to make room for it without collapsing when it fails to load.

## When not to use it

When the same job can be done on the page. An embed is another company's script, another company's outage and another company's cookie banner.

## Where it belongs on the page

Where the reader expects the tool, under the copy that explains what it does.

## What may be changed

The frame, the height, the label, and the wording of the fallback and the link out.

## What must not be changed

The slot keeps a fixed height, so the page does not jump when the tool loads. The fallback is real content with a real link, not a spinner, because a spinner that never resolves tells the reader nothing. Never state that a tool is loading when nothing is loading.

## Settings and Controls

- Set embed source, width, height, permissions, loading behavior, privacy consent, and fallback message.
- Choose framed, full-width, inline, pop-up, or responsive container behavior.
- Control error state, loading state, mobile height, and whether the embed may open externally.

## Creative Design Options

- **Layout.** Embedded card, full-width application, framed preview, side-by-side instructions, or pop-up tool.
- **Motion.** Loading skeleton, reveal, frame expansion, or transition to external experience.
- **Styling.** Branded frame, neutral frame, browser mockup, soft shadow, or edge-to-edge embed.

## Motion

The frame fades in on entrance. There is no loading skeleton that shimmers forever, because a shimmer with nothing behind it is a lie about what is happening.

## Responsive and Accessibility

The slot keeps a set height at each width rather than a ratio, since most embedded tools are taller on a phone. The link out is at least 44px.

## Defaults

Framed slot with a fixed height, label above, described fallback with a link out.

## What breaks this block

A slot with no height, which collapses and then jumps. A shimmering skeleton with nothing behind it. A cookie or consent notice left to the embed, when the page should ask first.

## What a static block cannot do

The embed itself cannot be included, because it is an external file and a script. Loading states, error states, resizing to the tool's content and consent gating all need a script too. Prepare the slot and hand the embed code to whoever assembles the page.

## Images

Images come from the customer's brief: their own photographs, their own logo, the artwork they supplied. If the brief carries none, use the placeholder that ships inside this block, which is inline SVG and needs no request.

Never reach for a stock photo address. Unsplash, Pexels, Pixabay, a CDN, a hotlinked image on someone else's site: all of them are external files, all of them are ruled out here, and a broken address leaves a hole in the layout with no warning.

Give every image an alt description that says what is in it. Decorative artwork takes `alt=""` so a screen reader passes over it rather than reading a filename.

## Color

The block carries its own color. Six variables are declared on the block itself, and every colored part reads one of them.

| Variable | Used for |
| --- | --- |
| `--ewd-surface` | card and panel faces |
| `--ewd-band` | the quiet band behind the content |
| `--ewd-ink` | headings and primary copy |
| `--ewd-soft` | supporting copy |
| `--ewd-accent` | the one emphasis color |
| `--ewd-deep` | the dark ground |

The values are channel triplets, `15 23 42` rather than `#0f172a`, so an opacity suffix still works: `rgb(var(--ewd-ink) / .70)`.

Rebrand the block by changing those six values, in one place. Do not spread hex values through the markup, and do not reach for a color name out of the page's Tailwind config. Every page the builder makes writes its own config with its own color names, so a name taken from one page resolves to nothing on the next. Measured on two live sites: `bg-surface` painted no background on either, and `muted` was a pale background on one and a dark text color on the other.

Do not invent CSS variable names such as `var(--color-surface)`. Nothing defines them, the browser throws the declaration away, and that color or border renders as nothing at all. Do not use `text-current/60` either, because an opacity modifier has no effect on `currentColor`.

Every piece of text has to reach 4.5 to 1 against the color actually behind it, which is the nearest ancestor that paints a background rather than the page. Large text, meaning 24px, or 18.66px when bold, needs 3 to 1. With the default values, ink on deep measures 1.05 to 1, accent on deep 3.62 to 1 and soft on deep 3.45 to 1, so none of those three belong on a dark panel. On a dark panel use the surface value for text. Soft measures 5.06 to 1 on the band and 5.42 on surface, and white on the accent measures 5.17 to 1.

A tint such as `rgb(var(--ewd-accent) / .10)` is mostly the color behind it, so judge contrast against the composited result.
