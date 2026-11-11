# Design of a Bandgap Voltage Reference Circuit

A temperature-independent voltage reference producing approximately 1.2 V, designed for use in voltage regulators, ADCs, DACs and power management ICs.

## Overview

Every analog and mixed-signal system needs a voltage it can trust. The problem is that almost nothing on a die holds still — a diode drop, a resistor divider, a transistor bias point all shift with temperature, supply voltage and process variation. An ADC referenced to a drifting voltage reports drifting numbers.

A bandgap reference solves this by cancelling drift against drift. Two voltages with opposite temperature behaviour are scaled and summed so their coefficients cancel, leaving a voltage near the silicon bandgap energy — roughly 1.2 V — that stays stable across the operating range.

## Operating principle

**CTAT — Complementary To Absolute Temperature**

The base-emitter voltage of a diode-connected BJT falls as temperature rises, giving a negative temperature coefficient:

$$V_{BE} = V_T \ln\left(\frac{I_C}{I_S}\right), \qquad V_T = \frac{kT}{q}$$

**PTAT — Proportional To Absolute Temperature**

Two matched BJTs run at different current densities, set by an emitter area ratio *n*. The difference in their base-emitter voltages rises linearly with temperature:

$$\Delta V_{BE} = V_T \ln(n)$$

This is converted to a current through resistor R0:

$$I_{PTAT} = \frac{\Delta V_{BE}}{R_0}$$

**Combining the two**

Scaling the PTAT term by a factor K set by the resistor ratios and summing it with the CTAT term gives:

$$V_{REF} = V_{BE} + K \cdot \Delta V_{BE}$$

K is chosen so the opposing slopes cancel at the operating point:

$$\frac{dV_{REF}}{dT} = 0$$

**Current mirrors**

Matched PMOS and NMOS mirrors copy the reference current into each branch, holding the PTAT and CTAT contributions in proportion and reducing sensitivity to supply variation:

$$I_{OUT} = I_{REF} \cdot \frac{(W/L)_{OUT}}{(W/L)_{REF}}$$

## Architecture

The design is organised into four functional blocks:

| Block | Function |
|---|---|
| CTAT generator | Diode-connected BJT (Q0) supplying V<sub>BE</sub> with a negative temperature coefficient |
| PTAT generator | Q1 and Q2 at different emitter areas, producing ΔV<sub>BE</sub> across R0 |
| Current mirror | PMOS sources at the top with NMOS mirroring, holding branch currents equal |
| Summing stage | Resistor-scaled combination of PTAT and CTAT components producing V<sub>REF</sub> |

Resistors R0 and R1 set the weighting between the two temperature-dependent components, which is the degree of freedom used to null the overall temperature coefficient.

## Why not a Zener reference

Conventional Zener-based references are simple but need a high supply voltage, draw more current, and still drift appreciably with temperature and process. Those constraints rule them out of low-voltage, low-power CMOS designs. The bandgap approach trades circuit complexity for stability, lower supply headroom, and far better temperature behaviour.

## Results

Simulation results covering the CTAT output, the PTAT output, the combined response, and the final reference voltage are presented in the full design report.

## Repository contents

```
bandgap-voltage-reference/
├── report.pdf       # full design paper
├── schematic/       # circuit schematics
├── results/         # simulation plots
└── README.md
```

## Future work

- **Curvature compensation** to correct the residual second-order temperature nonlinearity that remains after first-order cancellation
- **Lower supply operation** in advanced CMOS nodes, for wearable and IoT power budgets
- **Digital trimming and calibration** to compensate process variation after fabrication
- **SoC integration** within intelligent power management units

## Authors

**Kunche Gowtham** (22R21A0498) and **Bojja Ravi Chandra Prasad** (22R21A0471)
Department of Electronics and Communication Engineering, MLR Institute of Technology (Autonomous), Hyderabad

Under the guidance of **Mr. K. Maniraj**, Department of Electronics and Communication Engineering, MLRIT.

## References

1. P. R. Gray, P. J. Hurst, S. H. Lewis, R. G. Meyer, *Analysis and Design of Analog Integrated Circuits*, 5th ed., Wiley, 2009.
2. B. Razavi, *Design of Analog CMOS Integrated Circuits*, McGraw-Hill, 2017.
3. K. N. Leung, P. K. T. Mok, "A CMOS voltage reference based on weighted ΔV<sub>BE</sub> for CMOS low-dropout regulators," *IEEE JSSC*, vol. 38, no. 1, pp. 146–150, 2003.
4. G. A. Rincon-Mora, P. E. Allen, "A 1.1-V current-mode and piecewise-linear curvature-corrected bandgap reference," *IEEE JSSC*, vol. 33, no. 10, pp. 1551–1554, 1998.
5. C.-W. U, C. Liu, R. P. Martins, C.-S. Lam, "A 740-nW 8.7-ppm/°C bandgap voltage reference with curvature compensation," *IEEE TCAS-I*, vol. 70, no. 4, pp. 1502–1511, 2023.
6. J. Zou, Q. Wei, C. Ju, "An ultra-low noise bandgap voltage reference for precision sensor applications," *Microsystems & Nanoengineering*, vol. 9, 2023.
7. H. Zhuang, X. Chen, Q. Li, "High-accuracy bandgap reference design techniques: A review," *Chips*, vol. 4, no. 1, pp. 1–18, 2025.
8. J. Baker, *CMOS: Circuit Design, Layout, and Simulation*, 4th ed., Wiley-IEEE Press, 2019.
9. Y. Tsividis, *Operation and Modeling of the MOS Transistor*, 3rd ed., Oxford University Press, 2011.
