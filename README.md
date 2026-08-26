# 8051 Microcontroller Based Unipolar to Bipolar Sinusoidal Waveform Converter

An embedded systems project implementing real-time signal conversion using the **AT89S52 microcontroller**, an **ADC0804** (Analog-to-Digital Converter), and a **DAC0808** (Digital-to-Analog Converter). 

---

## 📌 Project Overview
The primary objective of this project is to read a 2 kHz unipolar sinusoidal waveform (0–4V) through an ADC[cite: 1], process it via an 8051 microcontroller, and reconstruct it as a bipolar sinusoidal waveform (±2V) using a DAC, while analyzing processing time delays and phase shifts[cite: 1].

* **Microcontroller:** AT89S52 (8051 family)[cite: 1]
* **ADC:** ADC0804 (8-bit, ~100 µs conversion time)[cite: 1]
* **DAC:** DAC0808 (8-bit unipolar output paired with an op-amp for $\pm$2V range)[cite: 1]
* **Input Signal:** 2 kHz, 0–4V unipolar sinusoid[cite: 1]
* **Output Signal:** 2 kHz, $\pm$2V bipolar sinusoid[cite: 1]

---

## ⚙️ Working Principle & Mapping Logic

To convert a unipolar range ($0–4\text{V}$, mapped to ADC digital output $0–204$) into a bipolar range ($\pm2\text{V}$, which requires a physical DAC voltage range of $0.5\text{V}$ to $4.5\text{V}$ with a $2.5\text{V}$ mid-point offset), a linear mapping equation is applied[cite: 1]:

$$\text{DAC\_out} = \text{ADC\_in} \times m + c$$

Using boundary conditions, the offset is calculated as:
* **Offset:** $26$ (decimal)[cite: 1]
* **Conversion Formula:** $\text{DAC\_out} = \text{ADC\_in} + 26$[cite: 1]

This transformation guarantees:
* $0\text{V} \rightarrow -2\text{V}$ (Digital: $26$)[cite: 1]
* $2\text{V} \rightarrow 0\text{V}$ (Digital: $128$)[cite: 1]
* $4\text{V} \rightarrow +2\text{V}$ (Digital: $230$)[cite: 1]

---

## 📊 Time Delay & Phase Shift Analysis

The overall systemic delay is accumulated from conversion, processing, and settling components[cite: 1]:
* **ADC Conversion Time:** $100\text{ }\mu\text{s}$[cite: 1]
* **MCU Processing Time:** $5\text{ }\mu\text{s}$[cite: 1]
* **DAC Settling Time:** $1\text{ }\mu\text{s}$[cite: 1]
* **Total Time Delay ($T_d$):** $100 + 5 + 1 = 106\text{ }\mu\text{s}$[cite: 1]

For a $2\text{ kHz}$ signal (Time period $T = 500\text{ }\mu\text{s}$), the phase lag is calculated as[cite: 1]:
$$\text{Phase Delay} = \frac{106}{500} \times 360^\circ \approx 76^\circ$$

---

## 🛠️ Pin Mapping

| Pin / Port | Component Interface | Direction / Function |
| :--- | :--- | :--- |
| **P0** (8-bit) | ADC0804 Data Lines ($D_0 - D_7$) | Input[cite: 1] |
| **P0** (8-bit) | DAC0808 Data Lines ($D_0 - D_7$) | Output[cite: 1] |
| **P1.3** | ADC0804 $/CS$ (Chip Select) | Active LOW[cite: 1] |
| **P1.4** | ADC0804 $/RD$ (Read Enable) | Active LOW[cite: 1] |
| **P1.5** | ADC0804 $/WR$ (Start Conversion) | Active LOW[cite: 1] |
| **P1.6** | DAC0808 $LE$ (Latch Enable) | Active HIGH[cite: 1] |
| **P3.2** | ADC0804 $/INTR$ (End of Conversion) | Active LOW[cite: 1] |

---

## 📈 Simulation & Results
The circuit operation and waveform reconstruction were simulated and verified using the logic analyzer in **Keil $\mu$Vision**[cite: 1].

* **Output Waveform Validation:** Confirmed smooth transition across the $\pm2\text{V}$ range without amplitude distortion.
* **Phase Discrepancy:** Observed a reliable hardware-consistent phase lag of approximately $76^\circ$[cite: 1].

---

## 🚀 Future Improvements
* Transition to a higher-resolution ADC/DAC (e.g., 12-bit) to minimize quantization steps[cite: 1].
* Implement interrupt-driven ADC handling to optimize execution overhead[cite: 1].
* Employ hardware pipelining to reduce structural propagation delay[cite: 1].
