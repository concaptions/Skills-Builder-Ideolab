---
name: contact-or-lead-form
description: A form inserted inside a section to collect visitor information. Use for an inline form, a form in a card, a two column form, a compact signup, or a form beside an image. Not for a multi step form, and not for checkout.
---

# Contact or Lead Form

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

Put them inside `@layer efm-base { }`. This matters: a layered declaration loses to an unlayered one whatever its specificity or its order, so Tailwind on a real page still wins every one of them, and the layer only shows through where there is no Tailwind. Without the layer, a rule such as `.efm h2` would outrank `.text-4xl` and the block would start overriding the page.

Start with the five reset lines. Tailwind normally supplies these, and without them the list markers show through, the type falls back to a serif, and the content runs wider than the screen. That was measured: a first attempt without them put a block 36px past the right edge of a 1280px viewport.

    @layer efm-base {
      .efm,.efm *,.efm *::before,.efm *::after{box-sizing:border-box}
      .efm{font-family:ui-sans-serif,system-ui,-apple-system,"Segoe UI",Roboto,sans-serif;line-height:1.5}
      .efm h1,.efm h2,.efm h3,.efm h4,.efm p,.efm figure,.efm blockquote{margin:0}
      .efm ul,.efm ol{margin:0;padding:0;list-style:none}
      .efm img,.efm svg,.efm video{display:block;max-width:100%}

Then the block's own shape: the section's background, text color and padding, the wrapper's maximum width, every row or grid the layout depends on, and the heading and body type sizes.

      .efm{background-color:rgb(var(--efm-surface));color:rgb(var(--efm-ink));padding:3rem 1.25rem}
      .efm .efm-wrap{max-width:64rem;margin-inline:auto}
      .efm .efm-row{display:grid;gap:1.5rem}
      .efm h2{font-size:1.875rem;line-height:1.2;font-weight:600}
      .efm h3{font-size:1.125rem;font-weight:600}
      .efm p{font-size:1rem;line-height:1.625}
    }

About twelve declarations in all. Do not try to restate every utility you used. This is the block's skeleton, not a second copy of Tailwind.

**Nothing else is assumed.** The block does not expect the page to define a color, a font, a helper class or a shared stylesheet.

This skill is self-contained. Do not read, reference or depend on any other skill, and do not assume that a shared stylesheet, a motion system, a component library or a set of helper classes exists somewhere. If something is not described here, it is not available.

## What it is for

A form inserted inside a section to collect visitor information.

The shortest set of questions that lets somebody get back to the visitor. Every extra field costs replies.

## When to use it

At the point where the reader is ready to make contact, and at the foot of a page as the last chance to.

## When not to use it

Before the reader knows what the company does. A form at the top of a page asks for something before anything is offered.

## Where it belongs on the page

In its own section or beside the contact details, never split across a page break.

## What may be changed

Which fields appear, which are required, the labels, the consent line, and the wording on the submit button.

## What must not be changed

Every field has a real `<label>` tied to it with `for`, not a placeholder standing in for one, because a placeholder disappears as soon as typing starts and is not read as a label. Required fields are marked in the text, not by colour alone. The consent checkbox is never pre ticked.

## Settings and Controls

- Choose fields, required fields, labels, placeholders, consent, destination, notifications, and success action.
- Set inline or multi-step behavior, width, alignment, validation, spam protection, and mobile layout.
- Control whether the form opens a thank-you message, redirects, downloads a resource, or starts a workflow.

## Creative Design Options

- **Layout.** Inline form, card, two-column form, multi-step form, compact signup, or form beside an image.
- **Motion.** Field focus, validation feedback, step progress, submit state, and success confirmation.
- **Styling.** Minimal lines, soft input cards, gradient submit button, dark form, or floating glass panel.

## Motion

The field border changes colour on focus, over about 160ms. Nothing else moves. A form that animates while somebody is typing into it is a distraction at the worst moment.

## Responsive and Accessibility

One column below 768px. Inputs are at least 44px tall and their text is at least 16px, which is what stops a phone browser zooming in when a field is focused. The focus ring is always visible.

## Defaults

Name, email, phone, message, consent checkbox, one column, inline layout, no card.

## What breaks this block

Placeholders used instead of labels, which leaves the field unlabelled once typing starts. Inputs under 16px, which makes the phone zoom. A required marker shown only in red, which is invisible to anybody who cannot see the colour.
- **An icon with no size of its own.** `<svg class="h-4 w-4" viewBox="0 0 20 20">` is 16px only while Tailwind is loaded. Strip the stylesheet away and the SVG has no intrinsic size and stretches to the width of whatever contains it: measured at 900 by 900 inside a 900px section, with the rest of the block pushed off the screen. Fix it with a `width` and a `height` attribute on the tag, and a base layer rule for any icon used more than once.

## What a static block cannot do

Validation messages, a multi step flow, a step progress indicator, spam protection, a submit state and a thank you message all need a script or a server. The form here posts to an address the customer supplies, and the browser's own `required` and `type` attributes do the checking.

## Images

Images come from the customer's brief: their own photographs, their own logo, the artwork they supplied. If the brief carries none, use the placeholder that ships inside this block, which is inline SVG and needs no request.

Never reach for a stock photo address. Unsplash, Pexels, Pixabay, a CDN, a hotlinked image on someone else's site: all of them are external files, all of them are ruled out here, and a broken address leaves a hole in the layout with no warning.

Give every image an alt description that says what is in it. Decorative artwork takes `alt=""` so a screen reader passes over it rather than reading a filename.

## Color

The block carries its own color. Six variables are declared on the block itself, and every colored part reads one of them.

| Variable | Used for |
| --- | --- |
| `--efm-surface` | card and panel faces |
| `--efm-band` | the quiet band behind the content |
| `--efm-ink` | headings and primary copy |
| `--efm-soft` | supporting copy |
| `--efm-accent` | the one emphasis color |
| `--efm-deep` | the dark ground |

The values are channel triplets, `15 23 42` rather than `#0f172a`, so an opacity suffix still works: `rgb(var(--efm-ink) / .70)`.

Rebrand the block by changing those six values, in one place. Do not spread hex values through the markup, and do not reach for a color name out of the page's Tailwind config. Every page the builder makes writes its own config with its own color names, so a name taken from one page resolves to nothing on the next. Measured on two live sites: `bg-surface` painted no background on either, and `muted` was a pale background on one and a dark text color on the other.

Do not invent CSS variable names such as `var(--color-surface)`. Nothing defines them, the browser throws the declaration away, and that color or border renders as nothing at all. Do not use `text-current/60` either, because an opacity modifier has no effect on `currentColor`.

Every piece of text has to reach 4.5 to 1 against the color actually behind it, which is the nearest ancestor that paints a background rather than the page. Large text, meaning 24px, or 18.66px when bold, needs 3 to 1. With the default values, ink on deep measures 1.05 to 1, accent on deep 3.62 to 1 and soft on deep 3.45 to 1, so none of those three belong on a dark panel. On a dark panel use the surface value for text. Soft measures 5.06 to 1 on the band and 5.42 on surface, and white on the accent measures 5.17 to 1.

A tint such as `rgb(var(--efm-accent) / .10)` is mostly the color behind it, so judge contrast against the composited result.
