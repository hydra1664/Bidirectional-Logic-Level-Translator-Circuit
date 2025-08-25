# Bidirectional Logic-Level Translator (3.3V ↔ 5V)

This is a small project where I built and simulated a discrete-component bidirectional logic-level translator to interface 3.3V devices with 5V peripherals.

## What this is
A simple bidirectional translator using NMOS transistors and pull-up resistors to safely shift logic between 3.3V and 5V domains. I wanted something easy to understand and simulate without using FET-breakout boards or dedicated translator ICs.

## Why I built it
I often work with 3.3V microcontrollers but sometimes need to talk to 5V modules. I wanted a discrete solution that is cheap, easy to inspect in simulation, and shows how level translation behaves during switching (including the transient quirks).

## Components used (as in the simulation)
- Pulse voltage sources for inputs (V3 = 3.3V pulse, V4 = 5V pulse)
- NMOS transistors (model used: SOI7336ADP in the sim)
- Resistors:
  - R1, R2, R3, R4 = 4.7kΩ (pull-ups/pull-downs as used in the circuit)
  - R5, R6 = 1MΩ (high value pulls for biasing where needed)
- DC rails: V1 = 3.3V, V2 = 5V

## Circuit overview
- Direction 3.3V → 5V:
  - 3.3V input drives an NMOS gate (M1), and the output node is pulled up to 5V through pull-up resistors.
- Direction 5V → 3.3V:
  - 5V input drives a second NMOS (M2) and the corresponding output node is pulled to 3.3V.
- Middle nodes for MOS control are referenced to the 3.3V rail so the MOS gates see correct voltages for switching.
- The circuit is symmetric so both directions can operate without direction-specific components.

## Simulation setup
- Tool: LTSpice
- Analysis: Transient
- Stop time used in the simulation: 5ms
- Input waveforms: pulse sources for showing switching in both directions
- Files: open the `.asc` files in `Circuit Files` to examine the schematic and net labels

## How to run the sim (quick steps)
1. Open LTSpice and load the schematic file from `Circuit Files` (for example `level_translator.asc`).
2. Make sure the transistor model `.model` included in the file is loaded or placed in the schematic directory.
3. Run a transient simulation and set stop time to 5ms (this is what I used).
4. Probe the input and output nets to observe `vin`, `vout`, and `i(Vdd)` waveforms.

## What I observed
- The translator successfully shifted logic levels in both directions: 3.3V inputs produced near-5V outputs and 5V inputs were pulled down to the 3.3V domain when expected.
- There are some transient spikes and small switching lag when the MOSFETs switch. These are likely due to gate charge, device capacitances, and the pull-up resistances used.
- The basic topology works well for digital signals at moderate speeds. For very fast or noisy interfaces, extra filtering or a dedicated translator IC might be better.

## Figures and images
I included some plots and a schematic screenshot. The images are saved in the `Images` folder. 
<img width="1920" height="1080" alt="Logic Level Translator Simulation" src="https://github.com/user-attachments/assets/adbbc589-5748-4861-9aff-f7b6ba9338e8" />

## Files included
I put the LTSpice circuit files in a folder named `Circuit Files` and all images in a folder named `Images`.
If you open the schematic in LTSpice the net names and probes I used should match those in the waveform images.

## Notes, tips, and possible improvements
- If you see switching spikes you can try:
  - Lowering pull-up resistor values for faster edge rates (tradeoff: more current).
  - Adding small series resistors (10–100Ω) to damp ringing.
  - Adding small capacitors to ground for local decoupling near the MOSFETs.
- For high-speed bidirectional data lines (like I2C at high rates), consider using a dedicated bidirectional translator or revisiting the resistor values and layout.
- Double-check the MOSFET model behavior at the voltage extremes if you change part numbers.

## My personal takeaway
I enjoyed building and simulating this because it reinforced how simple discrete translators work and showed the transient behavior that often gets glossed over when using ICs. It’s a handy circuit for hobby projects and for learning.

Thanks for checking this out.
