# ⚡ Electronic Circuits Mini Projects

> Two analog and digital electronics projects developed  a **Colpitts Oscillator with Current Shunt Feedback Amplifier** for stable 25 kHz sinusoidal signal generation, and a **NAND Gate implemented using PN Junction Diodes** demonstrating universal logic using discrete components.

---

---

## 📌 Project 1 — Colpitts Oscillator with Current Shunt Feedback Amplifier

### Aim
To design a Colpitts oscillator operating at **25 kHz** and enhance its output signal amplitude using a **current shunt feedback amplifier**.

---

### Block Diagram

```
[Colpitts Oscillator] ──► [Current Shunt Feedback Amplifier] ──► [DSO]
  LC tank + BJT              Negative feedback, gain control        Waveform analysis
```

---

### Background

The Colpitts oscillator is widely used in RF and clock signal generation due to its simplicity and frequency stability. However, the raw output signal often has insufficient amplitude for practical use. A **current shunt feedback amplifier** is cascaded to boost the signal while maintaining low distortion and stable gain through negative feedback.

---

### Oscillator Design

**Frequency Formula:**
```
f = 1 / (2π √(L × Ceq))

where  Ceq = (C1 × C2) / (C1 + C2)   [series equivalent of capacitive divider]
```

**For f = 25 kHz with L = 10 mH:**
```
Ceq = 1 / (4π² × f² × L)
    = 1 / (4π² × (25000)² × 0.01)
    ≈ 4.05 nF  →  choose C1 = C2 = 8.2 nF (standard value)
```

---

### Component Values

#### Colpitts Oscillator

| Component | Value | Purpose |
|---|---|---|
| Transistor | BC547 (NPN) | Active element providing gain |
| L | 10 mH | Inductor in LC tank |
| C1 | 8.2 nF | Capacitive divider — sets frequency |
| C2 | 8.2 nF | Capacitive divider — feedback path |
| R1 | 47 kΩ | Voltage divider bias (upper) |
| R2 | 10 kΩ | Voltage divider bias (lower) |
| RC | 4.7 kΩ | Collector load resistor |
| RE | 1 kΩ | Emitter resistor for thermal stability |
| CE | 10 µF | Emitter bypass capacitor |
| CC | 0.1 µF | Coupling capacitor |
| VCC | 12V DC | Power supply |

#### Current Shunt Feedback Amplifier

| Component | Value | Purpose |
|---|---|---|
| Transistor | BC547 (NPN) | Amplifying element |
| RF | 10 kΩ | Shunt feedback resistor (collector to base) |
| RC | 4.7 kΩ | Collector resistor |
| VCC | 12V DC | Power supply |

---

### Working Principle

1. The **LC tank circuit** (L, C1, C2) determines oscillation frequency via resonance.
2. The **capacitive voltage divider** (C1, C2) feeds a fraction of the output back to the transistor base — regenerative (positive) feedback sustains oscillations.
3. The **BJT** compensates for tank circuit energy losses by providing gain equal to or greater than 1 (Barkhausen criterion).
4. The **oscillator output** (low amplitude sine wave) is passed to the current shunt feedback amplifier.
5. The **feedback resistor RF** connects collector to base, applying negative feedback — this reduces distortion, stabilizes gain, and improves linearity.
6. The **amplified sine wave** is observed on the Digital Storage Oscilloscope (DSO) for waveform analysis.

**Barkhausen Criterion (for sustained oscillations):**
```
Loop Gain |Aβ| = 1  and  Phase shift = 0° (or 360°)
```

---

### Simulation (LTspice)

```spice
* Colpitts Oscillator — LTspice Netlist
VCC VCC 0 DC 12V
Q1  C B E BC547
R1  VCC B  47k
R2  B   0  10k
RC  VCC C  4.7k
RE  E   0  1k
CE  E   0  10u
L1  C   T  10m
C1  T   B  8.2n
C2  B   0  8.2n
CC  C   OUT 0.1u

.model BC547 NPN(Is=1e-14 Bf=200)
.tran 0 2ms 0 10ns
.probe V(OUT)
.end
```

Run **Transient Analysis** for 2ms with 10ns step. Probe `V(OUT)` to observe the 25 kHz sine wave.

---

### Results

| Parameter | Theoretical | Observed |
|---|---|---|
| Oscillation Frequency | 25 kHz | ~25 kHz |
| Waveform | Sinusoidal | Stable sine wave |
| Distortion | Minimal | Negligible |
| Amplifier Gain | Stable | Consistent |
| Signal Amplitude (post-amp) | Higher than raw | Confirmed amplified |

- Oscillations were sustained continuously without decay.
- Current shunt feedback significantly reduced harmonic distortion.
- Minor frequency deviation due to component tolerances — negligible impact.

---

### Applications

- Local oscillators in communication receivers
- RF signal generation
- Clock signal generation in digital systems
- Function generators and electronic test equipment

---
---

## 📌 Project 2 — NAND Gate using PN Junction Diodes

### Aim
To implement a **2-input NAND gate** using PN junction diodes and a pull-up resistor, demonstrating universal digital logic using discrete semiconductor components.

---

### Background

The NAND gate is a **universal gate** — any logic function (AND, OR, NOT, NOR, XOR) can be built from NAND gates alone. Conventionally implemented using transistors or ICs, this project explores an alternative using only PN junction diodes and a resistor. This highlights how the diode's unidirectional current flow property can replicate binary switching behavior.

---

### Circuit Description

```
VCC (+5V)
    │
   [R]  1 kΩ  (Pull-up)
    │
    ├────────────────────── OUTPUT (Y)
    │
    ├─── [D1 Cathode ←|← Anode] ─── INPUT A
    │
    └─── [D2 Cathode ←|← Anode] ─── INPUT B
                   │
                  GND
```

Both diode **cathodes** are tied together at the output node. The **pull-up resistor** connects the output to VCC, ensuring output defaults HIGH when no diode conducts.

---

### Component Values

| Component | Value | Purpose |
|---|---|---|
| D1, D2 | 1N4148 | PN junction switching diodes |
| R (Pull-up) | 1 kΩ | Holds output HIGH when diodes OFF |
| VCC | +5V | Logic supply |
| Logic HIGH | ~5V | Binary 1 |
| Logic LOW | ~0.7V | Binary 0 (diode Vf drop) |

---

### Working Principle

**Case 1 — Both inputs HIGH (A=1, B=1):**
- Both D1 and D2 are forward biased → both conduct
- Low-resistance path created from output to GND
- Output pulled LOW (~0.7V) → **Y = 0**

**Case 2 — Any input LOW:**
- At least one diode is reverse biased → that diode blocks current
- No complete path to GND
- Pull-up resistor holds output HIGH (~5V) → **Y = 1**

This directly satisfies the NAND truth table.

---

### Truth Table

| Input A | Input B | D1 State | D2 State | Output Y |
|:---:|:---:|:---:|:---:|:---:|
| 0 (0V) | 0 (0V) | Reverse biased | Reverse biased | **1 (~5V)** |
| 0 (0V) | 1 (5V) | Reverse biased | Forward biased | **1 (~5V)** |
| 1 (5V) | 0 (0V) | Forward biased | Reverse biased | **1 (~5V)** |
| 1 (5V) | 1 (5V) | Forward biased | Forward biased | **0 (~0.7V)** |

---

### Simulation (LTspice SPICE Netlist)

```spice
* NAND Gate using PN Junction Diodes
VCC VCC 0 DC 5V
VA  A   0 DC 5V    ; change to 0V to test LOW input
VB  B   0 DC 5V    ; change to 0V to test LOW input

D1  A  OUT  D1N4148
D2  B  OUT  D1N4148
R1  VCC OUT 1k

.model D1N4148 D(Is=2.52n Rs=0.568 N=1.752 Cjo=4p M=0.4 tt=20n)
.op
.end
```

Test all 4 input combinations by changing VA and VB between 0V and 5V. Use `.op` for DC operating point analysis.

**Tinkercad:** Place 2× diodes (1N4148), 1× 1kΩ resistor, 2× switches, and a 5V supply. Toggle switches and measure output with multimeter.

---

### Results

| Input A | Input B | V(OUT) Observed | Expected | Match |
|:---:|:---:|:---:|:---:|:---:|
| 0V | 0V | ~5.0V | HIGH | ✅ |
| 0V | 5V | ~5.0V | HIGH | ✅ |
| 5V | 0V | ~5.0V | HIGH | ✅ |
| 5V | 5V | ~0.7V | LOW | ✅ |

- All four input combinations produced the correct NAND logic output.
- Output LOW was ~0.7V (diode forward voltage) — within acceptable logic LOW threshold for TTL/CMOS.
- Pull-up resistor reliably maintained HIGH state when diodes were OFF.

---

### Design Considerations

- **Forward voltage drop** (Vf ≈ 0.7V for 1N4148): Output LOW is 0.7V instead of ideal 0V — still recognized as logic LOW by standard gates.
- **Pull-up resistor** (1 kΩ): Balances current draw and switching speed; too high a value slows transitions, too low wastes power.
- **Reverse leakage current**: Minimal for 1N4148 (< 5 nA) — no impact on logic levels.
- **Fanout limitation**: Diode logic has limited drive capability; not suitable for directly driving multiple gate inputs without buffering.

---

### Applications & Significance

- Educational tool for understanding digital logic at the component level
- Demonstrates the universality of the NAND gate
- Foundation for Diode-Transistor Logic (DTL) — precursor to TTL
- Useful in low-power, discrete component prototype circuits

---


Submitted as an academic project. All rights reserved by the authors.
