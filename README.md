# Website Builder Skills

Forty-eight skills. The seventeen in batch 1 and the twenty in batch 2 are here; batch 3 is built and follows when it is due.

| Folder | What is in it |
| --- | --- |
| `batch-1` | The sixteen full website sections, plus one method skill for rebuilding a section from a picture of it. |
| `batch-2` | Elements 01 to 20: the pieces placed inside a section. |
| `batch-3` | Elements 21 to 24, and the seven recommended additional sections. Built and held back, not in this repository yet. |

Each skill is delivered as three things:

| File | What it is |
| --- | --- |
| `SKILL.md` | The instructions. What it is for, when to use it, when not to, where it belongs on a page, what may be changed and what must not. Text only. |
| `element.html` | The code. A real, working block, ready to place on a page. |
| `thumbnail.png` | A picture of what that block actually looks like, rendered from the block itself. |

Each batch folder holds its own `PREVIEW.html`. Open one in a browser to see every block in that batch live and clickable on a single page. Each preview is self contained, so it can be opened on its own without the rest of the repository.

One further skill sits outside the batches:

| Skill | What it does |
| --- | --- |
| `quality-and-placement-check` | Checks a generated block before it is delivered. Sixteen checks on the block itself, then eight on the page it is going on, including where it sits in the running order. It is text only, because it teaches a method and produces nothing to look at. |

## Batch 1, the sections

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
| `image-to-page-section` | Rebuilds one section from a reference picture: a screenshot, a design export, a photograph of a printed page or a sketch. It reads the structure and the proportions out of the picture and rebuilds them in the customer's own colors and words. Its `element.html` is a worked example, and the comment at the top records what was read out of the reference and what was deliberately left behind. |

## Batch 2, elements 01 to 20

| Element | What it does |
| --- | --- |
| `element-headline-or-title` | A heading with its eyebrow and supporting line |
| `element-text-paragraph` | Body copy at a readable measure |
| `element-bullet-or-numbered-list` | A real list, with ticks or numbers |
| `element-clickable-button` | Primary, secondary and text buttons |
| `element-section-divider` | A pause between two parts of a section |
| `element-single-icon` | One inline SVG mark, decorative or linked |
| `element-single-image` | One picture in a fixed frame |
| `element-image-with-text` | A picture and the few lines that explain it |
| `element-icon-with-text` | A small promise with a mark beside it |
| `element-standard-video` | A player that loads when it is asked for |
| `element-sales-video` | A player with the offer and the action beside it |
| `element-audio-player` | The browser's own audio control, with a transcript |
| `element-photo-gallery` | A small grid of pictures inside a section |
| `element-sliding-images` | A real scroll rail that snaps |
| `element-contact-or-lead-form` | The shortest set of questions that gets a reply |
| `element-pop-up-window` | A panel opened by the visitor, never on a timer |
| `element-countdown-timer` | A stated deadline, written out |
| `element-progress-bar` | A known figure, drawn and written |
| `element-social-media-links` | Named links to the places that are actually posted to |
| `element-business-location-map` | The address in text, a drawn local map, directions out |

## Batch 3, elements 21 to 24 and the seven additions

| Skill | What it does |
| --- | --- |
| `element-third-party-widget` | A prepared slot for an outside tool, with a real fallback |
| `element-product-or-service` | One thing for sale, in a card that lines up with its siblings |
| `element-customer-review` | One attributed review, quoted properly |
| `element-price-and-package` | A tier, with the term beside the figure |
| `section-before-and-after` | The same subject twice, at the same size and angle |
| `section-sticky-scroll-story` | One pinned visual, chapters scrolling past it |
| `section-stats-and-results` | Measured figures with a source note |
| `section-testimonials` | Several real customers together |
| `section-offer-and-pricing` | The whole offer, with a monthly and annual choice |
| `section-countdown-offer` | A real closing date and the reason for it |
| `section-interactive-comparison` | A real table above 768px, cards below |

## How the blocks are built

Every `element.html` is a plain HTML block. It contains one `<section>`, Tailwind utility classes, and a scoped stylesheet.

- **No JavaScript.** Interaction that normally needs a script is done in CSS instead: the accordion uses `details` and `summary`, the tabs, gallery filters and the pricing term switch are radio groups, the card rails are real scroll containers with snapping, the logo stream is a CSS animation, and the video players and the panel load on `:target`.
- **No external files.** No image addresses, no icon library, no fonts. Placeholder artwork is inline SVG. Links to other sites are links, not files, and are used where a link is the point.
- **No page wrapper.** There is no doctype, head or body tag. The block is meant to be dropped into a page that already exists.
- **Colour lives in the block.** Six variables are declared on the block itself, as channel triplets so an opacity suffix works. That replaced an earlier approach that named theme colours from the page's Tailwind config, which was measured on two live builder sites and found not to work: every site writes its own colour names, so `bg-surface` painted nothing on either and `muted` was a pale background on one and a dark text colour on the other.
- **Each block carries its own CSS.** Inside `@layer`, so a page that loads Tailwind still overrides every line of it, and a plain HTML viewer with no stylesheet at all still renders the block properly. Measured: every block matches on 29 computed properties and on geometry, with Tailwind and with nothing, at 1280px and at 390px.
- **Reduced motion is handled in every block.** Anything that moves is removed when the visitor has asked for less motion.
- **Class prefixes.** Every block prefixes its own classes, so nothing collides with the rest of the page. Some also use ids, because their interaction depends on them. Placing one of those twice on the same page means changing its prefix.

Each skill also carries a section naming the options that cannot exist without a script, and what to build instead, so nothing is delivered as a control that looks alive and does nothing when it is pressed.

## What was checked

Each block was rendered in a real browser and driven, not just looked at.

- no script tags, no page wrapper, no external asset requests, no leftover markdown
- one `<section>` and nothing beside it, and no content hanging below the section
- no page errors, and nothing overflowing the viewport horizontally at 1280px or at 390px
- a reduced motion rule in every block, checked again with reduced motion actually asked for
- every piece of visible text measured for contrast against the colour behind it, 4.5 to 1, or 3 to 1 for large text
- every image carrying alt text and its dimensions
- every control clicked, and the page required to change
- keyboard operation confirmed with real key presses
- every block rendered twice, with Tailwind and with no stylesheet at all, and the two compared property by property

Batch 1 and batch 2 both pass with no failures and no warnings. The thumbnails are rendered from the same blocks, so a thumbnail can never drift away from the code it represents.
