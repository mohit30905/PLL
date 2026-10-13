# CMOS Phase Frequency Detector (PFD) Design

A transistor-level implementation of a CMOS Phase Frequency Detector (PFD) designed and simulated using the Cadence Virtuoso EDA toolsuite. This project showcases custom CMOS logic gate design, schematic hierarchy, and transient simulation analysis to validate phase-locked loop (PLL) building blocks.

---

## 📌 Project Overview
The Phase Frequency Detector (PFD) is a critical component in Phase-Locked Loops (PLLs) and Frequency Synthesizers. It compares the phase and frequency of a reference signal ($V_{ref}$) against a feedback signal ($V_{div}$). Based on the lead/lag relationship, it generates responsive `UP` and `DOWN` error control pulses to feed into a charge pump.

### Key Features:
* **Technology Node:** Generic Process Design Kit 90nm (`gpdk90`).
* **Architecture:** Conventional 3-state D-Flip-Flop (DFF) based PFD architecture with a NAND gate feedback reset path.
* **Dead-Zone Mitigation:** Optimized reset-path delay to eliminate dead-zone issues, ensuring response linearity even at near-zero phase offsets.

---

## 🛠️ Tools & Technologies Used
* **EDA Suite:** Cadence Virtuoso (IC6.x)
* **Simulation Engine:** Spectre Transient Simulator
* **Process Technology:** gpdk90 (90nm CMOS Technology)
* **Waveform Viewer:** Cadence ViVA (Virtuoso Visualization and Analysis)

---

## 📐 Circuit Architecture & Hierarchical Design

The project is structured hierarchically to maintain design cleanlines and reuse building blocks:

1. **Primitive Logic Gates:** Custom-designed Transistor-level `NAND`, `NOT`, and Transmission Gate switches.
2. **Sub-circuits (D-Flip-Flop):** Custom falling/rising-edge-triggered D-Flip-Flops built using custom CMOS logic gates with synchronous/asynchronous reset controls.
3. **Top-Level PFD:** Combines two DFF blocks and a fast feedback NAND gate to establish a standard 3-state state machine:
   * **State 00:** Idle ($UP = 0, DOWN = 0$)
   * **State 10:** Reference leads ($UP = 1, DOWN = 0$)
   * **State 01:** Feedback leads ($UP = 0, DOWN = 1$)
   * **State 11:** Unstable/Transient ($UP = 1, DOWN = 1 \rightarrow \text{Triggers Reset}$)

---

## 📊 Simulation Results & Functional Verification

Validation was achieved through extensive **Spectre Transient Simulations** under multiple phase/frequency delta scenarios:

* **Scenario A (Reference Leads):** $V_{ref}$ edges arrive before $V_{div}$. Correct generation of `UP` pulses proportional to the phase error, with `DOWN` remaining resting at logic low.
* **Scenario B (Feedback Leads):** $V_{div}$ edges arrive before $V_{ref}$. Correct generation of `DOWN` pulses proportional to the phase error, with `UP` resting at logic low.
* **Scenario C (Frequency Mismatch):** Verified correct frequency acquisition when input signals are completely out of phase and tracking toward lock.

*(Optional: Add screenshots of your Schematics or Spectre Waveform plots inside a `spec/` folder and link them here! Example below)*
---

## 📂 Directory Structure
```text
├── PFD/
│   ├── README.md               # Project documentation
│   ├── schematic/              # Top-level and subcircuit schematic views (or export netlists)
│   └── symbol/                 # Symbol views for hierarchical integration
