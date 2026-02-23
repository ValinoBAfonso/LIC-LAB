# LIC-LAB
LIC Lab Files
# Lab Report: Design and Analysis of a Common Source (CS) Amplifier

## 1. Introduction
The Common Source (CS) amplifier is a fundamental MOSFET amplifier topology providing high voltage gain and high input impedance. In this lab, we design a CS amplifier using the TSMC 180nm technology node, focusing on meeting specific power constraints while optimizing gain and bandwidth performance.

---

## 2. Theory
The CS amplifier utilizes a MOSFET with the source terminal grounded. The voltage gain ($A_v$) is primarily determined by the transconductance ($g_m$) and the drain resistance ($R_D$). 
* **Voltage Gain**: $A_v = -g_m (R_D \parallel r_o) \approx -g_m R_D$
* **Transconductance**: $g_m = \frac{2 I_D}{V_{ov}}$, where $V_{ov} = V_{GS} - V_{th}$
* **Phase**: The amplifier introduces a $180^\circ$ phase shift between input and output.
* <img width="1913" height="1109" alt="image" src="https://github.com/user-attachments/assets/e73155db-5e74-46e0-9612-020b60ea5281" />
<img width="1919" height="1081" alt="image" src="https://github.com/user-attachments/assets/8769c921-08c9-4dfe-8f82-8f0776ee9efb" />



---

## 3. Parameters
Based on the TSMC 180nm process specifications and design requirements:

| Parameter | Value |
| :--- | :--- |
| **Supply Voltage ($V_{DD}$)** | 1.8 V |
| **MOSFET Model** | CMOSN (180nm) |
| **Gate Bias ($V_{GS}$)** | 0.9 V |
| **Drain Resistance ($R_D$)** | 4.5 k$\Omega$ |
| **Load Capacitance ($C_L$)** | 10 pF |
| **Length ($L$)** | 180 nm |
| **Width ($W$)** | 1.54 $\mu$m |


---

## 4. DC Sweep and Analysis
The DC sweep was performed to determine the transition region and the proper biasing point for saturation.
* **Biasing Point**: $V_{GS}$ was set to 0.9 V to ensure the MOSFET operates in the saturation region.
* **Output DC Level**: The drain voltage ($V_{out}$) was measured at approximately 0.895 V.
* **Drain Current**: The simulated $I_D$ is $200.95 \mu\text{A}$.
* <img width="1919" height="1116" alt="image" src="https://github.com/user-attachments/assets/a5694314-26c9-421a-afe6-2edbea626613" />


---

## 5. AC Analysis and Frequency Response
The AC analysis shows the gain magnitude and phase across a wide frequency spectrum.
* **Mid-band Gain**: Measured at approximately **10.45 dB**.
* **-3dB Frequency ($f_H$)**: The high-frequency cutoff is **8.89 MHz**, primarily due to the 10pF load capacitance.
* **Phase**: Confirmed at $180^\circ$ in the mid-band region.
* <img width="1919" height="1173" alt="image" src="https://github.com/user-attachments/assets/bdd3c6e7-b965-46eb-a092-79ba4dda9ee0" />


---

## 6. Calculations

### A. Theoretical Design (Target $P \leq 1$mW)
* **Target Current**: $I_{D,max} = \frac{1\text{mW}}{1.8\text{V}} = 555.5 \mu\text{A}$.
* **Width for Target**: To reach this current, a larger width ($W \approx 4.04 \mu\text{m}$) would be required.

### B. Simulated Performance ($I_D \approx 200 \mu\text{A}$)
Using the values from the LTspice operating point:
* **Power Consumption**: $P = 1.8\text{V} \times 200.95 \mu\text{A} = \mathbf{0.36\text{ mW}}$.
* **Transconductance ($g_m$)**: $g_m = \frac{2 \times 200 \mu\text{A}}{0.9\text{V} - 0.36\text{V}} \approx \mathbf{0.74\text{ mA/V}}$.
* **Theoretical Gain ($A_v$)**: $A_v = 0.74\text{m} \times 4.5\text{k} = \mathbf{3.33}$.
* **Theoretical Gain (dB)**: $20 \log_{10}(3.33) \approx \mathbf{10.45\text{ dB}}$.

---

## 7. Inference
* **Phase Shift**: The output waveform is exactly $180^\circ$ out of phase with the input, verifying inverting amplification.
* **Design Validation**: The simulated gain of **10.45 dB** perfectly matches the theoretical calculation based on the $200 \mu\text{A}$ drain current.
* **Bandwidth**: The cutoff frequency ($f_H$) of **8.89 MHz** indicates the speed limitation of the circuit when driving a 10 pF load.
* **Efficiency**: The total power consumption of 0.36 mW is well below the 1 mW limit, leaving room for further gain optimization if needed.
