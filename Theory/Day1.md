# Day-1 : Inception of Open-Source EDA, OpenLANE and Sky130 PDK

The primary objective of Day 1 was to gain an understanding of the fundamentals of **Open-Source EDA**, **OpenLANE**, and the **Sky130 PDK**. This session laid the foundation for the complete physical design flow that was explored throughout the remainder of the workshop.

<img width="1655" height="1073" alt="A (2)" src="https://github.com/user-attachments/assets/9ac9c1c6-d103-47ca-b300-7adf46c38e37" />

Consider the execution of a simple stopwatch application. If we trace the complete flow from the beginning, the operating system processes it as shown below.

<img width="1800" height="1078" alt="a" src="https://github.com/user-attachments/assets/75822149-d812-49f8-aa8c-54d806b9c1a9" />

##  SoC using Open-Source Tools

A **System-on-Chip (SoC)** forms the core of most modern electronic systems by integrating multiple functional components onto a single chip. The essential building blocks required for ASIC design are illustrated below.

**ASIC Requirements**

The three fundamental components required for SoC development are:

- **RTL IPs**
- **EDA Tools**
- **PDK Data**

Traditionally, access to these industry-standard resources has been limited, making it difficult for students and researchers to gain practical experience with the complete VLSI design flow. The availability of **open-source RTL IPs, EDA tools, and PDKs** has significantly bridged the gap between academic learning and industry practices.

Before exploring the open-source physical design flow, it is important to understand these three key components.

### RTL IP (Register Transfer Level Intellectual Property)

RTL IPs are pre-designed and verified digital modules described at the **Register Transfer Level (RTL)**. These reusable building blocks can be integrated into larger digital systems, reducing both design effort and development time.

### EDA Tools (Electronic Design Automation)

EDA tools automate the various stages of the VLSI design flow, including synthesis, floorplanning, placement, routing, timing analysis, and verification. Automation improves design productivity, simplifies debugging, and accelerates chip development.

### PDK (Process Design Kit)

A **Process Design Kit (PDK)** acts as the interface between the semiconductor foundry and the IC designer. It contains the technology files required by EDA tools to accurately model the fabrication process.

A typical PDK includes:

- Process Design Rules (DRC, LVS)
- Standard Cell Libraries
- Timing and Delay Information
- I/O Libraries
- Technology Files
- Device Models

<img width="830" height="240" alt="38" src="https://github.com/user-attachments/assets/3b86ab28-f4c1-4255-9de9-7da97ec6573b" />

##  Floor Planning and Power Planning

**Floor Planning and Power Planning (FP/PP)** are essential stages of the physical design flow. Depending on the scope of the design, FP/PP can be performed at two levels:

- **Macro-level**
- **Chip-level**

<img width="813" height="293" alt="39" src="https://github.com/user-attachments/assets/fa31e680-fb62-424c-ae62-d2ebc776ad48" />

### Power Planning

During the **Power Planning** stage, the **Power Distribution Network (PDN)** is constructed to ensure reliable power delivery throughout the chip. A typical chip is powered through multiple **VDD** and **VSS** pins, which distribute power to all the components using **power rings** and **horizontal/vertical power straps**. A well-designed PDN minimizes voltage drop and provides stable power to the entire design.

<img width="1171" height="438" alt="40" src="https://github.com/user-attachments/assets/ed7e92a2-5109-498b-97b2-c13a818b2152" />

## Placement

Following the **Floor Planning and Power Planning (FP/PP)** stage, the design proceeds to **Placement**. During this stage, the standard cells from the synthesized gate-level netlist are positioned within the floorplan. Cells with strong connectivity are placed close to one another to minimize interconnect delay, reduce wirelength, and improve timing performance. 

<img width="858" height="345" alt="41" src="https://github.com/user-attachments/assets/a4b0ccf5-5480-4a90-ac5e-881c050bb41e" />


The first image shows the global placement and the second shows detailed placement.
The global placement tries to evaluate the most optimal positions for all cells. But these positions may not be legal, that means the cells might overlap or may go off the floorplan allocated.

The detailed placement corrects the cell-overlap issue in global placement by minimally altering the positions to make them legal.


Clock-Tree Synthesis (CTS) : Before proceeding to routing, we need to perform the CTS on the post-placement layout. The clock needs to reach multiple cells in the layout hence, to maintain the signal integrity it needs to go through stages of buffers and repeaters. This calls for an organized network to distribute the clock across the layout. This network is known as the Clock Tree.

<img width="835" height="321" alt="43" src="https://github.com/user-attachments/assets/79a580e7-8e97-430a-b413-84c7958b3907" />

## Routing

After completing **Clock Tree Synthesis (CTS)**, the design enters the **Routing** stage. During this phase, all the placed standard cells are interconnected according to the netlist by creating signal routes through metal layers. The routing tool determines an optimal path for each net while satisfying the design rules and utilizing the available routing resources. Information such as **metal pitch, track count, layer thickness, minimum wire width, via definitions, and routing rules** is obtained from the **Process Design Kit (PDK)**, enabling the router to generate a manufacturable layout.

<img width="835" height="321" alt="43" src="https://github.com/user-attachments/assets/dbc5e92b-de95-46d6-8e01-5df6cfd11f94" />


## Signoff

The **Signoff** stage is the final step of the physical design flow. After routing is completed, the final layout undergoes a series of verification checks to ensure that the design is functionally correct, manufacturable, and meets all design specifications before tape-out.

The signoff process primarily consists of the following verification stages:

### Physical Verification

- **Design Rule Check (DRC):** Ensures that the final layout complies with all the design rules specified by the Process Design Kit (PDK), including spacing, width, enclosure, and other manufacturing constraints.

- **Layout Versus Schematic (LVS):** Verifies that the extracted layout is electrically equivalent to the original gate-level netlist, ensuring that the implemented design matches the intended circuit.

### Timing Verification

- **Static Timing Analysis (STA):** Confirms that the final routed design satisfies all timing constraints by analyzing every timing path without simulation, ensuring reliable operation at the target clock frequency.

## OpenLANE Flow

OpenLANE integrates several open-source EDA tools to automate the complete **RTL-to-GDSII** physical design flow. Each stage of the flow utilizes dedicated tools for performing specific design tasks.

### Synthesis

- **Yosys** – Performs RTL synthesis to generate a gate-level netlist.
- **ABC** – Maps the synthesized netlist to the standard cells available in the target PDK. Different synthesis strategies can be selected using the integrated ABC scripts.
- **OpenSTA** – Performs Static Timing Analysis (STA) and generates timing reports for the synthesized design.
- **Fault** – Inserts scan chains for Design-for-Test (DFT) and supports Automatic Test Pattern Generation (ATPG) and test pattern compaction.

---

### Floorplanning and Power Distribution Network (PDN)

- **Init_fp** – Initializes the floorplan by defining the core area, standard-cell rows, and routing tracks.
- **IOPlacer** – Places the input and output ports around the chip boundary.
- **PDN** – Generates the Power Distribution Network to distribute power across the design.
- **Tapcell** – Inserts well-tap and decoupling capacitor (decap) cells to improve reliability.

---

### Placement

Placement is carried out in two stages:

- **Global Placement** – Standard cells are distributed across the floorplan, although overlaps may still exist.
- **Detailed Placement** – The placement is legalized by removing overlaps and aligning all cells to the standard-cell rows.

The tools involved are:

- **RePLace** – Performs global placement.
- **Resizer** – Applies optional optimization to improve timing and area.
- **OpenPhySyn** – Performs timing optimization after placement.
- **OpenDP** – Carries out detailed placement and legalizes the design.

---

### Clock Tree Synthesis (CTS)

- **TritonCTS** – Builds the clock distribution network and inserts clock buffers to minimize skew and latency.

---

### Routing

- **FastRoute** – Performs global routing and generates routing guides for the detailed router.
- **TritonRoute** – Executes detailed routing using the guides produced by FastRoute.
- **SPEF-Extractor** – Extracts parasitic resistance and capacitance information and generates the SPEF file.

---

### GDSII Generation

- **Magic** – Converts the routed DEF into the final **GDSII** layout, which is used for fabrication.

---

### Physical Verification

- **Magic** – Performs **Design Rule Check (DRC)** and **Antenna Rule Check**.
- **Netgen** – Performs **Layout Versus Schematic (LVS)** verification to ensure that the final layout matches the synthesized netlist.


