---
name: section-video
description: A general video section for brand stories, demonstrations, interviews, explainers, or updates. Use for a centered player, a video beside text, a full-width video, a device mockup, a background video, or a playlist. Not for a conversion-focused sales video with an offer and CTA timing, which is the sales video section.
---

# Video

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

A single video presented on its own: a walkthrough, an introduction, a piece of evidence. It informs rather than sells directly.

## When to use it

- The video shows something words cannot, such as a place, a process or a finished result.
- It is short, ideally under three minutes.
- The page still makes sense for the majority who will not press play.

## When not to use it

- The video carries information found nowhere else on the page.
- It is a sales pitch attached to an offer. That is a sales video section.
- There is no real video yet. A placeholder player damages trust.

## Where it belongs on the page

Middle of the page as evidence, or near the top of an about page as an introduction.

## What may be changed

- The embed URL, the poster image and the caption beneath.
- Aspect ratio, and whether the player is full width or inset.
- Whether a caption appears at all.
- Play control size and styling.

## What must not be changed

- Click to load. Until the visitor presses play there should be no third party embed on the page, which keeps it fast and stops the video host setting cookies for people who never watched.
- A descriptive title on the iframe.
- A visible focus ring on the play control, so it can be reached by keyboard.
- The reduced motion rules.

## Settings and Controls

| Setting | Options | Default |
| --- | --- | --- |
| Video source | Uploaded file, YouTube, Vimeo, or another external player | uploaded file |
| Poster image | Uploaded, a frame from the video, or none | uploaded |
| Aspect ratio | 16:9, 4:3, 1:1, or 9:16 | 16:9 |
| Playback controls | Full controls, minimal, or none | full controls |
| Captions | Track file, burned in, or none | track file |
| Transcript | Shown, collapsed, or hidden | collapsed |
| Sound behavior | Sound on when played, muted, or muted with an unmute control | sound on when played |
| Playback mode | Inline, modal, autoplay muted, background, or external player | inline |
| Show or hide | Headline, supporting copy, play button, duration, transcript, CTA | headline, play button, duration |
| Section width | Contained or full width | contained |
| Player height | From the aspect ratio, or fixed | from the aspect ratio |
| Mobile playback | Inline, modal, or open in the native player | inline |
| Fallback image | Shown when the video cannot load | the poster |

**Autoplay only works muted, on every browser.** If the point of the video is what is being said, autoplay is the wrong mode. Use inline with a poster instead.

**Background playback is decoration, not content.** Anything the visitor needs to know must also be in the text, because a background video is muted, loops, and is skipped by anyone on a slow connection.

**Load the embed only when the visitor asks for it.** A YouTube or Vimeo iframe present on load pulls in a large third-party payload and sets cookies before consent. Render the poster and a play button, and insert the iframe on the first click.

## Creative Design Options

**Layout**

- Centered player. The video alone, contained, with a headline above. The default.
- Video with text. Player one side, copy the other.
- Full-width video. Edge to edge.
- Device mockup. The video inside a phone, laptop or browser frame.
- Background video. The video sits behind a headline and a scrim.
- Video playlist. A main player with a list of other videos beside or below it.

**Player style**

Minimal, cinematic, framed, floating card, browser mockup, or branded controls.

**Motion**

Poster zoom, play-button pulse, hover preview, scroll reveal, or transition into modal playback.

**Background**

Soft gradient, image blur, ambient color sampled from the video, or animated light field.

## Motion

- **Entrance:** the player fades and rises as one at `standard` intensity.
- **Poster zoom** on hover, 2 to 4 percent, inside a fixed frame.
- **Play-button pulse** is idle motion, so it is opt in. When used, keep it slow and bounded, and stop it once the video plays.
- **Hover preview** plays a short muted loop on hover on a pointer device only. It never runs on touch, because there is no hover there, and it must not be the only way to see what the video contains.
- **Transition into modal playback** scales the poster into the modal. The modal must trap focus, close on Escape, and pause the video when it closes.
- **Animated light field** and ambient color are continuous, so they stop when the section leaves the viewport.

## Responsive and Accessibility

| Width | Behavior |
| --- | --- |
| Desktop, 1024px and up | Full layout as chosen. Hover preview active |
| Tablet, 768px to 1023px | Video-with-text stacks. Device mockup keeps its frame |
| Mobile, under 768px | Full width, one column, no hover preview. Device mockup frame is dropped, because the frame around a phone-sized video wastes the width |

- The play control is a real button with an accessible label naming the video, not a bare triangle glyph.
- Captions are provided. A video with speech and no captions is not usable by everyone.
- A transcript below the video serves anyone who cannot or will not play it, and it is indexable.
- Autoplaying video is always muted and carries `playsinline`, otherwise it takes over the screen on iOS.
- A background video is `aria-hidden` and carries no information not present in the text.
- Modal playback returns focus to the play button on close.
- With reduced motion, poster zoom, play-button pulse, hover preview and the modal scale transition are all removed, and autoplay does not start.

## Defaults

Uploaded source, poster shown, 16:9, full controls, captions on, transcript collapsed, sound on when played, inline playback, headline and play button and duration shown, contained width, centered player, framed player style, standard fade-and-rise entrance, poster zoom on hover, no pulse.

## What breaks this section

1. **An iframe on page load.** It is usually the heaviest request on the page and it sets third-party cookies before anyone has asked for the video.
2. **Autoplay with sound.** Browsers block it, so the video silently does not start and the section looks broken.
3. **A play button that is a styled div.** It cannot be reached by keyboard, so the video cannot be played at all without a mouse.

## What a static block cannot do

These blocks carry no JavaScript. A few of the options above have no static equivalent, so build the nearest thing that does work and leave the rest out. Never ship a control that looks alive and does nothing when it is pressed.

- Every option here is buildable. Modal playback uses `:target`, a playlist is a radio group with one panel per video, a background video is `autoplay muted loop playsinline` on a `video` element, and the transcript is a `details` element.

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
