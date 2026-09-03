# Website Builder Skills

Seventeen skills: sixteen website sections, and one for rebuilding a section from a picture of it.

Each one is delivered as three things:

| File | What it is |
| --- | --- |
| `SKILL.md` | The instructions. What it is for, when to use it, when not to, where it belongs on a page, what may be changed and what must not. Text only. |
| `element.html` | The code. A real, working block, ready to place on a page. |
| `thumbnail.png` | A picture of what that block actually looks like, rendered from the block itself. |

Open `PREVIEW.html` in a browser to see all seventeen blocks on one page, live and clickable.

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

## The method skill

| Skill | What it does |
| --- | --- |
| `image-to-page-section` | Rebuilds one section from a reference picture of it: a screenshot, a design export, a photograph of a printed page or a sketch. It reads the structure and the proportions out of the picture, and rebuilds them in the customer's own colors and words. Its `element.html` is a worked example, and the comment at the top records what was read out of the reference and what was deliberately left behind. |

## How the blocks are built

Every `element.html` is a plain HTML block. It contains one `<section>`, Tailwind utility classes, and a small scoped stylesheet for the parts Tailwind does not cover.

- **No JavaScript.** Interaction that normally needs a script is done in CSS instead: the accordion uses `details` and `summary`, the tabs and the gallery filters are radio groups, the card rail is a real scroll container with snapping, the logo stream is a CSS animation, and the video players load on `:target`.
- **No external files.** No image addresses, no icon library, no fonts. Placeholder artwork is inline SVG, so a block renders correctly on its own.
- **No page wrapper.** There is no doctype, head or body tag. The block is meant to be dropped into a page that already exists.
- **No hardcoded colors.** Blocks use the page's theme tokens (`surface`, `muted`, `textdark`, `textmute`, `accent`, `bgdark`), so the same section works on a light page and a dark one. Every skill names those tokens and lists the pairs that fail a contrast check.
- **Reduced motion is handled in every block.** Anything that moves is removed when the visitor has asked for less motion.
- **Class prefixes.** Every block prefixes its own classes, so nothing collides with the rest of the page. Four blocks also use ids, because their interaction depends on them: the photo gallery filters, the tabbed content, and the two video players. Placing one of those twice on the same page means changing its prefix.

Each skill also carries a section naming the options that cannot exist without a script, and what to build instead, so nothing is delivered as a control that looks alive and does nothing when it is pressed.

## What was checked

Each block was rendered in a real browser and driven, not just looked at.

- no script tags, no page wrapper, no external asset requests, no leftover markdown
- one `<section>` and nothing beside it, and no content hanging below the section
- no page errors, and nothing overflowing the viewport horizontally at 1280px or at 390px
- a reduced motion rule in every block, checked again with reduced motion actually asked for
- every piece of visible text measured for contrast against the color behind it, 4.5 to 1, or 3 to 1 for large text
- every image carrying alt text and its dimensions
- every control clicked, and the page required to change: the gallery filters narrow the grid and restore it, a tab press swaps the panel with exactly one visible, and the accordion opens while keeping only one item open
- keyboard operation confirmed with real key presses: the tabs and the filters are one tab stop with arrow keys moving the selection, and the accordion opens on Enter

All 129 checks pass across the seventeen blocks, and all seventeen pass the deeper sweep with no failures and no warnings. The thumbnails are rendered from the same blocks, so a thumbnail can never drift away from the code it represents.
