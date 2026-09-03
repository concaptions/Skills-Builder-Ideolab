---
name: section-image-and-text
description: A general-purpose storytelling section pairing written content with a strong visual. Use for image-left or image-right rows, an overlapping image and card, a full-bleed visual, text over an image, or alternating rows down a page. The workhorse section for any point that needs one picture and a paragraph.
---

# Image and Text

## What it is for

A picture beside a paragraph, usually repeated two or three times with the picture alternating sides. It is the workhorse for explaining several things at a pace the visitor controls.

## When to use it

- There are two to four points that each need a sentence or two plus a visual.
- The points are unrelated enough that a grid would flatten them.
- Real photographs or meaningful visuals exist for each row.

## When not to use it

- There is only one point. That is a single feature panel.
- There are six or more rows. The page becomes a corridor and people stop reading.
- The images are decorative stock. An empty picture beside every paragraph adds length and nothing else.

## Where it belongs on the page

Middle of the page, after the offer and before or around the proof. It is often the main body of a services page.

## What may be changed

- Number of rows, and which side the first image sits on.
- Image aspect and corner treatment.
- Copy, eyebrow, headings and the link at the end of each row.
- Vertical spacing between rows.

## What must not be changed

- The image coming first on mobile in every row. A stack of paragraphs with the image below reads as a wall of text.
- The alternating rule being driven by position rather than marked by hand, so adding or removing a row keeps the rhythm.
- Alt text on every meaningful image.
- The reduced motion rules.

## Settings and Controls

| Setting | Options | Default |
| --- | --- | --- |
| Image position | Left, right, above, behind the text, or alternating per row | right |
| Content width | Narrow, contained, or full width | contained |
| Section height | Auto, fixed, or full viewport | auto |
| Column ratio | Equal, text-led (60:40), or image-led (40:60) | equal |
| Crop | Square, 4:3, 3:2, 16:9, portrait, or original | 3:2 |
| Focal point | Where the crop centers, set as a point on the image | center |
| Padding | Compact, standard, or generous | standard |
| Alignment | Text top, center, or bottom relative to the image | center |
| Show or hide | Eyebrow, headline, paragraph, list, image caption, button, secondary link | headline, paragraph, button |
| Media source | Uploaded image, Brand Gallery image, generated image, background image, or video | uploaded image |
| Mobile stacking order | Image first, or text first | image first |
| Image width on mobile | Full width, or contained | contained |

**Focal point is the setting that stops faces being cropped.** Any crop other than the original ratio will cut something. Set the focal point on the subject, not on the middle of the frame.

**Mobile stacking order is a real decision.** Image first gives the reader something to look at immediately. Text first is right when the image only makes sense after the copy explains it, such as a chart or a diagram.

**Text over an image is a different job from image-behind-text.** Text over an image needs a scrim, a focal point set to empty space, and a fallback color behind it in case the image fails to load.

## Creative Design Options

**Layout**

- Image left. Text right.
- Image right. Text left. The default.
- Overlapping image and card. The copy sits in a card that overlaps the edge of the image.
- Full-bleed visual. The image runs to the edges of the viewport, with the copy contained above or below.
- Text over image. The copy sits on the image behind a scrim.
- Alternating rows. Several of these stacked, with the image swapping side each row.

**Image treatment**

Rounded frame, editorial crop, gradient overlay, cutout subject, collage, shadow, or decorative border.

**Motion**

Hover zoom, parallax, mask reveal, slow pan, floating depth, or image swap.

**Typography**

Word or line reveal, gradient phrase, gold-to-soft-gold line progression, or rolling emphasized text.

The gold-to-soft-gold treatment generalizes: it is a bright-to-soft progression of one brand color across successive lines, so it works on any palette.

## Motion

- **Entrance:** the row fades and rises as one unit at `standard` intensity. The image and text do not arrive separately, because a split entrance draws attention to the seam.
- **Hover zoom** is the default, at 2 to 4 percent.
- **Parallax, slow pan and floating depth** are scroll-linked, so they count as the page's one major showcase effect. A page with three alternating rows must not give all three parallax.
- **Mask reveal** wipes the image in on entrance, once. Use it on one row per page, not every row.
- **Rolling emphasized text** cycles a single word inside the headline. Bound it: either a set number of cycles, or stop when the section leaves the viewport.
- **Word or line reveal** applies to the headline only. Body paragraphs appear immediately.

## Responsive and Accessibility

| Width | Behavior |
| --- | --- |
| Desktop, 1024px and up | Side-by-side at the chosen ratio. Parallax and overlap active |
| Tablet, 768px to 1023px | Side by side at equal ratio, or stacked if the copy is long. Overlap is removed |
| Mobile, under 768px | Stacked in the chosen order. Parallax off. Text over image keeps the scrim and gains extra padding |

- The image needs alt text describing what it shows. A decorative image takes `alt=""`.
- An image caption is a `<figcaption>` inside a `<figure>`, not a loose paragraph.
- Text over an image must meet contrast against the darkest and the lightest part of the image it covers, measured with the scrim applied.
- A background video is muted, carries `playsinline`, has a poster image, and never carries information that is not also in the text.
- With reduced motion, parallax, pan, floating depth and mask reveals are removed, rolling text settles on its first word, and the image appears in place.

## Defaults

Image right, equal ratio, 3:2 crop, center focal point, contained width, auto height, standard padding, center aligned, headline and paragraph and button shown, rounded frame, image first on mobile, standard fade-and-rise entrance, hover zoom.

## What breaks this section

1. **Reordering in the markup instead of with `order`.** It flips the reading order for keyboard and screen-reader users, and it changes what appears first on mobile.
2. **Cropping without a focal point.** Faces lose their tops on the ratio that was not designed for.
3. **Giving every row parallax.** Three parallax rows on one page is the "one major showcase effect" rule broken twice.

## Color

Use the page's theme tokens. Never hardcode a hex value or a Tailwind palette color, because this section has to work on a dark editorial page and a light clinical one without being rewritten.
