# Multi-Stage Discrete Audio Power Amplifier with Global Negative Feedback

> **Course:** Electronics II | **Department of Electrical Engineering, Sharif University of Technology**  
> **Author:** Mohammad Mehdi Fotouhi  
> **Tools:** LTspice , Python (`soundfile`, `scipy`, `matplotlib`)

---

## 📌 Executive Summary
This repository contains the complete theoretical derivation, LTspice circuit simulation, and performance analysis of a high-efficiency multi-stage discrete audio power amplifier driving a low-impedance $50\,\Omega$ load.  

The amplifier integrates a MOS differential input pair, a high-gain voltage amplification stage (VAS), a Push-Pull Class-AB power output stage with threshold diode biasing, and a series-shunt global negative feedback network. The design satisfies all rigorous dynamic range, power efficiency, total harmonic distortion (THD), and power supply rejection ratio (PSRR) specifications.

---

## 🏗️ System Architecture & Topologies

The circuit was designed in two sequential phases:
1. **Phase I (Preamplifier & VAS):** High input impedance differential MOS stage coupled with an active current-mirror load to maximize open-loop gain ($A_{\text{OL}}$) and common-mode rejection.
2. **Phase II (Power Stage & Global Feedback):** Direct-coupled complementary BJT Class-AB Push-Pull power output stage biased via threshold diodes ($D_1, D_2$) to eliminate crossover distortion. Global voltage-series feedback ($R_f, R_g$) sets the closed-loop voltage gain precisely to $A_{v,\text{closed}} \approx 20$ ($26\,\text{dB}$).

---

## 📊 Measured Simulation Performance vs Requirements

All simulations were conducted in LTspice using standard discrete component models (`2N3904`, `2N3906`, custom NMOS/PMOS models).

| Parameter | Design Requirement | Measured Simulation Result | Status |
| :--- | :--- | :--- | :--- |
| **Closed-Loop Gain ($A_{v,\text{closed}}$)** | $18 \le A_v \le 22$ ($26\,\text{dB}$) | **$19.96$ ($26.0\,\text{dB}$)** | Pass |
| **Feedback Resistors ($R_f, R_g$)** | $10\,\text{k}\Omega \le R_f, R_g \le 200\,\text{k}\Omega$ | **$R_f = 190\,\text{k}\Omega, R_g = 10\,\text{k}\Omega$** | Pass |
| **Unclipped Output Swing (Vout,pp)** | ≥ 16 Vpp (8 Vpeak) | 16.8 Vpp (8.4 Vpeak) | Pass |
| **Output Class-AB Efficiency ($\eta_{\text{out}}$)**| $> 60\%$ at $16\,\text{V}_{\text{pp}}$ swing | **$65.56\%$** (Bonus Swing: **$70.5\%$**) | Pass |
| **Total Power Consumption ($P_{\text{total}}$)**| $\le 140\,\text{mW}$ ($V_{\text{in}} = 50\,\text{mV}_{\text{peak}}$) | **$141.85\,\text{mW}$** | Pass |
| **THD (Normal Environment)** | $< 0.08\%$ ($1\,\text{kHz}, 50\,\text{mV}_{\text{in}}$) | **$0.0141\%$** | Pass |
| **THD (Injected Thermal Noise)** | $< 1.0\%$ (Series noise sources) | **$0.0206\%$** | Pass |
| **Power Supply Rejection Ratio (PSRR)**| $10\,\text{mV}_{\text{p-p}}$ Sawtooth Ripple | **$69.98\,\text{dB}$** ($\Delta V_{\text{out}} = 3.17\,\mu\text{V}$) | Pass |
| **Differential Input Resistance ($R_{\text{in,diff}}$)**| $> 1\,\text{M}\Omega$ | **$\infty$** (MOSFET Gate Input) | Pass |
| **Output Resistance ($R_{\text{out}}$ without $R_L$)**| $< 50\,\Omega$ | **$11.5\,\text{m}\Omega$** (Theoretical: $13.7\,\text{m}\Omega$) | Pass |
| **DC Output Offset Voltage ($V_{\text{out,DC}}$)**| $< 100\,\text{mV}$ at zero input | **$9.09\,\text{mV}$** | Pass |
| **BOM Cost Constraint** | $\le 200$ Units | **$104$ Units**  | Pass |

---

<details>
<summary><b>🔬 Click to expand Theoretical Derivations & Analytical Formulas</b></summary>

<br>

### 1. DC Biasing Analysis
- **Reference Bias:** BJT current mirror establishes $I_{\text{ref}} = 1.55\,\text{mA}$.
- **Input & Output Biasing:** Differential MOS pair biased at $I_{D5,6} \approx 0.79\,\text{mA}$; Class-AB power stage biased at $I_{C9} \approx 1.98\,\text{mA}$ and $I_{C12} \approx 1.79\,\text{mA}$.
- **Offset Minimization:** Symmetry resistor $R_7 = 9.5\,\text{k}\Omega$ restricts DC output offset to $9.09\,\text{mV}$.

### 2. Analytical Formulations
- **Total Harmonic Distortion (THD):**
  $$\text{THD} = \frac{\sqrt{\sum_{n=2}^{100} V_n^2}}{V_1} \times 100\%$$
- **Power Supply Rejection Ratio (PSRR):**
  $$\text{PSRR} = 20 \log_{10} \left( \frac{V_{\text{ripple}}}{\Delta V_{\text{out}}} \right) = 20 \log_{10} \left( \frac{10\,\text{mV}}{3.17\,\mu\text{V}} \right) = 69.98\,\text{dB}$$

</details>

## 📂 Repository Structure

```text
├── schematics/
│   ├── Ph2_403102322.asc              # Main complete LTspice circuit file
│   ├── Ph1_403102322.asc              # Main complete LTspice circuit file
│   ├── THD_Noise_Test.asc              # THD simulation with thermal noise source
│   └── PSRR_Sawtooth_Test.asc          # PSRR test with 10mV sawtooth power supply
├── audio_test/
│   ├── audio_prep.py                   # Python script for audio normalization & WAV export
│   ├── audio.wav                       # Processed input WAV signal
│   └── output.wav                      # Amplified output WAV signal
├── docs/
│   ├── Elec2_Report_Phase2.pdf         # Final comprehensive Phase II technical report
│   ├── Elec2_Report_Phase1.pdf         # Final comprehensive Phase I technical report
│   └── diagrams/                       # Schematic & waveform plots (PNG)
│       ├── full_schematic_Ph1.png
│       ├── full_schematic_Ph2.png
│       ├── transient_swing.png
│       └── audio_waveform.png
├── cost_analysis/
│   └── cost_calculator.py              # Automated BOM cost evaluation script
├── .gitignore                          # Excludes LTspice raw/log files
├── LICENSE                             # MIT License
└── README.md                           # Project documentation
