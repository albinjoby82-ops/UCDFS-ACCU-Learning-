# Accumulator Busbars & HV Connections – Research Brief

## Project context
- **Competition:** Formula Student – FSUK and FSG (FS Rules 2026), or FSAE (undecided)
- **My scope:** busbars and HV connections for the accumulator
- **Pack:** 84s5p Molicel INR21700-P50B (420 cells), 6 segments of 14s5p
  - Cell: 5.0 Ah / 18 Wh, 3.6 V nom, 4.2 V max, 2.5 V min, 60 A continuous, DC IR ~12.8 mΩ, steel can, 71 g
  - Pack: 352.8 V max / 302.4 V nom / 210 V min, 25 Ah, ~7.56 kWh (~27.2 MJ), ~215 mΩ DC resistance (cells only), ~1.6 kA estimated short-circuit current
  - Segment: 58.8 V max, ~4.54 MJ (under the 120 V / 6 MJ segment limits; verify in rulebook)
- **Segment layout:** honeycomb, 5 cells per column × 14 columns; each column = one parallel group; columns alternate orientation (zig-zag series)
  - Top face: 7 busbars; bottom face: 6 busbars + 2 terminal bars (segment − at column 1, + at column 14, both on bottom face)
  - 15 voltage-sense taps per segment
  - Plan: large segmented plates covering each face (NOT one continuous plate – that would short the segment), possibly two-layer (thin weld layer + thicker spreader)
- **Motor/inverter:** EMRAX 208 MV winding + Bamocar PG-D3 700/400; expected peak DC current ~240–260 A; **busbar design current 300 A peak** (5 × 60 A cell limit); RMS current TBD from lap sim (estimate 80–120 A)
- **BMS:** ENNOID (slave version TBD: LTC6811 = 12 cells + 4 temps, LTC6812 = 15 cells + 8 temps, LTC6813 = 18 cells + 8 temps). Only ~8 thermistors per segment = ~11% coverage – likely below rule requirement
- **Cooling:** none currently – estimated 1.4–2.1 kW pack heat at 80–100 A RMS → large temperature rise over endurance; major risk
- **Preferences:** avoid gluing thermistors/sense wires to busbars

## Open questions
1. Welding access: hobby spot welder, resistance welder, or laser?
2. Container material; can a face act as a heatsink with airflow?
3. Which ENNOID slave version?
4. Final competition / rulebook choice

## Research list

### 1. Rules (start here)
- Pick the rulebook: FS Rules 2026 (FSUK/FSG) vs FSAE; read the EV/accumulator sections end to end
- Exact wording on: segment limits (120 V / 6 MJ), cell temperature monitoring (% of cells, sensor location, max temp), parallel-cell fusing, sense-wire fusing, creepage/clearance, fastener positive locking, maintenance plugs/separation, container insulation & fire rating
- Documentation requirements and deadlines (ESF, ASES, datasheets) – what must be justified in writing

### 2. Our requirements (feeds every calculation)
- Lap/endurance sim: peak DC current, RMS over endurance, peak durations
- EMRAX 208 MV datasheet/manual; Bamocar PG-D3 700/400 manual (DC current limit settings, DC-link capacitance)
- Molicel P50B datasheet: IR vs temperature, current limits, tab/can construction

### 3. Busbar electrical design
- Conductor resistance R = ρL/A; resistivity of Cu, Al, Ni
- Adiabatic heating for peaks and fault current: ΔT = I²tρ/(A²cd)
- Short-circuit / fuse I²t let-through; k-factor conductor sizing (IEC 60949)
- Current sharing in parallel groups: path-resistance differences vs cell IR (target <~1% of 12.8 mΩ); terminal placement
- Reference: CDA Publication 22 "Copper for Busbars"

### 4. Joining cells to busbars
- Resistance spot welding copper vs nickel; slotted-copper and nickel-sandwich techniques
- Laser welding thin Cu/Al (sponsor or university lab)
- Weld testing: pull/peel tests, 4-wire milliohm joint resistance
- Fusible links designed into busbars (weld-neck fusing)
- Cylindrical cell anatomy: positive button vs can rim (can = negative), insulating rings, heat tolerance during welding
- Note: pure nickel strip, Hilumin, brass are too resistive as main conductors at 60 A/cell

### 5. Busbar mechanical and layout
- Segmented plates in honeycomb: gap design, insulation sheets, creepage/clearance
- Two-layer construction (thin weld layer + thicker spreader): solder, rivet, or clamp
- Vibration, fatigue, thermal expansion in holders and busbars
- Plating (tin/nickel) and galvanic corrosion between dissimilar metals

### 6. Thermal design (biggest risk – no cooling)
- Pack heat: I²R per cell from RMS current; temperature rise over endurance
- End-face cooling of cylindrical cells: axial vs radial conduction; negative end conducts heat better
- Thermal interface materials: gap pads rated for both W/mK and dielectric breakdown voltage
- Heat rejection: natural vs forced convection, ducting, fans, container wall as heatsink
- Tools: simple 1D thermal-resistance network first, then FEA (ANSYS, SolidWorks Flow)

### 7. Thermistors and voltage sensing
- NTC basics: B-value, accuracy, divider circuit used by the BMS
- Mounting: integral busbar tabs, screw/ring-lug thermistors, sense PCBs/flex boards; solder vs weld vs mechanical
- HV isolation: thermistors and wires on busbars are at HV potential
- Sense-wire design: fusing/series resistors, gauge, routing, strain relief, connectors
- Negative terminals alternate faces (bottom for odd columns, top for even) – affects thermistor placement

### 8. BMS integration (ENNOID)
- Identify slave version; cell inputs and temperature channels
- Temperature coverage vs rule %; expansion options (multiplexer, second slave, custom board)
- Sense harness connector type and wiring order
- References: ENNOID-BMS GitHub/datasheet; foxBMS docs for LTC6813 designs

### 9. HV connections out of the segment
- Terminal studs / press-in nuts; bolted joint design (contact area, torque, Belleville washers, locking)
- Inter-segment separation: maintenance plug vs contactor; touch-safe locking connectors rated ≥352.8 V and 300 A
- HV cable sizing and derating; orange HV cable specs; proper lug crimping
- Main fuse and AIRs: DC voltage rating and interrupt rating vs ~1.6 kA fault estimate

### 10. Validation and documentation
- Test plan: joint resistance (every weld/bolted joint), pull tests, thermal camera under load, insulation resistance
- Study past teams' accumulator ESF sections, design reports, theses (e.g., VUB "Accumulator Design for a Formula Student Race Car")

## Suggested order (next two weeks)
1. Rules (1)
2. Lap sim currents (2)
3. Thermal estimate (6) – may change the design
4. Welding access → busbar design (3–5)
