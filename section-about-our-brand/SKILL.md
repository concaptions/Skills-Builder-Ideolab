---
name: section-about-our-brand
description: A complete brand-story section that explains identity, origin, values, leadership, or purpose. Use when a page needs to say who the business is and why it exists, as an about block on a homepage or the main body of an about page. Not for listing what the business sells, and not for customer outcomes.
---

# About Our Brand

## What you must return

One `<section>` element, and nothing else.

- No `<!doctype>`, `<html>`, `<head>` or `<body>`. The block is dropped into a page that already exists.
- No markdown code fence around it, and no commentary before or after it. Return the HTML itself. A stray ``` renders as visible text on the page, and inside a grid it becomes an extra item that pushes the layout out of shape.
- No `<script>`, no event handlers, no `hidden` attributes waiting to be toggled. Nothing here can run.
- No external files of any kind: no image addresses, no icon library, no web font.
- Everything the section needs lives inside that one element, including any `<style>` it uses. Prefix your own class names so they cannot collide with the rest of the page.
- The section carries its own horizontal padding and its own maximum width. Drop that wrapper and the content runs to both edges of the screen.

This skill is self-contained. Do not read, reference or depend on any other skill, and do not assume that a shared stylesheet, a motion system, a component library or a set of helper classes exists somewhere. If something is not described here, it is not available.

## What it is for

The story behind the business, told so a stranger can decide whether these are people they want in their home or handling their money. It carries credibility, not features.

## When to use it

- The business competes on trust, longevity or the people themselves rather than on price.
- There is a real story: why it started, what changed, what the owner refuses to do.
- Real figures exist, such as years trading, jobs completed or team size.
- The visitor is comparing two similar suppliers and needs a reason to prefer one.

## When not to use it

- There is nothing specific to say. A paragraph of "we are passionate about quality" is worse than no section at all.
- The claims cannot be evidenced. Invented figures are the fastest way to lose a sale.
- The page already has a team section doing the same job.

## Where it belongs on the page

Lower middle of the page, after the offer and the proof, before the closing call to action. It answers "who are these people" once the visitor already cares what they sell.

## What may be changed

- Story copy, headline and eyebrow.
- The number, position and wording of the supporting figures.
- Image aspect, and whether the stat panel overlaps the image or sits under it.
- Whether the layout is image left or image right.

## What must not be changed

- The stat panel dropping below the image on mobile. Overlapping it on a narrow screen covers the subject of the photograph.
- Real, checkable figures. If a number cannot be evidenced, remove it rather than soften it.
- The reduced motion rules.

## Settings and Controls

| Setting | Options | Default |
| --- | --- | --- |
| Story depth | Single block, short summary, or multi-part narrative | single block |
| Show or hide | Headline, supporting copy, founder image, quote, values, button, supporting media | headline, supporting copy, founder image, values |
| Item count | Number of values, milestones or story parts | 3 |
| Content width | Narrow, contained, or full width | contained |
| Section height | Auto, fixed, or full viewport | auto |
| Image ratio | Square, 4:3, 3:2, portrait, or 16:9 | portrait |
| Text-to-image ratio | Equal, text-led, or image-led | equal |
| Alignment | Left, centered, or split | left |
| Padding | Compact, standard, or generous | standard |
| Background source | Brand color, image, video, or no background media | no background media |

**Story depth is the setting that matters most.** *Single block* is one headline and one or two paragraphs. *Short summary* is a headline and a single tight paragraph, used when the section is a stop on the way to a full about page. *Multi-part narrative* breaks the story into two or three chapters, each with its own subheading, and is the only depth that needs the section to be tall.

**A background video or image needs the text to stay readable.** Put a scrim between the media and the copy, and set the focal point so a face or a horizon is not sitting behind a headline.

## Creative Design Options

**Layout**

- Story with image. Copy one side, a photograph the other. The default and the safest.
- Founder spotlight. A portrait carries the section, with the story and a signed quote beside it.
- Editorial statement. Wide measure, large type, no image, laid out like a magazine opening.
- Values grid. A short intro over a grid of named values, each with an icon and a line.
- Manifesto. A sequence of short declarative lines, centered, each on its own row.
- Story-and-milestones layout. The narrative on one side, dated milestones running down the other.

**Typography**

Gradient-highlighted words, oversized quote, layered headline, or multi-tone line treatment.

Apply one, and apply it to the same element throughout. A gradient on the headline and an oversized quote in the same section fight each other.

**Imagery**

Full-bleed image, overlapping portrait, framed photograph, soft background image, or image collage.

**Motion**

Image parallax, staggered values, line-by-line story reveal, animated quote, or subtle background movement.

## Motion

- **Entrance:** the section fades and rises as one, at `standard` intensity. With the values grid or milestones, stagger the items by 60ms.
- **Line-by-line story reveal** applies to the headline only. Body paragraphs appear immediately and are never animated in.
- **Image parallax and subtle background movement** are scroll-linked, so they count as the page's one major showcase effect. Do not use them on a page that already has a sticky stack or a scroll-driven story.
- **Animated quote** means the quote mark or the attribution line reveals after the quote text, not the quote animating letter by letter.
- **Hover:** the image takes the default 2 to 4 percent zoom.

## Responsive and Accessibility

| Width | Behavior |
| --- | --- |
| Desktop, 1024px and up | Full layout as chosen |
| Tablet, 768px to 1023px | Split layouts become one column. Values grid drops to two columns |
| Mobile, under 768px | One column, image above text. Values grid becomes one column. Parallax is switched off |

- The headline uses a real heading level in sequence. Value names are headings too, not styled text.
- A founder photograph needs alt text naming the person and their role, not "founder image".
- A quote uses `<blockquote>` with `<cite>` for the attribution.
- With reduced motion, parallax and background movement stop, the story reveal is removed, and all content is visible immediately.
- Text over a background image or video must clear the contrast threshold with the scrim applied, not without it.

## Defaults

Single block story, story-with-image layout, image right, portrait ratio, equal text-to-image, left aligned, contained width, standard padding, no background media, three values shown, framed photograph, standard fade-and-rise entrance with staggered values, image hover zoom.

## What breaks this section

1. **Fixing the section height with a long story.** The copy overflows or gets clipped. Use `auto` height unless the story is genuinely short.
2. **Animating the body paragraphs in.** The reader arrives at an empty column and waits. Only the headline reveals.
3. **A background image with no scrim.** It reads fine on the designer's photo and fails on the customer's.

## What a static block cannot do

These blocks carry no JavaScript. A few of the options above have no static equivalent, so build the nearest thing that does work and leave the rest out. Never ship a control that looks alive and does nothing when it is pressed.

- **Image parallax** and **subtle background movement** are scroll-linked. See below.

**Scroll-linked motion.** Anything whose state has to follow the scroll position uses `animation-timeline: view()` inside an `@supports` block. Firefox does not support it, so write the finished state as the default and let the animation be the enhancement. Never use a library or an observer for this.

## Images

Every image belongs to the customer. Use the photographs, logos and artwork supplied in the brief.

Never pull a photograph from a stock site. A link to Unsplash, Pexels, Shutterstock or any other outside address is wrong on two counts: it is not the customer's business in the picture, and the block stops working the moment that address changes.

If no photograph has been supplied yet, keep the placeholder that ships inside the block. It is drawn in the page itself, so it needs nothing from outside, it takes the theme colors, and it holds the right shape so the layout does not move when the real photograph arrives.

Give every image an `alt` description of what is in it, and a `width` and `height`, so the page does not jump as it loads.

## Color

Use the page's theme tokens. Never hardcode a hex value or a Tailwind palette color, because this section has to work on a dark editorial page and a light clinical one without being rewritten.

The tokens are `surface`, `muted`, `textdark`, `textmute`, `accent` and `bgdark`. Use them as Tailwind classes: `bg-surface`, `bg-muted`, `bg-bgdark`, `text-textdark`, `text-textmute`, `text-accent`, `border-accent`.

Do not invent CSS variable names such as `var(--color-surface)` or `var(--color-border)`. Nothing on the page defines them, so the browser throws the declaration away and that color, border or shape renders as nothing at all. Do not use `text-current/60` either, because an opacity modifier has no effect on `currentColor` and the text comes out at full strength.

A token is not readable just because it is a token. Every piece of text has to reach 4.5 to 1 against the color actually behind it, which is the nearest ancestor that paints a background rather than the page. Large text, meaning 24px, or 18.66px when it is bold, needs 3 to 1.

These four pairs are measured and they fail, so do not reach for them.

| Text | Background | Measured |
| --- | --- | --- |
| `text-textdark` | `bg-bgdark` | 1.05 to 1, near black on near black |
| `text-accent` | `bg-bgdark` | 3.62 to 1, the accent is a dark blue and it sinks into a dark panel |
| `text-textmute` | `bg-bgdark` | 3.93 to 1 |
| `text-textmute` | `bg-muted` | 4.44 to 1, although the same token clears on `bg-surface` at 4.76 to 1 |

On a dark panel, set text in `text-surface` and use a lowered opacity of it for the quieter line. The accent still works there as a button's background, where white sits on it at 6.3 to 1. On a muted panel, use a lowered opacity of `text-textdark` for quiet copy.

A tint such as `bg-accent/10` is mostly the color behind it, so judge contrast against the composited result, not against the accent itself.
