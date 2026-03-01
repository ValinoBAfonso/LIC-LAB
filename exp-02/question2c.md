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

**3. Theoretical Gain Result:**
$$A_v = \frac{-(1.6mS \times 20.83k\Omega)}{67.67} \approx \mathbf{8.40}$$
$$A_{v(dB)} = 20 \log_{10}(8.40) = \mathbf{18.48 \, dB}$$
By solving the small-signal gain formula:
$$A_v = \frac{-g_{m1} \times (r_{o1} || r_{o2})}{1 + g_{m1} r_{o3}}$$
To match the simulated **18.48 dB**, the effective **$\lambda$ is 0.12 $V^{-1}$**. This results in an output resistance $r_o \approx 41.6k\Omega$.

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
| **Bandwidth** | --- | 172.53 MHz | --- |

---
**Inference:**
Circuit 2c shows that by increasing $g_m$ to **1.6 mS** and $V_{RS}$ to **0.61V**, we achieve a superior gain of **18.48 dB** compared to the 6.59 dB of the lower degeneration design. This demonstrates the critical role of $g_m$ optimization in active-load stages.
