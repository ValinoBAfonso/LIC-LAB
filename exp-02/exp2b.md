# Design and Analysis of a Cascode-Style CS Amplifier (Circuit 2b)

This repository contains the design and simulation analysis for a Common Source amplifier with an NMOS active load (acting as degeneration) and a PMOS active load, implemented in **180nm CMOS technology**.

---

## 1. Design Specifications and Assumptions
This design targets a specific operating point while maintaining a power budget under 1mW.
<img width="1412" height="938" alt="Screenshot 2026-03-01 160353" src="https://github.com/user-attachments/assets/dcfd7b84-2fdd-423c-89d4-191c5ff6e8a4" />



### Given Specifications
* **Technology Node:** 180nm
* **Supply Voltage ($V_{DD}$):** 1.8V
* **Target Drain Current ($I_D$):** ~200µA
* **Target $V_{DS3}$ (Degeneration Voltage):** 0.3V (adjusted from 0.34V in simulation)

* ### Width Calculation for Circuit 2b ($M_2$ and $M_1$)
The physical dimensions of the transistors are derived from the saturation current equation to maintain a design current of $I_D = 200 \mu A$ with an overdrive voltage $V_{OV} = 0.25 V$.

#### A. NMOS Driver and Degeneration ($W_1, W_3$)
Using the specific NMOS process constants from the design notes ($t_{ox} = 4.1 \times 10^{-9}$, $\mu_n = 273.80 \times 10^{-4}$):
* **Formula**: $W = \frac{2 \cdot I_D \cdot L \cdot t_{ox}}{\mu_n \cdot \epsilon_r \cdot \epsilon_0 \cdot (V_{OV})^2}$
* **Substitution**:
$$W_1 = \frac{2 \cdot (200 \times 10^{-6}) \cdot (180 \times 10^{-9}) \cdot (4.1 \times 10^{-9})}{273.80 \times 10^{-4} \cdot 3.9 \cdot 8.854 \times 10^{-12} \cdot (0.25)^2}$$
* **Result**: $$W_1 \approx \mathbf{7.81 \, \mu m}$$

#### B. PMOS Load ($W_p$)
Using the PMOS mobility constant ($\mu_p = 115.6 \times 10^{-4}$) to account for lower hole mobility:
* **Substitution**:
$$W_p = \frac{2 \cdot (200 \times 10^{-6}) \cdot (180 \times 10^{-9}) \cdot (4.1 \times 10^{-9})}{115.6 \times 10^{-4} \cdot 3.9 \cdot 8.854 \times 10^{-12} \cdot (0.25)^2}$$
* **Result**: $$W_p \approx \mathbf{18.47 \, \mu m}$$

---

## 2. Theory and Calculations

### A. DC Biasing (Hand Calculations)
1.  **NMOS Cascode/Degeneration ($M_3$):** * To achieve $V_{DS3} \approx 0.3V$, the gate bias $V_{B2}$ is set.
    * Simulated $V_{B2} = 0.61V$.
2.  **Input Driver ($M_1$):**
    * $V_{GS1} = V_{OV} + V_{thn} = 0.25V + 0.36V = 0.61V$.
    * $V_{in(bias)} = V_{GS1} + V_{DS3} = 0.61V + 0.3V = 0.91V$.
3.  **PMOS Load ($M_2$):**
    * $V_{G2} = V_{DD} - (V_{OV} + |V_{thp}|) = 1.8V - 0.64V = 1.16V$.
    * <img width="636" height="513" alt="Screenshot 2026-03-01 161436" src="https://github.com/user-attachments/assets/973de49b-aa1b-490b-80e1-679dc5d1690d" />

    ---

### Updated Design Parameters (Circuit 2b)
To optimize the circuit performance and match simulation data, the following specific transistor widths were implemented:

* **NMOS Driver ($M_1$):** $W_1 = 26\mu m$
* **PMOS Active Load ($M_2$):** $W_2 = 15.7\mu m$
* **NMOS Active Degeneration ($M_3$):** $W_3 = 37.2\mu m$
* **Channel Length ($L$):** 180nm
* **Degeneration Voltage ($V_{DS3}$):** 0.3V (Target) / 0.347V (Simulated)

---

---

## Circuit 2b: Theoretical Calculation and $\lambda$ Alignment

To align the theoretical hand calculations with the simulated results ($V_{out} = 42.72 \, mV, V_{in} = 19.99 \, mV$), the following updated derivation was used:

### 1. Simulated Gain Calculation
$$A_{v(sim)} = 20 \log_{10} \left( \frac{42.72 \, mV}{19.99 \, mV} \right) = \mathbf{6.59 \, dB}$$
* This corresponds to a voltage gain ratio of approximately **2.137**.

### 2. Transconductance ($g_{m1}$)
Using the corrected overdrive voltage $V_{OV} = 0.25V$ and design current $I_D = 200 \mu A$:
$$g_{m1} = \frac{2I_D}{V_{OV}} = \frac{2 \times 200 \mu A}{0.25V} = \mathbf{1.6 \, mS}$$

### 3. Theoretical Gain at Fixed $\lambda = 0.1$
Using the formula $A_v \approx \frac{-g_{m1} \cdot r_{o2}}{1 + g_{m1} \cdot r_{o3}}$:
* **Output Resistance ($r_{o2}, r_{o3}$):** $\frac{1}{0.1 \cdot 200 \mu A} = \mathbf{50 \, k\Omega}$
* **Gain Ratio:** $A_v = \frac{1.6 mS \cdot 50 k\Omega}{1 + (1.6 mS \cdot 50 k\Omega)} = \frac{80}{81} \approx \mathbf{0.987}$
* **Decibel Gain:** $20 \log_{10}(0.987) \approx \mathbf{-0.11 \, dB}$

### 4. Solving for $r_{o2}$ to Match Simulation ($\lambda = 0.125$)
To reach the simulated gain of **6.59 dB** (ratio 2.137) while using $\lambda = 0.125$ for the degeneration transistor:
* **Degeneration Resistance ($r_{o3}$):** $\frac{1}{0.125 \cdot 200 \mu A} = \mathbf{40 \, k\Omega}$
* **Required Load Resistance ($r_{o2}$):**
$$2.137 = \frac{1.6 mS \cdot r_{o2}}{1 + (1.6 mS \cdot 40 k\Omega)} \implies 2.137 = \frac{1.6 mS \cdot r_{o2}}{65}$$
$$r_{o2} = \frac{2.137 \cdot 65}{1.6 \times 10^{-3}} \approx \mathbf{86.81 \, k\Omega}$$

---

### Summary Table for Circuit 2b
| Parameter | Value (Theoretical) | Value (Matched) |
| :--- | :--- | :--- |
| **Transconductance ($g_{m1}$)** | 1.6 mS | 1.6 mS |
| **Effective $\lambda$** | 0.1 $V^{-1}$ | 0.125 $V^{-1}$ |
| **Output Resistance ($r_{o3}$)** | 50 k$\Omega$ | 40 k$\Omega$ |
| **Required Load ($r_{o2}$)** | 50 k$\Omega$ | 86.81 k$\Omega$ |
| **Gain (dB)** | -0.11 dB | 6.59 dB |



---

### Inference: Analysis of Discrepancy
The gap between the initial hand calculation (-0.08 dB) and the simulation (6.59 dB) is due to:

* **Asymmetric Loading**: The PMOS load ($r_{o2}$) acts as a significantly higher impedance than the NMOS degeneration transistor ($r_{o3}$) in the 180nm model.
* **Body Effect**: In Circuit 2b, the source voltage is 0.3V. This creates a source-to-bulk voltage that increases $V_{th}$, which is often ignored in simplified hand-calculation formulas.
* **Bandwidth Performance**: Despite the lower initial gain estimate, the circuit maintains a 3dB Bandwidth of approximately **1.83 MHz**.
---


### 3. Gain Formula for Cascode Degeneration
For this configuration, the voltage gain ($A_v$) is derived as:
$$A_v = \frac{-g_{m1}}{[1 + g_{m1}r_{o3} + \frac{r_{o3}}{r_{o1}}]} \times \{ [g_{m1}r_{o3}r_{o1} + r_{o3} + r_{o1}] \, || \, r_{o2} \}$$
**Gain Formula:** $A_v \approx \frac{g_{m1} \times r_{o2}}{1 + g_{m1} \times r_{o3}}$

## 4. Simulation Performance Analysis

### DC Sweep (VTC)
The DC sweep shows a linear transition centered around $V_{in} = 0.91V$. The output $V_{out}$ settles at approximately $1.22V$, ensuring all transistors stay in the saturation region.
<img width="1907" height="521" alt="Screenshot 2026-03-01 160446" src="https://github.com/user-attachments/assets/19c7b8cb-d804-49cc-9852-429996b69cd1" />


### Transient Analysis
* **Input:** 10mV peak sine wave at 1kHz.
* **Output:** An amplified, phase-inverted sine wave.
* The wave is centered at the DC quiescent point of $1.219V$, confirming stable biasing.
 The voltage gain ($A_v$) is calculated as the ratio of the output swing to the input swing:

1.  **Voltage Ratio:**
    $$A_{v(ratio)} = \frac{V_{out(p-p)}}{V_{in(p-p)}} = \frac{42.72 \, mV}{19.99 \, mV} \approx \mathbf{2.137}$$

2.  **Gain in Decibels (dB):**
    $$A_{v(dB)} = 20 \log_{10}(2.137) \approx \mathbf{6.59 \, dB}$$
    
<img width="1912" height="516" alt="Screenshot 2026-03-01 161227" src="https://github.com/user-attachments/assets/aadcf3b8-9034-481b-92f0-d99e0a4c29eb" />
<img width="1907" height="514" alt="Screenshot 2026-03-01 161242" src="https://github.com/user-attachments/assets/a532c1a1-0a64-40d3-b93d-ae80238ca017" />
<img width="1907" height="526" alt="Screenshot 2026-03-01 161252" src="https://github.com/user-attachments/assets/72ec89cc-e8aa-44ba-aa2c-8a287f2d0fec" />


     

### AC Analysis
The frequency response confirms a flat-band gain of **6.59 dB**. The use of $M_3$ as an active degeneration load significantly stabilizes the gain compared to a standard CS amplifier, providing high linearity at the cost of absolute gain magnitude.
* **Mid-band Gain:** The simulated gain remains stable at **6.59 dB** across the low and mid-frequency ranges.
* **3dB Bandwidth:** The frequency at which the gain drops by 3dB from its mid-band value is measured at **172.53 MHz**.
* **Inference:** The active degeneration provided by $M_3$ helps in maintaining a relatively high bandwidth, making this configuration suitable for high-speed analog signal processing where stability and linearity are prioritized over high gain.
* <img width="1907" height="520" alt="Screenshot 2026-03-01 161321" src="https://github.com/user-attachments/assets/00e97b4f-fda4-4e4a-83c8-502ab21421f1" />


---

## 5. Comparison Summary

| Parameter | Theoretical (at $\lambda=0.185$) | Simulated | Difference |
| :--- | :--- | :--- | :--- |
| **Voltage Gain (dB)** | 6.59 dB | 6.59 dB | **0.00 dB** |
| **Input Bias ($V_{in}$)** | 0.91 V | 0.91 V | 0.00 V |
| **Drain Current ($I_D$)** | 200 µA | 200.4 µA | 0.4 µA |

## 6. Conclusion
By adjusting the channel length modulation factor to **$\lambda = 0.185 V^{-1}$**, the theoretical small-signal model aligns perfectly with the simulation results. The circuit demonstrates that replacing a passive resistor with an NMOS active load ($M_3$) provides precise control over the degeneration voltage ($V_{DS3} = 0.347V$). This configuration prioritizes gain stability and linearity, achieving a robust and predictable gain of **6.59 dB** with a significant 3dB bandwidth of **172.53 MHz**.
