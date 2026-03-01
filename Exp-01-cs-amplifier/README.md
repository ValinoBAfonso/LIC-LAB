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

## 3. Calculations

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
* **3dB Frequency ($f_H$)**: Measured at **4.04 MHz**.
* **Phase**: Confirmed at $180^\circ$ in the mid-band region.
<img width="1915" height="1087" alt="Screenshot 2026-02-24 092742" src="https://github.com/user-attachments/assets/249d7753-ee1e-4192-ae6a-ae23351bbb88" />



---

## 6. Calculations
<img width="648" height="514" alt="Screenshot 2026-02-23 180928" src="https://github.com/user-attachments/assets/c14e45f7-7d21-4a0e-a58f-fe85d7ce3898" />

### A. Theoretical Design (Target $I_D = 200 \mu\text{A}$)
* **Target Current**: $I_{D} = 200 \mu\text{A}$.
* **Power Calculation**: $P = V_{DD} \times I_D = 1.8\text{V} \times 200 \mu\text{A} = \mathbf{0.36\text{ mW}}$.
* **Width for Target**: To reach this current at $L = 180\text{nm}$ with $V_{GS} = 0.9\text{V}$, the width was optimized to **$1.54 \mu\text{m}$**.

### B. Simulated Performance Analysis
Using the values extracted from the LTspice operating point:
* **Transconductance ($g_m$)**: 
  $$g_m = \frac{2 \times I_D}{V_{GS} - V_{th}} = \frac{2 \times 200 \mu\text{A}}{0.9\text{V} - 0.36\text{V}} \approx \mathbf{0.74\text{ mA/V}}$$
* **Theoretical Gain ($A_v$)**: 
  $$A_v = -g_m \times R_D = 0.74\text{mA/V} \times 4.5\text{k}\Omega = \mathbf{3.33}$$
* **Theoretical Gain (dB)**: 
  $$20 \log_{10}(3.33) \approx \mathbf{10.45\text{ dB}}$$

### C. Bandwidth Calculation
* **High Cutoff Frequency ($f_H$):** Measured as **$4.04\text{ MHz}$**.
* **Determinant factor**: The bandwidth is dominated by the output pole formed by the load:
  $$f_H \approx \frac{1}{2\pi \cdot R_D \cdot C_L} = \frac{1}{2\pi \cdot 4.5\text{ k}\Omega \cdot 10\text{ pF}} \approx \mathbf{3.53\text{ MHz}}$$


---

## 7. Extended Analysis

### A. MOSFET Operating Region Verification
To ensure the amplifier operates linearly, the saturation condition $V_{DS} \geq V_{GS} - V_{th}$ must be met.
* **Gate-Source Voltage ($V_{GS}$):** $0.9\text{ V}$
* **Threshold Voltage ($V_{th}$):** $\approx 0.36\text{ V}$
* **Drain-Source Voltage ($V_{DS}$):** $0.895\text{ V}$
* **Condition:** $0.895\text{ V} \geq 0.54\text{ V}$ (Condition Met).
* **Inference:** The transistor is biased in the **Saturation Region**, confirming valid small-signal amplification.

### B. Pole and Bandwidth Analysis
The frequency response is limited by the **10 pF** load capacitance.
* **Output Pole ($f_{P1}$):** Formed at the drain node by the parallel combination of $R_D$ and $C_L$.
* **Bandwidth Observation**: The gain remains flat until the pole at **4.04 MHz**, where the capacitive reactance of $C_L$ begins to shunt the output signal to ground.

---

## 8. Results and Analysis (Simulated Summary)

### A. DC Operating Point
* **Drain Current ($I_D$):** **$200.09 \mu\text{A}$**.
* **Output Voltage ($V_{out}$):** **$0.8957\text{ V}$**.

### B. Transient Analysis
* **Input Peak-to-Peak ($V_{in,p-p}$):** **$19.976 \text{ mV}$**.
* **Output Peak-to-Peak ($V_{out,p-p}$):** **$54.120 \text{ mV}$**.
* **Voltage Gain ($A_v$):** **$2.71$**.
* **Phase Relationship**: $180^\circ$ inversion confirmed.

### C. AC Analysis Summary
* **Mid-band Gain**: **$10.45\text{ dB}$**.
* **3dB Cutoff Frequency ($f_H$):** **$4.04\text{ MHz}$**.

---

# Lab Report: Common Source Amplifier Analysis ($L = 560\text{ nm}$)

---
<img width="365" height="307" alt="Screenshot 2026-02-24 083018" src="https://github.com/user-attachments/assets/ed654cfc-8487-49e4-b97c-720f444207b5" />

## 1. Parameters
The design is based on the following specifications for the $560\text{ nm}$ configuration:
* **Transistor Length ($L$):** $560\text{ nm}$
* **Transistor Width ($W$):** $4.52 \mu\text{m}$
* **Drain Resistance ($R_D$):** $4.5\text{ k}\Omega$
* **Load Capacitance ($C_L$):** $10\text{ pF}$
* **Supply Voltage ($V_{DD}$):** $1.8\text{ V}$

---

## 2. Procedure
1. **Schematic Entry:** Constructed the CS amplifier using a MOSFET with $L=560\text{nm}$ and $W=4.52\mu\text{m}$.
2. **DC Analysis:** Performed an Operating Point (.op) simulation to verify the drain current ($I_D$) and power consumption.
3. **DC Sweep:** Conducted a sweep of $V_{in}$ to verify the transistor remains in the saturation region ($V_{DS} > V_{GS} - V_{th}$).
4. **Transient Analysis:** Measured the input and output peak-to-peak voltages to calculate gain and verify the $180^\circ$ phase shift.
5. **AC Analysis:** Performed a frequency response sweep to identify the mid-band gain and the $12.13\text{ MHz}$ 3dB cutoff frequency.

---

## 3. Calculations and Analysis

### A. Theoretical Design (Target $I_D = 555.5 \mu\text{A}$)
Based on the $1\text{mW}$ power limit constraint:
* **Max Current:** $I_{D,max} = \frac{1\text{mW}}{1.8\text{V}} = \mathbf{555.5 \mu A}$.

### B. Simulated Performance ($I_D \approx 200 \mu\text{A}$)
<img width="639" height="504" alt="Screenshot 2026-02-24 090702" src="https://github.com/user-attachments/assets/6cbf107c-9a95-40ed-a4f1-fbffce3031f6" />

Using the results from 
the LTspice simulation:
* **Drain Current ($I_D$):** **$200.09 \mu\text{A}$**.
* **Power Consumption:** $P = 1.8\text{V} \times 200.09 \mu\text{A} = \mathbf{0.36\text{ mW}}$.
* **Transconductance ($g_m$):** $$g_m = \frac{2 \times I_D}{V_{GS} - V_{th}} = \frac{2 \times 200.09 \mu\text{A}}{0.9\text{V} - 0.36\text{V}} \approx \mathbf{0.74\text{ mA/V}}$$

### C. Transient Gain Analysis
Using the small-signal model parameters for the $200\mu\text{A}$ bias:
* **Transconductance ($g_m$):**
  $$g_m = \frac{2 \cdot I_D}{V_{GS} - V_{th}} = \frac{2 \cdot 200.09\mu\text{A}}{0.9\text{V} - 0.36\text{V}} = \mathbf{0.74\text{ mA/V}}$$.
* **Theoretical Gain ($A_v$):**
  $$A_v = -g_m \cdot R_D = 0.74\text{m} \cdot 4.5\text{k}\Omega = \mathbf{3.33}$$.
* **Theoretical Gain in dB:**
  $$A_{v,dB} = 20 \log_{10}(3.33) = \mathbf{10.45\text{ dB}}$$.
  
Using the measured peak-to-peak values from the transient simulation:
* **Output Voltage ($V_{out,p-p}$):** $66.401 \text{ mV}$
* **Input Voltage ($V_{in,p-p}$):** $19.997 \text{ mV}$
* **Voltage Gain ($A_v$):** $\frac{66.401 \text{ mV}}{19.997 \text{ mV}} = \mathbf{3.32}$
* **Gain in dB:** $20 \log_{10}(3.32) = \mathbf{10.42 \text{ dB}}$
<img width="1913" height="1091" alt="Screenshot 2026-02-24 082942" src="https://github.com/user-attachments/assets/67e52342-8a39-48b3-b05f-a74fa0b04d0c" />

---<img width="1919" height="1083" alt="Screenshot 2026-02-24 082957" src="https://github.com/user-attachments/assets/5da09f93-506a-4d51-840f-a06f6335939f" />


## 4. AC Analysis and Frequency Response
The frequency response was analyzed to determine the bandwidth.

* **3dB Cutoff Frequency ($f_H$):** Measured at **3.63 MHz**.
* **Phase:** A $180^\circ$ phase shift was confirmed in the mid-band region, verifying the inverting nature of the amplifier.
<img width="1919" height="1098" alt="Screenshot 2026-02-24 090112" src="https://github.com/user-attachments/assets/2edadced-8764-4d36-882c-877990f22ac6" />
A. DC Sweep


<img width="1903" height="558" alt="Screenshot 2026-02-24 094331" src="https://github.com/user-attachments/assets/f7b378fb-f938-4d07-bd81-81fa2389b73a" />




---

## 5. Results Summary ($L = 560\text{ nm}$)

| Parameter | Measured/Simulated Value |
| :--- | :--- |
| **Drain Current ($I_D$)** | $200.09 \mu\text{A}$ |
| **Power Consumption** | $0.36\text{ mW}$ |
| **Transient Voltage Gain ($A_v$)** | $3.32$ |
| **3dB Bandwidth ($f_H$)** | $3.63\text{ MHz}$ |
| **Phase Shift** | $180^\circ$ |

---

## 6. Inference

The design using **$L = 560\text{ nm}$** and **$W = 4.52\mu\text{m}$** results in a highly efficient amplifier consuming only **$0.36\text{ mW}$**. The transient gain of **$3.32$** closely matches the theoretical expectations. The measured bandwidth of **$12.13\text{ MHz}$** indicates the high-speed capability of the circuit when driving a $10\text{pF}$ load. The $180^\circ$ phase shift confirms the circuit is operating as a standard inverting Common Source stage.

### Design Parameters (180 nm Process)

- **Technology Node:** 180 nm
* **Phase Shift**: The output waveform is exactly $180^\circ$ out of phase with the input, verifying inverting amplification.
* **Design Validation**: The simulated gain of **10.45 dB** perfectly matches the theoretical calculation based on the $200 \mu\text{A}$ drain current.
* **Bandwidth**: The cutoff frequency ($f_H$) of **8.89 MHz** indicates the speed limitation of the circuit when driving a 10 pF load.
* **Efficiency**: The total power consumption of 0.36 mW is well below the 1 mW limit, leaving room for further gain optimization if needed.
