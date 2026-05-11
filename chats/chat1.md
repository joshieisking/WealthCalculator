# Wealth Calculator UX

_Started 2026-05-11 12:01 UTC_

---

## User

This is a wealth calculator that I build with Claude to help me get a view on how I can get to my retirement goals. I think the design doesn't really work right now. It doesn't make it easy to see. The input parameters are at the bottom of the screen, and I have to scroll up and down to see things after changing them. If you had a blank sheet for design, what would be the optimal way to do it with the best ux in mind?

## Assistant

_[tool: local_ls]_

_[tool: local_read]_

_[tool: local_read]_

_[tool: local_read]_

_[tool: local_read]_

_[tool: local_read]_

_[tool: snip]_

_[tool: local_copy_to_project]_

_[tool: write_file]_

Before building, here's how I'm reading the problem and the design system I'm committing to — so you can sanity-check it.

**What's wrong with the current layout**
- Inputs at the bottom → you tweak, scroll up to read the effect, scroll back down. The single biggest UX failure.
- 6+ output modules all on screen at once (hero metrics, status, timeline, what-if, chart, planning cards, controls) — nothing is the focal point.
- The chart, which is actually the answer to almost every question you'd ask, is buried mid-page.
- Heavy terminal-style chrome (scan lines, F-keys, panel codes, blinking cursors) costs attention without giving information back. Also riffs on a branded look I shouldn't be recreating anyway.

**Optimal layout (what I'll build)**

1. **Pin inputs in a left rail** — sliders preferred over typed numbers for "what if I retire at 52?" exploration. Always visible. You never scroll to see what changed.
2. **One hero answer up top** — four KPI tiles: wealth at retirement, years to target, target date, buffer vs goal. Big tabular numbers.
3. **The chart is the centerpiece** — full width, milestones (kids' university, FIRE, target hit) drawn directly onto the trajectory as vertical annotations, with an optional what-if overlay.
4. **Lower "lens" panel** — tabbed: Timeline · Coast FIRE · 4% income · Education · What-if. One at a time, not five parallel cards.

**Aesthetic** — calm light fintech, not terminal cosplay. Warm off-white bg, ink text, single muted teal accent, JetBrains Mono for figures, Instrument Sans for UI. No emojis, no scan lines, no F-keys.

Let me build it.

_[tool: done]_

_[tool: fork_verifier_agent]_

Here's the redesign — `Wealth Planner.html`. Key moves vs. the original:

- **Inputs pinned in a left rail.** Sliders for age and rates so you can scrub and watch the chart change without ever scrolling.
- **Four KPI tiles up top** — wealth at retirement, years to target, target date, buffer vs goal — answer the headline questions at a glance.
- **The chart is the centerpiece**, full width, with milestones (retirement, target hit, kids' uni) drawn directly onto the timeline.
- **Lens tabs below** consolidate the five secondary views (Timeline · Coast FIRE · 4% Income · Education · What-if) into one panel instead of five competing cards. Each tab shows a one-line summary so you don't have to click to peek.
- Calm warm-light fintech aesthetic, restrained palette, tabular numerals throughout, no terminal chrome.

The calculation logic is preserved verbatim — only the UX changed. Let me know which lens or input you'd like to push further (e.g. drag-able retirement age directly on the chart, or a Monte Carlo band around the projection).

## User

Why is the monte carlo band needed? what value does it add? 

AT retirement is the assumption that there is no more contribution? I think having a note there would help. 

Right now the labesl on the lines are horizontal and they overlap. What would be the optimal way to fix this?

## Assistant

_[tool: snip]_

Good questions — let me answer the first two, then fix the third.

**1. Monte Carlo band — what it adds**
The current chart shows one smooth line at "7% per year forever." Real markets don't do that. A Monte Carlo band runs hundreds of randomized scenarios (e.g. 7% mean, 15% volatility) and shades the 10th–90th percentile outcomes. The honest version of your KPI becomes: *"likely between SGD 4.1M and 9.2M at age 50, median 6.0M"* instead of a single confident number. Value: it stops you from over-anchoring on one figure and exposes sequence-of-returns risk (a bad early decade can wreck you even at the same average). Worth adding if you're making real decisions on this; skip if you want a clean planning view. I didn't add it unilaterally because it doubles the chart's visual weight — your call.

**2. "At retirement = no contributions" note**
You're right, that assumption is hidden. I'll add a small caption under the chart and a footnote on the retirement KPI.

**3. Overlapping milestone labels — the right fix**
The current labels are all horizontal text floating at the top of each vertical line. They collide because four consecutive education years pile up. The optimal fix is two changes, not one:

- **Don't draw 8 vertical lines for 8 university years.** Each child's 4-year span is one event, not four. I'll collapse them into a single **horizontal range bracket** at the bottom of the chart (`├──── C1 uni ────┤`).
- **Rotate the remaining one-point milestones (Retirement, Target) to read vertically along the line**, so a long label only takes ~10px of horizontal space and can sit close to its neighbor without collision.

That removes the clutter at the source rather than fighting it with collision-detection.

_[tool: str_replace_edit]_

_[tool: snip]_

_[tool: str_replace_edit]_

_[tool: snip]_

_[tool: str_replace_edit]_

_[tool: str_replace_edit]_

_[tool: snip]_

_[tool: done]_

_[tool: fork_verifier_agent]_

_[tool: snip]_

Updated. Recap of the three fixes:

1. **Monte Carlo** — explained above; left out by default. Say the word if you want me to layer a 10th–90th percentile band onto the trajectory.
2. **Retirement assumption** — added to two spots: a chart caption directly under the trajectory ("After retirement, contributions stop — the portfolio grows passively"), and as a one-liner on the "Wealth at retirement" KPI tile.
3. **Label overlap** — solved structurally, not by collision-detection:
   - Single-point milestones (Retire, Target hit) → labels rotated 90° and hug their dashed line; tiny horizontal footprint, can't collide.
   - Education years collapsed from 4+4 vertical lines into two **horizontal range brackets** with a soft tinted band, labeled once each ("Child 1 · university", "Child 2 · university"). Eight labels → two.

## User

Apply comment

## Assistant

_[tool: snip]_

_[tool: str_replace_edit]_

_[tool: snip]_

_[tool: done]_

_[tool: snip]_

All milestone annotations now live in a dedicated lane **above** the plot — they never touch the wealth lines. Behavior:

- Each label is placed horizontally and sorted left-to-right; if a label would overlap its neighbor, it drops to the next row automatically. So the lane grows vertically as needed instead of letting text collide.
- Dashed connectors only start *below* the entire label band and run down to the x-axis — never crossing through any label.
- Education spans use the same row system: a horizontal bar with two arms drops from the label band to the plot, with a faint band across the plot to anchor it.

Result: with a tight cluster like Retire + Target hit in the same year, you'll see them stacked on two rows instead of overlapping. Resize the window and the layout rebalances on every redraw.

## User

Add tweakable controls to Wealth Planner.html: Make all the value inputs in the left sidebar with many zzeros have commas. Right now it's a bit hard to read.

## Assistant

_[tool: snip]_

_[tool: str_replace_edit]_

_[tool: snip]_

_[tool: str_replace_edit]_

_[tool: str_replace_edit]_

_[tool: snip]_

_[tool: done]_

_[tool: snip]_

Done. Two changes:

**Comma formatting on money inputs**
All four SGD fields (Current portfolio, Monthly contribution, Target wealth, Per-child uni) now show grouped digits — `5,000,000` instead of `5000000`. They're text inputs under the hood with a formatter that re-applies commas on every keystroke while preserving caret position, so editing in the middle of a number still works as expected.

**Tweaks panel** (toggle it from the toolbar)
- **Accent color** — Teal / Indigo / Terracotta / Olive. Drives the trajectory line, target line tints, and toggle accents.
- **Show milestone bands** — the faint vertical tint behind each child's university span.
- **Show target line** — the dashed terracotta target reference.
- **Compact KPIs** — shrinks the top tiles if you want more room for the chart.
- **Chart height** — slider from 300 to 560 px.

Values persist via the standard tweaks protocol, so reloading keeps your choices.

## User

Move the assumptions sidebar from the left to the right of the page

## Assistant

_[tool: snip]_

_[tool: str_replace_edit]_

_[tool: snip]_

_[tool: done]_

_[tool: snip]_

Sidebar moved to the right — flipped the grid columns and the rail's border now sits on its left edge.

## User

Apply drawing

## Assistant

_[tool: snip]_

_[tool: str_replace_edit]_

_[tool: snip]_

_[tool: done]_

_[tool: snip]_

You're right — stacking labels on the chart just moved the clutter, didn't remove it. The chart is a continuous trajectory and milestones are discrete events; they speak different visual languages and shouldn't share the same surface.

**Fix:** the chart now shows only one annotation — the **Retire** marker — because that's the actual regime change in the math (contributions stop). Everything else (Target hit, Child 1 & 2 university) moved out of the chart entirely into the **Timeline** tab below, which already lists them with years, ages, and amounts in a properly laid-out table.

The chart top padding shrank from 72px back to 28px and the trajectory now has the breathing room you'd expect.

## User

I still want to be able to see the milestones visible on the wealth trajectory chart. The major milestones of when my kids go to university. And I think i'd like to add one more point where my big loan mortgage will end. That will be my big expense. Home mortgage and my car mortgage.

## Assistant

_[tool: str_replace_edit]_

_[tool: grep]_

_[tool: snip]_

_[tool: str_replace_edit]_

Done — "financial independence" → "Financial Independence" in the topbar title.

## User

I need the dots to maybe have an icon or a letter in them for me to know what they are. I am colorblind so seeing colors on a screen is very difficult.

## Assistant

_[tool: snip]_

_[tool: str_replace_edit]_

_[tool: grep]_

_[tool: str_replace_edit]_

_[tool: done]_

_[tool: snip]_

Each milestone now carries a **letter glyph** so you can identify it without relying on color:

- **R** — Retire
- **T** — Target hit
- **H** — Home mortgage ends
- **V** — Vehicle loan ends
- **1** / **2** — Child 1 / Child 2 university (shown as a chip centered under the shaded span)

The glyphs appear in three places, always with the same letter so they cross-reference visually:
- On the **chart** itself — round chip below the x-axis for point events, rounded rect chip on the year axis for the education spans
- In the **legend strip** under the chart
- The dashed/solid lines and band tints still exist as redundant cues for sighted users, but the letters mean you never need to read the color to know what's what

White text on filled chips gives high contrast at any color setting. Let me know if any of the glyphs aren't intuitive and I'll swap them (e.g. **M** for mortgage, **C** for car if you'd prefer those).

## User

I don't see the glyphs. They are not appearing.

## Assistant

_[tool: snip]_

_[tool: grep]_

_[tool: str_replace_edit]_

_[tool: grep]_

_[tool: grep]_

_[tool: snip]_

_[tool: str_replace_edit]_

_[tool: snip]_

_[tool: done]_

The glyphs render below the x-axis now — `R`, `T`, `H`, `V` as filled circles and `1`, `2` as rounded chips under each university span. Layout padding was bumped so they have room and no longer clip against the canvas edge.

## User

Move the glyphs to the top of the chart instead of under. It looks very messy with the dates.

## Assistant

_[tool: snip]_

_[tool: str_replace_edit]_

_[tool: snip]_

_[tool: done]_

Glyphs and span bars now sit above the plot, clear of the year labels.

## User

Did you make the changes directly in my folder? Or what do I have to do now to apply the changes we created?

## Assistant

_[tool: snip]_

_[tool: present_fs_item_for_download]_

No — your local folder is untouched. The changes live in **this project's copy** of the file. Use the download card above to save `Wealth Planner.html` and drop it into your `Wealth Calculator` folder (overwriting the old version) to apply.

## User

Save as standalone HTML: Wealth Planner.html

## Assistant

_[tool: snip]_

_[tool: grep]_

_[tool: str_replace_edit]_

_[tool: super_inline_html]_

_[tool: snip]_

_[tool: show_html]_

_[tool: show_html]_

_[tool: get_webview_logs]_

_[tool: snip]_

_[tool: present_fs_item_for_download]_

Single self-contained file — works offline with no network access.

## User

When I hover on the glyph, can we highlight the legend at the bottom i.e. when I hover over the V glyph, the car loan ends is highlighted

## Assistant

_[tool: snip]_

_[tool: grep]_

_[tool: read_file]_

_[tool: str_replace_edit]_

_[tool: read_file]_

_[tool: snip]_

_[tool: str_replace_edit]_

_[tool: str_replace_edit]_

_[tool: grep]_

_[tool: snip]_

_[tool: read_file]_

_[tool: str_replace_edit]_

_[tool: snip]_

_[tool: done]_

_[tool: snip]_

Hover on any glyph chip on the chart and the matching legend item below highlights — works on both the round point chips (R/T/H/V) and the rectangular span chips (1/2). Cursor switches to a pointer to signal it's interactive.

## User

In the legend below the chart, you have added the glyphs, but they are not round, and they are in black text which makes it hard to read. Make the glyphs next to the label round, with white text.

## Assistant

_[tool: str_replace_edit]_

_[tool: grep]_

_[tool: str_replace_edit]_

_[tool: snip]_

_[tool: done]_

_[tool: snip]_

All six legend chips are now round (20×20), filled with their colour, white bold letter inside — matches the chips on the chart.

## User

The line above the legend stretches all the way across the chart, but the line below the legend doesn't. It truncates halfway and the text doesn't wrap properly as well.

## Assistant

_[tool: str_replace_edit]_

_[tool: snip]_

Two fixes:

- **Caption ran short** — `max-width: 78ch` was clipping the line under the legend. Removed it, so the foot text now spans the full chart width like the legend divider above.
- **Items breaking mid-label** — added `white-space: nowrap` to each legend item, so a glyph + label + year stays together and only wraps as a unit at the gap between items.

## User

Save as standalone HTML: Wealth Planner.html

## Assistant

_[tool: super_inline_html]_

_[tool: snip]_

_[tool: present_fs_item_for_download]_

Single self-contained file with the latest legend fixes — ready to drop into your folder.

