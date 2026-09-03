# Website Builder Skills

Sixteen website sections, built to the sections listed in the client requirements document.

Each section is delivered as three things:

| File | What it is |
| --- | --- |
| `SKILL.md` | The instructions. What the section is for, when to use it, when not to, where it belongs on a page, what may be changed and what must not. Text only. |
| `element.html` | The code. A real, working block, ready to place on a page. |
| `thumbnail.png` | A picture of what that block actually looks like, rendered from the block itself. |

## The sections

| Section | What it does |
| --- | --- |
| `section-about-our-brand` | The story behind the business, with supporting figures |
| `section-animated-logo-stream` | A looping row of client or accreditation logos |
| `section-animated-timeline` | Dated milestones, alternating either side of a spine |
| `section-customer-benefits` | Outcomes for the customer, in a grid |
| `section-expandable-content` | Questions and answers, one open at a time |
| `section-features-and-details` | What is included, plus a specification table |
| `section-graphic-illustration` | A labelled diagram with call outs |
| `section-horizontal-stack-cards` | A sideways rail of cards that snaps |
| `section-how-it-works` | Three numbered steps with a connector |
| `section-image-and-text` | Alternating picture and paragraph rows |
| `section-photo-gallery` | A filterable grid of photographs |
| `section-products-and-services` | Priced cards with one action each |
| `section-sales-video` | A video inside an offer, with the action always visible |
| `section-tabbed-content` | Parallel panels, one shown at a time |
| `section-vertical-stack-cards` | Cards that pile up as the page scrolls |
| `section-video` | A single video, loaded only when played |

## How the blocks are built

Every `element.html` is a plain HTML block. It contains one `<section>`, Tailwind utility classes, and a small scoped stylesheet for the parts Tailwind does not cover.

- **No JavaScript.** Interaction that normally needs a script is done in CSS instead: the accordion uses `details` and `summary`, the tabs are a radio group, the card rail is a real scroll container with snapping, the logo stream is a CSS animation, and the video players load on `:target`.
- **No external files.** No image URLs, no icon library, no fonts. Placeholder artwork is inline SVG, so a block renders correctly on its own.
- **No page wrapper.** There is no doctype, head or body tag. The block is meant to be dropped into a page that already exists.
- **No hardcoded colors.** Blocks use the page's theme tokens (`surface`, `muted`, `textdark`, `textmute`, `accent`, `bgdark`), so the same section works on a light page and a dark one.
- **Reduced motion is handled in every block.** Anything that moves is removed when the visitor has asked for less motion.
- **Class prefixes.** Every block prefixes its own classes, so nothing collides with the rest of the page. Three blocks also use ids, because their interaction depends on them: the photo gallery filters, the tabbed content, and the two video players. Placing one of those twice on the same page means changing its prefix.

## What was checked

Each block was rendered in a real browser and driven, not just looked at:

- no script tags, no page wrapper, no external asset requests
- no page errors, and nothing overflowing the viewport horizontally
- a reduced motion rule present in every block
- the gallery filters actually narrow the grid and restore it
- clicking a tab actually swaps the panel, and only one panel is ever visible
- the accordion opens, and keeps only one item open at a time

All 122 checks pass. The thumbnails are rendered from the same blocks, so a thumbnail can never drift away from the code it represents.
