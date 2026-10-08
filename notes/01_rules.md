# Learning notes: Rules (research-list item 1)

These are teaching notes, not design documents.
Rule content comes from search excerpts of the 2026 rulebook, not the PDF itself; re-check clause wording in the official rulebook.

---

## Part 1: What is a rulebook, and which one is yours?

### The idea
A Formula Student car has to pass an inspection before it is allowed to drive. The inspectors check it against a **rulebook**, a long PDF. For an electric car the rules about the battery are some of the strictest, because the battery stores a lot of energy at high voltage.

### There is more than one rulebook
The competition exists in two families:
- **Formula SAE (FSAE)**: the original American version.
- **Formula Student (FS)**: the version used in the UK, Germany and much of Europe.

They are similar but not identical. Rule numbers differ and some content differs. A rule called "EV 5.3.2" in one may be numbered completely differently in the other.

**FSUK** (UK) and **FSG** (Germany) both use the **FS Rules**. FSUK adds its own extra rules on top.

### Rules change every year
Each year gets its own version, such as "FS Rules 2026". Numbers and wording shift between years. Always read the version for the year you will race. A rulebook can also be updated during the year (v1.0, then v1.1).

### Each event can add its own rules
On top of the rulebook, each event has an **event handbook** with its own details. For example, the 2026 German handbook said the organisers would not bring a certain temperature-checking device. So you read both the rulebook and the handbook.

### What this means for you
Your first decision is which event you will go to. That picks the rulebook, and the year decides the version. The research assumed the FS Rules, because the brief names FSUK and FSG. If you race FSAE, every rule number in the research needs re-checking.

**Check:** Why can't you design to any rulebook you find online?
*Answer:* Rulebooks differ between families, years, versions and events, so only the one for your event and year counts.

---

## Part 2: The vocabulary

| Word | What it means | In your car |
|---|---|---|
| **TS** (tractive system) | All the high-voltage parts that move the car | Accumulator, inverter, motor, HV cables |
| **Accumulator** | The rulebook's word for the battery pack, including its container | Your 84s5p pack, 420 cells |
| **Cell** | One battery unit | One Molicel P50B |
| **Segment** | A block of cells that can be separated from the rest | 14s5p, 70 cells, 6 of them |
| **AMS** | Accumulator Monitoring System, the rulebook's name for the BMS | Your ENNOID |
| **AIR** | Accumulator Isolation Relay, the switches that disconnect the pack from the car | The contactors on the pack output |
| **Maintenance plug** | A connector a person pulls out by hand to split the pack into segments | Between your segments |
| **Scrutineers** | The inspectors who check your car against the rules | The people who pass or fail you |

### Two that people mix up
- **AIR vs maintenance plug.** The AIRs open automatically, driven by the safety circuit while the car is running. The maintenance plug is removed by a person, on the bench, so nothing stays live. A contactor cannot replace a plug.
- **Segment vs cell group.** Your "5p" group is five cells in parallel. A segment is 14 of those groups in series. The rules talk about segments.

### Notation
"14s5p" means 14 cells in **s**eries, each position made of 5 cells in **p**arallel. Series adds voltage, parallel adds current.

**Check:** Which part is switched by the safety circuit while the car runs?
*Answer:* The AIR.

---

## Part 3: Segment limits (120 V / 6 MJ / 12 kg)

### The idea
A whole pack is dangerous. A small block of it is much less so. The rules force you to build the pack from small blocks (segments) that can be pulled apart, so anyone handling one segment faces a bounded risk.

### The three limits
From the 2026 rule seen in excerpts (EV 5.3.2), each segment must be no more than:

| Limit | What it controls |
|---|---|
| **120 V DC** | Electric shock risk |
| **6 MJ** | How much energy could burn or arc if it all went wrong |
| **12 kg** | How heavy a block is to lift and handle |

### Checking your segment
- **Voltage:** 14 cells × 4.2 V (fully charged) = **58.8 V**. Under 120, with lots of room.
- **Energy:** 70 cells × 18 Wh = 1260 Wh. One watt-hour is 3600 J, so 1260 × 3600 = 4.54 million J = **4.54 MJ**. Under 6.
- **Mass:** 70 cells × 71 g = **4.97 kg** for the cells alone. Under 12, leaving about 7 kg for everything else.

### Why this matters for busbars
- Every gram in the segment counts against the 12 kg: plates, insulation, holders, any cooling plate.
- Energy is the limit you are closest to (4.54 of 6 MJ). It is why you cannot merge two segments to save a plug: two together are 9.07 MJ, which breaks the limit even though 117.6 V would still pass.

### What could not be confirmed
Two details need reading in the actual rule: how "energy" is calculated (probably nominal capacity × nominal voltage from the cell datasheet), and whether the 12 kg includes the container.

**Check:** With a 20 Wh cell, would a 14s5p segment still pass the energy limit?
*Answer:* 70 × 20 Wh = 1400 Wh = 5.04 MJ. Under 6 MJ, so it passes, but with less margin (0.96 MJ instead of 1.46 MJ).

---

## Part 4: Watching cell temperature

### The idea
A Li-ion cell that gets too hot can go into **thermal runaway**: it heats itself faster and faster and ends in fire. So the rules make the AMS (your BMS) measure temperatures and cut power if a cell gets too hot.

### What the rules demand (2026 excerpts, EV 5.8.3 and 5.8.4)
- **How many cells:** at least **30 %** of the cells, spread equally through the pack.
- **Limit:** the trip temperature is the cell datasheet's limit, but **never more than 60 °C**. If a cell is out of range for more than **1 second**, the AMS must trip and disconnect the battery.
- The AMS must also watch **every cell voltage** and the **pack current**. Voltage out of range for more than 500 ms also trips.

### Your numbers
- 30 % of 420 cells = **126 cells** in the pack.
- Per segment: 30 % of 70 = **21 sensors**.
- Your plan of 8 per segment is 11 %, so it would fail.
- The P50B itself allows up to 80 °C, but the rule caps you at **60 °C**. The rule, not the cell, sets your limit.

### Why this matters for busbars
Sensors sit on or near the busbar, so the busbar design must leave room for 21 of them per segment.

**Check:** How many temperature sensors does one 14s5p segment need, and why is 8 not enough?
*Answer:* 21 (30 % of 70 cells, if one sensor counts as one cell). 8 covers only 11 %.

### Extra: does the whole accumulator trip?
Yes. The 2026 excerpt says the AMS trips and "disconnects the battery": the shutdown circuit drops, the AIRs open, and the whole accumulator is cut from the car. The cells inside stay live (the AIRs only isolate the pack from the car). I believe the fault latches until reset (not confirmed for 2026). A false trip ends your run, so sensor quality and mounting matter as much as sensor count.

---

## Part 5: Where the sensor has to sit (the 2026 change)

### The idea
A cell is hottest in its **core** (the rolled-up layers inside the can), because that is where the heat is made. A sensor outside reads cooler than the core. The further the sensor is from the core, or the better the cell is cooled at the sensor's spot, the bigger that error.

The 2026 rules respond to this: the sensor placement is now regulated, and you have to prove it is good enough.

### What the rule says (EV 5.8.4 excerpts; wording changed in v1.0 and clarified in v1.1)
1. The sensor must measure a place **representative of the cell's maximum temperature**.
2. It must be either:
   - in **direct contact with the electrically exposed part of the cell**, or
   - **less than 10 mm along the high-current path** from the terminal, on the busbar that touches the terminal.
3. You must **justify the position** and **quantify the measurement error** in your documents (the ESF/ASES), taking thermal resistances and cooling effects into account.
4. If you use **thermal adhesive**, you must **derate** the maximum temperature you monitor.
5. A sensor may cover several cells only if the requirement is met for every one of them.

### Why this matters for you
- **Your busbar plates must have a sensor spot within 10 mm of the cell's weld**, on the current path. Sensor locations are part of the plate design, not an add-on.
- **"Don't glue thermistors to busbars"** fits the rule well: gluing triggers derating, so a mechanical mount (a tab, a screw lug or a spring clip) avoids it.
- **Error budget:** a sensor on the busbar sees the cell's end, not the core. If the core is 8 °C hotter, you must lower your trip point from 60 °C by about that much and say so in writing.

### Still to confirm in the actual text
- Whether one sensor can stand for a whole parallel group of 5 cells.
- What exactly counts as the "high-current path".
- Whether "terminal" means the positive or the negative one.

**Check:** Why does the glue rule fit your "no gluing" preference?
*Answer:* Using adhesive forces you to derate the monitored maximum temperature, so a mechanical mount avoids that penalty.

---

## Part 6: Fusing (parallel cells and sense wires)

### The idea
A **fuse** is a deliberately weak link. If too much current flows, it melts and breaks the circuit before wires or cells overheat. There are three places fusing matters in your accumulator.

### 1. The main fuse
One fuse in the pack's main current path. It protects against a big fault, such as a short across the whole pack (about 1.6 kA in your case). The rules require the accumulator to be fused in at least one place.

### 2. Fuses on parallel cells
In a 5p group, five cells share one pair of busbars. If one cell fails and shorts inside, the other four push current into it: roughly 300 A each, over 1 kA through the bad cell. A small fusible link on each cell cuts the bad cell out.

What I could confirm: the older **FSAE** wording says each parallel element needs its own overcurrent protection. There is an exception for fusible links that are not voltage-rated. For that exception, a main fuse rated at most **one third** of the links' combined rating must sit in series, and the AMS must detect an open link.

What I could not confirm: the **FS 2026** version of this rule. Read it before designing the busbar, because the links are cut into the busbar.

### 3. Fuses on sense wires
The thin wires from each busbar to the BMS can carry hundreds of amps if they short, and they can burn. A small fuse right at the busbar end of each wire stops that. I did not find a rule that demands it, but it is standard practice (open-source BMS boards fuse every sense line), so plan for it anyway.

### Why this matters for busbars
- If cell links are required, they are part of the plate geometry (a deliberately narrow neck per cell).
- A fuse must sit at the **start** of a wire, not the end, or the wire is unprotected.
- A link that is too thick never blows; one that is too thin blows in normal driving. My earlier research found a narrow working window; that detail will come when we reach the joining section.

### Still to confirm
- The FS 2026 wording on parallel-cell fusing, and whether single paralleled cells count as "elements".
- Whether sense-wire fusing is required.

**Check:** Why must the fuse sit at the busbar end of a sense wire, not the BMS end?
*Answer:* The fuse only protects the wire downstream of it. A fuse at the BMS end leaves the whole wire unprotected between the busbar and the fuse.

### Part 6 again, in plain terms
A fuse is a part made weak on purpose, so it fails first and cuts the power before wires or cells overheat. Three places: (1) the **main fuse** for a short across the whole pack (about 1,600 A); (2) a **thin strip on each cell**, so a broken cell's strip melts and the other four carry on; (3) a **tiny fuse on each sense wire** right where it leaves the busbar. A fuse protects only what comes after it, going away from the power source, which is why it sits at the busbar end.
Check: if one cell in a group of five fails, its own thin strip melts and disconnects it, so the other four are not damaged.

---

## Part 7: Creepage and clearance

### The idea
Two metal parts at different voltages must be kept far enough apart that electricity cannot jump or crawl between them. There are two ways it can get across:

- **Clearance:** the shortest distance **through the air** between the two parts. Electricity can jump a gap.
- **Creepage:** the shortest distance **along a surface** between them. Dirt, dust or moisture on an insulating surface lets electricity crawl along it, so the surface path has to be longer than the air gap.

Picture two bolts on a plastic board. Clearance is a straight line through the air between them. Creepage is the path an ant would walk over the board from one to the other.

### Why the rules care
Higher voltage can jump or crawl further, so the required distance grows with voltage. A gap that is fine at 8 V is not fine at 350 V.

### What I found
No numbers. The rulebook has a table of required distances, and you must read it. I did not find it in the excerpts.

### Which voltages matter in your pack
This is the useful part, and it comes from your own layout:
- **Next-door plates on the same face** differ by at most two cell groups, about **8.4 V**. A small gap is fine electrically. The danger there is debris or weld spatter bridging it.
- **Every plate to the container wall** is different. The container is tied to the car's chassis. Your segments are stacked in series, so the last segment's plates sit up to **352.8 V** above the chassis. Distances to the container wall, to the sense board and to cable lugs must be sized for **352.8 V**, not the segment's 58.8 V.

### Why this matters for busbars
- Leave a margin between plate edges and the container wall.
- Use insulating sheets under and around the plates.
- Deburr plate edges, because sharp points make electricity jump more easily.

**Check:** Which voltage do you design the plate-to-container distance for: 58.8 V or 352.8 V, and why?
*Answer:* 352.8 V. The container is at chassis potential and the stacked segments put the last one's plates that far above it.

---

## Part 8: Fastener positive locking

### The idea
Vibration slowly unscrews ordinary nuts and bolts. In a race car that is a real problem, and on a high-voltage joint a loose bolt means a hot joint or an arc.

**Positive locking** means a feature that physically stops a fastener turning loose, not one that relies on friction. If you can picture the nut turning and something solid blocking it, that is positive locking.

### Examples
| Positive locking (solid stop) | Friction only (usually not enough) |
|---|---|
| Safety wire through a drilled bolt head | Plain spring washer |
| Split pin (cotter pin) through a castle nut | Plain nut tightened hard |
| Lock tab washer bent against the nut | Thread-locking glue on its own |
| Nylon-insert or all-metal locking nut (often accepted) | Belleville washer on its own (good for tension, may not count) |

The last column is a general view, not a ruling. What counts is decided by the rules and the scrutineers.

### What the rules say (inspection sheet, not the rulebook itself)
The 2024 German inspection sheet says all fasteners must be secured by positive locking, **except** ones that are both non-conductive and non-structural. So a plastic screw holding a label is exempt. Every bolt on a busbar or HV terminal is not.

A 2019 Japanese local rule relaxed this for electrical connections if the team could show the required clamping force was applied and the structure kept outside forces off the joint. That is one event's rule, not yours.

### Why this matters for busbars
- Every bolted busbar, terminal stud and cable lug needs a locking method.
- A Belleville washer keeps clamp force as copper settles and expands, which is good, but it may not be accepted as positive locking by itself. You may need both.
- Plan the lock into the design, for example holes for safety wire, so you are not drilling later.

### Still to confirm
What the FS 2026 text says exactly, and what scrutineers accept for bolted copper joints. Ask before you choose.

**Check:** Why might a Belleville washer alone not count as positive locking?
*Answer:* It works by spring force and friction, so it keeps tension but does not physically block the fastener from turning.

---

## Part 9: Maintenance plugs

### The idea
Even with the car off, your battery segments are still live. Nothing about switching off the car drains them. So the rules require a way to **split the pack into separate segments by hand**, so anyone working inside the accumulator only faces one segment at a time (under 120 V and 6 MJ, Part 3).

That way to split it is the **maintenance plug**: a connector you pull out with your hand.

### How a plug differs from the AIRs (Part 2)
| | AIR (contactor) | Maintenance plug |
|---|---|---|
| Who operates it | The safety circuit, automatically | A person, by hand |
| When | While the car runs | On the bench, before working |
| Counts as a plug? | **No**: a contactor or switch does not count | Yes |

The reason a contactor does not count: a relay can fail closed or be switched on by mistake. A plug that is physically out of its socket cannot.

### What the rules ask for (inspection sheets, not the rulebook text itself)
- It separates the segments at **both poles** (positive and negative).
- It is removed **without tools**.
- It is **keyed**, so it cannot be plugged in the wrong way or into the wrong place.
- It has a **positive lock** (Part 8) so vibration cannot loosen it.
- Any parts of it that do not carry current must be **non-conductive**.
- When the container is opened or a segment is removed, the segments must be separated **using the plugs**.
- Each separated segment must still be under 120 V and 6 MJ.

### What it means for your pack
Two segments together are 9.07 MJ, over the 6 MJ limit, so **every segment boundary needs a plug**: five between the six segments, plus the two ends of the pack.

For the connector itself, the research found two candidate families (Amphenol RADLOK and PowerLok) that are rated for the voltage and current. Whether they meet the "positive locking, tool-less, can't be mis-mated" wording must be checked on the exact part number and in the FS 2026 text.

### Why this matters for busbars
The plug sits on the busbar's end terminal, so the terminal bar must be shaped for the plug's contact, and its cable and plug must be supported by the container, not hanging off the plates.

**Check:** Why can't a contactor count as a maintenance plug?
*Answer:* A contactor can fail closed or be switched on by accident; a plug that is physically removed cannot. The rule wants a physical separation.

---

## Part 10: Container insulation, fire rating, and the documents you must submit

### A. The container
The container is the box around the cells. It has three jobs: keep people away from live parts, protect the cells in a crash, and slow or contain a fire.

What I could confirm (excerpts and inspection sheets, not the full rulebook text):
- **Mounting and crash protection (EV 5.5, 2026):** the container must be fully inside the car's primary structure and attached to it, not taller than the impact structure seen from the side, and protected from impacts by the structure around it. Bolted attachments follow the rulebook's fastener rule. A minimum number of mounting points was added in 2025.
- **Holes:** a Dutch inspection sheet allows holes only for the harness, ventilation, cooling or fasteners, and only if the strength is not hurt. A 2025 note says they can reach the container edge.
- **Fire-retardant material:** an older paper cites a fire-retardant plastic standard (UL94 V0) for the container material. I could not confirm that for 2026.

What I could not find: the actual insulation requirement between live parts and the container wall, and the insulation-resistance test value. Read those in the EV sections.

For busbars this connects to Part 7: the plates sit up to 352.8 V above the container, so insulation sheets and clearances to the wall matter.

### B. The documents
| Document | What it is | What you must show |
|---|---|---|
| **ESF** (Electrical System Form) | Mandatory browser form describing the safety parts of your electrical system | Your accumulator design, fusing, insulation, sensor placement and error |
| **ASES / SES** | Spreadsheet proving the container or monocoque is structurally equivalent to the rule | Strength calculations; an external reviewer checks it (about 125 checkpoints in 2025) |
| **Datasheets** | Manufacturer sheets for cell, fuses, contactors, connectors, cable | Evidence that parts meet their ratings |

### C. What you must justify in writing
From the rules seen:
- Where each temperature sensor sits and why (Part 5).
- How big the measurement error is, and the derated trip temperature (Part 5).
- If you use adhesive, the derating (Part 5).
Expect also: your segment limit calculations (Part 3), fusing (Part 6), and insulation (Part 7). Check the form for the exact list.

### D. Deadlines
- The 2026 ESF deadline was **27 March 2026, 13:00 CET**.
- The 2026 ASES date was not found.
- The 2026 event is over, so for the next season look up the new dates. Deadlines move every year and are strict.

### Check
Name two things the ESF must justify about your temperature sensors.
*Answer:* Where they sit and why, and how large the measurement error is (plus the derating if adhesive is used).

---

## Section 1 finished
Parts 1 to 10 cover all three bullets of research-list section 1. Next section is your choice (suggested: section 2, our requirements).
