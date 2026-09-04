---
name: standard-video
description: A video player inserted inside an existing section. Use for a minimal, cinematic, browser framed, device mockup, floating card or full bleed player, with a poster and a transcript. Not for a conversion focused sales video, which is its own element.
---

# Standard Video

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

Put them inside `@layer evd-base { }`. This matters: a layered declaration loses to an unlayered one whatever its specificity or its order, so Tailwind on a real page still wins every one of them, and the layer only shows through where there is no Tailwind. Without the layer, a rule such as `.evd h2` would outrank `.text-4xl` and the block would start overriding the page.

Start with the five reset lines. Tailwind normally supplies these, and without them the list markers show through, the type falls back to a serif, and the content runs wider than the screen. That was measured: a first attempt without them put a block 36px past the right edge of a 1280px viewport.

    @layer evd-base {
      .evd,.evd *,.evd *::before,.evd *::after{box-sizing:border-box}
      .evd{font-family:ui-sans-serif,system-ui,-apple-system,"Segoe UI",Roboto,sans-serif;line-height:1.5}
      .evd h1,.evd h2,.evd h3,.evd h4,.evd p,.evd figure,.evd blockquote{margin:0}
      .evd ul,.evd ol{margin:0;padding:0;list-style:none}
      .evd img,.evd svg,.evd video{display:block;max-width:100%}

Then the block's own shape: the section's background, text color and padding, the wrapper's maximum width, every row or grid the layout depends on, and the heading and body type sizes.

      .evd{background-color:rgb(var(--evd-surface));color:rgb(var(--evd-ink));padding:3rem 1.25rem}
      .evd .evd-wrap{max-width:64rem;margin-inline:auto}
      .evd .evd-row{display:grid;gap:1.5rem}
      .evd h2{font-size:1.875rem;line-height:1.2;font-weight:600}
      .evd h3{font-size:1.125rem;font-weight:600}
      .evd p{font-size:1rem;line-height:1.625}
    }

About twelve declarations in all. Do not try to restate every utility you used. This is the block's skeleton, not a second copy of Tailwind.

**Nothing else is assumed.** The block does not expect the page to define a color, a font, a helper class or a shared stylesheet.

This skill is self-contained. Do not read, reference or depend on any other skill, and do not assume that a shared stylesheet, a motion system, a component library or a set of helper classes exists somewhere. If something is not described here, it is not available.

## What it is for

A video player inserted inside an existing section.

A film the reader chooses to watch, with a still frame and a clear play control until they do.

## When to use it

When seeing the thing move explains it better than a picture, and when the reader has a reason to spend the time.

## When not to use it

When the film says nothing the copy has not already said. Never as decoration that plays on its own with sound.

## Where it belongs on the page

Under the copy that gives the reader a reason to press play, never above it.

## What may be changed

The poster image, the aspect ratio, the player frame, whether a transcript is offered, and the caption beneath.

## What must not be changed

It never plays on its own with sound. The poster is a real image with a ratio set on its frame, so the page does not jump when the player loads. The play control is a real control with a name, not a triangle drawn on a div.

## Settings and Controls

- Set source, poster, aspect ratio, playback controls, captions, transcript, sound, and fallback image.
- Choose inline, modal, autoplay muted, background, or external playback.
- Control width, height, alignment, mobile playback, and accessible label.

## Creative Design Options

- **Player style.** Minimal, cinematic, browser frame, device mockup, floating card, or full bleed.
- **Motion.** Poster zoom, play pulse, hover preview, fade-in, or modal expansion.
- **Surround.** Gradient glow, blurred image field, branded frame, or ambient video colors.

## Motion

The poster zooms very slightly on hover and the play badge grows a little, over about 300ms. Nothing loops. Under `prefers-reduced-motion` both are removed.

## Responsive and Accessibility

16 by 9 at every width, so the frame never changes shape. The transcript is a `details` disclosure, which works on a phone and needs no script. Captions are the responsibility of the film, and the transcript is what makes the content readable when they are missing.

## Defaults

16 by 9, minimal frame, poster shown, click to load, transcript collapsed beneath.

## What breaks this block

Autoplay with sound, which most browsers block and every reader resents. A poster with no dimensions, which shifts the page. A play button made from a div, which cannot be reached by keyboard.
- **An icon with no size of its own.** `<svg class="h-4 w-4" viewBox="0 0 20 20">` is 16px only while Tailwind is loaded. Strip the stylesheet away and the SVG has no intrinsic size and stretches to the width of whatever contains it: measured at 900 by 900 inside a 900px section, with the rest of the block pushed off the screen. Fix it with a `width` and a `height` attribute on the tag, and a base layer rule for any icon used more than once.

## What a static block cannot do

A hover preview that starts playing, a modal that traps focus, and anything driven by playback position need a script. Click to load can be built with `:target`, which is what this block does: the poster is a link to the player.

## Images

Images come from the customer's brief: their own photographs, their own logo, the artwork they supplied. If the brief carries none, use the placeholder that ships inside this block, which is inline SVG and needs no request.

Never reach for a stock photo address. Unsplash, Pexels, Pixabay, a CDN, a hotlinked image on someone else's site: all of them are external files, all of them are ruled out here, and a broken address leaves a hole in the layout with no warning.

Give every image an alt description that says what is in it. Decorative artwork takes `alt=""` so a screen reader passes over it rather than reading a filename.

## Color

The block carries its own color. Six variables are declared on the block itself, and every colored part reads one of them.

| Variable | Used for |
| --- | --- |
| `--evd-surface` | card and panel faces |
| `--evd-band` | the quiet band behind the content |
| `--evd-ink` | headings and primary copy |
| `--evd-soft` | supporting copy |
| `--evd-accent` | the one emphasis color |
| `--evd-deep` | the dark ground |

The values are channel triplets, `15 23 42` rather than `#0f172a`, so an opacity suffix still works: `rgb(var(--evd-ink) / .70)`.

Rebrand the block by changing those six values, in one place. Do not spread hex values through the markup, and do not reach for a color name out of the page's Tailwind config. Every page the builder makes writes its own config with its own color names, so a name taken from one page resolves to nothing on the next. Measured on two live sites: `bg-surface` painted no background on either, and `muted` was a pale background on one and a dark text color on the other.

Do not invent CSS variable names such as `var(--color-surface)`. Nothing defines them, the browser throws the declaration away, and that color or border renders as nothing at all. Do not use `text-current/60` either, because an opacity modifier has no effect on `currentColor`.

Every piece of text has to reach 4.5 to 1 against the color actually behind it, which is the nearest ancestor that paints a background rather than the page. Large text, meaning 24px, or 18.66px when bold, needs 3 to 1. With the default values, ink on deep measures 1.05 to 1, accent on deep 3.62 to 1 and soft on deep 3.45 to 1, so none of those three belong on a dark panel. On a dark panel use the surface value for text. Soft measures 5.06 to 1 on the band and 5.42 on surface, and white on the accent measures 5.17 to 1.

A tint such as `rgb(var(--evd-accent) / .10)` is mostly the color behind it, so judge contrast against the composited result.
