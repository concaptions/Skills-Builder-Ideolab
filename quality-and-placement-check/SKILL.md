---
name: quality-and-placement-check
description: Check a generated section or element before it is delivered. Runs the block against the delivery rules, renders it and drives its controls, then checks it against the rest of the page it is going on, including where it sits in the running order. Use it after any block is generated or edited, and before a page is handed over.
---

# Quality and Placement Check

## What you must return

A report, not a rebuilt block. Do not silently repair what you are checking. Fix only when you are asked to fix.

The report has three parts and nothing else:

1. **A verdict line.** `PASS`, or `FAIL, 2 of 20 checks`.
2. **One line per check.** The check name, `PASS` or `FAIL` or `WARN`, and the measured value that decided it. A line with no number is not a result, it is an opinion. Write `Content stays inside the section: FAIL, 304px of content below the section box`, not `content overflows`.
3. **The fixes.** One line each, naming the selector or the attribute to change. Nothing else.

Every figure in the report must be one you measured on this block. Do not carry a number over from an earlier report, and do not repeat a figure from this skill as though you had measured it.

This skill is self-contained. Do not read, reference or depend on any other skill, and do not assume a shared checklist, a test harness or a report template exists somewhere. If something is not described here, it is not available.

## What it is for

Deciding whether a block is fit to hand over. The block checks answer "is this block correct on its own". The placement checks answer "is it correct on the page it is going on". A block can pass every rule and still be wrong in both ways: a tab strip whose tabs do nothing passes every rule about scripts and colours while being visibly broken, and a perfectly built testimonials section is still wrong placed above the section that explains what the product is.

## When to use it

- After a block has been generated from a skill, before it is shown to anyone.
- After a block has been edited, however small the edit. A colour change moves the contrast.
- Before a page is handed over, on the page as a whole, for the placement checks.
- When a block behaves differently in two places and nobody knows which one is wrong.

## When not to use it

- To improve a block. This reports, it does not design.
- To judge the writing. Whether the headline is any good is not a check with a number behind it.
- On a page you did not build and are not delivering.

## How to run it

Two passes. Both are needed, and the second is the one that finds real defects.

**Pass one, read the source.** Checks 1 to 7 and 14 below are answered by reading the block. Do not render anything yet.

**Pass two, render it and drive it.** Put the block on a page on its own, with the stylesheet the live page loads. Then:

- give the page about two seconds after load before reading any size, because a stylesheet loaded from a CDN is not applied the instant the page settles, and a check that reads sooner reads zeros
- pause animation and transition before measuring geometry, or the same block measures differently on two runs purely because one run was slower to start
- click every control and require the page to change
- read the page again with reduced motion asked for
- read it again at 390px wide
- read it again with no stylesheet at all

The last one matters more than it sounds. See check 15.

## The block checks

**1. No JavaScript.** No `<script>`, no `onclick` or any other `on*` attribute, no `javascript:` URL. Report the count.

**2. No page wrapper.** No doctype, no `<html>`, `<head>`, `<body>` or `<meta>`. The block is dropped into a page that already exists, and a second `<body>` in the middle of a document is discarded along with whatever the generator meant by it.

**3. No external files.** No `src`, `poster`, `<link href>`, `url()` or `@import` pointing at another host. A stock photo address is the usual offender. A plain `<a href="https://...">` is a link, not a file, and is not a failure. Report the addresses found.

**4. No leftover markdown.** No triple backtick lines anywhere in the block. They come from a chat window's own formatting rather than from the generated HTML, and inside a grid they become extra grid items that push the layout out of line.

**5. No invented colour variables.** Every `var(--x)` used must be defined in the block. A bare `var(--colour-accent)` that nothing declares is not a fallback to anything, the whole declaration is dropped, and the text quietly renders in the inherited colour. `var(--x, #2563eb)` carries its own fallback and is fine. Report each undefined name.

**6. Reduced motion handled.** If anything animates, transforms or transitions, there must be a `prefers-reduced-motion: reduce` rule that stops it, or a `motion-reduce:` class on the moving element. Then ask the browser for reduced motion and confirm nothing moves. A block with no motion at all passes this without needing the rule.

**7. Colour comes from the block's own values.** The base colours must come from the block's own declared variables, not from hard coded hex. A small number of decorative hex values inside an inline SVG is acceptable and should be reported as a warning with the values listed. Twenty of them is a block that will not follow the site's colours at all.

**8. Every image has alt text.** Every `<img>` carries `alt`. A decorative image carries `alt=""` and `aria-hidden="true"`, which is a pass, not a miss.

**9. Images carry width and height.** Without them the page jumps as each image arrives. Warn, do not fail.

**10. Icons are sized by something other than a class.** Every inline `<svg>` needs a `width` and a `height` attribute as well as its size classes. An SVG that carries only a `viewBox` has no size of its own. Measured with no stylesheet loaded, a 16px chevron rendered at 900 by 900 and filled the section it sat in. Report every `<svg>` with no width attribute.

**11. One section and nothing beside it.** Exactly one top level `<section>`, with nothing outside it except its own `<style>` and comments. Two sections, or a stray `<div>` at the end, means the block cannot be placed as one thing.

**12. Content stays inside the section.** Measure the section's own box, then the bottom of every visible descendant. Anything more than about two pixels below the section box is content hanging over whatever comes next on the page. Absolutely positioned panels are the usual cause. Report the overhang in pixels.

**13. The controls actually do something.** Click every `summary`, every `[role=tab]` and every `label` that points at a radio or a checkbox, and require the page to change. Do not count a `label` that points at a text field, that is a form label and clicking it correctly changes nothing.

The change signature has to be strong enough to notice the change. Counting visible elements is not enough, because a tab swap hides one panel and shows another and the count is identical. Position is not enough either, because a monthly price swapped for an annual one puts a different element at exactly the same place with the same height. Read tag, position, class and the first few words of text together.

Some no-change results are correct. Clicking the tab that is already selected changes nothing, and so does clicking the radio that is already checked. Report the ratio, and fail only when nothing at all responded.

**14. Nothing scrolls sideways.** At 1280px and again at 390px, the page's scroll width must equal its client width. Check it again after the controls have been clicked, because a panel that only appears when a tab is selected is not measured before that.

**15. It still holds together with no stylesheet.** Render the block twice, once with the page's stylesheet and once with none at all, and compare the box of every element. This is the check that catches what nothing else does: an icon with no intrinsic size, a track whose width came only from a utility class, a layout that collapses to a single column.

Two traps in running it. Wait the same length of time in both runs, or an animation that finished in one and not the other reports as a real difference: that produced two false failures the first time this was run, on a progress fill and a divider rule, both of which turned out to be correct. And pause animation before measuring.

**16. Quiet text is still readable.** Every piece of visible text, against the colour actually behind it, composited through any transparency. 4.5 to 1 for body text, 3 to 1 for large text. Report the worst figure in the block by name.

Watch for two things the walker gets wrong. Text painted with a gradient has no text colour to read, because a gradient is a background image, so it has to be handled on its own. And a label sitting on a tinted track is not on the section background: a value that measures 5.42 to 1 on white measured 4.50 to 1 on a track tinted six percent darker, which is a fail.

## The placement checks

These are run on the whole page, not on the block alone. A block that passes every check above can still be wrong here.

**17. It is in the right part of the page.** Each block belongs in a part of the running order. What the business is and what it sells comes before proof, proof comes before price, price comes before the closing ask. A testimonials block above the section that explains the product is asking the reader to trust a recommendation for something they have not been told about yet. Report the block's position and the position it belongs in, both as the section number.

**18. Nothing is said twice.** Two blocks doing the same job on one page, a benefits grid and a features grid carrying the same four promises, two pricing blocks, three closing asks. Report each pair by name and by what they repeat.

**19. The headings run in order.** One `h1` on the page. No level skipped, so no `h3` whose nearest heading above it is an `h1`. Every section carries a heading, and each section's `aria-labelledby` points at a heading that exists. Report each break with the heading text.

**20. No id is used twice.** Collect every `id` on the page. A repeated id breaks whichever `:target`, `label for` or `aria-labelledby` reaches it second, and the reader sees a control that does nothing. Placing the same block twice on one page is the usual cause, and the fix is to change the second copy's prefix everywhere, ids and class names both.

Check radio `name` attributes at the same time. Two copies of a tab strip sharing one group name means selecting a tab in the second one deselects the tab in the first.

**21. Two neighbouring blocks do not look like one.** Two sections in a row on the same background with the same width and the same padding read as one long section, and the reader loses the join. Report the pair and the two background values.

**22. One idea, one ask.** Count the calls to action on the page and where they go. Several buttons is fine. Several buttons going to different places, so that the page is asking for three unrelated things, is not. Report each button's text and its destination.

**23. Nothing collides between blocks.** Every block prefixes its own class names, so two blocks on one page must not share a prefix. Then render the whole page and check that clicking a control inside one block changes nothing inside any other block. A `:target` rule written without its own prefix will open a panel in a different section.

**24. The page as a whole does not scroll sideways.** A block can be inside its own width and still push the page out once it is beside others, most often through a scroll rail or a wide table. Measure the assembled page at 1280px and at 390px.

## What a rules check cannot tell you

A rules check says the block is allowed. It does not say the block works.

That distinction was paid for. A tab strip built from an outside model followed a set of written rules exactly, including reproducing a recipe for hiding panels, and passed every rule based check while its three tabs were completely dead: clicking any of them left the first panel on screen. Nothing in the source looked wrong. Only clicking the tabs found it.

So checks 11 to 16 and 23 are not optional extras to run when there is time. They are the ones that find things.

## What a finished report looks like

    FAIL, 2 of 24 checks

    No JavaScript                          PASS   0 script tags
    No page wrapper                        PASS
    No external files                      PASS
    No leftover markdown                   PASS
    No invented colour variables           PASS   6 used, 6 declared
    Reduced motion handled                 PASS   3 animations, all stopped
    Colour comes from the block             WARN   4 hex values, all inside one SVG
    Every image has alt text               PASS   5 of 5
    Images carry width and height          PASS
    Icons sized without a class            FAIL   3 svg with no width attribute
    One section and nothing beside it      PASS
    Content stays inside the section       PASS   0px below the section box
    The controls actually do something     PASS   3 of 4 responded, the fourth was already selected
    Nothing scrolls sideways               PASS   1280 and 390
    Holds together with no stylesheet      FAIL   svg.h-4 900x900, expected 16x16
    Quiet text is still readable           PASS   worst 5.06 to 1, "Every pound goes on parts"
    In the right part of the page          PASS   section 6 of 9, proof after product
    Nothing is said twice                  PASS
    Headings run in order                  PASS   one h1, no level skipped
    No id used twice                       PASS   31 ids, all unique
    Neighbouring blocks are distinct       PASS
    One idea, one ask                      PASS   4 buttons, all to /book
    Nothing collides between blocks        PASS   9 prefixes, all different
    Page does not scroll sideways          PASS

    Fixes
    Add width="16" height="16" to the three svg in .sof-opt, .sof-tick and .sof-chevron.
    Add .sof .sof-icon{width:1rem;height:1rem} to the base layer.

## What breaks this check

- **Reading instead of driving.** Every serious defect found so far was found by clicking something. Reading found spelling.
- **A change signature that is too weak.** Covered in check 13. A weak signature reports a working control as dead, which sends someone to fix code that was already correct.
- **A closed `details` measured as an overflow.** Its box is full height even though nothing inside it is painted. Measure only what is actually visible, and clip against any ancestor that hides its overflow. Without that, a working accordion reports 299px of spill.
- **Calling a link an external file.** `<a href="https://...">` is the point of a link. Only `src`, `poster`, `<link href>`, `url()` and `@import` load a file.
- **Counting a form label as a control.** A `label` pointing at a text input is not a toggle, and counting it makes a working contact form look dead.
- **Flagging `var(--x, fallback)`.** A declared fallback is legitimate. Only a bare `var(--x)` that nothing defines is a fault.
- **Measuring before the stylesheet has applied.** Under a local file, a CDN stylesheet needs about two seconds. Read sooner and every size comes back as zero or as the unstyled value, and the report is nonsense in both directions.
- **Unequal waits between two runs.** Covered in check 15.
- **Reporting a difference as a defect without deciding which side is right.** A block that renders differently with and without a stylesheet is a finding. Which of the two is correct still has to be stated.

## What this check cannot do

- It cannot tell you whether the copy is true. Prices, dates, names and claims have to come from the customer's brief and be checked against it by a person.
- It cannot judge a photograph.
- It cannot tell you whether the page as a whole is persuasive.
- It cannot decide whether a block should exist. It checks the blocks that are there.
