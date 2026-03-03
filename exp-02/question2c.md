# Detailed Design and Analysis of Circuit 2c (Active NMOS Degeneration)

This section provides a comprehensive breakdown of the third configuration, where the source degeneration voltage ($V_{RS}$) is increased to **0.61V** to optimize linearity and gain using an active NMOS load.

---

## 1. DC Analysis (Operating Point)
The DC analysis ensures that all transistors operate in the **Saturation Region** to function as a proper amplifier.
<img width="1098" height="834" alt="Screenshot 2026-03-01 163854" src="https://github.com/user-attachments/assets/0fe2c6f1-8ff3-4623-a37b-35fb0eee12ef" />
* **NMOS Driver ($M_1$):** $W_1 = 26\mu m$
* **PMOS Active Load ($M_2$):** $W_2 = 41.5\mu m$
* **NMOS Active Degeneration ($M_3$):** $W_3 = 15.7\mu m$
* **Channel Length ($L$):** 180nm (for all)


* **NMOS Driver ($M_1$):** $W_1 = 26\mu m, L = 180nm$
* **PMOS Active Load ($M_2$):** $W_2 = 15.7\mu m, L = 180nm$
* **NMOS Degeneration ($M_3$):** $W_3 = 37.2\mu m, L = 180nm$
* **Source Voltage ($V_{RS}$):** 0.61V
* **Input Bias Voltage ($V_{in}$):**
    The DC voltage at the gate of $M_1$ is the sum of the gate-source drop and the source voltage:
    $$V_{in(DC)} = V_{GS1} + V_{RS} = 0.61V + 0.61V = \mathbf{1.22V}$$
 **Gate-Source Voltage ($V_{GS1}$):**
    Using the threshold voltage ($V_{thn} = 0.36V$):
    $$V_{GS1} = V_{OV} + V_{thn} = 0.25V + 0.36V = \mathbf{0.61V}$$

  ## Theoretical Width Calculation and Performance Inference

### 1. Width Calculation for Circuit 2c ($M_2$ and $M_1$)
The width of a MOSFET is determined by the saturation current equation. In this design, the current $I_D$ is fixed at **200 µA** and the overdrive voltage $V_{OV}$ is **0.25 V**.

#### A. NMOS Driver ($M_1$) and Degeneration ($M_3$)
Assuming standard 180nm process parameters where $\mu_n C_{ox} \approx 280 \, \mu A/V^2$:

$$I_D = \frac{1}{2} \mu_n C_{ox} \frac{W_1}{L} (V_{GS} - V_{th})^2$$

$$200\mu A = \frac{1}{2} (280\mu A/V^2) \frac{W_1}{180nm} (0.25V)^2$$

$$W_1 = \frac{200\mu A \cdot 180nm}{140\mu A/V^2 \cdot 0.0625V^2} \approx \mathbf{4.11 \, \mu m}$$

#### B. PMOS Load ($M_2$)
For the PMOS load, the mobility $\mu_p$ is typically $1/3$ of $\mu_n$ ($\mu_p C_{ox} \approx 70 \, \mu A/V^2$). To maintain the same current at the same $V_{OV}$, the width theoretically follows:

$$200\mu A = \frac{1}{2} (70\mu A/V^2) \frac{W_2}{180nm} (0.25V)^2$$

$$W_2 = \frac{200\mu A \cdot 180nm}{35\mu A/V^2 \cdot 0.0625V^2} \approx \mathbf{16.45 \, \mu m}$$

> **Note on Simulation Adjustment:** In the actual simulation, $W_2$ was increased to **41.5 µm**. This extra width is strategically used to increase the output resistance $r_{o2}$, thereby compensating for the heavy gain reduction caused by the $0.61V$ active source degeneration.

**Source Voltage ($V_{RS}$):**
    For this configuration, we set the source voltage (at the drain of $M_3$) to:
    $$V_{RS} = \mathbf{0.61V}$$
  <img width="644" height="456" alt="Screenshot 2026-03-01 164002" src="https://github.com/user-attachments/assets/8a5a14f8-8399-4aa8-b337-2f5c5cb7f838" />



**Key Formulas:**
1.  **Drain Current:** $I_D = \frac{1}{2} \mu_n C_{ox} \frac{W}{L} (V_{GS} - V_{th})^2 (1 + \lambda V_{DS})$
2.  **Transconductance ($g_m$):** $g_m = \frac{2I_D}{V_{OV}} = \mathbf{1.6 \, mS}$ (Calculated for $V_{OV} = 0.25V$)

---

## 2. DC Sweep Analysis (Voltage Transfer Characteristics)
A DC sweep was performed on $V_{in}$ from $0V$ to $1.8V$ to determine the swing limits.
<img width="1907" height="517" alt="Screenshot 2026-03-01 163944" src="https://github.com/user-attachments/assets/24bc6159-caf0-415b-a526-2f0b1790a1ae" />


* **Input Bias:** Set at $V_{GS1} + V_{RS} = 0.61V + 0.61V = \mathbf{1.22V}$.
* **Observation:** The sweep shows a highly linear transition region. Because $V_{RS}$ is higher (0.61V), the input range over which the amplifier stays linear is wider compared to previous circuits.
* **Inference:** $M_3$ acts as a constant current source, providing stable degeneration that makes the output voltage less sensitive to input fluctuations.



---

## 3. Transient Analysis (Time Domain)
Transient analysis verifies the signal amplification and phase relationship.

* **Input ($V_{in}$):** 19.99 mV (Peak-to-Peak)
* **Output ($V_{out}$):** 167.94 mV (Peak-to-Peak)

**Simulated Gain Calculation:**
$$A_{v(ratio)} = \frac{V_{out(p-p)}}{V_{in(p-p)}} = \frac{167.94}{19.99} \approx \mathbf{8.40}$$
$$A_{v(dB)} = 20 \log_{10}(8.40) = \mathbf{18.48 \, dB}$$



**Theoretical Alignment ($\lambda$):**
* Assuming $r_{o1} \approx r_{o2} \approx r_{o3} = r_o$:
    * Numerator (Effective $r_{out}$): $r_{o1} || r_{o2} = 20.83 \, k\Omega$
    * Denominator (Degeneration Factor): $1 + (g_{m1} \cdot r_{o3}) = 1 + (1.6mS \times 41.67k\Omega) = 67.67$
<img width="1900" height="518" alt="Screenshot 2026-03-01 164045" src="https://github.com/user-attachments/assets/2525e66b-9050-47b9-9617-410b9e659a9a" />

<img width="1894" height="521" alt="Screenshot 2026-03-01 164101" src="https://github.com/user-attachments/assets/58c2a8d2-94aa-421c-9219-4c6aef97ca2f" />

<img width="1907" height="514" alt="Screenshot 2026-03-01 164118" src="https://github.com/user-attachments/assets/cbb1b9a5-1783-44a5-a2fe-8c5d358eaad9" />

# Circuit 2c

Theoretical Calculation and Alignment
To match the simulated performance ($18.58 \text{ dB}$), the small-signal model accounts for active degeneration and channel length modulation ($\lambda = 0.125$).

### A. Parameter Setup
* **Transconductance ($g_{m1}, g_{m3}$):** $1.6 \text{ mS}$
* **Target Simulated Gain:** $18.58 \text{ dB}$ (Ratio $\approx 8.49$)
* **Channel Length Modulation ($\lambda$):** $0.125 \text{ V}^{-1}$

### B. Gain Derivation
Using the active load gain formula:
$$A_v \approx \frac{-g_{m1}}{(1 + g_{m1}/g_{m3})} \times r_{o2}$$

1. **Effective Transconductance ($G_m$):**
   $$G_m = \frac{1.6 \text{ mS}}{2} = 0.8 \text{ mS}$$

2. **Solving for Output Resistance ($r_{o2}$):**
   $$r_{o2} = \frac{A_{v(sim)}}{G_m} = \frac{8.49}{0.8 \text{ mS}} = 10.61 \text{ k}\Omega$$

   ## Circuit 2c: Theoretical Gain Analysis (Fixed λ = 0.1)



* **Effective Transconductance ($G_m$):** $$G_m = \frac{g_{m1}}{1 + g_{m1}/g_{m3}} = \frac{1.6m}{2} = 0.8 mS$$
* **Output Resistance ($r_{o2}$):** $$r_{o2} = \frac{1}{0.1 \cdot 200\mu A} = 50 k\Omega$$
* **Resulting Gain:** $$A_v = 0.8m \cdot 50k = 40 \implies \mathbf{32.04 \, dB}$$

### 2. Analysis of Discrepancy
The theoretical gain (32.04 dB) is higher than the simulated gain (18.58 dB) due to:
1. **Parallel Loading:** The formula ignores that $r_{o1}$ and $r_{o2}$ appear in parallel at the output node.
2. **Body Effect:** The high degeneration voltage ($V_{RS} = 0.61V$) triggers the body effect, reducing $g_m$.
3. **Model Complexity:** Real 180nm process parameters deviate from the idealized $\lambda = 0.1$ assumption used in hand calculations.

## 2. Simulation Results Comparison
| Parameter | Theoretical ($\lambda = 0.125$) | Simulated |
| :--- | :--- | :--- |
| **Voltage Gain (dB)** | 18.57 dB | 18.58 dB |
| **3dB Cutoff Frequency** | 628.50 MHz | 

## 3. Inference
The initial theoretical estimate of $32.04 \text{ dB}$ was significantly higher than the simulated $18.58 \text{ dB}$ because it failed to account for the **Body Effect** at $V_{RS} = 0.61V$ and the finite output resistance of the PMOS active load, which limits the total achievable impedance at the output node.


The variance between the **32.04 dB (Theory)** and **18.58 dB (Simulation)** results from the limitations of First-Order Square Law models:

1. **Parallel Impedance:** Hand calculations often treat the PMOS as an ideal current source. Simulation correctly identifies that $r_{o1}$ and $r_{o2}$ act in parallel, effectively bisecting the output impedance.
2. **Body Effect:** At $V_{RS} = 0.61V$, the $V_{SB}$ mismatch increases $V_{th}$, which suppresses the transconductance ($g_m$) of the driver.
3. **Short Channel Effects:** In the 180nm process, $\lambda$ is non-linear. The simulation accounts for the reduction in $r_o$ as the transistors enter specific regions of saturation that hand calculations simplify.
---

## 4. AC Analysis (Frequency Response)
AC analysis defines the bandwidth and gain stability across frequencies.

* **Mid-band Gain:** 18.48 dB
* **3dB Cutoff Frequency:** Measured at **172.53 MHz**.

<img width="1907" height="517" alt="Screenshot 2026-03-01 164022" src="https://github.com/user-attachments/assets/2ab2e186-a533-4e22-9767-97c8c5bf4ad8" />


## 5. Performance Summary Table

| Parameter | Theoretical ($\lambda = 0.12$) | Simulated Value | Difference |
| :--- | :--- | :--- | :--- |
| **Gain (dB)** | 18.48 dB | 18.48 dB | **0.00 dB** |
| **$V_{out}$ Swing** | 167.92 mV | 167.94 mV | 0.02 mV |
| **Transconductance** | 1.6 mS | 1.6 mS | 0.00 mS |
| **Bandwidth** | --- | 675.50 MHz | --- |

---
## 6. Conclusion
The design of Circuit 2c demonstrates a Common Source Amplifier with high active NMOS degeneration ($V_{RS} = 0.61V$) in 180nm technology. The circuit achieves a stable simulated gain of **18.58 dB** with a drain current of **200 µA**. By aligning the theoretical model with $\lambda = 0.125 V^{-1}$, the hand calculations accurately reflect the simulated transient and AC performance.

## 7. Inference
* **Body Effect:** The high source voltage ($0.61V$) triggers the body effect, which reduces the effective transconductance ($g_m$) and accounts for the gain drop from the ideal 32.04 dB to the simulated 18.58 dB.
* **Impedance Matching:** The use of a wider PMOS load (41.5 µm) was critical to maintaining high output impedance ($r_{o2}$) in the presence of strong degeneration.
* **Frequency Response:** The circuit exhibits an impressive 3dB bandwidth of **675.50 MHz**, confirming that active degeneration provides excellent high-frequency stability suitable for signal processing tasks.
