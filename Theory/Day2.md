# Day-2 : Floorplanning and Introduction to Library Cells

Day 2 marked the beginning of the **Physical Design (PD) flow** with the **Floorplanning** stage, also referred to as **Design Planning**. Before initiating floorplanning, several sanity checks are performed to verify the quality of the synthesized netlist and timing constraints. This process, known as **Netlist Quality Analysis (Netlist QA)**, helps identify issues in the netlist or constraints and reports them back to the synthesis team for correction. Detecting these problems early is essential, as fixing them becomes significantly more difficult and time-consuming in the later stages of the physical design flow.

Constraint validation can be performed using the **Synopsys Galaxy Constraint Analyzer (GCA)**.

---

##  Chip Floorplan Considerations

After completing the Netlist QA and sanity checks, the floorplanning stage begins. This stage consists of several important design considerations.

###  Defining the Aspect Ratio

The first step is to determine the **core** and **die** dimensions by selecting an appropriate **aspect ratio**, defined as:

```text
Aspect Ratio = Core Height / Core Width
```

The aspect ratio depends on the total area occupied by the synthesized standard cells. Another important parameter is the **utilization factor**, which represents the ratio of the standard-cell area to the total core area.

A utilization of approximately **50–60%** is generally preferred to leave sufficient free space for placement optimization, buffering, and routing during the later stages of the design flow. Selecting an appropriate utilization helps achieve an efficient floorplan.

---

### Placement of Pre-Placed Cells

The next step is to determine the locations of **pre-placed cells**, such as analog blocks, memory macros, and hard IPs. These blocks are fixed before the standard-cell placement stage because they are often timing-critical or sensitive to noise.

Large combinational logic blocks containing thousands of gate instances can also be partitioned into smaller modules using logical cuts. This simplifies the design hierarchy and improves placement efficiency. When the same logic is reused multiple times, these blocks can be **black-boxed** and instantiated as reusable modules in the design.


<img width="797" height="552" alt="37" src="https://github.com/user-attachments/assets/8f9847bd-03ba-4cb3-a59a-f6a559fbdb24" />

## Defining Chip Size and Core Area

The chip consists of two regions:

- **Core Area:** Contains all the standard cells that implement the digital logic.
- **Die Area:** The complete chip area, including the core, I/O pads, and boundary region.

The core size is determined based on:
- Number of standard cells
- Area occupied by the cells
- Target utilization (typically 50–70%)
- Space required for routing

Choosing a very small core leads to congestion, while a very large core wastes silicon area.

<img width="1817" height="1078" alt="Screenshot 2026-07-10 213508" src="https://github.com/user-attachments/assets/a934f75c-ca1e-4d45-b123-261b4741275d" />


---

# Standard Cells

## Introduction to Standard Cells

Standard cells are pre-designed and pre-verified logic building blocks used to implement digital circuits. Examples include:

- Inverter
- NAND Gate
- NOR Gate
- Flip-Flop
- Multiplexer

Each standard cell has a fixed height but may have different widths depending on its functionality and drive strength.

Using standard cells simplifies chip design since they are already optimized for speed, power, and area.

---

# Decoupling Capacitors

## Why are Decoupling Capacitors Needed?

Whenever a large number of transistors switch simultaneously, they require a sudden burst of current. Due to the resistance and inductance of the power network, the required current may not reach the cells instantly.

To solve this problem, **decoupling capacitors (decaps)** are placed close to the standard cells.

They act like small local energy storage elements:
- Store electrical charge.
- Supply current immediately when needed.
- Recharge after switching activity.

This helps maintain a stable supply voltage and reduces delay caused by sudden current demand.

<img width="1918" height="1078" alt="Screenshot 2026-07-10 215525" src="https://github.com/user-attachments/assets/d50e42f7-0769-4e80-956a-a0c99386724c" />

<img width="1757" height="1073" alt="Screenshot 2026-07-10 220230" src="https://github.com/user-attachments/assets/ebab4f72-0ac3-4332-99e0-b4c8c327c369" />

<img width="1515" height="1031" alt="Screenshot 2026-07-11 091619" src="https://github.com/user-attachments/assets/e393c42b-74f8-4885-90d6-53f8f6fb9c95" />

---

# 16-Bit Data Bus

## Current Demand in a Data Bus

Consider a **16-bit data bus**, where each wire carries one bit of data. During a clock transition, all 16 lines may switch simultaneously from **0 to 1** or **1 to 0**.

When this happens:

- Every switching gate requires current from the power supply.
- The total current demand increases significantly because multiple gates switch at the same instant.
- The power distribution network may not be able to deliver this current immediately due to its inherent resistance and inductance.

As a result, the supply voltage can momentarily drop, and the ground voltage can momentarily rise. These effects are known as **Voltage Droop** and **Ground Bounce**.

To reduce these problems, **decoupling capacitors** are placed close to the standard cells. They act as small local charge reservoirs, supplying current instantly during switching until the power network catches up.

<img width="1496" height="876" alt="Screenshot 2026-07-11 092201" src="https://github.com/user-attachments/assets/88913391-7fb7-4b75-ac0b-b0b8e454b47a" />


# Voltage Droop and Ground Bounce

## Voltage Droop

Voltage droop occurs when the supply voltage temporarily decreases because many gates switch at the same time and draw large current.

A lower supply voltage makes transistors switch more slowly, which can increase circuit delay.

---

## Ground Bounce

Ground bounce occurs when the ground voltage rises above its ideal value during heavy switching.

This changes the effective voltage seen by the transistors and may lead to incorrect logic levels or timing errors.

Decoupling capacitors help reduce both voltage droop and ground bounce.


<img width="1751" height="1025" alt="Screenshot 2026-07-11 092342" src="https://github.com/user-attachments/assets/6c79614b-97a8-436d-840d-1106b7289d35" />


---

# Pin Placement

## What is Pin Placement?

Pin placement determines the locations of input and output pins around the chip boundary.

A good pin placement:
- Reduces wire length.
- Minimizes routing congestion.
- Improves timing.
- Makes routing easier.

Poor pin placement can increase delays and routing complexity.

<img width="1313" height="958" alt="Screenshot 2026-07-11 093409" src="https://github.com/user-attachments/assets/aaebdd13-7566-43a8-bc33-2718f7900ebf" />

<img width="1748" height="1078" alt="Screenshot 2026-07-11 093852" src="https://github.com/user-attachments/assets/8e1624cd-4195-4c45-8fab-4ce8e7116383" />

<img width="1488" height="1067" alt="Screenshot 2026-07-11 094133" src="https://github.com/user-attachments/assets/e67ab584-b5e1-43ab-9368-233b6360cb3d" />

<img width="1683" height="1022" alt="Screenshot 2026-07-11 094801" src="https://github.com/user-attachments/assets/0f599ca0-ac6c-45f4-982f-e6024a202d2d" />

---

# Standard Cell Design Flow

<img width="1337" height="938" alt="Screenshot 2026-07-11 201450" src="https://github.com/user-attachments/assets/b6a96fa9-06ca-4f57-83e0-8fe3e198fcf3" />

<img width="641" height="858" alt="Screenshot 2026-07-11 204839" src="https://github.com/user-attachments/assets/9c11ac6f-7352-4f2e-96d9-4744fed63e4a" />


The standard cell design process generally consists of:

1. Circuit Design
2. SPICE Simulation
3. Layout Design
4. Design Rule Check (DRC)
5. Layout Versus Schematic (LVS)
6. Parasitic Extraction
7. Timing and Characterization
8. Library Generation

After characterization, the standard cell becomes part of the technology library and can be used during synthesis and physical design.

---

# Timing Constraints


<img width="1826" height="1006" alt="Screenshot 2026-07-11 212753" src="https://github.com/user-attachments/assets/77b70c9b-5bd3-41ca-b30a-5abe09680555" />


## Propagation Delay

<img width="1918" height="1072" alt="Screenshot 2026-07-11 213503" src="https://github.com/user-attachments/assets/7292150c-3e17-4b6d-9f21-d080b09be668" />


Propagation delay is the time taken for a change at the input of a gate to appear at its output.

Smaller propagation delay results in faster circuit operation.

---

## Setup Time

Setup time is the minimum time before the clock edge during which the input data must remain stable for correct operation.

---

## Hold Time

Hold time is the minimum time after the clock edge during which the input data must continue to remain stable.

---

## Clock Skew

Clock skew is the difference in arrival time of the clock signal at different flip-flops.

Large clock skew may lead to timing violations and unreliable operation.

---

## Clock Slew

Clock slew is the time taken for the clock signal to transition from logic LOW to logic HIGH (or HIGH to LOW).

A smaller clock slew results in faster switching and better timing performance.
