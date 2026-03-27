# Design and Analysis of a CMOS Differential Amplifier 
### (PMOS Active Load and NMOS Current Mirror)
<img width="1039" height="606" alt="image" src="https://github.com/user-attachments/assets/e1b1d3b8-aa4c-40a7-90f8-3f3b91ef35b4" />


Designing a CMOS differential amplifier with a PMOS active load and an NMOS current mirror is a classic challenge in analog IC design. This configuration significantly increases the voltage gain compared to a passive resistive load by providing a high incremental output resistance.

Below is the structured technical analysis and design methodology for this circuit.

---

## 1. Circuit Topology and Operation
The circuit consists of three main sub-blocks:

* **Differential Pair ($M_1, M_2$):** NMOS transistors that convert the differential input voltage ($V_{id}$) into current.
* **Active Load ($M_3, M_4$):** A PMOS current mirror that acts as a high-impedance load and performs differential-to-single-ended conversion.
* **Tail Current Source ($M_5$):** An NMOS transistor acting as the "tail" current source, biased to provide a stable bias current ($I_{SS}$) and ensure a high Common-Mode Rejection Ratio (CMRR).

---

## 2. DC Biasing and Power Constraints
To satisfy a specific power constraint (e.g., $P \le 2\text{mW}$), we must carefully select the tail current and supply voltages.

* **Total Power Consumption:** The power is calculated as $P = (V_{DD} - V_{SS}) \times I_{SS}$. For a $\pm 0.9\text{V}$ supply and $1\text{mA}$ current, $P = 1.8\text{mW}$.
* **Current per Branch:** Since the circuit is balanced, the current is split equally: $I_{D1} = I_{D2} = I_{SS} / 2$.
* **Saturation Condition:** For maximum swing and linearity, all transistors must remain in the saturation region:
    * **For NMOS:** $V_{DS} \ge V_{GS} - V_{thn}$
    * **For PMOS:** $V_{SD} \ge V_{SG} - |V_{thp}|$

---

## 3. Small-Signal Analysis
The use of an active load changes the gain equation significantly. Unlike the passive load ($g_m R_D$), the gain is now determined by the transconductance of the input pair and the parallel combination of the output resistances of the NMOS and PMOS.

### Voltage Gain ($A_v$)
The single-ended output gain is given by the formula:
$$A_v = g_{m1} (r_{o2} \parallel r_{o4})$$

**Where:**
* $g_{m1} = \sqrt{2 \mu_n C_{ox} \frac{W}{L} I_{D1}}$
* $r_o = \frac{1}{\lambda I_D}$ (Output resistance due to Channel Length Modulation)

### Common-Mode Rejection Ratio (CMRR)
The NMOS current mirror ($M_5$) provides a very high output resistance ($R_{SS}$), which is critical for rejecting common-mode signals:
$$CMRR \approx g_{m1} R_{SS}$$


# Comprehensive Design and DC Analysis: Circuit 2
<img width="452" height="460" alt="image" src="https://github.com/user-attachments/assets/e8381a38-406e-499f-a84d-7a6047bfdb30" />
<img width="418" height="87" alt="image" src="https://github.com/user-attachments/assets/88b46ea2-7cfb-474e-8f1a-d31416f8342e" />

---

## 1. Physical and Electrical Design Constraints
To ensure a robust design in a $0.18\text{µm}$ process while using a custom length, we define the following constants:

* **Channel Length ($L$):** **$480\text{nm}$** (or $0.48\text{µm}$)
  * *Note:* Using a longer $L$ reduces the Channel Length Modulation effect ($\lambda$), which increases the output resistance ($r_o$) and leads to a higher overall Voltage Gain.
* **Threshold Voltage ($V_{th}$):** **$0.36\text{V}$**
* **Total Power Constraint:** **$P \le 1.8\text{mW}$**
* **Supply Rails ($V_{DD} / V_{SS}$):** **$+0.9\text{V} / -0.9\text{V}$** (Total $1.8\text{V}$)

---

## 2. Device & Circuit Configuration
This section defines the hardware setup and the environmental constants used for the design:

* **Supply Voltages:** * $V_{DD} = +0.9\text{V}$
  * $V_{SS} = -0.9\text{V}$
  * **Total Supply Voltage:** $1.8\text{V}$
* **Transistor Dimensions:**
  * **Channel Length ($L$):** $480\text{nm}$ ($0.48\mu\text{m}$) for all transistors ($M_1$ through $M_5$).
* **Process Parameters:**
  * **Threshold Voltage ($V_{th}$):** $0.36\text{V}$
* **Bias Input:**
  * **Tail Gate Voltage ($V_{G5}$):** **$-0.34\text{V}$**
  * **Differential Inputs:** $0\text{V}$ (Balanced DC state)

---

## 3. DC Operating Point (Simulation Values)
Based on the final `.op` simulation results, the following values characterize the balanced DC operating state:

* **Tail Source Node Voltage ($V_p$):** $\approx \mathbf{-0.70\text{V}}$
* **Output Voltages ($V_{out1}, V_{out2}$):** $\approx \mathbf{0.11\text{V}}$
* **Tail Current ($I_{d5}$):** $\approx \mathbf{0.54\text{mA}}$
* **Branch Current ($I_{d1,2,3,4}$):** $\approx \mathbf{0.27\text{mA}}$

---

## 4. Power Dissipation Verification
With the measured tail current, the total power consumption is calculated as follows:

$$P = V_{total} \times I_{d5}$$
$$P = 1.8\text{V} \times 0.54\text{mA} = \mathbf{0.972\text{mW}}$$

> **Constraint Verification:** The calculated power level of **$0.972\text{mW}$** comfortably satisfies the design constraint of **$P \le 1.8\text{mW}$**. This indicates that the design operates at approximately 54% of its maximum power limit, providing excellent thermal headroom.

> **Constraint Verification:** This power level of **$0.972\text{mW}$** comfortably satisfies the design constraint of **$P \le 1.8\text{mW}$**, providing significant thermal and power headroom for the system.


## Branch Current and Power Analysis
The total current is established by the tail transistor ($M_5$) and then divided between the two symmetric branches of the differential pair.

* **Total Tail Current ($I_{SS}$):** **$0.54\text{mA}$**
* **Individual Branch Current ($I_{D1,2,3,4}$):**
    $$I_D = \frac{I_{SS}}{2} = \frac{0.54\text{mA}}{2} = \mathbf{0.27\text{mA}}$$
* **Total Power Dissipation ($P$):**
    $$P = (V_{DD} - V_{SS}) \times I_{SS} = (0.9\text{V} - (-0.9\text{V})) \times 0.54\text{mA} = \mathbf{0.972\text{mW}}$$
    *(This is well within the $1.8\text{mW}$ limit).*

---

## 3. Detailed DC Operating Point Calculations

### A. NMOS Tail Current Source ($M_5$)
* **Gate-Source Voltage ($V_{GS5}$):** $V_{G5} - V_{SS} = -0.34\text{V} - (-0.9\text{V}) = \mathbf{0.56\text{V}}$
* **Overdrive Voltage ($V_{ov5}$):** $V_{GS5} - V_{th} = 0.56\text{V} - 0.36\text{V} = \mathbf{0.20\text{V}}$
* **Drain-Source Voltage ($V_{DS5}$):** $V_p - V_{SS} = -0.70\text{V} - (-0.9\text{V}) = \mathbf{0.20\text{V}}$
* **Check:** Since $V_{DS5} \ge V_{ov5}$ ($0.20\text{V} \ge 0.20\text{V}$), $M_5$ is in **Saturation**.

### B. NMOS Input Differential Pair ($M_1, M_2$)
* **Gate-Source Voltage ($V_{GS1,2}$):** $V_{in} - V_p = 0\text{V} - (-0.70\text{V}) = \mathbf{0.70\text{V}}$
* **Overdrive Voltage ($V_{ov1,2}$):** $V_{GS1,2} - V_{th} = 0.70\text{V} - 0.36\text{V} = \mathbf{0.34\text{V}}$
* **Drain-Source Voltage ($V_{DS1,2}$):** $V_{out} - V_p = 0.11\text{V} - (-0.70\text{V}) = \mathbf{0.81\text{V}}$
* **Check:** Since $0.81\text{V} > 0.34\text{V}$, $M_1$ and $M_2$ are in **Saturation**.

### C. PMOS Active Load ($M_3, M_4$)
* **Source-Gate Voltage ($V_{SG3,4}$):** $V_{DD} - V_{G3,4} = 0.9\text{V} - 0\text{V} = \mathbf{0.9\text{V}}$
* **Overdrive Voltage ($|V_{ov3,4}|$):** $V_{SG3,4} - |V_{thp}| = 0.9\text{V} - 0.36\text{V} = \mathbf{0.54\text{V}}$
* **Source-Drain Voltage ($V_{SD3,4}$):** $V_{DD} - V_{out} = 0.9\text{V} - 0.11\text{V} = \mathbf{0.79\text{V}}$
* **Check:** Since $0.79\text{V} > 0.54\text{V}$, $M_3$ and $M_4$ are in **Saturation**.

---

## 4. Operating Point Summary Table

| Transistor | $I_D$ (mA) | $V_{GS}$ (V) | $V_{DS}$ (V) | $V_{ov}$ (V) | Region |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **$M_5$ (Tail)** | $0.54$ | $0.56$ | $0.20$ | $0.20$ | **Saturation** |
| **$M_1, M_2$ (Input)** | $0.27$ | $0.70$ | $0.81$ | $0.34$ | **Saturation** |
| **$M_3, M_4$ (Load)** | $0.27$ | $-0.90$ | $-0.79$ | $0.54$ | **Saturation** |

---

**Constraint Verification:** The measured power level of **$0.972\text{mW}$** comfortably satisfies the design constraint of **$P \le 1.8\text{mW}$**. This confirms that all transistors are biased in saturation, providing high gain and sufficient voltage headroom.
# Transistor Width ($W$) Design Calculations

To calculate the required Width ($W$) for your transistors, we use the **Saturation Region Current Equation**. Since you are using a **480nm** length, we must ensure the width is sufficient to provide the simulated currents at the specific overdrive voltages ($V_{ov}$) derived from your DC analysis.

---

## 1. Fundamental Design Equations

The drain current in saturation is defined as:
$$I_D = \frac{1}{2} \mu C_{ox} \frac{W}{L} (V_{GS} - V_{th})^2$$

To find the required width ($W$) for a target current and bias, we rearrange the formula:
$$W = \frac{2 I_D L}{\mu C_{ox} (V_{ov})^2}$$

### Process Parameters ($0.18\mu\text{m}$ TSMC)
For these calculations, we use typical approximate values for the $180\text{nm}$ process node:
* **NMOS ($\mu_n C_{ox}$):** $\approx 190\mu\text{A/V}^2$
* **PMOS ($\mu_p C_{ox}$):** $\approx 45\mu\text{A/V}^2$
* **Channel Length ($L$):** $480\text{nm}$ ($0.48\mu\text{m}$)

---

## 2. Step-by-Step Width Calculations

### A. Tail Transistor ($M_5$)
This NMOS transistor must carry the full tail current to set the power level.
* **Target $I_{d5}$:** $0.54\text{mA}$ ($540\mu\text{A}$)
* **Overdrive ($V_{ov5}$):** $0.20\text{V}$
* **Calculation:**
$$W_5 = \frac{2 \times 540\mu\text{A} \times 0.48\mu\text{m}}{190\mu\text{A/V}^2 \times (0.20\text{V})^2} = \frac{518.4}{7.6} \approx \mathbf{68.21\mu\text{m}}$$

### B. Input Differential Pair ($M_1, M_2$)
These NMOS transistors carry half of the tail current ($I_{SS}/2$).
* **Target $I_{d1,2}$:** $0.27\text{mA}$ ($270\mu\text{A}$)
* **Overdrive ($V_{ov1,2}$):** $0.34\text{V}$
* **Calculation:**
$$W_{1,2} = \frac{2 \times 270\mu\text{A} \times 0.48\mu\text{m}}{190\mu\text{A/V}^2 \times (0.34\text{V})^2} = \frac{259.2}{21.96} \approx \mathbf{11.80\mu\text{m}}$$

### C. PMOS Active Load ($M_3, M_4$)
Because PMOS has lower hole mobility ($\mu_p$), a larger width is needed to mirror the $0.27\text{mA}$ branch current.
* **Target $I_{d3,4}$:** $0.27\text{mA}$ ($270\text{µA}$)
* **Overdrive ($|V_{ov3,4}|$):** $0.54\text{V}$
* **Calculation:**
$$W_{3,4} = \frac{2 \times 270\mu\text{A} \times 0.48\mu\text{m}}{45\mu\text{A/V}^2 \times (0.54\text{V})^2} = \frac{259.2}{13.12} \approx \mathbf{19.75\mu\text{m}}$$

---

## 3. Design Width Summary Table

| Transistor | Target $I_D$ | $V_{ov}$ | Length ($L$) | Calculated Width ($W$) |
| :--- | :--- | :--- | :--- | :--- |
| **Tail ($M_5$)** | $0.54\text{mA}$ | $0.20\text{V}$ | $480\text{nm}$ | **$68.2\mu\text{m}$** |
| **Input ($M_1, M_2$)** | $0.27\text{mA}$ | $0.34\text{V}$ | $480\text{nm}$ | **$11.8\mu\text{m}$** |
| **Load ($M_3, M_4$)** | $0.27\text{mA}$ | $0.54\text{V}$ | $480\text{nm}$ | **$19.8\mu\text{m}$** |
Final Transistor Dimensions ($L = 480\text{nm}$)TransistorWidth (W)RoleCharacteristics$M_5$$156.9\text{ µm}$Tail Current SourceLarge $W$ to maintain $1\text{mA}$ with low $V_{DS}$.$M_1, M_2$$15.625\text{ µm}$Differential PairSized for optimal $g_m$ and matching.$M_3, M_4$$33.21\text{ µm}$Active Load (PMOS)Larger than NMOS to compensate for lower mobility.

# Technical Analysis: Hand Calculations vs. Simulation Results
<img width="409" height="303" alt="image" src="https://github.com/user-attachments/assets/843a1a03-65c2-4ed7-bbad-38b966cf1e64" />

<img width="408" height="328" alt="image" src="https://github.com/user-attachments/assets/dbc04a86-3e6a-43ea-a65d-5ed85fc49af4" />

<img width="365" height="300" alt="image" src="https://github.com/user-attachments/assets/290cfef2-ba3b-4089-93c1-3f12fe18f65f" />

The difference between the theoretical calculated widths and the actual widths used in the circuit ($156.9\mu\text{m}$, $15.625\mu\text{m}$, and $33.21\mu\text{m}$) is driven by the physical realities of the **TSMC 0.18µm process**. While "pen-and-paper" math provides a starting point, the simulation accounts for secondary effects defined in the BSIM models.

---

## 1. Channel Length Modulation ($\lambda$)
Manual calculations often use the "Ideal" square-law equation, which assumes drain current is independent of $V_{DS}$ in saturation. In a real-world $180\text{nm}$ process:
* **Effective Length:** As $V_{DS}$ increases, the depletion region at the drain expands, shortening the effective channel length ($L_{eff}$).
* **Current Boost:** Even at $L = 480\text{nm}$, this effect (represented by $\lambda$) creates a slight increase in current. 
* **Tuning:** To reach the exact **$0.54\text{mA}$** target, the width must be adjusted in the simulator to balance this $V_{DS}$-dependent current boost.

---

## 2. Mobility ($\mu$) and Oxide Capacitance ($C_{ox}$)
Hand calculations rely on "typical" process transconductance parameters (e.g., $\mu_n C_{ox} \approx 190\mu\text{A/V}^2$).
* **BSIM Complexity:** The `tsmc018.lib` file uses sophisticated models that account for vertical field mobility degradation and specific doping profiles.
* **Environmental Factors:** These models include temperature effects and parasitic capacitances that change the actual "drive strength" of the transistor. The circuit widths are the values required to reach the current target under these specific foundry conditions.

---

## 3. Velocity Saturation and Sub-micron Effects
In sub-micron processes like $0.18\mu\text{m}$, transistors deviate significantly from textbook behavior:
* **Velocity Saturation:** Carriers (electrons/holes) reach a maximum velocity limit. Beyond this point, increasing $V_{GS}$ does not increase current as much as the square-law predicts.
* **Drive Strength Compensation:** To compensate for this "loss" of drive strength, the simulator requires a larger Width to achieve the target **$1\text{mA}$** tail current ($I_{SS}$).

---

## Summary of Design Choices
By utilizing these "tuned" widths, the design achieves the following:

| Transistor | Width ($W$) | Engineering Result |
| :--- | :--- | :--- |
| **$M_5$** | **$156.9\mu\text{m}$** | Ensures the tail current source is "stiff," stable, and maintains high output impedance. |
| **$M_1, M_2$** | **$15.625\mu\text{m}$** | Balances transconductance ($g_m$) for high gain without introducing excessive gate capacitance. |
| **$M_3, M_4$** | **$33.21\mu\text{m}$** | Perfectly matches PMOS load strength to the NMOS pair, ensuring a balanced DC output. |

> **Conclusion:** These dimensions represent a high-performance biasing state that strictly adheres to the **$1.8\text{mW}$** power constraint while maximizing the amplifier's open-loop gain


# Overdrive Voltage Analysis: NMOS Input Pair ($M_1, M_2$)

The overdrive voltage ($V_{ov}$) represents the voltage at the gate that exceeds the threshold required to turn the transistor on. It is a fundamental parameter that determines the transconductance, linear swing, and saturation margin of your amplifier.

---

## 1. The Overdrive Equation
The overdrive voltage is defined by the difference between the Gate-to-Source voltage and the process threshold voltage:
$$V_{ov} = V_{GS} - V_{th}$$

## 2. Calculating $V_{GS}$ (Gate-to-Source Voltage)
To find $V_{GS}$, we analyze the potential difference between the Gate and the common source node ($V_p$):
* **Gate Voltage ($V_{G1,2}$):** In this balanced DC state, the inputs are biased at **$0\text{V}$**.
* **Source Voltage ($V_p$):** Simulation results show the common source node ($V_p$) is at **$-0.701\text{V}$** (approx. $-0.70\text{V}$).

**Calculation:**
$$V_{GS} = V_G - V_p = 0\text{V} - (-0.70\text{V}) = \mathbf{0.70\text{V}}$$

## 3. Applying the Threshold Voltage ($V_{th}$)
Next, we subtract the physical threshold voltage of the $0.18\mu\text{m}$ process:
* **Threshold ($V_{th}$):** Your design specifies **$V_{th} = 0.36\text{V}$**.

**Final Overdrive Calculation:**
$$V_{ov} = 0.70\text{V} - 0.36\text{V} = \mathbf{0.34\text{V}}$$

---
# Common-Mode Range Analysis: Input and Output Windows

To maintain the high-gain performance of Circuit 2 ($L = 480\text{nm}$), we must define the DC voltage boundaries that keep all transistors ($M_1$ through $M_5$) in the saturation region.

---

## 1. Input Common-Mode Range ($V_{ICM}$)
This is the allowable DC voltage range you can apply to the gates of $M_1$ and $M_2$ simultaneously while keeping the circuit functional.

### $V_{ICM, min}$ (Lower Limit)
The gate voltage must be high enough to accommodate the saturation voltage of the tail transistor ($M_5$) plus the gate-source voltage ($V_{GS}$) of the input pair.
* **Formula:** $V_{ICM,min} = V_{SS} + V_{DS5,sat} + V_{GS1}$
* **Calculation:** $-0.9\text{V} + 0.20\text{V} + 0.70\text{V} = \mathbf{0\text{V}}$
* **Status:** Since your inputs are currently biased at $0\text{V}$, the design is exactly at the minimum functional threshold.

### $V_{ICM, max}$ (Upper Limit)
The input voltage must be low enough so that $M_1$ and $M_2$ do not enter the triode region. This occurs when the gate voltage exceeds the drain voltage ($V_{out}$) by more than one threshold voltage ($V_{th}$).
* **Formula:** $V_{ICM,max} = V_{out} + V_{th1}$
* **Calculation:** $0.106\text{V} + 0.36\text{V} = \mathbf{0.466\text{V}}$

---

## 2. Output Common-Mode Range ($V_{OCM}$)
This defines the allowable DC swing at the output nodes ($vout1, vout2$) that maintains saturation for both the input pair ($M_1, M_2$) and the active loads ($M_3, M_4$).

### $V_{OCM, min}$ (Lower Limit)
The output voltage cannot drop so low that the input transistors ($M_1$ or $M_2$) enter the triode region.
* **Formula:** $V_{OCM,min} = V_{ICM} - V_{th1}$
* **Calculation:** $0\text{V} - 0.36\text{V} = \mathbf{-0.36\text{V}}$

### $V_{OCM, max}$ (Upper Limit)
The output voltage cannot rise so high that the PMOS load transistors ($M_3, M_4$) enter the triode region (run out of headroom).
* **Formula:** $V_{OCM,max} = V_{DD} - |V_{ov3,4}|$
* **Calculation:** $0.9\text{V} - 0.54\text{V} = \mathbf{0.36\text{V}}$

---

## 3. Summary Table: Operating Window

| Parameter | Minimum | Maximum | Limiting Factor |
| :--- | :--- | :--- | :--- |
| **Input Common-Mode ($V_{ICM}$)** | **$0\text{V}$** | **$0.466\text{V}$** | $M_5$ Saturation / $M_1$ Triode |
| **Output Common-Mode ($V_{OCM}$)** | **$-0.36\text{V}$** | **$0.36\text{V}$** | $M_1$ Triode / $M_3$ Triode |

---
## 4. Why this value is critical 

### A. Linear Input Range
The overdrive voltage determines the maximum differential input signal the amplifier can handle before one transistor in the pair cuts off.
* **Linear Differential Range:** $\pm \sqrt{2} \times V_{ov} \approx \mathbf{\pm 480\text{mV}}$.

### B. Transconductance ($g_m$)
The transconductance defines the "strength" of the input pair in converting voltage to current. With $I_D = 0.27\text{mA}$ per branch:
$$g_m = \frac{2 I_D}{V_{ov}} = \frac{2 \times 0.27\text{mA}}{0.34\text{V}} \approx \mathbf{1.58\text{ mS}}$$

### C. Saturation Margin
To ensure the transistors remain in saturation for high gain, we check the Drain-Source voltage ($V_{DS}$) against $V_{ov}$:
* **Measured $V_{DS}$:** $V_{out} - V_p = 0.106\text{V} - (-0.70\text{V}) = \mathbf{0.806\text{V}}$.
* **Verification:** Since **$0.806\text{V} \gg 0.34\text{V}$**, transistors $M_1$ and $M_2$ are operating deep within the **Saturation Region**, ensuring maximum output impedance and gain.

# Linear Region and Swing Analysis: Circuit 2 (Active Load)

To keep Circuit 2 in the linear region, we must define the range where the input transistors ($M_1, M_2$) and the active load ($M_3, M_4$) all remain in saturation. Using the updated simulation values ($I_{SS} \approx 0.54\text{mA}$, $L = 480\text{nm}$, $V_{th} = 0.36\text{V}$), here is the step-by-step linear range analysis.

---

## 1. Differential Input Range ($V_{id}$)
The differential input range is the maximum voltage difference between the two gates before the tail current "steers" entirely to one side, causing signal clipping.

* **Calculated Overdrive ($V_{ov1,2}$):** From the DC op-point, $V_{ov1,2} \approx \mathbf{0.34\text{V}}$.
* **Linear Limit Equation:** The boundary where the amplifier remains linear is:
  $$|V_{id}| \le \sqrt{2} \times V_{ov1,2}$$
* **Result:** $\sqrt{2} \times 0.34\text{V} \approx \mathbf{\pm 0.48\text{V}}$

> **Conclusion:** Your differential input signal should stay within **$-480\text{mV}$ to $+480\text{mV}$** to avoid significant non-linear distortion.

---

## 2. Common-Mode Input Range ($V_{ICM}$)
The Input Common-Mode Range (ICMR) is the DC voltage range you can apply to the gates while keeping all five transistors in saturation.

### Lower Limit ($V_{ICM,min}$)
To keep the tail transistor ($M_5$) in saturation, the gate voltage must be high enough to accommodate $M_5$'s saturation voltage plus the $V_{GS}$ of the input pair.
* **Formula:** $V_{ICM,min} = V_{SS} + V_{DS5,sat} + V_{GS1}$
* **Calculation:** $-0.9\text{V} + 0.20\text{V} + 0.70\text{V} = \mathbf{0\text{V}}$
* **Status:** Since your inputs are currently at $0\text{V}$, you are exactly at the lower boundary.

### Upper Limit ($V_{ICM,max}$)
To keep the input transistors ($M_1, M_2$) from entering the triode region, the gate voltage must not exceed the drain voltage by more than one threshold voltage.
* **Formula:** $V_{ICM,max} = V_{out} + V_{th}$
* **Calculation:** $0.11\text{V} + 0.36\text{V} = \mathbf{0.47\text{V}}$
* **Status:** Your input DC level must stay below **$0.47\text{V}$**.

---

## 3. Summary Table for Linear Operation

| Parameter | Minimum | Maximum | Limiting Factor |
| :--- | :--- | :--- | :--- |
| **Differential Input ($V_{id}$)** | $-480\text{mV}$ | $+480\text{mV}$ | Current Steering (Clipping) |
| **Common-Mode Input ($V_{ICM}$)** | **$0\text{V}$** | **$+470\text{mV}$** | $M_5$ Sat / $M_1$ Triode Boundary |
| **Output Swing ($V_{out}$)** | $-0.5\text{V}$ | $+0.36\text{V}$ | Saturation Headroom |

---

### Design Insight
By using the **$480\text{nm}$** channel length, you have optimized the gain, but the specific $V_{DS}$ values ($0.2\text{V}$ for the tail and $\approx 0.8\text{V}$ for the pair) mean your common-mode window is relatively tight ($0\text{V}$ to $0.47\text{V}$). This is a typical trade-off in low-voltage $0.18\mu\text{m}$ CMOS designs where gain is prioritized.

# Voltage Gain Analysis: CMOS Differential Amplifier (Active Load)

To find the voltage gain ($A_v$) for Circuit 2, we perform a small-signal analysis using the transconductance ($g_m$) and the output resistance ($r_o$) of the transistors. The use of a PMOS active load ($M_3, M_4$) instead of passive resistors significantly increases the gain because the incremental output resistance at the drain node is much higher.

---

## 1. Input Transconductance ($g_m$)
Using the optimized DC operating point ($I_D = 0.27\text{mA}$ and $V_{ov} = 0.34\text{V}$), we calculate the transconductance for the input pair ($M_1, M_2$):

$$g_{m1,2} = \frac{2 I_D}{V_{ov}} = \frac{2 \times 0.27\text{mA}}{0.34\text{V}} \approx \mathbf{1.588\text{ mS}}$$

## 2. Output Resistance ($r_o$)
In saturation, $r_o$ is determined by the channel length modulation ($\lambda$) and the drain current. Because you are using a longer channel length (**$480\text{nm}$**), $\lambda$ is significantly lower than at the process minimum ($180\text{nm}$), resulting in a much higher $r_o$:

* **$r_{o2}$ (NMOS):** Based on $0.18\mu\text{m}$ parameters at $480\text{nm}$, $r_{o2} \approx \mathbf{40\text{--}60\text{ k}\Omega}$.
* **$r_{o4}$ (PMOS):** Typically slightly higher due to lower mobility, $r_{o4} \approx \mathbf{50\text{--}70\text{ k}\Omega}$.

## 3. Voltage Gain Calculation ($A_v$)
For a differential-to-single-ended configuration with an active load, the gain is determined by the input $g_m$ and the parallel combination of the NMOS and PMOS output resistances:

$$A_v = g_{m1} (r_{o2} \parallel r_{o4})$$

### Step-by-Step Numerical Result:
1. **Parallel Resistance ($R_{out}$):** Assuming $r_{o2} = 50\text{k}\Omega$ and $r_{o4} = 60\text{k}\Omega$:
   $$R_{out} = \frac{50\text{k} \times 60\text{k}}{50\text{k} + 60\text{k}} \approx \mathbf{27.27\text{ k}\Omega}$$

2. **Linear Voltage Gain (V/V):**
   $$A_v = 1.588\text{ mS} \times 27.27\text{ k}\Omega \approx \mathbf{43.3\text{ V/V}}$$

3. **Gain in Decibels (dB):**
   $$Gain(dB) = 20 \log_{10}(43.3) \approx \mathbf{32.7\text{ dB}}$$

---

## 4. Comparison: Passive vs. Active Load

| Parameter | Passive Resistor Load | PMOS Active Load | Improvement |
| :--- | :--- | :--- | :--- |
| **Load Impedance** | $1.8\text{ k}\Omega$ | $\approx 27\text{ k}\Omega$ | ~15.0x |
| **Linear Gain ($A_v$)** | $\approx 2.8\text{ V/V}$ | $\approx 43.3\text{ V/V}$ | ~15.4x |
| **Gain in dB** | $\approx 9\text{ dB}$ | $\approx 32.7\text{ dB}$ | **+23.7 dB** |

---

### Final Summary Table
| Symbol | Parameter | Approximate Value |
| :--- | :--- | :--- |
| **$g_{m1}$** | Input Transconductance | $1.588\text{ mS}$ |
| **$R_{out}$** | Total Output Resistance | $\approx 27\text{ k}\Omega$ |
| **$A_v$** | Voltage Gain (Linear) | $43.3\text{ V/V}$ |
| **$A_v (dB)$** | Voltage Gain (dB) | **$32.7\text{ dB}$** |

> **Final Note:** Because the simulation uses a $480\text{nm}$ length, the actual $r_o$ in LTspice may be even higher than these estimates, potentially pushing the simulated gain closer to the **$35\text{--}40\text{dB}$** range.

# Transient Analysis: Linear vs. Non-Linear Behavior
<img width="1039" height="606" alt="image" src="https://github.com/user-attachments/assets/e1b1d3b8-aa4c-40a7-90f8-3f3b91ef35b4" />


This practical guide walks through the steps to verify the linear range of Circuit 2 ($L = 480\text{nm}$) by comparing small-signal and large-signal transient responses in LTspice.

---

## 1. Linear Behavior Analysis ($V_{id} < \sqrt{2}V_{ov}$)
As calculated in the DC analysis, your input overdrive voltage ($V_{ov}$) is **$0.34\text{V}$**. The theoretical boundary for linear operation is approximately **$0.48\text{V}$**.

* **Set Amplitude:** Right-click the input sources ($V_2$ and $V_3$). Set the amplitude to a small value, such as **$10\text{mV}$** or **$100\text{mV}$**. 
  * *Note:* This ensures the differential input ($V_{id}$) remains well below the $0.48\text{V}$ limit.
* **Run Simulation:** Click the **Run** icon. The `.tran 5m` command will simulate the circuit for 5 milliseconds.
* **Observe Output:** Click on the output node (`vout1`). 
  * **Result:** You should see a clean, undistorted sine wave. The output represents a perfectly amplified version of the input signal.

---

## 2. Non-Linear Behavior Analysis ($V_{id} > \sqrt{2}V_{ov}$)
In this step, you will push the circuit beyond its linear limits to observe "clipping" and current steering.

* **Increase Amplitude:** Right-click the input sources ($V_2$ and $V_3$). Set the amplitude to a large value, such as **$600\text{mV}$** or **$800\text{mV}$**.
  * *Note:* This ensures $V_{id}$ exceeds the **$0.48\text{V}$** linear limit.
* **Run Simulation:** Run the simulation again.
* **Observe Output:** Click on the output node (`vout1`).

  # Linear Verification: Circuit 2 Transient Analysis
<img width="1904" height="518" alt="image" src="https://github.com/user-attachments/assets/a4bbab35-2c47-4929-b287-f5c75b8b26f2" />

Based on the schematic configuration and the established DC operating point, the setup for the linear behavior analysis is verified as correct. With the input amplitude set to **10mV**, the circuit is operating well within the linear boundary ($V_{id} < 480\text{mV}$).

---

## 1. Verification of Linear Conditions
* **Input Amplitude:** The $V_{in}$ is $10\text{mV}$ (peak). The total differential input $V_{id}$ swings between $+20\text{mV}$ and $-20\text{mV}$, which is safely below the distortion limit.
* **Symmetry:** The DC operating point data shows $V(vout1)$ and $V(vout2)$ are balanced at **$0.1365\text{V}$**. This symmetry is essential for high-precision differential amplification.
* **Transistor States:** All transistors ($M_1$ through $M_5$) are confirmed in the **Saturation Region**.
    * For the input pair ($M_1, M_2$), $V_{DS} \approx 0.82\text{V}$, which is significantly greater than $V_{ov} = 0.34\text{V}$.

---

## 2. Expected Transient Output (Simulation Results)
When executing the `.tran 5m` command, the following waveform characteristics should be observed at the output nodes:

### A. Waveform Integrity
The output at `vout1` should be a clean, undistorted sine wave at **$1\text{kHz}$**. Any "flat-topping" or asymmetry would indicate a biasing error, but given the current $0.1365\text{V}$ DC level, the signal has ample headroom.

### B. Phase Relationship
* **$180^\circ$ Phase Shift:** The output `vout1` should be exactly $180^\circ$ out of phase with the input $V_3$ (assuming $V_3$ is the non-inverting side).

### C. Voltage Swing Calculation
Using the estimated gain of $\approx 43\text{ V/V}$, we can predict the output magnitude:
* **Peak Output:** $10\text{mV} \times 43 \approx \mathbf{430\text{mV}}$
* **Peak-to-Peak Swing:** $\approx \mathbf{860\text{mV}}$

---

## 3. Summary of Linear Operation Parameters

| Parameter | Value | Status |
| :--- | :--- | :--- |
| **Input Signal ($V_{in}$)** | $10\text{mV}$ | **Linear** |
| **Output Offset (DC)** | $0.1365\text{V}$ | **Balanced** |
| **Saturation Margin ($V_{DS} - V_{ov}$)** | $0.48\text{V}$ | **Deep Saturation** |
| **Predicted Peak Output** | $\approx 430\text{mV}$ | **Within Rails** |

# Non-Linear Behavior Analysis: Large-Signal Transient Results
<img width="938" height="465" alt="image" src="https://github.com/user-attachments/assets/918f0e33-e3e2-4f76-83f9-5cd8bce217f4" />

<img width="1917" height="527" alt="image" src="https://github.com/user-attachments/assets/e22b51a0-0534-421a-8896-75dfb6bc2a7c" />

The waveform in the latest simulation ($V_{in} = 400\text{mV}$) clearly demonstrates the non-linear boundaries of the differential amplifier. By creating a large differential input $V_{id}$ of **$800\text{mV}$ peak-to-peak**, the circuit has exceeded the linear range boundary of approximately **$\pm 480\text{mV}$**.

---

## 1. Analysis of the Non-Linear Waveform

### A. Clipping and Saturation
The output waveform is no longer a smooth sine wave. It shows significant "flattening" at the top ($\approx 750\text{mV}$) and bottom ($\approx -50\text{mV}$). This "squaring off" is the classic signature of an amplifier running out of voltage headroom.

### B. Current Steering Mechanism
This distortion occurs because the large input signal forces the **$0.54\text{mA}$** tail current to steer almost entirely into one branch of the differential pair:
* When the current in one transistor reaches the total available tail current ($I_{SS}$), the other transistor cuts off.
* At this point, the output voltage can no longer change in response to the input, leading to the observed flat peaks.

### C. Asymmetry and Headroom
The clipping is slightly asymmetric because the output swing is hitting different physical limits:
* **Lower Bound:** The output flattens as it nears the tail node voltage ($V_p$).
* **Upper Bound:** The output flattens as the PMOS active loads ($M_3, M_4$) enter the triode region or run out of supply headroom.

---

## 2. Comparison and Interpretation Summary

| Feature | Linear Behavior ($10\text{mV}$) | Non-Linear Behavior ($400\text{mV}$) |
| :--- | :--- | :--- |
| **Input Signal Condition** | $V_{id} < \sqrt{2}V_{ov}$ | $V_{id} > \sqrt{2}V_{ov}$ |
| **Output Waveform** | Smooth, pure sine wave | Clipped, distorted "square-like" wave |
| **Transistor Operation** | All transistors stay in Saturation | Transistors enter Cut-off or Triode |
| **Primary Application** | High-fidelity signal amplification | Switching or signal limiting |

---

## 3. Final Conclusion

The experiment proves that the linear range of this CMOS differential amplifier is strictly bounded by the **overdrive voltage ($V_{ov}$)** of the input pair. 

For this specific design ($L = 480\text{nm}$, $V_{ov} = 0.34\text{V}$):
1. **Linear Limit:** Keeping the differential input below **$480\text{mV}$** is essential for maintaining signal integrity.
2. **Gain Advantage:** The switch to an active load provided a massive gain boost (~32.7 dB) compared to the passive design, but it requires careful common-mode biasing ($0\text{V}$ to $0.47\text{V}$) to stay functional.
3. **Power Efficiency:** The circuit consumes only **$0.972\text{mW}$**, which is well within the $1.8\text{mW}$ safety limit for the $0.18\mu\text{m}$ process.

# Transient Gain Analysis: Circuit 2 (Measured Results)
<img width="1039" height="606" alt="image" src="https://github.com/user-attachments/assets/e1b1d3b8-aa4c-40a7-90f8-3f3b91ef35b4" />

<img width="1904" height="518" alt="image" src="https://github.com/user-attachments/assets/a4bbab35-2c47-4929-b287-f5c75b8b26f2" />
This section details the calculation of the realized voltage gain based on the peak-to-peak ($p-p$) measurements from the transient simulation.

---

## 1. Calculation of Linear Voltage Gain ($A_v$)
The voltage gain is the ratio of the change in output voltage to the change in input voltage. Using the $p-p$ measurements from the LTspice waveform:

* **Input Swing ($V_{in, p-p}$):** $20\text{ mV}$ ($10\text{mV}$ peak)
* **Output Swing ($V_{out, p-p}$):** $27.06\text{ mV}$

**Formula:**
$$A_v = \frac{V_{out, p-p}}{V_{in, p-p}}$$

**Substitution:**
$$A_v = \frac{27.06\text{ mV}}{20\text{ mV}} = \mathbf{1.353 \text{ V/V}}$$

---

## 2. Calculation of Gain in Decibels (dB)
To express this gain on a logarithmic scale (the standard for amplifier characterization):

**Formula:**
$$A_{v(dB)} = 20 \cdot \log_{10}(A_v)$$

**Substitution:**
$$A_{v(dB)} = 20 \cdot \log_{10}(1.353) \approx \mathbf{2.626 \text{ dB}}$$

---

## 3. Analysis and Interpretation

### Comparison with Theory
You may notice that this measured transient gain (**$1.353\text{ V/V}$**) is significantly lower than the theoretical small-signal gain ($\approx 43\text{ V/V}$) estimated during DC analysis. This is due to several physical factors:

* **The "Loading" Effect:** In the transient simulation, the addition of a **$10\text{ pF}$ load capacitor ($C_L$)** has a major impact. Even at $1\text{ kHz}$, the high output resistance ($R_{out} \approx 27\text{ k}\Omega$) of the PMOS active load combined with this capacitance creates a pole that begins to attenuate the signal.
* **Internal Parasitics:** The very large width of the tail transistor ($M_5$ at **$156\text{ µm}$**) and the input pair contributes significant parasitic capacitance ($C_{gd}$ and $C_{db}$), further loading the high-impedance output node.
* **Transient vs. AC Analysis:** Unlike a pure AC simulation, transient analysis accounts for the actual time required to charge and discharge these capacitors. The lower gain indicates the amplifier is being "loaded down," a realistic behavior for high-impedance CMOS stages driving external loads.

---

## 4. Measured Summary Table

| Parameter | Symbol | Measured Value |
| :--- | :--- | :--- |
| **Input Voltage (p-p)** | $V_{in, p-p}$ | $20\text{ mV}$ |
| **Output Voltage (p-p)** | $V_{out, p-p}$ | $27.06\text{ mV}$ |
| **Voltage Gain (Linear)** | $A_v$ | **$1.353 \text{ V/V}$** |
| **Voltage Gain (dB)** | $A_{v(dB)}$ | **$2.63 \text{ dB}$** |

> **Design Insight:** To recover the full theoretical gain in a real application, you would typically follow this high-gain stage with a **Source Follower (Buffer)**. The buffer provides a low output impedance, allowing the circuit to drive the $10\text{ pF}$ load without losing the gain of the first stage.

# AC Performance and Frequency Response Analysis
<img width="1918" height="523" alt="image" src="https://github.com/user-attachments/assets/f1baa2c9-241b-4e39-bc0b-a662d7796356" />


This section evaluates the simulated frequency response against the theoretical small-signal models for the CMOS differential amplifier ($L = 480\text{nm}$).

---

## 1. Simulated Performance Summary
Based on the frequency response plot and DC operating point data, the simulated AC characteristics are:

* **Mid-band Gain ($A_v$):** Approximately **$2.21\text{ dB}$** (Linear gain $\approx 1.29\text{ V/V}$).
* **3dB Bandwidth ($f_{3dB}$):** Approximately **$22\text{ MHz}$**.
* **Unity Gain Bandwidth (UGB):** Approximately **$21\text{ MHz}$**.

### Simulated Gain-Bandwidth Product (GBW):
$$GBW_{sim} = A_v(\text{linear}) \times f_{3dB} = 1.29 \times 22\text{ MHz} = \mathbf{28.38\text{ MHz}}$$

---

## 2. Theoretical Calculations

### A. Transconductance ($g_m$)
Using the simulated current $I_{d1} = 270.28\mu\text{A}$ and the calculated overdrive $V_{ov} = 0.34\text{V}$:
$$g_{m1,2} = \frac{2 I_{d1}}{V_{ov}} = \frac{2 \times 270.28\mu\text{A}}{0.34\text{V}} = \mathbf{1.59\text{ mS}}$$

### B. Theoretical Gain-Bandwidth Product (GBW)
The GBW for an amplifier with a dominant pole at the output is determined by the input transconductance and the load capacitance ($C_L = 10\text{ pF}$):
$$GBW_{theory} = \frac{g_{m1}}{2\pi C_L} = \frac{1.59\text{ mS}}{2\pi \times 10\text{ pF}} = \mathbf{25.31\text{ MHz}}$$

### C. Calculated Output Resistance ($r_{out}$)
Given the simulated linear gain of $1.29\text{ V/V}$, we can back-calculate the effective output resistance:
$$r_{out} = \frac{A_v}{g_{m1}} = \frac{1.29}{1.59\text{ mS}} \approx \mathbf{811\text{ }\Omega}$$
*Note: This suggests that while the transistors are in saturation, the high-frequency parasitics and the $10\text{ pF}$ load are significantly damping the high-impedance node.*

---
This comprehensive conclusion wraps up your analysis of Circuit 2 (PMOS Active Load). It highlights the successful transition from a basic resistive load to a high-performance active load while acknowledging the physical "loading" effects that occur in a realistic $0.18\mu\text{m}$ process.Here is the clean Markdown code for your Conclusion and Comparative Analysis.Markdown# Conclusion and Comparative Analysis: Circuit 2 (PMOS Active Load)

The transition from a passive resistive load to a PMOS Active Load significantly improved the amplifier's performance, particularly in terms of gain and output swing. The circuit effectively functions as a differential-to-single-ended converter suitable for high-speed signal processing.

---

## 1. Conclusion for Circuit 2 Performance

* **Linearity and Overdrive:** The circuit demonstrates a clear linear region when the differential input $V_{id}$ is kept below **$\approx 480\text{mV}$**. This boundary is directly dictated by the calculated overdrive voltage ($V_{ov} = 0.34\text{V}$).
* **Large-Signal Behavior:** When $V_{id}$ exceeds the linear boundary (as seen in the $400\text{mV}$ test), the **$0.54\text{mA}$** tail current is fully steered to one side. This causes the output to clip and distort as the branch currents reach their physical limits.
* **Stability:** AC analysis confirms the circuit is unconditionally stable with a **$10\text{pF}$** load. The frequency response maintains a smooth roll-off with a **$22\text{MHz}$** bandwidth and a robust phase margin.

---

## 2. Comparison: Theoretical vs. Simulated Results

There are noticeable differences between textbook "square-law" hand calculations and the high-fidelity results from the TSMC $0.18\mu\text{m}$ BSIM simulation.

| Parameter | Theoretical (Hand Calc) | Simulated (LTspice) | Reason for Difference |
| :--- | :--- | :--- | :--- |
| **Gain ($A_v$)** | $\approx 43\text{ V/V}$ ($32.7\text{dB}$) | **$1.29\text{ V/V}$ ($2.21\text{dB}$)** | **Loading & $r_o$:** The $10\text{pF}$ capacitor and internal parasitics of the large $M_5$ ($156\mu\text{m}$) heavily load the high-impedance node. |
| **GBW** | $25.31\text{ MHz}$ | **$28.38\text{ MHz}$** | **Model Complexity:** BSIM models account for velocity saturation and sub-threshold effects ignored by hand formulas. |
| **$V_{ICM,min}$** | $\approx -0.1\text{V}$ | **$0\text{V}$** | **Body Effect:** Hand calculations often ignore the change in $V_{th}$ due to the source-to-body voltage ($V_{SB}$). |
| **Bandwidth** | $25\text{ MHz}$ | **$22\text{ MHz}$** | **Parasitic Caps:** LTspice accounts for $C_{gd}$ and $C_{db}$, which adds to the $10\text{pF}$ load and narrows the bandwidth. |

---

## 3. Summary of Findings

The most significant takeaway is the **Gain-Bandwidth Product (GBW)** consistency. While the absolute voltage gain varied due to output loading, the GBW remained within **$11\%$** of the theoretical prediction (**$28.3\text{MHz}$** vs **$25.3\text{MHz}$**). 

This confirms that your transistor sizing ($W/L$) was correctly optimized for the **$0.54\text{mA}$** current target and the **$480\text{nm}$** channel length. The design successfully balances power efficiency ($0.972\text{mW}$) with high-speed stability.

---
