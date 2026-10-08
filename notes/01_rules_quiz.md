# Section 1 quiz: Rules

Ten questions on the section 1 notes (`01_rules.md`). Answers are in my own words, with corrections where needed.

## 1. Why not design to "FS Rules 2024" if racing in 2026?
**My answer:** new book every year.
**Result:** Correct. Each year has its own version; a year's rules can also be updated mid-year (v1.0, v1.1), and each event has its own handbook.

## 2. Which is automatic while the car runs, and which is removed by hand: the AIR or the maintenance plug?
**My answer:** the AIR is automatic, the plug is by hand.
**Result:** Correct.

## 3. Segment limits: work out voltage and energy of a 14s5p segment (14 x 4.2 V; 70 x 18 Wh) and say if it passes.
**My answer:** 58.8 V and 4.54 MJ, passes.
**Result:** Correct. Cells alone are 4.97 kg, also under 12 kg.

## 4. How many temperature sensors does one segment need, and why?
**My answer:** 21, because 30% of 70 cells is 21.
**Result:** Correct (126 for the pack, against 8 per segment planned). Assumes one sensor counts for one cell; the rule text may allow one sensor to cover a parallel group.

## 5. Name one allowed sensor place, and what you must do if you use glue.
**My answer:** direct contact with the cell, and derate the max temperature.
**Result:** Correct. The other place is on the busbar, less than 10 mm along the current path from the terminal.
**Learned:** *derate* means set your trip point lower to leave a safety margin. If the sensor reads 8 degrees cooler than the hottest point of the cell, a 60 degree cap means tripping at about 52 degrees on the sensor.

## 6. One cell in a parallel group shorts. What protects the other four, and where does a sense-wire fuse sit?
**My answer:** each cell has its own fuse strip; fuse at the busbar end.
**Result:** Correct. Note: the per-cell rule is confirmed only in the older FSAE wording; check FS 2026.

## 7. Clearance vs creepage, and which voltage for plate-to-container distance?
**My answer:** clearance is air, creepage is along the surface, 352.8 V.
**Result:** Correct. The container is at chassis potential and the stacked segments put the last plates that far above it.

## 8. What is positive locking, and why might a Belleville washer alone not count?
**My answer:** a lock that physically stops it turning; Belleville only uses friction.
**Result:** Correct. A Belleville washer is still useful for keeping clamp force, so you may use it alongside a positive lock.

## 9. Why can't a contactor count as a maintenance plug, and how many plugs does the 6-segment pack need?
**My answer:** (to be answered)

## 10. Name two things the ESF must justify about the temperature sensors.
**My answer:** (not yet asked)
