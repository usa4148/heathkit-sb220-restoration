---
title: "Heathkit SB-220 Linear Amplifier — Restoration Plan"
subtitle: "Rev. 1 — field sources and service bulletins, checked against the original Heath manual"
date: "October 2026"
geometry: margin=0.9in
fontsize: 10pt
colorlinks: true
linkcolor: black
urlcolor: black
citecolor: black
toccolor: black
toc: true
---

<!-- Build: pandoc sb220-restoration-plan.md -o sb220-restoration-plan.pdf --pdf-engine=xelatex -V mainfont="DejaVu Serif" -->

> **DANGER — LETHAL VOLTAGE.** The SB-220 has **about 2500 V DC (CW/TUNE) to 3000 V DC (SSB)** on its plate supply (manual p. 63). That supply is a bank of eight 200 µF electrolytics, enough stored energy to kill. Treat every restoration step as potentially fatal.
>
> - **Unplug the line cord** and wait. Watch the front-panel HV meter fall to zero with the cover *on*, then remove the cover.
> - The **top-cover interlock** (brass spring and metal spacer) shorts the HV supply to chassis when the cover comes off (pp. 61, 70, 81). It is a last-ditch safety, **not** a discharge procedure. A failed bleeder string or a bent spring means full voltage can still be present. The manual says not to touch the HV supply "until all possible high voltage points have been checked with a separate voltmeter" (p. 70).
> - **Discharge before touching anything.** Use a grounded shorting stick with a series resistor (a few kΩ, high-wattage) across each capacitor string, then a solid short. Leave a clip lead shorting HV to chassis while you work.
> - **Never power the amplifier with the cover off.** The interlock short-circuits the supply and the manual warns of damage (p. 70). For resistance checks the manual has you hold the interlock open with a rubber foot (p. 61); **remove it before any power-up.**
> - Work one-handed when anything is live. Use a 2 kW-rated 50 Ω dummy load for all testing, and keep others away from the bench.

# 1. Source documents

| Document | Where | Use |
|---|---|---|
| **Heath SB-220 Assembly Manual**, 595-682-07 (©1969, BAMA scan) | User-supplied PDF | Primary reference: parts list (pp. 5–7), test (pp. 61–63), installation (pp. 65–67), operation and tune-up (pp. 69–74), troubleshooting (pp. 75–77), specifications (pp. 79–80), circuit description (pp. 81–84). |
| **Heath SB-220 Manual**, 595-1122-16 (later Heath/Zenith printing) | User-supplied PDF (RadioAmateur.eu) | Same content and page numbering; chassis photographs (pp. 87–89) label C3–C17, R12–R19, ZD1, C29, RFC1/2, D18 and the interlock. |
| **SB-220 service bulletins** SB-220-1 … -30 (1970–87) | Heath list (puck.nether.net), Aug 1997 and Jan 1998 | Factory fixes. |
| AF6C, *Heathkit of the Month #49: SB-220* | w6ze.org | History, circuit summary, known problems. |
| AG6K (R. Measures), *Circuit Improvements for the SB-220* | qsl.net/wa5bxo/ag6k | Equalizers, multiplier resistors, relay-contact protection, step-start, parasitics, zener replacement. |
| VA7JW, *An SB-220 Compendium: Modifications & Upgrades* (Rev 2, 2014) | orcadxcc.org | Harbach and AG6K kits, parts and values. |
| KV5V, *Notes on Rebuilding an SB-220* (2013) | hotera.org | Full rebuild; grid grounding, fuses, clamp diodes, socket wear. |
| KA2C, *Heathkit SB-220* restoration | ka2c.com | Measured failures (filter caps, 4.7 MΩ multipliers, gassy tube, seized fan). |
| **Heathkit-HFAmps** (groups.io) | groups.io/g/HeathKit-HFAmps | Dedicated Heathkit HF amplifier group. Topic pages would not load for this review (§11). |
| Heath list threads; Heathkit-Radios (groups.io) | puck.nether.net, groups.io | Field experience (grid chokes, flashover, cooling). |

# 2. Amplifier at a glance

## 2.1 Specifications (manual pp. 3, 79–80)

| Item | Value |
|---|---|
| Type | Grounded-grid linear, two **Eimac 3-500Z** triodes in parallel, class B |
| Bands | 80, 40, 20, 15, 10 m (the later **SB-221** dropped 10 m) |
| Drive | 100 W for full output; 52 Ω input, broadband tuned pi input per band (factory-adjusted coils) |
| Input power | **SSB 2000 W PEP; CW 1000 W; RTTY 1000 W** |
| Duty cycle | SSB continuous voice; **CW continuous, max 10 min key-down; RTTY 50 %, max 10 min transmit** |
| IMD | −30 dB or better (third order) |
| Output | 50 Ω unbalanced, SWR 2:1 or less; SO-239 in and out |
| HV | **~2500 V (CW/TUNE)** and **~3000 V (SSB)** no-load. The MODE switch selects primary taps. |
| Line | **120 VAC @ 20 A max or 240 VAC @ 10 A max**, 50/60 Hz. **240 V recommended.** |
| Protection | **Two 10 A circuit breakers**, one per primary leg (rear panel) |
| Metering | Plate meter 0–1 A; multimeter for GRID 0–350 mA, REL PWR, HV 0–3.5 kV |
| Rear panel | Line cord, breakers, ANT RELAY (phono), ALC (phono), RF IN/OUT (SO-239), ground post |
| Size / weight | 14-7/8 × 8-1/4 × 14-1/2 in., 50 lb |

## 2.2 How it works (manual pp. 81–84)

**HV supply**

- **T1** (54-237) has dual tapped primaries; the MODE switch picks full or tapped windings. The secondary feeds a **full-wave voltage doubler**: **D1–D14** silicon diodes (seven per leg) and **C10–C17**, eight **200 µF** electrolytics in two series strings of four.
- **R12–R19** are **30 kΩ 7 W** film resistors. They equalize the capacitor voltages and bleed the bank after power-off.
- RFC1/RFC2 with **C6/C7 (.001 µF 6 kV)** keep RF out of the supply.
- **C1/C2** bypass the AC line.
- **RFC6** gives the plate circuit a DC path to ground: if DC reaches the output network it shorts the HV and trips the breakers.

**Filament and bias**

- **T2** (54-238) supplies **5 VAC @ 30 A** to the filaments through bifilar choke **RFC3**. The two #47 meter lamps are on the filament line.
- A second T2 winding feeds half-wave rectifier **D16** and filter **C4 (20 µF)**, making about **+120 V**. This bias line cuts the tubes off on receive, drives relay **RL1**, and sets the ALC threshold.

**Bias in transmit**

The filament center tap goes to ground through RL1 contacts 9 and 6, zener **ZD1 (1N3996A, 5.1 V, 10 W stud)** and grid shunt **R3**. ZD1 sets the operating bias and idle current.

**Relay**

- **RL1** is a TPDT, 110 VDC coil (69-55).
- About **120 VDC** sits on the ANT RELAY jack; the exciter keys the amplifier by grounding it.

**Grids and output**

- Grid pins 2, 3 and 4 on each tube are bypassed to ground.
- **PC1/PC2** are the plate parasitic chokes.
- The output pi network is **C55** (tune, 250 pF), **C57** (load, 840 pF), **C56** (fixed 80 m padder), **L6** and **L7**.

**ALC**

- The threshold is about **60 V**, set by the R4/R5 divider.
- RF sampled through C47 and R21/R22 is rectified by **D18 (1N458)** when it exceeds the threshold.

**Meters**

| Meter | Circuit |
|---|---|
| Plate | Shunt **R1 (1 Ω 5 W)**, multiplier **R2 (3600 Ω 5 W)** |
| Grid | Shunt **R3 (0.82 Ω 2 W)** |
| REL PWR | **R24 (68 kΩ 1 W)** / **R25** divider, **D17 (1N191 Ge)**, C54, sensitivity pot R26 |
| HV | Multipliers **R6–R8 (4.7 MΩ 1 W)** and shunt **R9 (5600 Ω)** |

## 2.3 Tubes and semiconductors (parts list pp. 5–6)

| Ref | Part | Heath p/n | Function |
|----|----------------------|--------|------------------------------------------|
| V1, V2 | 3-500Z | 411-245 | Final amplifier (instant-heating) |
| D1–D14 | Silicon diode (57-27, ×15 incl. D16) | 57-27 | HV doubler |
| D16 | Silicon diode | 57-27 | Bias/relay supply rectifier |
| D17 | 1N191 germanium | 56-26 | REL PWR detector |
| D18 | 1N458 silicon | 56-24 | ALC rectifier |
| ZD1 | 1N3996A 5.1 V 10 W zener, stud | 56-82 | Operating bias / idle current (early units: 56-75, see SB-220-1) |

# 3. What fails, ranked

## 3.1 HV filter capacitors and equalizers (top priority)

| Item | Detail | Action |
|---|---|---|
| **C10–C17**, 8 × 200 µF (25-224) | Over 50 years old, heated by the 7 W equalizers next to them. VA7JW puts their expected life at about 25 years; KA2C found **cracked bases**. Voltage rating isn't printed in the manual; AF6C gives **450 V**. | **Replace all eight.** Field choices: 220 µF/450 V snap-ins (KA2C, Newark SLP221M450H3P3) or 560 µF/450 V (AG6K); or a **Harbach FB-220** capacitor board. Keep the two series strings matched. |
| **R12–R19**, 30 kΩ 7 W equalizer/bleeders | They run hot, cook the caps and drift. If one opens, its capacitor takes more than its share of voltage and **the bank won't bleed down**. | Replace. AG6K uses **100 kΩ 3 W metal-oxide** (about 70 % less heat, matched within ±5 %) at the cost of a slower bleed. Kit boards include new bleeders. **Whatever you fit, confirm the bank discharges to a safe level in a known time.** |
| C4, 20 µF (25-19) bias/relay filter | Ages like any electrolytic | Replace. VA7JW uses **47 µF 160 V**. |
| SB-220-6 "Temple" capacitors | Rim at the terminal end | Mount so the rim protrudes from the insulator (if keeping originals). |

## 3.2 Bias zener ZD1 and grid circuit

| Item | Failure / symptom | Action (source) |
|---|---|---|
| **ZD1** | **Short → tubes fully on** (idle about 0.3 A instead of 0.09–0.12 A); can destroy tubes. Often killed by grid-current pulses from parasitics or flashover. Most failures were in RTTY service. | Check idle current first (§4.6). Factory fix: SBM-220-1 10 W stud zener (SB-220-1). The 1N3996A is obsolete; NTE5177AK is the cited substitute (VA7JW). *Optional mod:* a string of silicon rectifiers (about 0.8 V each, adjustable; AG6K 7 diodes, Harbach RM-220 up to 8 × 1N5408). |
| **R3**, 0.82 Ω 2 W grid shunt | Fails with ZD1 or on discharge through the interlock (AG6K) | Check; replace. |
| Grid RF chokes (RFC4/RFC5) | **Blow** after a filament-to-grid short or tube flashover (W0TUT; Heath list 2010). A 24 Ω 1 W resistor was used as a stopgap (W0TUT). | Inspect and measure. **The grids need a DC path to ground or the amplifier can oscillate.** |
| Troubleshooting chart | Idle over 0.15 A in CW/TUNE → ZD1. No idle current → relay, ZD1, V1/V2 (p. 76). | — |

## 3.3 Tubes (3-500Z)

- **Gas** after long storage leads to **internal flashover**. That can destroy the tubes, meters and small parts, and blow the grid chokes (Heath list; KV5V). KA2C found one gassy tube.
- Before normal use, run stored tubes with filament and HV on, no drive, for **several hours** to let the getter clean them up (Heath list).
- **Filament-to-grid shorts** are often the end result of a VHF parasitic near **110 MHz** (AG6K, AF6C).
- **Open filaments or low output** come from cold joints in the high-current filament circuit; **0.012 Ω drops more than 0.5 V** at 30 A (SB-220-9). Oxidized socket contacts cause the same thing (KV5V found half power from one unlit filament).
- AF6C: filament voltage below **4.75 V** shortens tube life.
- Manual check (p. 75): continuity between **pins 1 and 5** confirms the filament.
- Cooling: non-Eimac tubes without the phenolic spacer sit flush on the socket and block airflow (Heath list). Plates may show **dull red**; **bright orange or yellow means fix tuning or drive immediately** (p. 70).
- Replace tubes as a pair if one is weak.

## 3.4 Power supply diodes, metering resistors, meters

| Item | Failure | Action (source) |
|---|---|---|
| D1–D14 doubler string | Age and leakage; a shorted diode stresses the rest | Test each; most rebuilders fit **1N5408** (3 A) in place of the 1 A originals (KV5V, VA7JW). Avoid series R/C "equalizers" across modern rectifiers (AG6K). |
| **R6–R8**, 4.7 MΩ 1 W HV multipliers | **Overstressed** (about 1 kV each on a 350 V-rated part) → erratic HV meter. KA2C found them "badly out of spec". | Replace with **2 W flameproof spiral-film** (AG6K) or HV-rated resistors. |
| R1 / R2 / R3 meter shunts | Burn on flashover or a hard interlock discharge | Measure. *Optional:* protective diodes across the meters (W8JI via KV5V; Harbach RM-220). |
| Meter movements | Intermittent from electrolysis at the dissimilar-metal junctions | Clean; conductive-paint repair (AG6K). |

## 3.5 Relay, keying and interface

| Item | Failure | Action (source) |
|---|---|---|
| **RL1** (TPDT 110 VDC) | Contacts oxidize; noisy; a hardened grommet transmits clatter | Clean contacts with a paper strip (KA2C). *Optional:* coil diode (AG6K), vacuum relays for QSK (VA7JW, KV5V). |
| **ANT RELAY line, about 120 VDC** | **Welds or pits modern transceivers' keying contacts.** C52 discharge (AG6K); ICOM relay contacts welded (**SB-220-30**). | **Do not key directly from a modern radio.** Use an external relay interface or a **soft-key board** (Harbach SK-220, W7RY QSK). At minimum add 100–200 Ω in series with the relay line (AG6K). |
| ALC with Kenwood/modern exciters | Too much ALC voltage → overdrive | 8 V zener across the ALC jack (**SB-220-26**). *Optional:* adjustable ALC pot (VA7JW). |
| Relay won't activate (p. 76) | D16, C4, ANT RELAY jack, T2, RL1 | Check in that order. |

## 3.6 Transformers, line and breakers

- **T2 burns out on 240 V** if a T1 primary or the MODE switch (SW2) contacts open, cross-connecting the primaries (**SB-220-19, -27**). Ohm out every primary; for 240 V only, apply the SB-220-27 wire-nut change.
- **Wiring:** 240 V needs its own circuit of 12 AWG, 10 A per hot leg; 120 V needs 10 AWG and 20 A. **Not on ordinary house wiring at full rating** (p. 65). The green cord wire is chassis ground.
- **Inrush:** the stiff primary inrush stresses the power switch and breakers. *Optional:* step-start (AG6K: 20 Ω 10 W resistors and a relay at about 2/3 HV; KV5V: 2 × 15 Ω 5 W per leg with a 2 s delay; Harbach SS-220). AG6K notes T2 already limits filament inrush.
- KV5V found a T1 secondary lead **arced through its sleeving**. Inspect HV lead dress.

## 3.7 RF deck and mechanical

| Item | Symptom | Fix (source) |
|---|---|---|
| **C55 arcing to the plate coil bracket** | Arcs at high power | Bracket 204-2102 or file 1/8 in. off the old one (**SB-220-5**); KV5V filed the C29 bracket edges. |
| Board spacers at D7/D14 | Arcing | Phenolic spacers at those two spots only (**SB-220-2**). |
| **PC1/PC2** parasitic chokes | 4 turns resonate on 10 m → low output | 3 turns (**SB-220-15**). *Optional:* low-Q nichrome suppressors (AG6K, Harbach PS-220). |
| 10 m loading | Can't reach full output with LOAD at 4 | Preset LOAD to about 7–8 (**SB-220-18**). |
| **C56** (80 m load padder) | Breakdown | **SB-220-24/-25**; texts not available. Inspect and replace with an HV-rated part. |
| **RFC6** | Overheats, breakers trip | **SB-220-23** (text not available). Check C29 and the plate circuit for DC leakage. |
| **C29** plate coupling cap | Listed under "no high voltage" and "no RF output" (p. 76) | Inspect and test at voltage. |
| **C7** (.001 µF 6 kV) | — | If unavailable, two 5000 pF 3 kV in series (**SB-220-28**). |
| **Fan** (oilite bearings) | **Seizes** → tube pins and sockets overheat | **Oil yearly, one drop per bearing** (p. 74); early motors have no oil holes (AG6K). KA2C freed a seized shaft. Replacement motors are covered in SB-220-4/-13/-14/-17. |
| Bandswitch | Final TUNE has no effect | Wafer 2 rotated 180°, or L7 installed wrong (p. 76). |
| Input coax to bandswitch reversed | Hard to drive (p. 77) | Correct. |
| Interlock spring/spacer | Bent → no HV, or **no protection** | Inspect each time the cover comes off. |
| Input coil slugs | Factory-set and glued (p. 61) | Leave alone unless input SWR is poor (KA2C re-tuned 40 m). |

# 4. Restoration plan

## 4.1 Phase 0: Assess cold

- Unplug, wait, verify zero volts, discharge (see the safety box).
- Photograph everything. Note mods: soft-key, step-start, suppressors, diode-string bias, QSK, 160 m, WARC.
- Identify the version (SB-220 or SB-221, manual number) and which service bulletins have been done: ZD1 type, phenolic spacers, plate coil bracket, PC1/PC2 turns, fan motor.
- Pull the tubes. Check filament continuity (pins 1–5) and look for gas (blue glow when first run) or tube damage.
- Inspect: filter caps (bulging, cracked bases, leakage), equalizers (scorching), ZD1 and R3, the grid chokes, RFC6, C29, C55 bracket, T1/T2 leads, the interlock spring, the fan.
- **Manual resistance check (p. 61):** hold the interlock open with a rubber foot. Then:
  - plate connectors to chassis ≈ **200 kΩ** after the meter settles;
  - lug 3 of each tube socket to chassis ≈ **20 Ω**.

  **Remove the rubber foot afterwards.**
- Ohm out every T1 and T2 primary and the MODE switch contacts (SB-220-19, -27).

## 4.2 Phase 1: Supply rebuild

1. Replace C10–C17 (or fit a capacitor board) and R12–R19.
2. Test D1–D14 and replace any that are leaky (or all, with 1N5408s).
3. Replace C4, and D16 if leaky.
4. Replace R6–R8. Check R1–R3.
5. Bias: keep a new 5.1 V 10 W stud zener, **or** *(optional)* convert to a diode string.

## 4.3 Phase 2: RF deck and mechanical

Apply SB-220-2, -5 and -15 if missing. Inspect or replace the grid chokes. Clean the bandswitch and relay. Oil the fan. Check C29, C56 and C7. Re-dress the HV leads.

## 4.4 Phase 3: Clean

Vacuum and brush, without solvent on the meter faces. Compressed air through the variable capacitors. Light oil on bushings (KA2C).

## 4.5 Phase 4: First power-up

1. Cover on, interlock free. **Dummy load connected.** Breakers pushed in.
2. Use a **variac in steps**. KA2C read about 600 V DC on the HV at 25 VAC in. Watch for heat, arcing and smoke.
3. At full line, with the controls per p. 62 (METER on HV, MODE CW/TUNE), POWER ON:
   - filaments, meter lamps and fan run;
   - HV reads **about 2500 V**;
   - **MODE → SSB** reads **about 3000 V**;
   - **no reading on either meter except on the HV range** (p. 63).
4. **Condition the tubes:** filaments and HV on, no drive, for several hours after long storage (Heath list).
5. Power off and **verify that the bank bleeds down** on the HV meter.

## 4.6 Phase 5: Idle current and tune-up (manual pp. 71–73)

**Initial settings:** LOAD at 1 (80/40 m) or 4 (20/15/10 m); SENSITIVITY at 12 o'clock; METER on REL PWR; MODE CW/TUNE.

**Tune-up, CW/RTTY:**

1. Tune the exciter into the amplifier with the amplifier off; REL PWR shows exciter output.
2. Turn the exciter to zero drive and switch the amplifier on.
3. Key the exciter in TUNE: **resting plate current ≈ 0.12 A**.
   - Above **0.15 A** means ZD1 (p. 76).
   - SB-220-1 gives **0.09–0.12 A** as normal.
4. Raise drive to **0.3 A**, peak TUNE and LOAD.
5. At **0.4 A**, repeak. Expect about **0.35 A, 2100 V, 110 mA grid**.
6. For **1 kW input**: about **0.5 A, 2000 V, 100–200 mA grid** (Fig. 3-4 plate V vs. I chart).

**SSB:**

| Mode setting | Average plate current | Peaks |
|---|---|---|
| **1000 W PEP** (exciter on SSB, amplifier MODE on CW/TUNE) | 0.12–0.2 A | ≤ 0.25 A |
| **2000 W PEP** (both on SSB) | 0.2–0.3 A | ≤ 0.33 A |

- **Never switch CW/TUNE ↔ SSB while transmitting** (p. 72).
- **Never advance exciter drive past the point where REL PWR stops rising** (p. 70); further drive only produces distortion (Figs. 3-5 to 3-7).
- Watch the plate color; let the fan run for several minutes after heavy use before switching off (p. 70).

## 4.7 Phase 6: Optional modifications

Install only what you need, after a stock baseline.

| Mod | Purpose | Source |
|---|---|---|
| Soft-key / QSK board | Safe keying from modern radios; QSK | Harbach SK-220, W7RY, VA7JW §1.14 |
| Step-start (SS-220 or homebrew) | Inrush on stiff 240 V | AG6K, KV5V, Harbach |
| Rectifier/metering board, diode-string bias, meter diodes | Replaces fragile ZD1; protects meters | Harbach RM-220, AG6K |
| **Grid protection:** direct-grounded grids, fast fuse in B+ and filament CT, clamp diode, "glitch" protection in the HV lead | Survive tube flashover | W8JI (direct grid grounding, cited by VA7JW and KV5V); KV5V 1 A fast fuses. Size any glitch resistor from W8JI's notes. |
| Low-Q parasitic suppressors | VHF parasitic near 110 MHz | AG6K (QST Nov 1990), Harbach PS-220 |
| Equalizers → 100 kΩ 3 W | Cooler capacitor bank | AG6K |
| Input network re-Q | Better input SWR for solid-state exciters | AG6K table |
| 30–43 pF from relay terminal 9 to ground | 10 m bypass SWR | AG6K |
| Fan half-speed and standby switch | Noise | VA7JW §1.12 |
| AC line filter, MOVs, hour meter | — | VA7JW |
| **160 m and WARC bands** | Extra coverage | Field mods; AF6C says 12/17 m need modification. Not in the manual. |

**Avoid removing the interlock.** AG6K did, but warns the discharge can destroy R3 and the meter. KV5V calls removal a bad idea and moves the HV fuse into the lead to the plate choke instead.

# 5. Symptom quick-reference (manual pp. 76–77 plus bulletins)

| Symptom | Check |
|---|---|
| No AC power | Breakers; terminal strip AE jumpers or 120/240 V wiring |
| No high voltage | D1–D14, C10–C17, R12–R19, T1, R1/R3, **C29**, **cover off (interlock)** |
| Idle > 0.15 A (CW/TUNE) | **ZD1** (SB-220-1) |
| No idle current | RL1, ZD1, tubes |
| Relay won't pull in | D16, C4, ANT RELAY jack, T2, RL1 |
| Breakers trip | Plate circuit DC to ground (RFC6, C29), arcing (C55 bracket), shorted diodes or caps, parasitic |
| Final TUNE has no effect | Bandswitch wafer 2 rotated 180°, L7, load, tubes |
| No RF output | Relay wiring, L7, shorted coax, C29 |
| Low output / one tube dark | Filament joints and sockets (SB-220-9), tube |
| Low 10 m output | PC1/PC2 turns (SB-220-15); LOAD 7–8 (SB-220-18) |
| Hard to drive | Input coax to bandswitch reversed; input coil tuning |
| ALC dead | Terminal strip BE parts, ALC jack, D18 |
| Grid meter pegs / bangs, blown grid choke | Parasitic or flashover → tubes, ZD1, R3, chokes |
| Erratic HV meter | R6–R8 |
| Meter dead on one range | GRID: R1/R3; REL PWR: D17, R24–R26, C54; HV: R6–R9; meter switch |
| T2 burned out on 240 V | Open T1 primary or SW2 (SB-220-19, -27) |
| Transceiver keying relay welded | 120 V relay line (SB-220-30) → interface |

# 6. Operation notes

- Use a dedicated 240 V circuit (12 AWG, 10 A per leg). Ground the amplifier at the operating position (p. 66).
- Coax: RG-8/U or RG-11/U; **not** RG-58/59 at this power (p. 65).
- Drive about 100 W. Monitor peaks with a scope if you can (an SB-610 is recommended).
- Observe the CW and RTTY 10-minute limits. Field advice: run RTTY at about 300–400 W.
- The tubes are instant-heating, so you can operate right after switch-on once tuned (p. 70).
- Leave the fan running for a few minutes after a long session.

# 7. Tips and tricks

- Keep a written discharge routine and follow it every time the cover comes off.
- Log the HV (CW and SSB), idle current and the tune-up readings per band after restoration; they are your baseline.
- Note the LOAD setting for each band (p. 71).
- Mark the original tube positions; replace the pair together.
- When working with the interlock defeated for a resistance check, put a tag on the line cord.
- Weigh tube condition before buying a new pair. A good used Eimac pair often outlasts new imports, but test both (KV5V found new RF Parts 3-500ZGs drove more easily).

# 8. Maintenance schedule

| Interval | Task |
|---|---|
| Each session | Check idle current (≈ 0.12 A) and HV at key-up; watch plate color. |
| Yearly | **Remove dust; one drop of light machine oil per fan bearing** (p. 74). Inspect the interlock. Clean relay contacts. |
| 2–3 years | Tune-up readings vs. your baseline; check the bleeder discharge time; retighten filament and HV connections. |
| 5 years | Test the tubes (and condition them if stored); check the ESR and leakage of C10–C17 and C4; measure R6–R8 and R12–R19. |
| After any arc or flashover | Check ZD1, R3, the grid chokes, meter shunts, C29 and the tubes before running again. |

# 9. Parts sources

- **Harbach Electronics**: FB-220 capacitor board, RM-220 rectifier/metering board, SS-220 soft-start, SK-220 soft-key, PS-220 suppressor kit.
- AG6K parts and suppressor kits (Measures).
- Mouser, Digi-Key, Newark: 450 V snap-in electrolytics, 1N5408, metal-oxide and flameproof HV resistors, HV ceramic caps.
- RF Parts: 3-500Z/ZG tubes, sockets.
- Heathkit-HFAmps and Heathkit-Radios (groups.io): donor parts, transformers, coils.

# 10. Sources

- Heath, *SB-220 Assembly Manual* 595-682-07 (1969) and *SB-220 Manual* 595-1122-16.
- Heath list (puck.nether.net):
  - *Heathkit Service Bulletins – SB-220 (Updated & Complete)*, Jan 1998;
  - *SB2XX Series*, Aug 1997;
  - "SB-220 Troubleshooting" (Aug 2010);
  - "RFC-5 blowing in Heath SB-220" (Jul 2004).
- AF6C (R. Eckweiler), *Heathkit of the Month #49: SB-220*, w6ze.org.
- AG6K, *Circuit Improvements for the Heath SB-220*, qsl.net/wa5bxo/ag6k/sb220ci.html.
- VA7JW, *An SB-220 Compendium* Rev 2, orcadxcc.org.
- KV5V, *Notes on Rebuilding an SB-220* (2013), hotera.org.
- KA2C, *Heathkit SB-220*, ka2c.com.
- **Heathkit-HFAmps** (groups.io/g/HeathKit-HFAmps). Topics seen: directly grounded grids, QSK, SB-220 recommendations, load cap, fan, startup, relay problems, mains power. Pages did not load (§11).
- Heathkit-Radios (groups.io).

# 11. Notes on manual coverage

**Confirmed from the manual:**

- Specifications and power requirements.
- Supply topology: doubler, D1–D14, C10–C17, R12–R19.
- Bias via ZD1; ALC threshold; metering resistors.
- Resistance-check targets.
- HV readings (≈ 2500 / 3000 V).
- Idle (≈ 0.12 A) and all tune-up readings and SSB limits.
- Duty-cycle limits, wiring requirements, the troubleshooting chart, and periodic maintenance (fan oil).

Both printings (595-682-07 and 595-1122-16) agree.

**Not in the manual:**

- **Filter capacitor voltage rating** (the parts list gives only "200 µF electrolytic"; 450 V comes from AF6C).
- Values for C29, C52, C56 and RFC4–RFC6 (the fold-out schematic wasn't legible in the scans).
- Service bulletins SB-220-10, -11, -12 and -20 to -25: titles only, text unavailable.
- **Every modification in §4.7** is a field change, not factory.

**Heathkit-HFAmps (groups.io):** topic pages returned fetch errors, so no content from that group is quoted. Check its threads on grounded grids, QSK, recommendations and relay faults directly before choosing mods.
