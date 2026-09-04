---
name: headline-or-title
description: A headline or title block placed inside an existing section. Use for a section heading, an eyebrow with a headline, a two-line split headline, or a headline with a short supporting line. Not for the page hero, and not for a heading that is already part of another block.
---

# Headline or Title

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
- Give every inline `<svg>` a `width` and a `height` attribute as well as its size classes. An SVG that carries only a `viewBox` has no size of its own, so `h-4 w-4` is the only thing holding a 16px icon at 16px. Measured in a plain HTML viewer with no Tailwind, that same icon rendered at 900 by 900 and swallowed the section around it. The attribute is the weakest thing on the page, so Tailwind still wins wherever it is loaded, and the attribute only shows through where it is not. Size any icon that repeats in the base layer as well.

**Write the block's own base layer.** Tailwind utilities carry the detail, but a plain HTML viewer has no Tailwind, and a block built only from utilities arrives there as unstyled text on a white page. So write the few declarations that carry the block's shape into its own stylesheet as well.

Put them inside `@layer ehd-base { }`. This matters: a layered declaration loses to an unlayered one whatever its specificity or its order, so Tailwind on a real page still wins every one of them, and the layer only shows through where there is no Tailwind. Without the layer, a rule such as `.ehd h2` would outrank `.text-4xl` and the block would start overriding the page.

Start with the five reset lines. Tailwind normally supplies these, and without them the list markers show through, the type falls back to a serif, and the content runs wider than the screen. That was measured: a first attempt without them put a block 36px past the right edge of a 1280px viewport.

    @layer ehd-base {
      .ehd,.ehd *,.ehd *::before,.ehd *::after{box-sizing:border-box}
      .ehd{font-family:ui-sans-serif,system-ui,-apple-system,"Segoe UI",Roboto,sans-serif;line-height:1.5}
      .ehd h1,.ehd h2,.ehd h3,.ehd h4,.ehd p,.ehd figure,.ehd blockquote{margin:0}
      .ehd ul,.ehd ol{margin:0;padding:0;list-style:none}
      .ehd img,.ehd svg,.ehd video{display:block;max-width:100%}

Then the block's own shape: the section's background, text color and padding, the wrapper's maximum width, every row or grid the layout depends on, and the heading and body type sizes.

      .ehd{background-color:rgb(var(--ehd-surface));color:rgb(var(--ehd-ink));padding:3rem 1.25rem}
      .ehd .ehd-wrap{max-width:64rem;margin-inline:auto}
      .ehd .ehd-row{display:grid;gap:1.5rem}
      .ehd h2{font-size:1.875rem;line-height:1.2;font-weight:600}
      .ehd h3{font-size:1.125rem;font-weight:600}
      .ehd p{font-size:1rem;line-height:1.625}
    }

About twelve declarations in all. Do not try to restate every utility you used. This is the block's skeleton, not a second copy of Tailwind.

**Nothing else is assumed.** The block does not expect the page to define a color, a font, a helper class or a shared stylesheet.

This skill is self-contained. Do not read, reference or depend on any other skill, and do not assume that a shared stylesheet, a motion system, a component library or a set of helper classes exists somewhere. If something is not described here, it is not available.

## What it is for

A heading inserted into an existing section.

One heading, with the optional eyebrow and supporting line that belong to it. It names what follows, so a reader who skims only the headings still knows what the page contains.

## When to use it

When a section needs its own heading and the surrounding block does not already provide one, or when a long section needs a second heading part of the way down to break it into two ideas.

## When not to use it

When the section already carries a heading, because two headings in one section leave the reader unsure which one owns the content. Not for the page hero either, which is a section of its own with its own rules.

## Where it belongs on the page

Directly above the content it names, with more space above it than below it, so it reads as belonging to what follows rather than to what came before.

## What may be changed

The eyebrow text and whether it appears at all, the headline wording, the supporting line, the alignment, and which phrase carries the accent color.

## What must not be changed

The heading level must stay in sequence with the page. A section heading under a page h1 is an h2, and the supporting line is a paragraph, never a smaller heading. Do not use a heading tag because it is large, and do not use a div because you want it small.

## Settings and Controls

- Set heading level, width, maximum lines, alignment, text size, line height, letter spacing, and mobile size.
- Enter or generate the text; choose whether selected words or lines may be treated separately.
- Choose link behavior, wrapping rules, and whether the headline remains visible before animation completes.

## Creative Design Options

- **Typography.** Solid, gradient, outlined, masked image, dimensional, highlighted, or multi-tone text.
- **Layout.** Left, center, right, narrow editorial, oversized display, split line, or text layered with an image.
- **Motion.** Word, line, or letter reveal; rolling words; rotating phrases; text scramble; typewriter; or gradient sweep.
- **Special treatment.** Each line can use a related color progression, such as bright gold to progressively softer gold.

## Motion

One entrance: the group fades and rises together, over about 400ms. The eyebrow, headline and supporting line move as one, not one after another, because three staggered lines of a single heading reads as a stutter. Under `prefers-reduced-motion` the entrance is removed and everything is present at once.

## Responsive and Accessibility

Below 768px the headline steps down one size and the alignment stays as set. The heading is real text, never an image of text, so it can be read out, translated and searched. Keep the accent phrase inside the heading rather than in a separate element, so a screen reader reads one continuous sentence.

## Defaults

Eyebrow shown, headline in two lines, supporting line shown, centered, one accent phrase, fade and rise.

## What breaks this block

Skipping a heading level, so the page goes h2 then h4 and a screen reader reports a missing level. Setting the eyebrow as a heading tag, which makes a two word label into a document landmark. Putting the accent phrase in its own block element, which breaks the sentence into two for anyone listening.
- **An icon with no size of its own.** `<svg class="h-4 w-4" viewBox="0 0 20 20">` is 16px only while Tailwind is loaded. Strip the stylesheet away and the SVG has no intrinsic size and stretches to the width of whatever contains it: measured at 900 by 900 inside a 900px section, with the rest of the block pushed off the screen. Fix it with a `width` and a `height` attribute on the tag, and a base layer rule for any icon used more than once.

## What a static block cannot do

A typewriter effect, text that counts or scrambles into place, and a headline that changes word on a timer all need a script. Choose the wording instead. A gradient headline is possible with background-clip, but its contrast cannot be measured reliably, so keep a solid color for anything a reader must be able to read.

## Images

Images come from the customer's brief: their own photographs, their own logo, the artwork they supplied. If the brief carries none, use the placeholder that ships inside this block, which is inline SVG and needs no request.

Never reach for a stock photo address. Unsplash, Pexels, Pixabay, a CDN, a hotlinked image on someone else's site: all of them are external files, all of them are ruled out here, and a broken address leaves a hole in the layout with no warning.

Give every image an alt description that says what is in it. Decorative artwork takes `alt=""` so a screen reader passes over it rather than reading a filename.

## Color

The block carries its own color. Six variables are declared on the block itself, and every colored part reads one of them.

| Variable | Used for |
| --- | --- |
| `--ehd-surface` | card and panel faces |
| `--ehd-band` | the quiet band behind the content |
| `--ehd-ink` | headings and primary copy |
| `--ehd-soft` | supporting copy |
| `--ehd-accent` | the one emphasis color |
| `--ehd-deep` | the dark ground |

The values are channel triplets, `15 23 42` rather than `#0f172a`, so an opacity suffix still works: `rgb(var(--ehd-ink) / .70)`.

Rebrand the block by changing those six values, in one place. Do not spread hex values through the markup, and do not reach for a color name out of the page's Tailwind config. Every page the builder makes writes its own config with its own color names, so a name taken from one page resolves to nothing on the next. Measured on two live sites: `bg-surface` painted no background on either, and `muted` was a pale background on one and a dark text color on the other.

Do not invent CSS variable names such as `var(--color-surface)`. Nothing defines them, the browser throws the declaration away, and that color or border renders as nothing at all. Do not use `text-current/60` either, because an opacity modifier has no effect on `currentColor`.

Every piece of text has to reach 4.5 to 1 against the color actually behind it, which is the nearest ancestor that paints a background rather than the page. Large text, meaning 24px, or 18.66px when bold, needs 3 to 1. With the default values, ink on deep measures 1.05 to 1, accent on deep 3.62 to 1 and soft on deep 3.45 to 1, so none of those three belong on a dark panel. On a dark panel use the surface value for text. Soft measures 5.06 to 1 on the band and 5.42 on surface, and white on the accent measures 5.17 to 1.

A tint such as `rgb(var(--ehd-accent) / .10)` is mostly the color behind it, so judge contrast against the composited result.
