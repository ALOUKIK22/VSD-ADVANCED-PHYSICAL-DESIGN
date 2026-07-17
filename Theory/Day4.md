# Day-4 : Pre-Layout Timing Analysis

Day 4 focused on **Pre-Layout Static Timing Analysis (STA)** and the integration of the custom standard cell into the OpenLANE flow. The objective was to generate the LEF for the custom inverter, incorporate it into the design, and analyze the timing performance before routing. Using **OpenSTA**, the critical timing paths were identified, timing constraints were applied, and various optimization techniques such as synthesis strategy selection, buffering, fanout reduction, and cell replacement were performed to improve the overall timing performance before proceeding to Clock Tree Synthesis (CTS).

# Timing Analysis

## What is Timing Analysis?

Timing analysis is the process of checking whether a digital circuit can operate correctly at the desired clock frequency. It calculates the delay of signals as they travel from one register to another through combinational logic.

The main objective is to ensure that data reaches the destination flip-flop before the next clock edge. If the delay is too large, incorrect data may be captured, leading to timing violations.

---

# Ideal Clock

An **ideal clock** is an imaginary clock used during the early stages of physical design. It is assumed to reach every flip-flop at exactly the same time without any delay or distortion.

Characteristics of an ideal clock:

- Zero clock latency
- Zero clock skew
- No clock jitter
- Perfect clock waveform
- Used before Clock Tree Synthesis (CTS)

Since the actual clock network has not yet been built, ideal clocks simplify timing analysis and help designers estimate whether the design can meet timing requirements.

---

# Timing with an Ideal Clock

<img width="1347" height="672" alt="Screenshot 2026-07-14 104251" src="https://github.com/user-attachments/assets/39d7b156-e1e7-480f-b704-a1c06718cb62" />

Consider two flip-flops connected through combinational logic.

```
Flip-Flop 1 → Combinational Logic → Flip-Flop 2
```

When Flip-Flop 1 launches data on a clock edge, the data passes through the combinational logic and must arrive at Flip-Flop 2 before the next clock edge.

If

**θ = Propagation Delay of the data path**

and

**T = Clock Period**

then the basic timing condition is

```
θ < T
```

where,

- **θ** = Total propagation delay from the launching flip-flop to the capturing flip-flop.
- **T** = Time between two consecutive clock edges.

If this condition is satisfied, the data reaches the destination before the next clock edge and is captured correctly.

If

```
θ > T
```

the data arrives late, causing a **setup timing violation**, and the flip-flop may capture incorrect data.

<img width="1068" height="476" alt="Screenshot 2026-07-14 104609" src="https://github.com/user-attachments/assets/da5818d4-6231-4ec7-8cda-bf98d1bf9f8d" />


---

# Clock Jitter

Clock jitter is the small variation in the arrival time of the clock edge from one clock cycle to another.

Ideally, every clock edge should occur at fixed intervals. However, due to noise, power supply fluctuations, and imperfections in the clock generation circuitry, the clock edge may arrive slightly earlier or later than expected.

Clock jitter reduces the available time for data propagation and therefore makes timing closure more difficult.

A clock with low jitter provides more reliable and predictable circuit operation.


<img width="516" height="678" alt="Screenshot 2026-07-14 104959" src="https://github.com/user-attachments/assets/24933751-9212-44d8-b4c3-9ceffb8420ff" />

---

# Why Ideal Clocks are Used

Using an ideal clock during the initial stages of physical design helps designers:

- Perform early timing analysis.
- Identify slow data paths.
- Estimate the maximum operating frequency.
- Detect timing violations before clock tree synthesis.
- Simplify timing calculations by ignoring clock network delays.

- ![Uploading Screenshot 2026-07-14 105441.png…]()


After Clock Tree Synthesis (CTS), the ideal clock is replaced by the real clock network, and timing analysis is repeated considering clock latency, skew, and other practical effects.
