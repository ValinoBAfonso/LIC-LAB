# Design and Analysis of a CS Amplifier with Active Load and Source Degeneration

This repository contains the design, theoretical calculations, and simulation results for a **Common Source (CS) Amplifier** using an NMOS driver with source degeneration and a PMOS active load, implemented in **180nm CMOS technology**.

---

## 1. Design Specifications and Assumptions
Based on the design requirements, the following parameters and constraints were used for the implementation:

### Given Specifications
* **Technology Node:** 180nm
* **Supply Voltage ($V_{DD}$):** 1.8V
* **Power Dissipation ($P$):** $\leq 1mW$
* **Load Capacitance ($C_L$):** 10pF
* **NMOS Channel Length ($L_n$):** 180nm
* <img width="1231" height="681" alt="Screenshot 2026-03-01 153940" src="https://github.com/user-attachments/assets/ed48dbc1-86b6-4d16-9128-658a4d413801" />


### Key Assumptions
* **Overdrive Voltage ($V_{OV}$):** Assumed to be 0.25V for both transistors.
* **Threshold Voltages:** $V_{thn} = 0.36V$ and $V_{thp} = -0.39V$.
* **Drain Current ($I_D$):** Derived from power dissipation ($P = V_{DD} \times I_D$). With $1mW / 1.8V \approx 555.5\mu A$, a set value of **$200\mu A$** was chosen to ensure the power constraint is comfortably met.
* **Source Resistor Voltage Drop ($V_{RS}$):** Assumed to be 0.2V to provide sufficient degeneration while maintaining headroom.

---

## 2. Theory and Step-by-Step Calculations

### A. NMOS Driver ($M_1$) Design
1.  **Gate-Source Voltage ($V_{GS1}$):**
    $$V_{GS1} = V_{OV} + V_{thn} = 0.25V + 0.36V = 0.61V$$
2.  **Output Bias Voltage ($V_{out}$):**
    $$V_{out} = \frac{V_{DD} + V_{RS}}{2} = \frac{0.9 + 0.2}{1} = 1.1V$$
3.  **Source Resistor ($R_s$):**
    $$R_s = \frac{V_{RS}}{I_D} = \frac{0.2V}{200\mu A} = 1000\Omega$$
4.  **Input Bias Voltage ($V_{G1}$):**
    $$V_{G1} = V_{GS1} + V_{RS} = 0.61V + 0.2V = 0.81V$$
5.  **Saturation Check ($V_{DS} \geq V_{GS} - V_{th}$):**
    $$V_{DS1} \geq 0.61V - 0.36V \rightarrow V_{DS1} \geq 0.25V$$

### B. PMOS Active Load ($M_2$) Design
1.  **Source-Gate Voltage ($V_{SG2}$):**
    $$V_{SG2} = V_{OV} + |V_{thp}| = 0.25V + 0.39V = 0.64V$$
2.  **PMOS Gate Voltage ($V_{G2}$):**
    $$V_{G2} = V_{DD} - V_{SG2} = 1.8V - 0.64V = 1.16V$$

###  Theoretical Gain Calculation
Using the simplified small-signal model where $g_{m1} = \frac{2I_D}{V_{OV}} = 1.11 \times 10^{-3} S$ and $r_{out} = 25k\Omega$:
$$A_v = \frac{-g_{m1} \times r_{out}}{1 + g_{m1}R_s} = \frac{-27.75}{1 + (1.11 \times 10^{-3} \times 1000)} \approx 13.15$$
**In decibels:** $20 \log_{10}(13.15) = \mathbf{22.37 \, dB}$

#### Case 1: Considering Channel Length Modulation ($\lambda \neq 0$)
The exact formula used for design is:
$$A_v = \frac{-g_{m1}}{[1 + g_{m1}R_s + R_s/r_{o1}]} \times \{ [g_{m1}R_s r_{o1} + R_s + r_{o1}] \, || \, r_{o2} \}$$

#### Case 2: Simplified Formula ($\lambda_1 = 0, \lambda_2 \neq 0$)
When internal resistance $r_{o1}$ is very high, the formula simplifies to:
$$A_v = \frac{-g_{m1} \times r_{o2}}{1 + g_{m1}R_s}$$


### C. Simulated Gain Calculation (From LTspice)
### Transient Analysis
Transient simulation shows an inverted output sine wave, consistent with the $180^\circ$ phase shift of a Common Source stage
The simulated gain is derived from the AC analysis output magnitudes:
* **Output Voltage Magnitude ($V_{out}$):** 246.399
* **Input Voltage Magnitude ($V_{in}$):** 19.99
* **Simulated Voltage Ratio:** $A_{v(sim)} = \frac{246.399}{19.99} \approx 12.326$
* **Simulated Gain (dB):** $20 \log_{10}(12.326) = \mathbf{21.82 \, dB}$
* **3dB Frequency:** The bandwidth cutoff frequency is **275.829 MHz**.
* <img width="1909" height="523" alt="Screenshot 2026-03-01 154604" src="https://github.com/user-attachments/assets/d66b95b9-f130-4611-8b42-4b7b5fd84c36" />
<img width="1915" height="518" alt="Screenshot 2026-03-01 154618" src="https://github.com/user-attachments/assets/a972f305-f742-4b90-991f-fc110242a2e0" />
<img width="1909" height="520" alt="Screenshot 2026-03-01 154635" src="https://github.com/user-attachments/assets/59f1317d-cef5-4cc2-a212-b140446fb7c9" />

---

## 3. Simulation Results

### DC Analysis & Sweep
* **Operating Point:** $V_{in} = 0.81V$, $V_{out} = 1.118V$, $I_D = 199.8\mu A$.
### DC Sweep Analysis
The DC sweep (VTC) confirms that the output bias point is correctly set at approximately 1.1V. The sharp slope in the transition region indicates high open-loop gain while the transistors are in saturation.

<img width="1909" height="520" alt="Screenshot 2026-03-01 154359" src="https://github.com/user-attachments/assets/fbb79362-d1f0-47b4-bdfe-38a682a9707c" />



### AC Analysis (Frequency Response)
* **Simulated Gain:** $20 \log_{10} \left( \frac{246.399}{19.99} \right) \approx \mathbf{21.82 \, dB}$.
* **3dB Bandwidth:** The frequency cutoff occurs at **$275.829 \, MHz$**.

* <img width="1909" height="516" alt="Screenshot 2026-03-01 154538" src="https://github.com/user-attachments/assets/7fdf49f6-a826-4188-b834-58081b0680bb" />


---

## 4. Inference and Conclusion

### Comparison Table
| Parameter | Theoretical | Simulated | Difference |
| :--- | :--- | :--- | :--- |
| **Voltage Gain (dB)** | 22.37 dB | 21.82 dB | **0.55 dB** |
| **Drain Current ($I_D$)** | 200 µA | 199.8 µA | 0.2 µA |

## 5. Difference Between Simulated and Calculated Gain

There is a discrepancy of **0.55 dB** between the theoretical ($22.37\text{ dB}$) and simulated ($21.82\text{ dB}$) results. This is attributed to the following:

* **Body Effect ($g_{mb}$):** Theoretical formulas often ignore the body effect because the source is usually grounded. However, with $R_s$ present, the source is at $0.2V$, increasing $V_{SB}$ and shifting the threshold voltage, which slightly reduces the effective $g_m$ and gain.
* **Channel Length Modulation ($\lambda$):** Hand calculations assumed a fixed $\lambda = 0.1 V^{-1}$. The `tsmc018.lib` model uses a more complex, non-linear model for $r_o$ that varies with $V_{DS}$, leading to a slightly lower output impedance in simulation.
* **Red Pen Reference (Effect of $R_s$):**
    * **Without $R_s$:** The gain would be significantly higher ($A_v = -g_m \times r_{out} \approx 27.75$ or $28.8\text{ dB}$).
    * **With $R_s$:** The denominator $(1 + g_m R_s)$ acts as a feedback factor that reduces the gain but makes it much more stable against transistor parameter variations.

---

### Key Observations
* **Gain Discrepancy:** The minor **0.55 dB** difference is attributed to the body effect and precise channel length modulation parameters in the `tsmc018.lib` model not captured in hand calculations.
* **Degeneration Benefits:** $R_s$ stabilizes the gain against process variations and improves the input linear range.
* **Saturation:** Both transistors remained in the saturation region during the signal swing, as verified by $V_{DS1} \geq 0.25V$.

## 5. Summary
The design successfully meets the power constraint ($P < 1mW$) and technology specifications. The alignment between the manual design calculations (0.81V and 1.16V bias) and the simulated performance confirms a robust design for high-speed analog signal amplification.
