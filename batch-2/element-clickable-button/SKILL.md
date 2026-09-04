---
name: clickable-button
description: A call to action or navigation control inserted into a section. Use for a solid, outline, gradient, glass, icon only, pill or square button, in a primary, secondary or text role. Not for a whole call to action section, and not for a link inside a sentence.
---

# Clickable Button

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

Put them inside `@layer ebt-base { }`. This matters: a layered declaration loses to an unlayered one whatever its specificity or its order, so Tailwind on a real page still wins every one of them, and the layer only shows through where there is no Tailwind. Without the layer, a rule such as `.ebt h2` would outrank `.text-4xl` and the block would start overriding the page.

Start with the five reset lines. Tailwind normally supplies these, and without them the list markers show through, the type falls back to a serif, and the content runs wider than the screen. That was measured: a first attempt without them put a block 36px past the right edge of a 1280px viewport.

    @layer ebt-base {
      .ebt,.ebt *,.ebt *::before,.ebt *::after{box-sizing:border-box}
      .ebt{font-family:ui-sans-serif,system-ui,-apple-system,"Segoe UI",Roboto,sans-serif;line-height:1.5}
      .ebt h1,.ebt h2,.ebt h3,.ebt h4,.ebt p,.ebt figure,.ebt blockquote{margin:0}
      .ebt ul,.ebt ol{margin:0;padding:0;list-style:none}
      .ebt img,.ebt svg,.ebt video{display:block;max-width:100%}

Then the block's own shape: the section's background, text color and padding, the wrapper's maximum width, every row or grid the layout depends on, and the heading and body type sizes.

      .ebt{background-color:rgb(var(--ebt-surface));color:rgb(var(--ebt-ink));padding:3rem 1.25rem}
      .ebt .ebt-wrap{max-width:64rem;margin-inline:auto}
      .ebt .ebt-row{display:grid;gap:1.5rem}
      .ebt h2{font-size:1.875rem;line-height:1.2;font-weight:600}
      .ebt h3{font-size:1.125rem;font-weight:600}
      .ebt p{font-size:1rem;line-height:1.625}
    }

About twelve declarations in all. Do not try to restate every utility you used. This is the block's skeleton, not a second copy of Tailwind.

**Nothing else is assumed.** The block does not expect the page to define a color, a font, a helper class or a shared stylesheet.

This skill is self-contained. Do not read, reference or depend on any other skill, and do not assume that a shared stylesheet, a motion system, a component library or a set of helper classes exists somewhere. If something is not described here, it is not available.

## What it is for

A CTA or navigation control inserted into a section.

The one thing you want the reader to do next, made obvious and easy to hit.

## When to use it

At the end of a section that has made its case, and anywhere the reader is likely to be ready to act.

## When not to use it

When there is nothing new to act on, and when the section already carries a button. Two primary buttons in one section split the decision and both get pressed less.

## Where it belongs on the page

After the content that earns it, aligned with the content rather than floated off on its own.

## What may be changed

The label, the destination, the size, the style, whether an icon is included and on which side, and whether the button is full width on a phone.

## What must not be changed

It must be a real `<a>` when it navigates and a real `<button>` when it acts. A styled div is not reachable by keyboard, is not announced as a control, and cannot be pressed with the space bar. The label must say what happens: Book a survey, not Click here.

## Spacing

Reported and measured on this block: 24px between the sentence and the row of buttons, and 12px between the buttons. Both are too tight, and the second one made the outlined button and the text button read as a single shape.

- **32px** between the last line of copy and the row. A 44px control needs more air above it than the gap between two lines of type.
- **16px** between buttons, and the same when they wrap to a second line.
- A text button carries no border, so give it the same horizontal padding as the others, about 14px, or it sits hard against its neighbour.
- On a narrow screen the primary button goes full width and the others stay their own width underneath it.

## Settings and Controls

- Set label, destination, size, width, alignment, icon, icon position, disabled state, and mobile width.
- Choose internal page, external page, file, pop-up, form, checkout, phone, email, or scroll-to-section action.
- Control primary, secondary, or text-button role and whether the button opens in a new window.

## Creative Design Options

- **Style.** Solid, outline, gradient, glass, icon-only, pill, square, soft shadow, or illuminated CTA.
- **Hover.** Enlarge, lift, magnetic pull, color shift, arrow movement, border draw, or glow.
- **Motion.** Realistic shine sweep, gradient movement, pulse, icon bounce, press feedback, or success state.
- **Emphasis.** Optional premium CTA treatment; repeating shine should be controlled rather than automatic everywhere.

## Motion

A lift of a few pixels and a shadow on hover and on focus, over about 200ms. A shine sweep is available but should be used on one button per page at most, because a repeating shine everywhere reads as a banner advert. Under `prefers-reduced-motion` the transition is removed and the hover state changes color only.

## Responsive and Accessibility

At least 44px tall so it can be hit with a thumb. Full width below 480px when it is the primary action. The focus ring is visible and is never removed. An icon only button carries a real accessible name.

## Defaults

Solid primary, medium size, arrow icon on the right, lift on hover, auto width.

## What breaks this block

A div with a click handler, which is not a control at all. Removing the focus outline, which strands anybody using a keyboard. A label that reads Click here, which tells a screen reader user nothing when the links are listed on their own.
- **An icon with no size of its own.** `<svg class="h-4 w-4" viewBox="0 0 20 20">` is 16px only while Tailwind is loaded. Strip the stylesheet away and the SVG has no intrinsic size and stretches to the width of whatever contains it: measured at 900 by 900 inside a 900px section, with the rest of the block pushed off the screen. Fix it with a `width` and a `height` attribute on the tag, and a base layer rule for any icon used more than once.
- **A height attribute on an illustration that fills its column.** The reverse mistake, and it shows up on a page that has Tailwind as well as one that does not. The width follows the column while the height is pinned to the attribute, so the drawing is squashed: measured at 656 by 380 where it should have been 656 by 519. An svg whose width follows its container keeps its height free.
- **A height attribute on an illustration that fills its column.** The reverse mistake, and it shows up on a page that has Tailwind as well as one that does not. The width follows the column while the height is pinned to the attribute, so the drawing is squashed: measured at 656 by 380 where it should have been 656 by 519. An svg whose width follows its container keeps its height free.

## What a static block cannot do

A success state after submission, a magnetic pull that follows the pointer, and a press animation that persists all need a script. Hover, focus and active states cover almost everything a static button needs.

## Images

Images come from the customer's brief: their own photographs, their own logo, the artwork they supplied. If the brief carries none, use the placeholder that ships inside this block, which is inline SVG and needs no request.

Never reach for a stock photo address. Unsplash, Pexels, Pixabay, a CDN, a hotlinked image on someone else's site: all of them are external files, all of them are ruled out here, and a broken address leaves a hole in the layout with no warning.

Give every image an alt description that says what is in it. Decorative artwork takes `alt=""` so a screen reader passes over it rather than reading a filename.

## Color

The block carries its own color. Six variables are declared on the block itself, and every colored part reads one of them.

| Variable | Used for |
| --- | --- |
| `--ebt-surface` | card and panel faces |
| `--ebt-band` | the quiet band behind the content |
| `--ebt-ink` | headings and primary copy |
| `--ebt-soft` | supporting copy |
| `--ebt-accent` | the one emphasis color |
| `--ebt-deep` | the dark ground |

The values are channel triplets, `15 23 42` rather than `#0f172a`, so an opacity suffix still works: `rgb(var(--ebt-ink) / .70)`.

Rebrand the block by changing those six values, in one place. Do not spread hex values through the markup, and do not reach for a color name out of the page's Tailwind config. Every page the builder makes writes its own config with its own color names, so a name taken from one page resolves to nothing on the next. Measured on two live sites: `bg-surface` painted no background on either, and `muted` was a pale background on one and a dark text color on the other.

Do not invent CSS variable names such as `var(--color-surface)`. Nothing defines them, the browser throws the declaration away, and that color or border renders as nothing at all. Do not use `text-current/60` either, because an opacity modifier has no effect on `currentColor`.

Every piece of text has to reach 4.5 to 1 against the color actually behind it, which is the nearest ancestor that paints a background rather than the page. Large text, meaning 24px, or 18.66px when bold, needs 3 to 1. With the default values, ink on deep measures 1.05 to 1, accent on deep 3.62 to 1 and soft on deep 3.45 to 1, so none of those three belong on a dark panel. On a dark panel use the surface value for text. Soft measures 5.06 to 1 on the band and 5.42 on surface, and white on the accent measures 5.17 to 1.

A tint such as `rgb(var(--ebt-accent) / .10)` is mostly the color behind it, so judge contrast against the composited result.
