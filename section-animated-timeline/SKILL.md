---
name: section-animated-timeline
description: A history, roadmap, process, or milestone section that progresses visually. Use for dated milestones running vertically, horizontally, alternating sides, as a roadmap, a chapter timeline, or a sticky timeline. Not for an unordered set of steps with no dates or sequence, and not for a short three-step process, which is the how it works section.
---

# Animated Timeline

## What it is for

A sequence with dates or order that matters: company history, a project roadmap, a process with stages, or a case study told in order.

## When to use it

- The order of events is part of the meaning.
- There are four to eight milestones. Fewer reads as a list, more becomes a chore.
- Each milestone has a date, a label or a clear position in a sequence.

## When not to use it

- The items have no order. That is a benefits or features section.
- There are only three short steps. That is a how it works section.
- The dates are vague or invented. A timeline invites scrutiny of exactly those dates.

## Where it belongs on the page

Middle of the page for a history or roadmap. In a case study it can carry the whole body of the page.

## What may be changed

- Number of milestones, their dates and their copy.
- Orientation, alternating sides, single side, or horizontal.
- Marker shape and colour, and whether the spine is solid or dashed.
- Whether the reveal happens at all.

## What must not be changed

- Every milestone being fully readable with no scroll effect applied. The reveal is an enhancement layered on top, never the thing that makes the text appear.
- The reveal ending well before the middle of the viewport, otherwise the last milestone can never activate and stays invisible at the bottom of the page.
- The list being a real ordered list, so the sequence survives without styling.
- The reduced motion rules.

## Settings and Controls

| Setting | Options | Default |
| --- | --- | --- |
| Milestone count | 3 to 12 | 5 |
| Dates | Year, month and year, full date, quarter, or a label instead | year |
| Ordering | Oldest first, newest first | oldest first |
| Direction | Vertical, horizontal | vertical |
| Content fields | Date, label, description, image, icon | date, label, description |
| Images | One per milestone, one shared changing image, or none | none |
| Navigation method | Scrolling, clicking, dragging, automatic playback, or static | scrolling |
| Show or hide | Dates, labels, descriptions, images, icons, arrows, progress, CTA | dates, labels, descriptions, progress |
| Active milestone | Which one is highlighted on load | first |
| Starting point | Top, bottom, or a named milestone | top |
| Mobile stacking | All milestones stacked, or one at a time | all stacked |
| Content shown at once | All descriptions, or only the active one | all |

**Only the active description is a risk.** It halves the text on the page and hides content from anyone who does not interact. Use it when descriptions are long. Keep everything visible when they are one line.

**Automatic playback needs a pause control** and must stop when the section leaves the viewport, like every continuous motion.

**A future-dated roadmap is a promise.** Mark unshipped milestones clearly as planned, and put a date on the section so a stale roadmap is obvious rather than misleading.

## Creative Design Options

**Layout**

- Vertical. Milestones down a spine on the left. The default, and the one that survives long descriptions.
- Horizontal. Milestones along a line, for short labels and few items.
- Alternating sides. Milestones left and right of a central spine.
- Roadmap. A path with stages rather than dates, for what is coming.
- Chapter timeline. Grouped into named periods, each with its own heading.
- Sticky timeline. The spine and the active date pin while the milestone content scrolls past.

**Motion**

Line grows, dot activates, milestone glows, number counts up, or image changes.

**Styling**

Gradient path, illuminated nodes, hand-drawn line, brand-color chapters, or dimensional roadmap.

**Interaction**

Jump by year, click milestones, reveal details, or synchronize milestones with supporting media.

## Motion

- **Line grows** is the section's signature: the spine fills as the reader scrolls. It is scroll-linked, so this is the page's one major showcase effect.
- **Dot activates** when its milestone reaches the reading line. Change color, fill and scale of the dot only, never the milestone row, so nothing shifts.
- **Number counts up** applies to a metric inside a milestone, runs once, and has the final value in the markup.
- **Image changes** crossfades between stacked images so the box never empties.
- **Milestone glows** is `subtle` and applies to the active item only.
- **Entrance:** milestones fade and rise in order, staggered by 60ms.

## Responsive and Accessibility

| Width | Behavior |
| --- | --- |
| Desktop, 1024px and up | Full layout as chosen. Line growth and sticky spine active |
| Tablet, 768px to 1023px | Alternating sides becomes single-sided. Horizontal becomes vertical |
| Mobile, under 768px | Vertical, spine on the left, all milestones stacked. Sticky off. Automatic playback off |

- The timeline is an ordered list, `<ol>`, so the sequence is in the markup.
- Dates are in a `<time datetime="...">` element with a machine-readable value.
- The spine, the dots and any decorative path are `aria-hidden`.
- Clickable milestones and year jumps are real buttons, and the active one is marked as current.
- If only the active description is shown, the hidden ones are still reachable, and each is revealed by a real control rather than by hover alone.
- With reduced motion, the line renders full, dot activation and glow are removed, count-ups show the final value, image changes are instant, and automatic playback does not start.

## Defaults

Five milestones, year dates, oldest first, vertical, date and label and description shown, no images, scroll navigation, progress shown, active milestone first, all stacked on mobile, gradient path, staggered fade-and-rise entrance, line grows once.

## What breaks this section

1. **Driving the fill from a raw scroll percentage.** It stops between dots and looks like a rendering error. Drive it from the last passed milestone.
2. **Scaling the whole milestone row when it activates.** Everything below it shifts, and the page jitters all the way down.
3. **A roadmap with no "as of" date.** Six months later it reads as a list of things that never happened.

## Color

Use the page's theme tokens. Never hardcode a hex value or a Tailwind palette color, because this section has to work on a dark editorial page and a light clinical one without being rewritten.

The tokens are `surface`, `muted`, `textdark`, `textmute`, `accent` and `bgdark`. Use them as Tailwind classes: `bg-surface`, `bg-muted`, `bg-bgdark`, `text-textdark`, `text-textmute`, `text-accent`, `border-accent`.

Do not invent CSS variable names such as `var(--color-surface)` or `var(--color-border)`. Nothing on the page defines them, so the browser throws the declaration away and that color, border or shape renders as nothing at all. Do not use `text-current/60` either, because an opacity modifier has no effect on `currentColor` and the text comes out at full strength.
