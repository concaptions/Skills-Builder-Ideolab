---
name: text-paragraph
description: A body copy block inserted into an existing section. Use for standard width prose, a narrow editorial column, two columns, a pull quote lead, a side note, or copy set beside media. Not for a list, and not for a heading.
---

# Text Paragraph

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

Put them inside `@layer etx-base { }`. This matters: a layered declaration loses to an unlayered one whatever its specificity or its order, so Tailwind on a real page still wins every one of them, and the layer only shows through where there is no Tailwind. Without the layer, a rule such as `.etx h2` would outrank `.text-4xl` and the block would start overriding the page.

Start with the five reset lines. Tailwind normally supplies these, and without them the list markers show through, the type falls back to a serif, and the content runs wider than the screen. That was measured: a first attempt without them put a block 36px past the right edge of a 1280px viewport.

    @layer etx-base {
      .etx,.etx *,.etx *::before,.etx *::after{box-sizing:border-box}
      .etx{font-family:ui-sans-serif,system-ui,-apple-system,"Segoe UI",Roboto,sans-serif;line-height:1.5}
      .etx h1,.etx h2,.etx h3,.etx h4,.etx p,.etx figure,.etx blockquote{margin:0}
      .etx ul,.etx ol{margin:0;padding:0;list-style:none}
      .etx img,.etx svg,.etx video{display:block;max-width:100%}

Then the block's own shape: the section's background, text color and padding, the wrapper's maximum width, every row or grid the layout depends on, and the heading and body type sizes.

      .etx{background-color:rgb(var(--etx-surface));color:rgb(var(--etx-ink));padding:3rem 1.25rem}
      .etx .etx-wrap{max-width:64rem;margin-inline:auto}
      .etx .etx-row{display:grid;gap:1.5rem}
      .etx h2{font-size:1.875rem;line-height:1.2;font-weight:600}
      .etx h3{font-size:1.125rem;font-weight:600}
      .etx p{font-size:1rem;line-height:1.625}
    }

About twelve declarations in all. Do not try to restate every utility you used. This is the block's skeleton, not a second copy of Tailwind.

**Nothing else is assumed.** The block does not expect the page to define a color, a font, a helper class or a shared stylesheet.

This skill is self-contained. Do not read, reference or depend on any other skill, and do not assume that a shared stylesheet, a motion system, a component library or a set of helper classes exists somewhere. If something is not described here, it is not available.

## What it is for

Body copy inserted into an existing section.

A passage of prose that has to be read, not skimmed. Measure and line length matter more than anything decorative here.

## When to use it

When the section needs explanation rather than a list of points, and when the reader is expected to read whole sentences rather than scan.

## When not to use it

When the content is really a list of separate points, which reads far better as a list. Not for a single sentence either, which belongs with the heading it supports.

## Where it belongs on the page

Under the heading it belongs to, and above the media or the call to action that follows from it.

## What may be changed

The copy, the measure, the alignment, whether it runs in one column or two, whether a lead sentence or a drop cap is used, and which phrase carries emphasis.

## What must not be changed

The measure must stay readable. Prose set wider than about 75 characters loses the reader between lines, and a two column layout must not appear below 768px, because a two column measure on a phone is a few words wide.

## Settings and Controls

- Set width, height limit, alignment, columns, line height, font size, paragraph spacing, and mobile behavior.
- Choose whether links, highlighted phrases, drop cap, expandable text, or read-more control are included.
- Control text source, character limit, overflow, and whether the paragraph follows or wraps around media.

## Creative Design Options

- **Layout.** Standard width, narrow editorial, two columns, pull-quote lead, side note, or text wrapping around an image.
- **Typography.** Highlighted phrases, color emphasis, soft gradient phrases, drop cap, lead sentence, or inset quotation.
- **Motion.** Fade, rise, paragraph reveal, staggered line reveal, highlighted phrase sweep, or progressive disclosure.
- **Background.** Plain, lightly tinted panel, glass card, image overlay, or bordered editorial block.

## Motion

The paragraph fades and rises as one, over about 400ms. A line by line reveal is available but should be reserved for a single short lead paragraph, never for a long passage, because the reader arrives faster than the animation. Under `prefers-reduced-motion` the text is present immediately.

## Responsive and Accessibility

One column below 768px whatever the setting. Body copy never falls below 16px. Line height of at least 1.6 for prose. The emphasised phrase is a `<strong>` or a `<em>` when it carries meaning, and a plain span when it is only decorative.

## Defaults

Standard width, one column, no drop cap, one emphasised phrase, fade and rise.

## What breaks this block

A two column paragraph that stays two columns on a phone. A drop cap made from a separate element, which a screen reader reads as a stray letter before the sentence. Justified text, which opens rivers of white space at this measure.

## What a static block cannot do

Expandable text and a read more control need a script unless they are built as a `details` disclosure, which is what to use if the copy really must start collapsed. A progressive reveal tied to scroll position also needs a script. Shorten the copy instead.

## Images

Images come from the customer's brief: their own photographs, their own logo, the artwork they supplied. If the brief carries none, use the placeholder that ships inside this block, which is inline SVG and needs no request.

Never reach for a stock photo address. Unsplash, Pexels, Pixabay, a CDN, a hotlinked image on someone else's site: all of them are external files, all of them are ruled out here, and a broken address leaves a hole in the layout with no warning.

Give every image an alt description that says what is in it. Decorative artwork takes `alt=""` so a screen reader passes over it rather than reading a filename.

## Color

The block carries its own color. Six variables are declared on the block itself, and every colored part reads one of them.

| Variable | Used for |
| --- | --- |
| `--etx-surface` | card and panel faces |
| `--etx-band` | the quiet band behind the content |
| `--etx-ink` | headings and primary copy |
| `--etx-soft` | supporting copy |
| `--etx-accent` | the one emphasis color |
| `--etx-deep` | the dark ground |

The values are channel triplets, `15 23 42` rather than `#0f172a`, so an opacity suffix still works: `rgb(var(--etx-ink) / .70)`.

Rebrand the block by changing those six values, in one place. Do not spread hex values through the markup, and do not reach for a color name out of the page's Tailwind config. Every page the builder makes writes its own config with its own color names, so a name taken from one page resolves to nothing on the next. Measured on two live sites: `bg-surface` painted no background on either, and `muted` was a pale background on one and a dark text color on the other.

Do not invent CSS variable names such as `var(--color-surface)`. Nothing defines them, the browser throws the declaration away, and that color or border renders as nothing at all. Do not use `text-current/60` either, because an opacity modifier has no effect on `currentColor`.

Every piece of text has to reach 4.5 to 1 against the color actually behind it, which is the nearest ancestor that paints a background rather than the page. Large text, meaning 24px, or 18.66px when bold, needs 3 to 1. With the default values, ink on deep measures 1.05 to 1, accent on deep 3.62 to 1 and soft on deep 3.45 to 1, so none of those three belong on a dark panel. On a dark panel use the surface value for text. Soft measures 5.06 to 1 on the band and 5.42 on surface, and white on the accent measures 5.17 to 1.

A tint such as `rgb(var(--etx-accent) / .10)` is mostly the color behind it, so judge contrast against the composited result.
