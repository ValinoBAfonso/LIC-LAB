# NMOS Differential Pair Design Analysis
**Process Technology:** TSMC 0.18µm CMOS

---

## 1. Circuit Overview & Architectural Logic
This circuit is a **Source-Coupled NMOS Differential Pair**. It serves as the fundamental building block of an operational amplifier, designed to amplify the voltage difference between two nodes while rejecting **Common Mode** noise (signals appearing on both inputs simultaneously).
<img width="1012" height="762" alt="image" src="https://github.com/user-attachments/assets/df340fe0-bfea-4056-aedb-4046eb313859" />


### Design Parameters & MOSFET Dimensions
To meet performance requirements and stay within the power budget, the following dimensions were selected:

* **Channel Length ($L$):** $0.48\mu\text{m}$
    * *Reasoning:* While the technology allows for a minimum of $0.18\mu\text{m}$, a longer channel reduces the **Channel Length Modulation** ($\lambda$) effect. This significantly increases the output resistance ($r_o$), leading to more stable gain and precise DC bias.
* **Channel Width ($W$):** $29.3\mu\text{m}$
    * *Reasoning:* Calculated to handle a drain current of $0.5\text{mA}$ per branch while maintaining an **Overdrive Voltage** ($V_{OV}$) of approximately $0.3\text{V}$, balancing high speed with sufficient signal swing.

---

## 2. Operating Point Analysis (The Q-Point)
The Operating Point defines the static DC state where transistors are biased in the **Saturation Region** ($V_{DS} > V_{GS} - V_{th}$), allowing them to act as controlled current sources.
# Operating Point (OP) & Small-Signal Parameter Validation
<img width="386" height="407" alt="image" src="https://github.com/user-attachments/assets/3fc35e39-e063-4fb8-a901-a084f89e341c" />

## 1. Node Voltage and Operating State
Based on the simulation results, we analyze the DC bias conditions:

* **Source Node ($V_p$):** The simulation shows $V(vp) = -0.700789\text{V}$. Since the inputs ($V_2$ and $V_3$) are at $0\text{V}$ DC, the Gate-Source voltage ($V_{GS}$) for both transistors is $0 - (-0.7) = \mathbf{0.7\text{V}}$.
* **Drain Nodes ($V_{out1}, V_{out2}$):** The data shows these nodes are at $6.99 \times 10^{-17}\text{V}$ (effectively $0\text{V}$).

### Saturation Region Check
For $M_1$ and $M_2$ to operate in saturation, the condition $V_{DS} \ge V_{GS} - V_{th}$ must be met:
* $V_{DS} = 0 - (-0.7) = \mathbf{0.7\text{V}}$
* $V_{GS} - V_{th} = 0.7 - 0.4 = \mathbf{0.3\text{V}}$

**Result:** Since $0.7\text{V} > 0.3\text{V}$, both transistors are correctly biased in the **Saturation Region**.

---

## 2. Current Distribution
* **Tail Current ($I_{I1}$):** The current source is verified at $1\text{mA}$.
* **Branch Currents ($I_D$):** The data shows $Id(M1) = 0.0005\text{A}$ and $Id(M2) = 0.0005\text{A}$.

**Interpretation:** The $1\text{mA}$ total current is splitting perfectly into $0.5\text{mA}$ per branch. This confirms the circuit is balanced and symmetric.

---

## 3. Small-Signal Parameters (Derived from OP)
Using the verified DC values from the operating point table:

* **Overdrive ($V_{ov}$):** $V_{GS} - V_{th} = 0.7\text{V} - 0.4\text{V} = \mathbf{0.3\text{V}}$
* **Transconductance ($g_m$):**
    $$g_m = \frac{2I_D}{V_{ov}} = \frac{2 \times 0.5\text{mA}}{0.3\text{V}} = \mathbf{3.33\text{mS}}$$

---

## 4. Voltage Gain ($A_v$)
Based on the schematic's load resistors ($R_1, R_2 = 1.8\text{k}\Omega$):

* **Formula:** $A_v = -g_m \times R_D$
* **Calculation:** $-3.33\text{mS} \times 1.8\text{k}\Omega = \mathbf{-5.99}$
* **Gain in dB:** $20 \log_{10}(5.99) \approx \mathbf{15.55\text{dB}}$

---

## 5. Power Dissipation
* **Total Current:** $1\text{mA}$
* **Total Voltage Supply ($V_{DD} - V_{SS}$):** $0.9\text{V} - (-0.9\text{V}) = 1.8\text{V}$
* **Power:** $1.8\text{V} \times 1\text{mA} = \mathbf{1.8\text{mW}}$

**Conclusion:** This confirms you are meeting your **$P \le 2\text{mW}$** constraint while maintaining the targeted gain and biasing.

### A. Current & Power Calculations
* **Tail Current ($I_{SS}$):** Fixed at $1\text{mA}$ by the tail current source.
* **Branch Current ($I_D$):** With a symmetric circuit ($V_{G1} = V_{G2} = 0\text{V}$), the current splits equally:
    $$I_D = \frac{I_{SS}}{2} = \frac{1\text{mA}}{2} = \mathbf{0.5\text{mA}}$$
* **Power Consumption ($P$):** $$P = (V_{DD} - V_{SS}) \times I_{SS} = (0.9\text{V} - (-0.9\text{V})) \times 1\text{mA} = \mathbf{1.8\text{mW}}$$
    > **Status:** Satisfies the $P \le 2\text{mW}$ design constraint.

### B. Resistor Calculation (Fixing the Output)
To maximize output swing, the output DC voltage ($V_{out}$) is set to $0\text{V}$. Using Ohm’s Law to calculate the required load resistance $R_D$:

$$V_{out} = V_{DD} - (I_D \times R_D)$$
$$0\text{V} = 0.9\text{V} - (0.5\text{mA} \times R_D) \implies R_D = \frac{0.9\text{V}}{0.5\text{mA}} = \mathbf{1.8\text{k}\Omega}$$

---

## 3. Transconductance ($g_m$) & Threshold ($V_{th}$) Comparison
The transconductance ($g_m$) defines the efficiency of the MOSFET in converting input voltage to output current.

| Parameter | Formula / Source | Theoretical ($V_{th}=0.4\text{V}$) | TSMC Simulated ($V_{th}=0.36\text{V}$) |
| :--- | :--- | :--- | :--- |
| **Threshold ($V_{th}$)** | Process Model | $0.4\text{V}$ (Ideal) | $0.36\text{V}$ (BSIM Model) |
| **Overdrive ($V_{OV}$)** | $V_{GS} - V_{th}$ | $0.3\text{V}$ | $0.3\text{V}$ |
| **Gate-Source ($V_{GS}$)** | $V_{th} + V_{OV}$ | $0.7\text{V}$ | $0.66\text{V}$ |
| **$g_m$ Value** | $2 \cdot I_D / V_{OV}$ | $3.33\text{mS}$ | $3.45\text{mS}$ |


# Differential Pair Voltage Swing & Compliance Analysis
**Objective:** To determine the boundary conditions for Input and Output Common-Mode ranges ($V_{ICM}$ and $V_{OCM}$) to ensure transistors remain in the **Saturation Region**.

---

## 1. Analysis of $V_{ICM, min}$ (Input Common-Mode Minimum)
The minimum input voltage is dictated by the **Tail Current Source**. It must maintain enough voltage across it ($V_{DS,sat} \approx 0.2\text{V}$) to stay in saturation and sustain the $1\text{mA}$ current.

### Mathematical Logic
The voltage at the shared source node ($V_p$) is: 
$$V_p = V_{ICM} - V_{GS}$$

To keep the current source active: 
$$V_p \ge V_{SS} + V_{DS,sat}$$

Therefore: 
$$V_{ICM, min} = V_{SS} + V_{DS,sat} + V_{GS}$$

### Calculations
* **Case A ($V_{th} = 0.4\text{V}$):**
    * $V_{GS} = V_{th} + V_{OV} = 0.4\text{V} + 0.3\text{V} = 0.7\text{V}$
    * $V_{ICM, min} = -0.9\text{V} + 0.2\text{V} + 0.7\text{V} = \mathbf{0\text{V}}$
* **Case B ($V_{th} = 0.36\text{V}$):**
    * $V_{GS} = V_{th} + V_{OV} = 0.36\text{V} + 0.3\text{V} = 0.66\text{V}$
    * $V_{ICM, min} = -0.9\text{V} + 0.2\text{V} + 0.66\text{V} = \mathbf{-0.04\text{V}}$

> **Observation:** A lower $V_{th}$ provides slightly more "downward" headroom for the input signal.

---

## 2. Analysis of $V_{ICM, max}$ (Input Common-Mode Maximum)
The maximum input is limited by the **Differential Pair ($M_1, M_2$)**. If the input $V_G$ goes too high, the Drain-to-Gate voltage drops below $V_{th}$, pushing the transistors into the **Triode Region**.

### Mathematical Logic
To stay in saturation: 
$$V_{DS} \ge V_{GS} - V_{th}$$

Simplified for input voltage: 
$$V_{ICM, max} = V_{out, DC} + V_{th}$$

### Calculations
* **Case A ($V_{th} = 0.4\text{V}$):**
    * $V_{ICM, max} = 0\text{V} + 0.4\text{V} = \mathbf{0.4\text{V}}$
* **Case B ($V_{th} = 0.36\text{V}$):**
    * $V_{ICM, max} = 0\text{V} + 0.36\text{V} = \mathbf{0.36\text{V}}$

> **Observation:** A lower $V_{th}$ reduces "upward" headroom. The transistors will "Clip Out" into Triode faster if $V_{th}$ is $0.36\text{V}$.

---

## 3. Analysis of $V_{OCM, min}$ (Output Common-Mode Minimum)
This defines how low the output signal can swing before the transistors fail to remain in saturation.

### Mathematical Logic
The output node is the Drain ($V_D$). For saturation: 
$$V_D \ge V_G - V_{th}$$

Therefore: 
$$V_{OCM, min} = V_{ICM} - V_{th}$$

### Calculations (Assuming $V_{ICM} = 0\text{V}$)
* **Case A ($V_{th} = 0.4\text{V}$):**
    * $V_{OCM, min} = 0\text{V} - 0.4\text{V} = \mathbf{-0.4\text{V}}$
* **Case B ($V_{th} = 0.36\text{V}$):**
    * $V_{OCM, min} = 0\text{V} - 0.36\text{V} = \mathbf{-0.36\text{V}}$

---



| Parameter | Formula | Result ($V_{th}=0.4\text{V}$) | Result ($V_{th}=0.36\text{V}$) |
| :--- | :--- | :--- | :--- |
| **Input Min ($V_{ICM,min}$)** | $V_{SS} + V_{DS,sat} + V_{GS}$ | $0\text{V}$ | $-0.04\text{V}$ |
| **Input Max ($V_{ICM,max}$)** | $V_{out,DC} + V_{th}$ | $0.4\text{V}$ | $0.36\text{V}$ |
| **Output Min ($V_{OCM,min}$)** | $V_{ICM} - V_{th}$ | $-0.4\text{V}$ | $-0.36\text{V}$ |
| **Output Max ($V_{OCM,max}$)** | $V_{DD}$ | $+0.9\text{V}$ | $+0.9\text{V}$ |

## Differential Input Swing & Linear Range Analysis

### 1. The Theoretical Concept
The linear operation limit of the differential pair is defined by:
$$-\sqrt{2}V_{OV} \le V_{id} \le \sqrt{2}V_{OV}$$

### 2. Calculations
* **Case 1 ($V_{th} = 0.4\text{V}$):** $V_{id, range} = \pm \sqrt{2} \times 0.3\text{V} = \pm \mathbf{424\text{mV}}$
* **Case 2 ($V_{th} = 0.36\text{V}$):** $V_{id, range} = \pm \sqrt{2} \times 0.29\text{V} = \pm \mathbf{410\text{mV}}$

### 3. Summary Table
| Parameter | $V_{th} = 0.4\text{V}$ | $V_{th} = 0.36\text{V}$ |
| :--- | :--- | :--- |
| **Overdrive ($V_{OV}$)** | $0.3\text{V}$ | $0.29\text{V}$ |
| **Linear $V_{id}$ Range** | $\pm 424\text{mV}$ | $\pm 410\text{mV}$ |

## Gain using Small Signal Analysis: Theoretical Calculation ($V_{th} = 0.4\text{V}$)

### 1. Transconductance Calculation
$$V_{OV} = V_{GS} - V_{th} = 0.7\text{V} - 0.4\text{V} = 0.3\text{V}$$
$$g_m = \frac{2 \cdot I_D}{V_{OV}} = \frac{1\text{mA}}{0.3\text{V}} = 3.33\text{mS}$$

### 2. Voltage Gain ($A_v$)
$$A_v = -g_m \cdot R_D = -(3.33\text{mS} \cdot 1.8\text{k}\Omega) = -5.99$$
$$A_{v,dB} = 20 \cdot \log_{10}(5.99) = \mathbf{15.55\text{dB}}$$

### 3. Summary Table
| Parameter | Value |
| :--- | :--- |
| Transconductance ($g_m$) | 3.33mS |
| Linear Gain ($A_v$) | 5.99 |
| Gain in dB | 15.55dB |

##Gain Analysis ($V_{th} = 0.36\text{V}$)

### 1.Parameters
* **Overdrive:** $V_{OV} = 0.66\text{V} - 0.36\text{V} = 0.3\text{V}$
* **Transconductance ($g_m$):** Calculated as $3.33\text{mS}$, simulated at **$3.45\text{mS}$** due to BSIM modeling.

### 2. Gain Results
* **Linear Gain:** $A_v = -(3.45\text{mS} \cdot 1.8\text{k}\Omega) = -6.21$
* **Gain in dB:** $A_{v,dB} = 20 \cdot \log_{10}(6.21) = \mathbf{15.86\text{dB}}$

### 3. Comparison Summary
| Metric | Theoretical | Simulated |
| :--- | :--- | :--- |
| $g_m$ | 3.33mS | 3.45mS |
| Gain (dB) | 15.55dB | 15.86dB |


# Discussion & Conclusions: Theoretical vs. Physical Reality

## 1. Theoretical Baseline vs. Physical Reality
* **Ideal Case ($V_{th} = 0.4\text{V}$):** This value is typically used in hand calculations and introductory VLSI textbooks. It serves as a "clean" baseline to verify if your circuit logic is mathematically sound before moving to complex simulations.
* **TSMC 0.18µm Reality ($V_{th} = 0.36\text{V}$):** The BSIM3v3 model used for TSMC 0.18µm processes reflects the actual physical behavior of the silicon. Factors like short-channel effects, doping concentrations, and oxide thickness result in a threshold voltage closer to $0.36\text{V}$.

---

## 2. Impact on Transconductance ($g_m$) and Gain
The most significant conclusion is the relationship between $V_{th}$ and $g_m$.

With a fixed tail current of $1\text{mA}$ ($0.5\text{mA}$ per branch), a lower $V_{th}$ ($0.36\text{V}$) means the transistor is "easier" to turn on. 

**Conclusion:** The lower $V_{th}$ results in a slightly higher **Simulated Transconductance ($3.45\text{mS}$)** compared to the **Theoretical ($3.33\text{mS}$)**. This leads to a higher voltage gain ($15.86\text{dB}$ vs $15.55\text{dB}$).

---

## 3. Common-Mode Range Sensitivity
Analyzing both values reveals how sensitive your "Operating Window" is to process variations.

* **Minimum Input ($V_{ICM,min}$):** A lower $V_{th}$ reduces $V_{GS}$, which actually increases your downward headroom by **$40\text{mV}$**.
* **Maximum Input ($V_{ICM,max}$):** Conversely, a lower $V_{th}$ reduces your upward headroom. The transistors will enter the Triode Region **$40\text{mV}$ sooner** if $V_{th}$ is $0.36\text{V}$.

**Final Design Verdict:**
Design engineers must use both values to ensure the circuit works even if the manufacturing process drifts slightly (Process Corners). This highlights the importance of analyzing "Fast" and "Slow" models in a professional E&C workflow.

## Step 2: Transient Analysis Calculations

### 1. Linear Boundary Calculation
The linear steering limit is defined as:
$$V_{id, limit} = \sqrt{2} \cdot V_{OV} = \sqrt{2} \cdot 0.3\text{V} \approx \mathbf{424\text{mV}}$$

### 2. Case A: Small-Signal (Linear)
* **Input:** $V_{id} = 100\text{mV}$
* **Output:** $V_{od} = 6.21 \cdot 100\text{mV} = \mathbf{621\text{mV}}$
* **Result:** Clean amplification; transistors remain in saturation.

### 3. Case B: Large-Signal (Non-Linear Clipping)
* **Input:** $V_{id} = 600\text{mV}$
* **Expected (Linear):** $3.7\text{V}$
* **Actual (Physical Limit):** $\mathbf{1.8\text{V}}$
* **Result:** The output clips at $V_{DD} - V_{SS}$. The waveform distorts into a square-like wave as $I_{SS}$ is fully steered to one branch.

  # Transient Calculation for $V_{id} = 200\text{mV}$

## 1. Input Parameters
For a differential input of $200\text{mV}$, the signals at the gates of $M_1$ and $M_2$ are defined as:
* **$V_{in1}$ (at $M_1$ Gate):** $+100\text{mV}$
* **$V_{in2}$ (at $M_2$ Gate):** $-100\text{mV}$
* **Differential Input ($V_{id}$):** $V_{in1} - V_{in2} = 100\text{mV} - (-100\text{mV}) = \mathbf{200\text{mV}}$
<img width="706" height="388" alt="image" src="https://github.com/user-attachments/assets/f9175d9c-5f02-4ca2-9701-6baebf6341b1" />

---

## 2. Expected Output ($V_{out,diff}$)
Using the simulated gain ($A_v$) derived from the TSMC 0.18µm process analysis:
* **Gain ($A_v$):** $\approx 6.21$

**Calculation:**
$$V_{out,diff} = A_v \times V_{id} = 6.21 \times 200\text{mV} = \mathbf{1.242\text{V}}$$

---

## 3. Single-Ended Swing Check
To verify that the output does not saturate or hit the power rails ($V_{DD} = +0.9\text{V}$ and $V_{SS} = -0.9\text{V}$), we analyze the swing at each individual drain node. Each output node ($V_{out1}$ and $V_{out2}$) will swing half of the total differential output:

**Peak Swing per node:** $$\frac{1.242\text{V}}{2} = \mathbf{0.621\text{V}}$$

**Verification:**
Since $0.621\text{V} < 0.9\text{V}$, the signal has sufficient headroom. The output will remain a clean, undistorted sine wave.

---

## Summary of the Interpretation (Case A)

| Parameter | Value | Status |
| :--- | :--- | :--- |
| **Differential Input ($V_{id}$)** | $200\text{mV}$ | Within Linear Range ($< 424\text{mV}$) |
| **Differential Output ($V_{od}$)** | $1.242\text{V}$ | No Clipping ($< 1.8\text{V}$) |
| **Transistor State** | Saturation | **Linear Amplification** |
<img width="1904" height="530" alt="image" src="https://github.com/user-attachments/assets/9fd08b02-ceda-4eba-89dc-14c4aa884a26" />
# 1. Waveform Interpretation (Case A)
Based on the transient simulation plots, the following physical behaviors are observed in the circuit:

### A. Input Signal Analysis ($V_{n002}, V_{n003}$)
The applied signals at the gates are $100\text{mV}$ peak (Green/Blue traces). This results in a total **Differential Input ($V_{id}$)** of **$200\text{mV}$ peak-to-peak**.

### B. Output Signal Analysis ($V_{out1}, V_{out2}$)
The output waveforms (Red/Cyan traces) reach approximately **$500\text{mV}$ to $600\text{mV}$ peak**. This aligns with the calculated single-ended swing, confirming that the gain is functioning as expected.

### C. Phase Relationship
It is important to note that $V_{out1}$ (Red) is **$180^\circ$ out of phase** with its corresponding input. This inversion is the standard expected behavior for a common-source based differential pair architecture.

### D. Linearity and Region of Operation
The waveform peaks are rounded and smooth, without any flattening or "squaring" of the signal. This visual evidence confirms that:
$$V_{id} (200\text{mV}) < \sqrt{2}V_{OV} (424\text{mV})$$

**Conclusion:** The transistors are successfully staying within the **Saturation Region** throughout the entire signal cycle, providing clean, linear amplification.

---

| Observation | Physical Status |
| :--- | :--- |
| **Waveform Shape** | Sinusoidal (Smooth) |
| **Voltage Headroom** | Sufficient (No Clipping) |
| **Operating Mode** | Linear / Small-Signal |

# Case B Transient Calculation for $V_{id} = 600\text{mV}$

## 1. Input Parameters
In this large-signal test case, the differential input is increased to $600\text{mV}$:
* **$V_{in1}$ (at $M_1$ Gate):** $+300\text{mV}$
* **$V_{in2}$ (at $M_2$ Gate):** $-300\text{mV}$
* **Differential Input ($V_{id}$):** $V_{in1} - V_{in2} = 300\text{mV} - (-300\text{mV}) = \mathbf{600\text{mV}}$
<img width="657" height="425" alt="image" src="https://github.com/user-attachments/assets/35401ba5-c1fa-498c-80e5-b80ab46b40b7" />

---

## 2. Expected Output ($V_{out,diff}$)
Using the simulated small-signal gain ($A_v \approx 6.21$) derived earlier:

* **Theoretical Linear Calculation:** $6.21 \times 600\text{mV} = \mathbf{3.726\text{V}}$
* **The Reality (Current Steering):** The output cannot physically reach $3.7\text{V}$. The maximum possible differential swing is strictly limited by the total available tail current ($I_{SS} = 1\text{mA}$) and the load resistors ($R_D = 1.8\text{k}\Omega$).

**Maximum Differential Output:** $$V_{od, max} = I_{SS} \times R_D = 1\text{mA} \times 1.8\text{k}\Omega = \mathbf{1.8\text{V}}$$

---
<img width="1907" height="524" alt="image" src="https://github.com/user-attachments/assets/dc9f3550-c8a2-4a62-b762-0f8e5abd6aa9" />

## 3. Single-Ended Swing & Clipping Check
To understand why the signal distorts, we examine the individual output nodes relative to the supply rails ($V_{DD} = 0.9\text{V}$, $V_{SS} = -0.9\text{V}$):

* **The Mathematical Demand:** Each node would need to swing $3.726\text{V} / 2 = \mathbf{1.863\text{V}}$ peak.
* **The Physical Limit:** * When $M_1$ takes the entire $1\text{mA}$ tail current, $V_{out1}$ drops to: $0.9\text{V} - (1\text{mA} \times 1.8\text{k}\Omega) = \mathbf{-0.9\text{V}}$.
    * When $M_1$ turns completely **OFF**, $V_{out1}$ is pulled up to $V_{DD} = \mathbf{+0.9\text{V}}$.

**Result:** Since the required $1.863\text{V}$ swing is significantly larger than the $0.9\text{V}$ rail capacity, the signal clips heavily. The sine wave "hits the ceiling" of the supply rails and flattens out into a quasi-square wave.

---

## Summary of the Interpretation (Case B)

| Parameter | Value | Status |
| :--- | :--- | :--- |
| **Differential Input ($V_{id}$)** | $600\text{mV}$ | **Exceeds Linear Range** ($> 424\text{mV}$) |
| **Differential Output ($V_{od}$)** | $\approx 1.8\text{V}$ | **Hard Clipping** (Limited by $I_{SS}$) |
| **Transistor State** | Non-Linear / Cut-off | **Current Steering / Switching** |

# Final Comparison: Case A vs. Case B

The transient behavior of the NMOS Differential Pair demonstrates a clear transition from linear amplification to non-linear current steering as the input magnitude increases.

* **Case A ($200\text{mV}$):** The input signal is small enough that both transistors remain in the **Saturation Region**. The differential pair successfully shares the tail current without reaching its physical boundaries.
    * **Result:** A clean, undistorted **$1.24\text{V}$** peak-to-peak sine wave.

* **Case B ($600\text{mV}$):** The input signal is so large that it exceeds the linear range ($>\sqrt{2}V_{OV}$). One transistor "steals" the entire $1\text{mA}$ tail current from the other, forcing one branch into cut-off.
    * **Result:** The output saturates at **$1.8\text{V}$** ($I_{SS} \times R_D$) and takes on a distorted, **"square-ish"** appearance due to hard clipping at the supply rails.

---

### Side-by-Side Summary

| Feature | Case A ($200\text{mV}$) | Case B ($600\text{mV}$) |
| :--- | :--- | :--- |
| **Input Signal ($V_{id}$)** | $200\text{mV}$ | $600\text{mV}$ |
| **Output Peak-to-Peak** | $1.24\text{V}$ | $1.8\text{V}$ (Capped) |
| **Waveform Quality** | Clean Sinusoid | Distorted / Clipped |
| **Current Behavior** | Balanced Steering | Full Switching |
| **Operating Regime** | Linear Small-Signal | Large-Signal Saturation |

## 2. Common-Mode Range Calculations
The Input and Output Common-Mode ranges define the "Operating Window" for the differential pair.

### Input Common-Mode ($V_{ICM}$)
* **Minimum ($V_{ICM,min}$):** Ensures the tail current source remains saturated.
* **Maximum ($V_{ICM,max}$):** Ensures the differential pair does not enter Triode.

| Case | $V_{ICM,min}$ | $V_{ICM,max}$ |
| :--- | :--- | :--- |
| **$V_{th}=0.4\text{V}$** | $0\text{V}$ | $0.4\text{V}$ |
| **$V_{th}=0.36\text{V}$** | $-0.04\text{V}$ | $0.36\text{V}$ |

### Output Common-Mode ($V_{OCM}$)
* **Maximum:** $V_{DD} = 0.9\text{V}$
* **Minimum:** $V_{ICM} - V_{th} = -0.36\text{V}$ (for $V_{th}=0.36\text{V}$)

## 3. Comparative Summary
| Metric | Case A ($200\text{mV}$) | Case B ($600\text{mV}$) |
| :--- | :--- | :--- |
| Linearity | Linear | Non-Linear (Clipped) |
| Transistor State | Saturation | Periodic Cut-off |
| Current Behavior | Dynamic Sharing | Hard Steering |
<img width="275" height="431" alt="image" src="https://github.com/user-attachments/assets/9a6ce73f-bace-4904-88e8-5f33acc17526" />

## Simulation-Based Performance Analysis

### 1. Device Parameters (BSIM Model)
| Parameter | Value |
| :--- | :--- |
| Drain Current ($I_d$) | 0.5mA |
| Gate-Source Voltage ($V_{gs}$) | 0.701V |
| Threshold Voltage ($V_{th}$) | 0.446V |
| Transconductance ($g_m$) | 3.72mS |

### 2. Linear Range & Gain
* **Overdrive ($V_{ov}$):** $0.255\text{V}$
* **Linear Limit ($\pm\sqrt{2}V_{ov}$):** $\pm 361\text{mV}$
* **Differential Gain ($A_v$):** $-6.696$ (**16.51dB**)

### 3. Load Effects ($C_L = 10\text{pF}$)
* **Small-Signal:** Clean sinusoidal response with a gain of ~6.7.
* **Large-Signal:** Output clipping occurs beyond $V_{id} = 361\text{mV}$ due to full current steering.
## AC Frequency Response Analysis

### 1. Ideal Response ($C_L = 0\text{pF}$)
* **Gain:** $15.71\text{dB}$ (Flat)
* **Phase:** $180^\circ$
* **Note:** Represents the maximum bandwidth of the design.

### 2. Loaded Response ($C_L = 10\text{pF}$)
* **Low-Freq Gain:** $15.71\text{dB}$
* **Cutoff Threshold:** $12.71\text{dB}$ ($15.71 - 3$)
* **Phase Behavior:** Shift toward $90^\circ$ due to the output pole.
* **Dominant Pole:** Created by the parallel combination of $R_D$ and $C_L$.

<img width="1912" height="524" alt="image" src="https://github.com/user-attachments/assets/b65f8ba1-b6fa-4820-b503-a3b22c0a08e5" />
<img width="301" height="198" alt="image" src="https://github.com/user-attachments/assets/352626f9-e7af-4f9b-92ff-12337bdd63c3" />
## Small-Signal Transient Validation

### 1. Parameters
* **Input Swing ($V_{in, p-p}$):** $20\text{mV}$
* **Output Swing ($V_{out, p-p}$):** $124.09\text{mV}$

### 2. Gain Calculation
$$A_v = \frac{124.09\text{mV}}{20\text{mV}} = 6.204$$
$$A_{v,dB} = 20 \log_{10}(6.204) = \mathbf{15.85\text{dB}}$$

### 3. Key Observations
* **Linearity:** Confirmed; signal is undistorted as $V_{in} \ll \sqrt{2}V_{ov}$.
* **Phase:** Balanced output verified with $180^{\circ}$ phase difference between $V_{out1}$ and $V_{out2}$.
## Small-Signal Transient Validation


### Summary Table
| Parameter | Simulated Value | Status |
| :--- | :--- | :--- |
| **Differential Input ($V_{in, p-p}$)** | 20mV | Within Linear Range |
| **Differential Output ($V_{out, p-p}$)** | 124.09mV | Measured by Cursors |
| **Voltage Gain ($A_v$)** | 6.204 | Calculated Ratio |
| **Voltage Gain ($dB$)** | **15.85dB** | Logarithmic Form |


# Differential Amplifier: Comprehensive Performance Analysis
*<img width="1919" height="525" alt="image" src="https://github.com/user-attachments/assets/ed4133c6-a2ed-470f-958f-bbd16d00d9ab" />

<img width="1917" height="516" alt="image" src="https://github.com/user-attachments/assets/b23ef699-cd04-4395-a845-15f4438cb91d" />


## 1. Transient Analysis and Linear Behavior
The transient response of the differential pair was evaluated with a load capacitance ($C_L$) of 10pF. This analysis is crucial for understanding how the circuit handles time-varying signals and identifying the physical boundaries of its amplification capabilities.

### Linear Region ($V_{id} < \sqrt{2}V_{OV}$)
When the differential input signal is kept small (specifically 20mV peak-to-peak in this test), the amplifier operates within its most predictable and useful state.

* **Physical Mechanism:** In this region, both NMOS transistors ($M_1$ and $M_2$) remain firmly in the **Saturation Region**. The tail current is shared dynamically between the two branches without either transistor approaching cut-off.
* **Observed Output:** The simulation reveals a clean, undistorted sinusoidal output. The measured peak-to-peak voltage is **124.09mV**.
* **Phase Relationship:** The signals $V(vout1)$ and $V(vout2)$ exhibit a perfect **180° phase difference**. This balanced operation is the hallmark of a properly biased differential pair, ensuring that common-mode noise is rejected while the differential signal is preserved.

### Non-Linear Region ($V_{id} > \sqrt{2}V_{OV}$)
As the differential input ($V_{id}$) scales up toward and beyond the calculated linear limit of $\approx$ 361mV, the circuit enters a non-linear switching mode.

* **Current Steering:** The large input voltage forces one transistor to "steal" the entire 1mA tail current. For example, if $V_{in1}$ is much higher than $V_{in2}$, $M_1$ carries the full 1mA while $M_2$ enters the **Cut-off Region**.
* **Observed Output Distortion:** The output waveforms lose their sinusoidal shape and exhibit **"Hard Clipping"** or flattening at the peaks. This happens because the voltage drop across the drain resistor ($R_D$) cannot exceed $I_{SS} \times R_D$. Once the tail current is fully steered, the output is physically capped, regardless of further increases in the input voltage.

---

## 2. Gain and Bandwidth Analysis
The gain and frequency characteristics were derived using a combination of Operating Point (DC) data and AC frequency sweeps.

### Voltage Gain ($A_v$) Analysis
* **Transient-Based Calculation:** By comparing the peak-to-peak output (124.09mV) to the input (20mV), the large-signal gain is calculated as **$\approx$ 6.2** (or **15.85dB**).
* **AC Simulation Results:** The mid-band gain obtained via AC analysis is approximately **15.71dB**. The slight difference between transient and AC results is expected due to the small-signal approximations used in AC modeling versus the full-swing physics of transient simulation.

### Frequency Response with $C_L = 10\text{pF}$
The addition of a 10pF load capacitor creates a dominant pole at the output nodes, significantly impacting the high-frequency performance.

* **-3dB Bandwidth:** This represents the **"Cutoff Frequency"** where the power delivered to the load drops by half. The gain decreases from its mid-band value to 12.71dB at a frequency of **9.43MHz**.
* **Unity Gain Bandwidth (UGB):** This is the frequency at which the amplifier's gain drops to 1 (0dB). For this design, the UGB is measured at **56.21MHz**.
* **Roll-off Characteristics:** Beyond the 9.43MHz pole, the gain declines at a constant rate of **-20dB/decade**. This confirms the system behaves as a first-order low-pass filter, where the bandwidth is strictly determined by the product of $R_D$ and $C_L$.

---

## 3. Operational Summary

| Parameter | Value | Source / Method |
| :--- | :--- | :--- |
| **Mid-band Gain** | 15.71dB ($\approx$ 6.1) | AC Simulation |
| **-3dB Bandwidth** | 9.43MHz | AC Frequency Sweep |
| **Unity Gain Bandwidth** | 56.21MHz | AC Frequency Sweep |
| **Load Capacitance ($C_L$)** | 10pF | Design Specification |
| **Overdrive Voltage ($V_{OV}$)** | 0.255V | Derived from OP ($V_{GS} - V_{th}$) |
| **Linear Input Limit** | $\approx \pm 361\text{mV}$ | Calculated ($\sqrt{2}V_{OV}$) |
| **Power Dissipation** | 1.8mW | Calculated ($V_{supply} \times I_{SS}$) |

Note: The slight difference between $57.52\text{ MHz}$ and $56.21\text{ MHz}$ occurs because the MOSFET's internal parasitic capacitances (like $C_{gd}$ and $C_{gs}$) create additional high-frequency poles that slightly deviate from a perfect single-pole model.

# Conclusion: Theoretical vs. Simulated Performance

The differences between these values are primarily due to MOSFET second-order effects—such as channel length modulation ($r_o$)—which are included in the high-fidelity BSIM model but are usually ignored in simplified hand calculations.

## 1. Voltage Gain Analysis
* **Theoretical Gain ($A_v = -g_m \times R_D$):** Using the ideal $g_m$ from your BSIM data (3.72 mS) and $R_D = 1.8\text{ k}\Omega$, the theoretical gain is **16.51 dB (6.7 V/V)**.
* **Simulated Gain:** Your AC analysis shows a slightly lower gain of **15.71 dB ($\approx$ 6.1 V/V)**.
* **Difference:** The 0.8 dB drop in simulation occurs because the actual gain is $A_v = -g_m \times (R_D \parallel r_o)$. The finite output resistance ($r_o$) of the transistors reduces the effective load, thereby lowering the total gain.

---

## 2. Bandwidth and GBP
* **Theoretical Bandwidth:** Based on $f_{-3dB} = 1 / (2\pi R_D C_L)$, the calculated cutoff is **8.84 MHz**.
* **Simulated Bandwidth:** Your simulation measures the -3dB point at **9.43 MHz**.
* **Gain-Bandwidth Product (GBP):**
    * **Theoretical:** $\approx$ **59.2 MHz**.
    * **Simulated:** **56.21 MHz** (Unity Gain Bandwidth).
* **Difference:** The simulation shows a slightly higher bandwidth but a lower UGB. This is because the reduced gain (15.71 dB vs 16.51 dB) pushes the cutoff frequency higher, while the overall product remains slightly lower due to parasitic capacitances ($C_{gd}, C_{db}$) adding to the external 10 pF load.

---

## 3. Operational Limits
* **Theoretical Linear Range:** Calculated as $\pm \sqrt{2} \times V_{ov} \approx$ **$\pm$ 361 mV**.
* **Simulated Observation:** In transient analysis, your 20 mV peak-to-peak input yielded a perfectly linear **124.09 mV** output. However, as the input increased toward the supply rails in other plots, clipping was observed exactly as predicted when the tail current (1 mA) became fully steered into one branch.

---

## Summary Table: Theoretical vs. Simulated

| Parameter | Theoretical | Simulated | % Deviation |
| :--- | :--- | :--- | :--- |
| **Gain (dB)** | 16.51 dB | 15.71 dB | $\approx$ 4.8% |
| **-3dB BW** | 8.84 MHz | 9.43 MHz | $\approx$ 6.6% |
| **UGB / GBP** | 59.2 MHz | 56.21 MHz | $\approx$ 5.0% |
| **Power (P)** | 1.8 mW | 1.8 mW | 0% (Matches $I_1 \times V_{total}$) |
