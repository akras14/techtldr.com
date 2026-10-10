---
title: "How British workshops learned to measure a millionth of an inch"
slug: "how-machines-learned-precision"
date: 2026-10-07T23:35:00-07:00
summary: "Between the 1770s and the 1850s, British workshops went from struggling to bore a round cylinder to detecting a millionth of an inch. They got there by bootstrapping: building accurate references (flat plates, true screws, standard bars) out of inaccurate parts, then using machines to copy that accuracy into more machines."
source: "https://glinscott.github.io/how-machines-learned-precision/"
source_title: "How Machines Learned Precision"
source_author: "Gary Linscott"
source_site: "glinscott.github.io"
source_date: "2026-10-01"
---

Between the 1770s and the 1850s, British workshops went from struggling to bore a round cylinder to detecting a millionth of an inch. They got there by bootstrapping: building accurate references (flat plates, true screws, standard bars) out of inaccurate parts, then using machines to copy that accuracy into more machines.

The original is an interactive essay with 3D models you can rotate and simulations you can run. It's worth visiting for those alone.

## The problem

A lathe can only be as accurate as its own parts. A bent rail produces a bent cut, and an unevenly spaced lead screw produces an uneven thread. In the 1770s, the finest mark on a workshop rule was a sixteenth of an inch, about 1.6 mm, so workers couldn't even *measure* their errors well.

## 1. The cylinder (Wilkinson, ~1775)

Watt's steam engine kept its cylinder hot and sealed the piston with dry packing, so the bore had to be truly round along its whole length. His 1769 test cylinder was off by about ⅜ inch at its worst point.

Boring mills of the time held the cutter on a bar supported at one end only, so the bar sagged and followed the crooked casting. **John Wilkinson ran a heavy bar right through the cylinder, supported at both ends.** The cutter now followed the bar instead of the casting. By 1776, a 50-inch cylinder came out accurate to about the thickness of a worn shilling (under 1 mm), roughly 30× better relative to size. For two decades, nearly every Boulton & Watt engine used his cylinders.

## 2. The slide rest (Maudslay, 1790s)

Turners used to hold the cutting tool by hand. Heavy cuts in iron made the tool bounce and leave a rippled surface, called **chatter**. A slide rest clamps the tool to an iron carriage that runs along rails, so the cutting forces go into iron instead of arms. Slide rests already existed; Henry Maudslay made them rigid enough for iron and combined them with a lead screw and change gears in one practical lathe. Now the tool followed the rails exactly, so the rails had to be straight.

## 3. Flat, from nothing (the three-plate method)

Fitters used a scraper and a thin film of colour: rub the work against a flat reference, and colour marks the high spots to scrape away. But where does the first flat reference come from? Rubbing two plates together only proves they fit *each other*, since a dome nests perfectly in a hollow.

**The fix uses three plates.** Scrape all three pairs (A–B, B–C, C–A) to fit each other. Two domes or two hollows can't fit, so the curves get shallower with each round, until all three are flat. The author calls it one of the most beautiful ideas in engineering, and toolmakers still use it. Planing machines (from about 1817) later did the rough work: Whitworth estimated that truing a square foot of cast iron fell from 12 shillings of hand labor to under a penny.

## 4. The screw, without copying a screw

Threads used to be copied from older threads, inheriting their errors, so every nut had to be paired with its own bolt. Maudslay **generated** a thread from geometry instead: a knife set at a precise angle against a soft rotating cylinder. The pitch depended only on the cylinder's circumference and the knife angle.

To improve it, he guided a new cut with **two imperfect screws** and placed the tool at the midpoint of a bar joining their nuts, so their uneven errors partly cancelled. He checked the overall pitch by measuring travel over many turns, which divides the measurement error by the number of turns. The result was a five-foot master screw with 50 threads per inch.

**Change gears** then let one accurate lead screw cut many different pitches. The tooth counts on the two end wheels set the ratio, and an idler gear in between fixes the direction without changing it.

## 5. Measuring with a screw

Maudslay's bench micrometer, nicknamed "the Lord Chancellor" because it settled disputes, used a fine screw to resolve a thousandth of an inch, about 60× finer than a rule. The essay explains two error sources it had to manage:

- **Closing force:** pressing harder bends the frame and changes the reading.
- **Backlash:** slack between screw and nut means reversing direction shifts the reading. The fix is to always close onto the part from the same direction.

Getting the same reading every time isn't the same as being accurate. You also need a part whose length is already known.

## 6. The millionth of an inch (Whitworth, 1850s)

Joseph Whitworth wanted workshops to measure by **comparing parts against standard bars**, not by reading a rule. His measuring machine:

- used a **split, adjustable nut** to take up backlash,
- slowed the handwheel with a **worm gear** so a shaky hand barely mattered,
- multiplied out to 20 × 200 × 250 = **one millionth of an inch per division**,
- detected contact with a **"gravity piece"**: a small steel slip that falls until the jaws grip it.

At the 1851 Great Exhibition, he detected someone else's millionth-inch adjustments made without his knowledge. Even the warmth of a finger touching the bar was enough to change the result. His gauges brought this accuracy to ordinary workshops: a plug just one ten-thousandth of an inch under size went from a tight fit to visibly loose.

## Why it mattered

Accurate machine tools could make parts for more machine tools, copying and improving their accuracy with each generation. The millionth of an inch that Whitworth could barely detect is now an ordinary manufacturing tolerance for gauge blocks.
