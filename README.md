# CMOS Phase-Locked Loop (PLL) Design

A transistor-level implementation of a CMOS Phase-Locked Loop (PLL) designed and simulated using **Cadence Virtuoso** and the **Spectre simulator**. The project integrates essential PLL building blocks, including a Phase Frequency Detector (PFD), Charge Pump (CP), Loop Filter, Current-Starved Voltage-Controlled Oscillator (VCO), and Frequency Divider.

The PLL is designed to synchronize the output frequency of the VCO with a reference input through a feedback loop. The design achieves a simulated locked output frequency of **515.6 MHz** from a **65 MHz reference input** using a frequency division ratio of **N = 8**.

---

## 📌 Project Overview

A Phase-Locked Loop is a feedback control system widely used in clock generation, frequency synthesis, clock recovery, and communication systems. It adjusts the output frequency and phase of an oscillator based on the phase and frequency difference between a reference signal and a divided feedback signal.

This project focuses on the transistor-level design, hierarchical integration, and transient simulation of a CMOS PLL using Cadence Virtuoso.

### Key Specifications

| Parameter | Specification |
|---|---|
| Design | CMOS Phase-Locked Loop (PLL) |
| EDA Tool | Cadence Virtuoso IC6.x |
| Simulation Engine | Spectre |
| Process Technology | Generic Process Design Kit 90 nm (`gpdk90`) |
| Reference Frequency | 65 MHz |
| Frequency Divider Ratio | N = 8 |
| Simulated Locked Output Frequency | 515.6 MHz |
| VCO Architecture | Current-Starved Ring Oscillator |
| VCO Control Voltage Range | 0–1.8 V |
| VCO Frequency Range | Approximately 479–531 MHz |
| Design Verification | Transient Simulation |

---

## 🛠️ Tools and Technologies

- **Cadence Virtuoso:** Schematic capture and hierarchical circuit design.
- **Cadence Spectre:** Transient circuit simulation.
- **Cadence ViVA:** Waveform visualization and frequency analysis.
- **gpdk90:** Generic 45 nm CMOS process design kit.
- **CMOS Circuit Design:** Transistor-level implementation of logic gates and analog/mixed-signal blocks.

---

## 📐 PLL Architecture

The PLL consists of the following functional blocks connected in a closed feedback loop:

1. **Phase Frequency Detector (PFD):** Compares the reference clock with the divided feedback clock and generates UP and DOWN control pulses according to their phase and frequency difference.

2. **Charge Pump (CP):** Converts the PFD output pulses into corresponding charging and discharging currents.

3. **Loop Filter:** Filters the charge-pump output to generate a smoother VCO control voltage and influence the dynamic response of the PLL.

4. **Current-Starved VCO:** Generates an oscillating output whose frequency depends on the applied control voltage. The current-starved architecture controls the charging and discharging currents of the oscillator stages.

5. **Frequency Divider:** Divides the VCO output frequency by eight and feeds the resulting signal back to the PFD.

### PLL Feedback Operation

```text
           Reference Clock
              65 MHz
                 |
                 v
        +------------------+
        |       PFD        |
        +------------------+
                 |
             UP / DOWN
                 |
                 v
        +------------------+
        |   Charge Pump    |
        +------------------+
                 |
                 v
        +------------------+
        |   Loop Filter    |
        +------------------+
                 |
               Vctrl
                 |
                 v
        +------------------+
        |  Current-Starved |
        |       VCO        |
        +------------------+
                 |
           PLL Output
           515.6 MHz
                 |
                 v
        +------------------+
        |  Divide by 8     |
        +------------------+
                 |
                 +----------> PFD Feedback
```

---

## 🔬 Functional Blocks

### 1. Phase Frequency Detector (PFD)

The PFD compares the rising edges of the reference and feedback clocks. It generates UP and DOWN pulses that indicate the direction and magnitude of the phase difference.

- Generates UP pulses when the reference signal leads the feedback signal.
- Generates DOWN pulses when the feedback signal leads the reference signal.
- Uses a feedback reset path to return the detector to its idle state after both signals have arrived.

### 2. Charge Pump and Loop Filter

The charge pump responds to the UP and DOWN signals by charging or discharging the loop-filter network. The resulting control voltage adjusts the oscillation frequency of the VCO.

The loop filter also helps control PLL stability, ripple, and settling behavior.

### 3. Current-Starved Voltage-Controlled Oscillator (VCO)

The VCO is implemented using a current-starved oscillator architecture. Its oscillation frequency is controlled by the input voltage, `Vctrl`.

The simulated tuning range is approximately:

- **Minimum frequency:** 479 MHz
- **Maximum frequency:** 531 MHz
- **Control voltage sweep:** 0–1.8 V

The frequency-versus-control-voltage characteristic is used to determine the operating point required for the target PLL frequency.

### 4. Frequency Divider

The frequency divider scales the VCO output by a factor of eight before feeding it back to the PFD.

At the simulated locked frequency:

\[
f_{\mathrm{feedback}}=\frac{f_{\mathrm{VCO}}}{N}
\]

\[
f_{\mathrm{feedback}}=\frac{515.6}{8}
=64.45\text{ MHz}
\]

This feedback frequency is close to the 65 MHz reference frequency. The difference is approximately 0.55 MHz, so the reported operating point should be treated as a simulated near-lock condition unless phase stability and frequency convergence have also been verified.

---

## 📊 Simulation Results

The design was evaluated using Cadence Spectre transient simulations.

### Reported Operating Conditions

| Measurement | Value |
|---|---:|
| Reference Input Frequency | 65 MHz |
| Divider Ratio | 8 |
| Reported VCO Output Frequency | 515.6 MHz |
| Calculated Feedback Frequency | 64.45 MHz |
| Reference-to-Feedback Difference | 0.55 MHz |
| VCO Tuning Range | 479–531 MHz |
| VCO Control Voltage Range | 0–1.8 V |

### Verification Objectives

- Verify the functionality of the individual transistor-level circuit blocks.
- Observe UP and DOWN pulse generation from the PFD.
- Verify the VCO frequency response to changes in `Vctrl`.
- Observe the behavior of the PLL feedback loop during transient simulation.
- Evaluate frequency convergence and the relationship between the reference and divided feedback clocks.

*Note: Simulation plots and waveforms can be added to this section as they become available.*

---

## 📂 Repository Structure

The repository contains the circuit schematics and design files for the PLL and its component blocks.

```text
PLL/
├── README.md
├── PFD/
│   └── Schematic and circuit files
├── Charge_Pump/
│   └── Schematic and circuit files
├── Loop_Filter/
│   └── Circuit files
├── VCO/
│   └── Current-starved VCO schematic and circuit files
├── Frequency_Divider/
│   └── Schematic and circuit files
└── PLL_Top/
    └── Top-level PLL schematic and circuit files
```

*Directory names above illustrate a suggested organization. Actual folder names may differ from those in the repository.*

---

## 🚀 Future Improvements

- Further characterize the VCO tuning curve and estimate its gain.
- Verify steady-state frequency and phase locking under different initial conditions.
- Analyze acquisition time, steady-state phase error, and control-voltage ripple.
- Evaluate PLL stability and sensitivity to component and process variations.
- Perform additional simulation-based characterization of the complete feedback loop.

---

## 👨‍💻 Project Repository

**GitHub:** https://github.com/mohit30905/PLL

**Designed and simulated using Cadence Virtuoso and Spectre.**
