
# Adder and Subtractor Circuit

# Op-Amp Circuit Design: Part (b)

This design implements the output function:

y₂(t) = 2x₂(t) + 6x₃(t)

## 1. Circuit Topology

Since all coefficients are positive, we use a Non-Inverting Summing Amplifier. This configuration allows us to sum the inputs and apply gain without inverting the signal phase.

### Schematic Requirements

- Op-Amp: Standard (e.g., LM741 or TL081)
- Supply Voltage (VCC): ±15 V
- Input Stage: A resistive summing network connected to the non-inverting (+) terminal.
- Gain Stage: A standard non-inverting feedback loop.

## 2. Component Selection

To achieve the specific weights of 2 and 6, we calculate the resistor values based on the non-inverting summing formula:

Vout = (1 + Rf/Rg) × ((V₁R₂ + V₂R₁)/(R₁ + R₂))

| Component | Label | Value | Description |
|------------|--------|--------|-------------|
| Resistor 1 | R₂ | 30 kΩ | Connected to input x₂(t) |
| Resistor 2 | R₃ | 10 kΩ | Connected to input x₃(t) |
| Feedback Resistor | Rf | 70 kΩ | Connected between Output and Inverting (-) pin |
| Gain Resistor | Rg | 10 kΩ | Connected between Inverting (-) pin and Ground |

## 3. Mathematical Verification

### Step 1: Voltage at Non-Inverting Pin (V+)

Using the voltage divider principle between the two inputs:

V+ = [x₂(t)·10k + x₃(t)·30k] / (10k + 30k)

V+ = (10/40)x₂(t) + (30/40)x₃(t)

V+ = 0.25x₂(t) + 0.75x₃(t)

### Step 2: Amplifier Gain (Av)

The gain of the non-inverting stage is determined by Rf and Rg:

Av = 1 + Rf/Rg

Av = 1 + 70k/10k

Av = 8

### Step 3: Final Output (y₂)

y₂(t) = Av × V+

y₂(t) = 8 × [0.25x₂(t) + 0.75x₃(t)]

y₂(t) = 2x₂(t) + 6x₃(t)

## 4. Operation Limits

Given the input values:

x₂(t) = 0.5 sin(2000πt) (Peak ±0.5 V)

x₃(t) = +1 V

Maximum Peak Output:

y₂,max = 2(0.5) + 6(1) = 7 V

Since 7 V is well within the ±15 V supply rails, the op-amp will operate in its linear region without clipping.

# Working Principle: Non-Inverting Summing Amplifier

The Non-Inverting Summing Amplifier works by combining multiple input signals into a single voltage and then amplifying that combined signal without changing its polarity (phase).

It operates in two distinct stages:

## 1. Passive Summing Stage (The "Mixer")

In this stage, the input voltages (x₁, x₂, ... xn) are connected to the Non-Inverting (+) Terminal through a network of resistors.

- No Current Flow: Because an ideal op-amp has infinite input impedance, no current flows into the (+) terminal.
- Weighted Average: The resistors form a voltage divider. The voltage at the (+) pin becomes a weighted average of all inputs.
- Phase Preservation: Since the signals enter the (+) terminal, a positive increase in input results in a positive increase in output.

## 2. Active Amplification Stage (The "Booster")

Once the inputs are mixed at the (+) terminal, the op-amp acts as a standard Non-Inverting Amplifier.

- Negative Feedback: Resistors Rf (feedback) and Rg (ground) create a feedback loop to the Inverting (-) terminal.
- Voltage Follower Logic: The op-amp automatically adjusts its output so that the voltage at the (-) terminal matches the voltage at the (+) terminal.
- Gain Multiplication: The mixed voltage at the (+) terminal is multiplied by the gain:

Av = 1 + Rf/Rg

## 3. The Final Result

The output is the mathematical product of the Mixed Input and the Gain.

Vout = Gain × Weighted Average of Inputs

# Circuit Diagram

<img width="864" height="871" alt="image" src="https://github.com/user-attachments/assets/286b5e77-f599-4ea4-a780-700b9c91d6ab" />


# DC Analysis

<img width="940" height="703" alt="image" src="https://github.com/user-attachments/assets/98fa73d9-1299-4b63-b88f-4fa7bce8744b" />


This section documents the results of the DC Operating Point (.op) simulation used to verify the circuit's accuracy against the design requirements.

### 1. Simulation Setup

The simulation was performed with the following static inputs to test the gain coefficients:

- Input x₂(t): 0 V (Reference Ground)
- Input x₃(t): +1 V (DC Source)
- Power Supply (VCC): ±15 V

### 2. DC Operating Point Data

| Parameter | Node/Variable | Simulated Value |
|------------|---------------|----------------|
| Summing Node Voltage | V(vin) | 0.74994 V |
| Final Output Voltage | V(vout) | 5.9998 V |
| Positive Rail | V(n002) | 15.0 V |
| Negative Rail | V(n003) | -15.0 V |
| Op-Amp Input Current | Ix(U1:1) | 8.007 × 10⁻⁸ A |

### 3. Mathematical Validation

The target design equation is:

y₂(t) = 2x₂(t) + 6x₃(t)

Verification Calculation:

By substituting the simulation inputs (x₂ = 0, x₃ = 1):

y₂ = 2(0) + 6(1)

y₂ = 6.0 V

# Transient Analysis

<img width="940" height="506" alt="image" src="https://github.com/user-attachments/assets/2c4c05cc-06a9-4c2d-bbaf-189c4df260d9" />


### Transient Analysis Report

The transient analysis (time-domain simulation) was performed to observe the circuit's response to a time-varying input x₂(t) combined with a constant DC offset from x₃(t).

### 1. Simulation Parameters

- Input x₂(t): 0.5 sin(2000πt) (Sine wave: 1 V peak-to-peak, frequency 1 kHz)
- Input x₃(t): +1 V (Constant DC)
- Time Scale: 0 ms to 5 ms (Shows 5 full cycles of the 1 kHz signal)

### 2. Waveform Observations

From the simulation plot of V(vout), we can verify the following:

- DC Offset (Center Line): The sine wave is centered at 6 V.
- Peak-to-Peak Amplitude: The wave fluctuates between 5 V and 7 V.
- Frequency: One full cycle every 1 ms, confirming 1 kHz operation.

### 3. Verification of the Design Equation

The output waveform follows:

y₂(t) = 2(0.5 sin(2000πt)) + 6(1)

y₂(t) = sin(2000πt) + 6

#### Mathematical vs Simulated Match

- Theoretical Max: 7 V
- Theoretical Min: 5 V
- Simulation Result: Peaks reach exactly 7.0 V and 5.0 V

# AC Analysis

<img width="940" height="507" alt="image" src="https://github.com/user-attachments/assets/13121718-1ae5-4c6a-af75-82a827581017" />


### 1. Magnitude Analysis (Solid Line)

- Mid-band Gain: Approximately 6 dB

Calculation:

20 log₁₀(2) ≈ 6.02 dB

- Bandwidth/Cut-off Frequency: Approximately 100 kHz
- Roll-off: −20 dB/decade

### 2. Phase Analysis (Dotted Line)

- Low Frequency: Phase shift ≈ 0°
- High Frequency: Phase gradually decreases and approaches −180°

# Conclusion

The objective of this design task was to implement an operational amplifier circuit capable of performing:

y₂(t) = 2x₂(t) + 6x₃(t)

## 1. Design Integrity

The Non-Inverting Summing Amplifier topology successfully performs the required weighted summation while preserving phase.

## 2. Simulation Validation

### DC Analysis

Confirmed accurate resistor ratios (10 kΩ, 30 kΩ, 70 kΩ) and output ≈ 6 V for a 1 V DC input.

### Transient Analysis

Verified output oscillation between 5 V and 7 V with a 1 kHz sine-wave input.

### AC Analysis

Verified flat-band gain of 6 dB and bandwidth exceeding 100 kHz.

## 3. Operational Performance

With a ±15 V supply, the circuit operates comfortably within the linear region. The maximum peak output of 7 V provides an 8 V safety margin before saturation.

## 4. Final Verdict

The circuit design is fully validated and meets all specified mathematical and electrical requirements. The LTspice simulation results closely match the theoretical calculations, demonstrating readiness for practical hardware implementation.
