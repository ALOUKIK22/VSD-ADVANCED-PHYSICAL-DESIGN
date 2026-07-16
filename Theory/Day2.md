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
