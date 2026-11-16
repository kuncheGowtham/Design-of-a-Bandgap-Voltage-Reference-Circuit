![Bandgap Voltage Reference](https://raw.githubusercontent.com/kuncheGowtham/Design-of-a-Bandgap-Voltage-Reference-Circuit/main/assets/bandgap-banner.svg)

# Design of a Bandgap Voltage Reference Circuit

A CMOS bandgap voltage reference (BGR) that combines CTAT and PTAT voltage components to produce an output of approximately 1.2 V, close to the silicon bandgap energy.

## Project overview

The design demonstrates how two temperature-dependent voltages with opposite temperature coefficients can be weighted and combined to achieve temperature compensation. It focuses on the BGR topology, biasing, current mirroring, resistor weighting, and the relationship between the CTAT and PTAT components.

## Technology and topology

The circuit uses a CMOS process with bipolar devices for the CTAT and PTAT cores:

- **CTAT core:** Diode-connected BJT Q0 supplies `V_BE`, which has a negative temperature coefficient.
- **PTAT core:** BJTs Q1 and Q2 operate at different emitter areas and generate `delta-V_BE`, which increases with temperature.
- **Current mirrors:** PMOS devices at the top act as current sources, while NMOS devices mirror and maintain equal current across branches.
- **Resistor weighting:** Resistors R0 and R1 set the weighting between the CTAT and PTAT components.
- **Output:** The combined reference is approximately 1.2 V.

## Design equations

The main relationships used in the design are:

```text
V_BE = V_T * ln(I_C / I_S),       where V_T = kT/q

delta-V_BE = V_T * ln(n),         where n is the emitter area ratio

I_PTAT = delta-V_BE / R0
I_OUT = I_REF * ((W/L)_OUT / (W/L)_REF)

V_REF = V_BE + K * delta-V_BE
```

The resistor ratio determines `K`. Temperature compensation is targeted by satisfying:

```text
dV_REF/dT = 0
```

This condition represents cancellation between the negative temperature coefficient of `V_BE` and the positive temperature coefficient of `delta-V_BE`.

## Simulation results

The project includes figures from the paper showing:

- BGR output and CTAT output
- PTAT output
- CTAT output
- Combined CTAT and PTAT output

The figures illustrate the individual temperature behaviours and how their weighted combination produces the compensated reference output.

## Future scope

- Add curvature compensation for residual temperature nonlinearity.
- Investigate operation at reduced supply voltages in advanced CMOS.
- Add digital trimming and calibration for process variation.
- Integrate the reference into an SoC power-management block.

## Authors

**Kunche Gowtham** (22R21A0498) and **Bojja Ravi Chandra Prasad** (22R21A0471)

Department of Electronics and Communication Engineering, MLR Institute of Technology (Autonomous), Hyderabad.

Under the guidance of **Mr. K. Maniraj**.
