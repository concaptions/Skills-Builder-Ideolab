---
name: social-media-links
description: A set of social links inserted inside a section. Use for an icon row, an icon and label list, or a set of branded badges. Not for a share bar that posts to a network, which needs a script.
---

# Social Media Links

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
- Give every **icon** a `width` and a `height` attribute as well as its size classes. An SVG that carries only a `viewBox` has no size of its own, so `h-4 w-4` is the only thing holding a 16px icon at 16px. Measured in a plain HTML viewer with no Tailwind, that same icon rendered at 900 by 900 and swallowed the section around it. The attribute is the weakest thing on the page, so Tailwind still wins wherever it is loaded, and the attribute only shows through where it is not. Size any icon that repeats in the base layer as well.
- Do the opposite for an **illustration that fills its column**. An SVG set to the full width of its container takes its height from the viewBox ratio, and a `height` attribute overrides that and squashes the drawing: measured, one illustration went from 656 by 519 to 656 by 380. Leave its height free, or pair the attributes with `height:auto` in the base layer. The test is simple: a fixed size gets both attributes, a width that follows the column does not get a height.

**Write the block's own base layer.** Tailwind utilities carry the detail, but a plain HTML viewer has no Tailwind, and a block built only from utilities arrives there as unstyled text on a white page. So write the few declarations that carry the block's shape into its own stylesheet as well.

Put them inside `@layer esm-base { }`. This matters: a layered declaration loses to an unlayered one whatever its specificity or its order, so Tailwind on a real page still wins every one of them, and the layer only shows through where there is no Tailwind. Without the layer, a rule such as `.esm h2` would outrank `.text-4xl` and the block would start overriding the page.

Start with the five reset lines. Tailwind normally supplies these, and without them the list markers show through, the type falls back to a serif, and the content runs wider than the screen. That was measured: a first attempt without them put a block 36px past the right edge of a 1280px viewport.

    @layer esm-base {
      .esm,.esm *,.esm *::before,.esm *::after{box-sizing:border-box}
      .esm{font-family:ui-sans-serif,system-ui,-apple-system,"Segoe UI",Roboto,sans-serif;line-height:1.5}
      .esm h1,.esm h2,.esm h3,.esm h4,.esm p,.esm figure,.esm blockquote{margin:0}
      .esm ul,.esm ol{margin:0;padding:0;list-style:none}
      .esm img,.esm svg,.esm video{display:block;max-width:100%}

Then the block's own shape: the section's background, text color and padding, the wrapper's maximum width, every row or grid the layout depends on, and the heading and body type sizes.

      .esm{background-color:rgb(var(--esm-surface));color:rgb(var(--esm-ink));padding:3rem 1.25rem}
      .esm .esm-wrap{max-width:64rem;margin-inline:auto}
      .esm .esm-row{display:grid;gap:1.5rem}
      .esm h2{font-size:1.875rem;line-height:1.2;font-weight:600}
      .esm h3{font-size:1.125rem;font-weight:600}
      .esm p{font-size:1rem;line-height:1.625}
    }

About twelve declarations in all. Do not try to restate every utility you used. This is the block's skeleton, not a second copy of Tailwind.

**Nothing else is assumed.** The block does not expect the page to define a color, a font, a helper class or a shared stylesheet.

This skill is self-contained. Do not read, reference or depend on any other skill, and do not assume that a shared stylesheet, a motion system, a component library or a set of helper classes exists somewhere. If something is not described here, it is not available.

## What it is for

A set of social links inserted inside a section.

The handful of places the business actually posts, linked plainly.

## When to use it

In a footer or a contact section, and anywhere the reader might want to see the work rather than read about it.

## When not to use it

Near the top of a page. Social links at the top send readers away before they have read anything. Do not list an account that has not been posted to in a year.

## Where it belongs on the page

In the footer, or at the end of a contact section under the phone number and the address.

## What may be changed

Which networks appear, the order, whether labels show beside the icons, the shape of the button, and the arrangement.

## What must not be changed

Every link carries a real accessible name, because a row of icons with no names is announced as a run of blank links. The icon is `aria-hidden` and the name is text, either visible or visually hidden. External links open in the same tab unless there is a reason not to.

## The marks

The block carries a set of thirty, so a builder is never stuck without the mark for the one place a business actually posts to: Facebook, Instagram, YouTube, LinkedIn, X, TikTok, WhatsApp, Threads, Pinterest, Snapchat, Reddit, GitHub, Telegram, Discord, Twitch, Vimeo, Google, Trustpilot, Yelp, Nextdoor, Houzz, Checkatrade, Bluesky, Mastodon, Substack, Medium, Spotify, Podcasts, RSS and email.

- **Drawn here, not downloaded.** No external files are allowed in these blocks, and a set drawn in one line weight reads as one set rather than as a row of borrowed logos. A brand's own logo file also carries terms of use. Where a customer supplies their own mark, it drops straight in.
- **Every mark is one style.** Same 24 unit box, same 1.9 stroke, same round caps. Mixing a filled brand logo into a line set is the quickest way to make a footer look assembled by two different people.
- **The set is not the row.** Show only the places the business posts to. A wall of thirty marks tells a visitor nothing and dilutes the two that matter.
- **If a platform is missing**, draw it in the same box at the same weight rather than reaching for an icon font.

## Settings and Controls

- Choose networks, order, URLs, labels, icon size, link behavior, and accessible names.
- Set horizontal, vertical, compact, or floating placement and mobile spacing.

## Creative Design Options

- **Layout.** Icon row, icon-and-label list, floating rail, branded badges, or share bar.
- **Motion.** Hover fill, lift, rotate, label reveal, ripple, or subtle icon bounce.
- **Styling.** Brand colors, monochrome, outline, gradient, glass, or soft circular buttons.

## Motion

The button fills with colour and lifts a little on hover and focus, over about 200ms. Nothing repeats.

## Responsive and Accessibility

The row wraps rather than shrinking. Every target is at least 44px so it can be hit with a thumb, even when the icon inside it is 20px.

## Defaults

Icon row, four networks, circular buttons, visually hidden names, fill on hover.

## What breaks this block

Icon links with no accessible name. Targets smaller than a thumb. A brand colour that leaves the icon below 4.5 to 1 against it.
- **An icon with no size of its own.** `<svg class="h-4 w-4" viewBox="0 0 20 20">` is 16px only while Tailwind is loaded. Strip the stylesheet away and the SVG has no intrinsic size and stretches to the width of whatever contains it: measured at 900 by 900 inside a 900px section, with the rest of the block pushed off the screen. Fix it with a `width` and a `height` attribute on the tag, and a base layer rule for any icon used more than once.
- **A height attribute on an illustration that fills its column.** The reverse mistake, and it shows up on a page that has Tailwind as well as one that does not. The width follows the column while the height is pinned to the attribute, so the drawing is squashed: measured at 656 by 380 where it should have been 656 by 519. An svg whose width follows its container keeps its height free.
- **A height attribute on an illustration that fills its column.** The reverse mistake, and it shows up on a page that has Tailwind as well as one that does not. The width follows the column while the height is pinned to the attribute, so the drawing is squashed: measured at 656 by 380 where it should have been 656 by 519. An svg whose width follows its container keeps its height free.

## What a static block cannot do

A share action that opens a network composer, a follower count and a floating rail that appears on scroll all need a script. These are links, and links are enough.

## Images

Images come from the customer's brief: their own photographs, their own logo, the artwork they supplied. If the brief carries none, use the placeholder that ships inside this block, which is inline SVG and needs no request.

Never reach for a stock photo address. Unsplash, Pexels, Pixabay, a CDN, a hotlinked image on someone else's site: all of them are external files, all of them are ruled out here, and a broken address leaves a hole in the layout with no warning.

Give every image an alt description that says what is in it. Decorative artwork takes `alt=""` so a screen reader passes over it rather than reading a filename.

## Color

The block carries its own color. Six variables are declared on the block itself, and every colored part reads one of them.

| Variable | Used for |
| --- | --- |
| `--esm-surface` | card and panel faces |
| `--esm-band` | the quiet band behind the content |
| `--esm-ink` | headings and primary copy |
| `--esm-soft` | supporting copy |
| `--esm-accent` | the one emphasis color |
| `--esm-deep` | the dark ground |

The values are channel triplets, `15 23 42` rather than `#0f172a`, so an opacity suffix still works: `rgb(var(--esm-ink) / .70)`.

Rebrand the block by changing those six values, in one place. Do not spread hex values through the markup, and do not reach for a color name out of the page's Tailwind config. Every page the builder makes writes its own config with its own color names, so a name taken from one page resolves to nothing on the next. Measured on two live sites: `bg-surface` painted no background on either, and `muted` was a pale background on one and a dark text color on the other.

Do not invent CSS variable names such as `var(--color-surface)`. Nothing defines them, the browser throws the declaration away, and that color or border renders as nothing at all. Do not use `text-current/60` either, because an opacity modifier has no effect on `currentColor`.

Every piece of text has to reach 4.5 to 1 against the color actually behind it, which is the nearest ancestor that paints a background rather than the page. Large text, meaning 24px, or 18.66px when bold, needs 3 to 1. With the default values, ink on deep measures 1.05 to 1, accent on deep 3.62 to 1 and soft on deep 3.45 to 1, so none of those three belong on a dark panel. On a dark panel use the surface value for text. Soft measures 5.06 to 1 on the band and 5.42 on surface, and white on the accent measures 5.17 to 1.

A tint such as `rgb(var(--esm-accent) / .10)` is mostly the color behind it, so judge contrast against the composited result.
