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
<img width="1919" height="1089" alt="image" src="https://github.com/user-attachments/assets/a5807e11-7534-4d07-a80d-9c249922a6a7" />
<img width="1919" height="1065" alt="image" src="https://github.com/user-attachments/assets/f848e96c-8667-4125-afc8-006800125528" />
## Calculations

## 6. Calculations

### A. Theoretical Hand Calculation (Design Target)
Based on the lab requirements for a $1\text{mW}$ power limit:
* **Max Drain Current ($I_D$):** $I_{D,max} = \frac{1\text{mW}}{1.8\text{V}} = \mathbf{555.5 \mu A}$.
* **Width for Target current:** $W = 4.04 \mu\text{m}$ (Theoretical value for $1\text{mW}$ limit).

### B. DC Analysis (Simulated $200\mu\text{A}$ Operating Point)
Based on the LTspice Operating Point results for your chosen width $W = 1.54 \mu\text{m}$:
* **Drain Current ($I_D$):** $\mathbf{200.09 \mu A}$.
* **Drain Voltage ($V_{out}$):** $0.8957\text{ V}$.
* **Actual Power Consumption:** $P = 1.8\text{V} \times 200.09\mu\text{A} = \mathbf{0.36\text{ mW}}$ (Within $1\text{mW}$ limit).

### C. Gain Calculation ($200\mu\text{A}$ Bias)
Using the small-signal model parameters for the $200\mu\text{A}$ bias:
* **Transconductance ($g_m$):**
  $$g_m = \frac{2 \cdot I_D}{V_{GS} - V_{th}} = \frac{2 \cdot 200.09\mu\text{A}}{0.9\text{V} - 0.36\text{V}} = \mathbf{0.74\text{ mA/V}}$$.
* **Theoretical Gain ($A_v$):**
  $$A_v = -g_m \cdot R_D = 0.74\text{m} \cdot 4.5\text{k}\Omega = \mathbf{3.33}$$.
* **Theoretical Gain in dB:**
  $$A_{v,dB} = 20 \log_{10}(3.33) = \mathbf{10.45\text{ dB}}$$.

### D. Simulated Gain (Transient Measurements)
Extracted from the input/output waveforms:
* **Input Peak-to-Peak ($V_{in,pp}$):** $909.99\text{mV} - 890.02\text{mV} = \mathbf{19.97\text{ mV}}$.
* **Output Peak-to-Peak ($V_{out,pp}$):** $922.79\text{mV} - 868.67\text{mV} = \mathbf{54.12\text{ mV}}$.
* **Measured Gain ($A_v$):** $\frac{54.12\text{mV}}{19.97\text{mV}} = \mathbf{2.71}$
* **Measured Gain in dB:** $20 \log_{10}(2.71) = \mathbf{8.66\text{ dB}}$

### E. AC Analysis and Bandwidth
From the frequency response plot:
* **Mid-band AC Gain:** **$10.45\text{ dB}$**.
* **High Cutoff Frequency ($f_H$):** Measured at **$8.89\text{ MHz}$**.




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


<img width="1919" height="1091" alt="image" src="https://github.com/user-attachments/assets/0f52205c-f7b6-456b-9770-0aae6d174855" />

  

### C. Bandwidth Calculation
* **High Cutoff Frequency ($f_H$):** Measured as **$12.13\text{ MHz}$**.
* **Determinant factor:** The bandwidth is limited by the output pole formed by $R_D$ and $C_L$:
  $$f_H \approx \frac{1}{2\pi \cdot R_D \cdot C_L} = \frac{1}{2\pi \cdot 4.5\text{ k}\Omega \cdot 10\text{ pF}} \approx \mathbf{3.53\text{ MHz}}$$
* *Note: The simulated $f_H$ is higher than the simple $RC$ calculation due to internal MOSFET capacitances and higher-order effects in the 180nm model.*

* ---

## 7. Extended Analysis

### A. MOSFET Operating Region Verification
To ensure the amplifier operates linearly, we must verify the saturation condition: $V_{DS} \geq V_{GS} - V_{th}$.
* **Gate-Source Voltage ($V_{GS}$):** $0.9\text{ V}$
* **Threshold Voltage ($V_{th}$):** $\approx 0.36\text{ V}$ (Extracted from model)
* **Drain-Source Voltage ($V_{DS}$):** $0.895\text{ V}$
* **Condition:** $0.895\text{ V} \geq (0.9\text{ V} - 0.36\text{ V}) \Rightarrow 0.895\text{ V} \geq 0.54\text{ V}$.
* **Inference:** The transistor is safely in the **Saturation Region**, confirming valid amplification.

### B. Pole and Bandwidth Analysis
The high-frequency response is dominated by the output pole ($P_1$).
* **Output Pole ($f_{P1}$):** $f_{P1} = \frac{1}{2\pi R_D C_L}$
* **Effect of Load:** With $C_L = 10\text{ pF}$, the bandwidth is significantly lower than an unloaded stage, which explains the $8.89\text{ MHz}$ cutoff.


---
## c. Results and Analysis (Simulated)
<img width="632" height="509" alt="image" src="https://github.com/user-attachments/assets/429540fb-080d-4cb9-aa8b-f940627d6113" />

### A. DC Operating Point Analysis
Based on the LTspice Operating Point simulation:
* **Drain Current ($I_D$):** **$200.09 \mu\text{A}$**.
* **Output Node Voltage ($V_{out}$):** **$0.8957\text{ V}$**.
* **Supply Voltage ($V_{DD}$):** $1.8\text{ V}$.
* **Input Gate Bias ($V_{GS}$):** $0.9\text{ V}$.

### B. Transient Analysis (Time Domain)
The transient response was measured to determine the practical voltage gain:
* **Input Peak-to-Peak ($V_{in,p-p}$):** Measured as **$19.976 \text{ mV}$** ($909.99\text{mV} - 890.02\text{mV}$).
* **Output Peak-to-Peak ($V_{out,p-p}$):** Measured as **$54.120 \text{ mV}$** ($922.79\text{mV} - 868.67\text{mV}$).
* **Voltage Gain ($A_v$):** $\frac{54.120 \text{ mV}}{19.976 \text{ mV}} \approx \mathbf{2.71}$.
* **Phase Relationship:** The output waveform is $180^\circ$ out of phase with the input, confirming the inverting nature of the Common Source stage.


### C. AC Analysis (Frequency Domain)
The frequency response was swept from $0.1\text{Hz}$ to $100\text{GHz}$:
* **Mid-band Gain:** **$10.45\text{ dB}$**.
* **3dB Cutoff Frequency ($f_H$):** Measured at **$8.89\text{ MHz}$**.
* **Bandwidth Limitation:** The sharp roll-off at high frequencies is due to the interaction of the **$4.5\text{ k}\Omega$** drain resistor and the **$10\text{ pF}$** load capacitor.

---

## D. Summary of Performance
| Parameter | Measured Value |
| :--- | :--- |
| **Drain Current ($I_D$)** | $200.09 \mu\text{A}$ |
| **Power Consumption** | $0.36\text{ mW}$ |
| **AC Mid-band Gain** | $10.45\text{ dB}$ |
| **Bandwidth ($f_H$)** | $8.89\text{ MHz}$ |



## 8. Conclusion and Precautions
* **Conclusion:** The design successfully meets the power constraint of $< 1\text{ mW}$. While the current was lower than the theoretical maximum, the gain of $10.45\text{ dB}$ provides a stable and predictable output for the given $180\text{nm}$ process.
* **Precautions:**
    1. Ensure the `.lib` path for `tsmc018.lib` is correct to avoid simulation errors.
    2. Maintain $V_{DS} > V_{GS} - V_{th}$ to prevent the signal from clipping or entering the triode region.
    3. Use small-signal input amplitudes (e.g., $10\text{ mV}$) to avoid non-linear distortion.
---

## 9. Inference
* **Phase Shift**: The output waveform is exactly $180^\circ$ out of phase with the input, verifying inverting amplification.
* **Design Validation**: The simulated gain of **10.45 dB** perfectly matches the theoretical calculation based on the $200 \mu\text{A}$ drain current.
* **Bandwidth**: The cutoff frequency ($f_H$) of **8.89 MHz** indicates the speed limitation of the circuit when driving a 10 pF load.
* **Efficiency**: The total power consumption of 0.36 mW is well below the 1 mW limit, leaving room for further gain optimization if needed.
