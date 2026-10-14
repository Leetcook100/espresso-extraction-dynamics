# Espresso Extraction Dynamics Lab

[![Live Demo](https://img.shields.io/badge/Demo-GitHub%20Pages-brightgreen?style=flat-square)](https://leetcook100.github.io/espresso-extraction-dynamics/)

An interactive, in-browser hydrodynamic and extraction kinetics simulator for espresso brewing. This project models fluid resistance, non-linear puck compaction, thermal solute transport, and multi-dimensional sensory balance in real time.

👉 **[Live Interactive Demo](https://leetcook100.github.io/espresso-extraction-dynamics/)**

---

## ☕ Core Mathematical & Physical Modeling

The simulation treats the coffee puck as a dynamic porous medium and integrates physical fluid transport with a semi-empirical extraction rate equation using forward Euler numerical integration ($\Delta t = 1.0\,\text{s}$).

### 1. Fluid Dynamics & Hydraulic Resistance (Modified Darcy's Law)
Flow rate $Q(t)$ is governed by instantaneous pressure drop $\Delta P$ and non-linear puck impedance $R(d, P)$:

$$Q(t) = \max\left(0.05, \frac{P}{R(d, P) \cdot c_1} \cdot c_2 \cdot f_{\text{prewet}}(t) \cdot \lambda_{\text{channel}}\right)$$

Where:
* **Particle Size Decay**: Resistance scales with grind diameter $d$ via an empirical power law:
  $$R_{\text{hydraulic}} = \left(\frac{R_0}{d}\right)^{2.8} \cdot \kappa_{\text{compaction}}$$
* **Puck Compaction Factor ($\kappa_{\text{compaction}}$)**: Above 8.5 bar, hydraulic stress compresses the bed matrix, causing a non-linear resistance penalty:
  $$\kappa_{\text{compaction}} = 1.0 + \max(0, P - 8.5) \times 0.22$$
* **Pre-infusion Envelope ($f_{\text{prewet}}$)**: Models bed saturation during the first 5 seconds:
  $$f_{\text{prewet}}(t) = \begin{cases} \left(\frac{t}{5}\right)^{2.2} & t \le 5 \\ 1.0 & t > 5 \end{cases}$$
* **Channeling Breakdown ($\lambda_{\text{channel}}$)**: When $d < 280\,\mu\text{m}$ and $P > 9.5\,\text{bar}$, structural failure (channeling) accelerates downstream flow instability after $t > 18\,\text{s}$.

### 2. Solute Dissolution Kinetics (Thermal ODE)
Extraction Yield (EY%) is determined by the cumulative mass of dissolved organic solutes $M(t)$ relative to total dose ($D = 18.0\,\text{g}$):

$$\frac{dM}{dt} = k(T, d) \cdot Q(t) \cdot \left[\frac{M_{\max} - M(t)}{M_{\max}}\right] \cdot \eta_{\text{channel}}$$

* **Arrhenius Temperature Scaling**: Dissolution velocity increases exponentially with water temperature $T$:
  $$k(T, d) = k_0 \cdot \exp\left(\frac{T - 93.0}{14.0}\right) \cdot \left(\frac{380}{d}\right)^{1.3}$$
* **Extraction Efficiency**: Follows first-order saturation kinetics with theoretical maximum dissolvable ceiling $M_{\max} = 28\%$ of dry mass.

---

## 🎯 Model Calibration (The Inverse Problem)

Unlike fundamental physical constants, parameters in phenomenological porous media models must be calibrated against empirical benchmarks. 

This model is calibrated against the **SCA Golden Cup Standard** for espresso:
$$\text{Target: } 18.0\,\text{g Dose} \xrightarrow{32\,\text{s}} 36.0\,\text{g Yield} \implies \text{EY} \approx 20.1\% \text{ at } (380\,\mu\text{m}, 93.0\,^\circ\text{C}, 9.0\,\text{bar})$$

Through discrete Riemann sum optimization:
1. **$R_0$ solved to $557$**: Calibrates cumulative flow $\int_0^{32} Q(t)\,dt$ exactly to $36.0\,\text{g}$.
2. **$k_0$ solved to $0.174$**: Guarantees total solute $M(32) = 3.619\,\text{g}$, achieving a balanced $10.05\%$ Total Dissolved Solids (TDS) and $20.1\%$ EY.

---

## 📊 Sensory Utility Mapping (Radar Matrix)

Chemical outputs (EY% and TDS%) are mapped to a 5-axis normalized sensory utility vector $S \in [1, 10]^5$:

* **Acidity**: Highest at under-extraction ($\text{EY} < 18\%$) or low temperatures; smooths into balanced citric/malic acidity at the sweet spot.
* **Sweetness**: Centered around optimal caramel/sugar breakdown ($\text{EY} = 20.1\%$).
* **Bitterness**: Driven by chlorogenic acid lactone and phenylindane dissolution at elevated temperatures ($>94\,^\circ\text{C}$) or high water throughput.
* **Body (TDS)**: Gauges emulsion viscosity as a function of dissolved solids concentration and emulsification pressure.
* **Balance & Clarity**: Penalty-driven utility metric reflecting sensory harmony and absence of localized micro-channeling.

---

## 🛠️ Tech Stack & Implementation
* **Pure Client-Side Execution**: Zero server latency, running 100% in the browser.
* **Chart.js**: Dynamic dual-axis time-series curves and responsive pentagon radar visualizer.
* **Tailwind CSS**: Modern UI layout with instantaneous DOM reactivity.

## 🚀 Local Development
Clone and run immediately—no package installation or compilation required:
```bash
git clone [https://github.com/Leetcook100/espresso-extraction-dynamics.git](https://github.com/Leetcook100/espresso-extraction-dynamics.git)
cd espresso-extraction-dynamics
# Open index.html directly in any browser
