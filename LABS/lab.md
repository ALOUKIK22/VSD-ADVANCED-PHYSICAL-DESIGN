# LAB WORK

This repository contains the lab work completed during the **5-Day VSD Physical Design Workshop**. It documents the concepts learned, commands executed, implementation flow, observations, and results obtained throughout the workshop.

---

# Day 1: Introduction to Open-Source ASIC Design Flow with OpenLANE and Sky130 PDK

## SkyWater PDK

The Sky130 Process Design Kit (PDK) used throughout this workshop is located under the `$PDK_ROOT` directory. 3 subdirectories are required for the OpenLANE physical design flow.
<img width="701" height="156" alt="1" src="https://github.com/user-attachments/assets/499fd343-b143-41e1-b990-0340474b38d8" />
## Initializing OpenLANE

The OpenLANE interactive environment is launched using the following command:

```bash
./flow.tcl -interactive
```

The `flow.tcl` script controls the complete OpenLANE design flow. Running it with the `-interactive` option enables manual execution of each stage, allowing better understanding and control over the physical design process.
## Importing the OpenLANE Package

Before executing any OpenLANE commands, the required package must be loaded into the Tcl environment using the following command:

```tcl
package require openlane 0.9
```

This command imports the OpenLANE package along with its required libraries and environment variables, enabling access to the tool's commands for the physical design flow.
<img width="710" height="254" alt="2" src="https://github.com/user-attachments/assets/ae88c2c6-cfa9-41bc-893e-2360cf26dd2b" />
## Design Directory Structure

Each design in OpenLANE follows a well-defined directory structure. The two primary components are:

- **`src/`** – Contains the Verilog source files (`.v`) and timing constraint files (`.sdc`) required for synthesis and implementation.
- *Note* - In each and every step you will be fetching of flow files for a particular location, that location needs to be created- that is design step.
- **`config.tcl`** – Stores design-specific configuration parameters that control various stages of the OpenLANE flow, including synthesis, floorplanning, placement, clock tree synthesis, routing, and timing analysis.
<img width="1093" height="406" alt="3" src="https://github.com/user-attachments/assets/f6d02c96-c952-467f-b374-68e4db5343b8" />
## Preparing the Design

Before starting the implementation flow, the design must be initialized using the `prep` command. This command creates the required directory structure, loads the design configuration, and prepares the workspace for subsequent stages of the OpenLANE flow.

```tcl
prep -design <design_name>
```

The `-design` option specifies the name of the design directory to be processed. In this workshop, the design used is **`picorv32a`**.

```tcl
prep -design picorv32a
```

After executing the command, OpenLANE creates a new **`runs/`** directory containing the generated reports, intermediate files, and implementation results for the selected design.
<img width="1118" height="257" alt="4" src="https://github.com/user-attachments/assets/74c35da3-287f-4397-b329-f26c809baaba" />
<img width="1664" height="529" alt="5" src="https://github.com/user-attachments/assets/89ef62d2-e821-4e6e-9fba-d365ed961112" />
* Adv - OpenLANE's benefit is that it can be changed on the line.
## Task 1: Calculating the Flip-Flop Ratio

As part of the synthesis analysis, the first task was to determine the **Flip-Flop Ratio**, which represents the proportion of D Flip-Flops (DFFs) with respect to the total number of cells in the synthesized design.

The Flip-Flop Ratio is calculated using the following expression:

**Flip-Flop Ratio = Number of D Flip-Flops / Total Number of Cells**

From the synthesis report:

- **Total Number of Cells:** 14,876
- **Number of D Flip-Flops:** 1,613

Therefore,

**Flip-Flop Ratio = 1613 / 14876 = 0.1084 (10.84%)**
<img width="368" height="181" alt="6" src="https://github.com/user-attachments/assets/44ac233f-28cd-4936-b4b3-a922ef5a4358" />
<img width="366" height="68" alt="7" src="https://github.com/user-attachments/assets/45d4bb1c-4d9a-4328-b07c-fa6a4904d505" />
# Day 2: Floorplanning and Introduction to Library Cells

## Floorplanning and Placement Configuration

Before executing the floorplanning stage, it is important to review the configuration parameters that control both floorplanning and placement. These variables define the physical layout of the design, including core dimensions, utilization, aspect ratio, and placement strategy.

The floorplanning and placement configuration variables are shown below.
<img width="1855" height="876" alt="8" src="https://github.com/user-attachments/assets/adb1b95f-9482-4367-8f18-bc7a88b8781e" />
## Default Floorplanning Parameters
<img width="1097" height="685" alt="9" src="https://github.com/user-attachments/assets/7984720f-580a-4e58-acc2-6e8b867e3e2d" />
## Running Floorplan


```tcl
run_floorplan
```

<img width="1826" height="890" alt="10" src="https://github.com/user-attachments/assets/ae286eb2-2852-4c6b-ab49-a3962e18376c" />

<img width="1852" height="164" alt="11" src="https://github.com/user-attachments/assets/4889381f-8262-4b7b-b658-bc76f39de70c" />
## Viewing the Generated Floorplan in Magic

After completing the floorplanning stage, the generated layout can be visualized using the **Magic Layout Editor**. To successfully load the floorplan, the following input files are required:

- **Magic Technology File (`sky130A.tech`)** – Defines the process technology and layout rules for the Sky130 PDK.
- **Floorplan DEF File** – Contains the physical floorplan information generated by OpenLANE.
- **Merged LEF File** – Provides the abstract physical descriptions of the standard cells and technology layers required to interpret the DEF file.

The floorplan can be loaded in Magic using the following command:

```bash
magic -T <path_to_sky130A.tech> \
lef read <path_to_merged.lef> \
def read <floorplan.def>
```

<img width="1446" height="791" alt="12" src="https://github.com/user-attachments/assets/9c0f6686-d3d0-4116-9b22-1fc6246f2559" />
## Running Placement and Visualizing the Layout in Magic

Once the floorplanning stage is completed, the standard cells are placed within the defined core area using the following OpenLANE command:

```tcl
run_placement
```

After placement, OpenLANE generates the placement results in the **`results/placement`** directory. The generated placement DEF file can then be loaded into the **Magic Layout Editor** to inspect the physical arrangement of the standard cells.

Use the following command to open the placement layout in Magic:

```bash
magic -T <path_to_sky130A.tech> \
lef read <path_to_merged.lef> \
def read <placement.def>
```

The Magic Layout Editor provides a graphical representation of the placed design, allowing verification of standard cell placement, cell alignment, utilization, and overall layout quality before proceeding to the subsequent implementation stages.
The placement layout can be opened in the Magic Layout Editor using the following command:

```bash
magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech 
lef read ../../tmp/merged.lef 
def read picorv32a.placement.def
```

After executing the command, Magic loads the placed design and provides a graphical view of the standard cell placement. This allows verification of cell alignment, placement density, utilization, and the overall physical organization of the design before proceeding to the next stage of the implementation flow.

The placement layout generated by the Magic tool is shown below.

<img width="1445" height="783" alt="13" src="https://github.com/user-attachments/assets/9ade4959-0725-4a66-a0fd-02d5e6ac98a2" />
# Day 3: Designing Standard Cells Using Magic and NGSpice

## Importing the CMOS Inverter Design Files

To perform custom standard cell design, the CMOS inverter layout files are obtained from the **`vsdstdcelldesign`** GitHub repository. This repository contains the layout, SPICE netlist, and supporting library files required for designing and characterizing the inverter cell.

Clone the repository using the following command:

```bash
git clone https://github.com/nickson-jose/vsdstdcelldesign
```

After cloning, the project files are available in the local workspace and can be used for layout inspection, SPICE extraction, simulation, and subsequent integration into the OpenLANE design flow.
## Inspecting the CMOS Inverter Layout in Magic

After cloning the design files, the CMOS inverter layout can be examined using the **Magic Layout Editor**. Magic provides a graphical interface for viewing the physical layout, verifying device geometry, inspecting metal layers, and checking the connectivity of the standard cell.

Launch the layout using the following command:

```bash
magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech sky130_inv.mag
```

<img width="1169" height="65" alt="14" src="https://github.com/user-attachments/assets/24783a14-3b13-46e5-81f4-614bd6aa0b05" />

Once the layout is loaded, the inverter cell can be inspected for transistor placement, routing, layer information, and overall layout structure before proceeding with extraction and simulation.

The CMOS inverter layout displayed in the Magic Layout Editor is shown below.

<img width="1449" height="787" alt="15" src="https://github.com/user-attachments/assets/b5f69e8c-0140-404b-8eec-c3faa78c26c4" />
## Examining Individual Devices in the Layout

To inspect a specific layer or device within the layout hierarchy, position the cursor over the desired object and repeatedly press **`s`** until the required hierarchy level is selected.

## Viewing Selected Object Information Using tkcon

After selecting the desired layout object in the Magic Layout Editor, the **tkcon** terminal can be used to display detailed information about the selected component.

Execute the following command in the **tkcon** terminal:

```tcl
what
```

The `what` command displays the properties of the currently selected object, including its layer, device type, connectivity, and other relevant layout information. This feature is useful for verifying the physical implementation and understanding the structure of individual layout elements.


<img width="977" height="336" alt="16" src="https://github.com/user-attachments/assets/53eeffd3-6f35-4486-b483-b97273d1bdaf" />
## Extracting the SPICE Netlist from the Layout

After verifying the CMOS inverter layout, the next step is to extract the corresponding SPICE netlist, including the parasitic information associated with the layout. This extracted netlist is later used for circuit simulation and performance analysis in **NGSpice**.

To generate the extraction file, execute the following command in the **tkcon** terminal:


<img width="982" height="85" alt="17" src="https://github.com/user-attachments/assets/daa15fb2-2a57-4daf-983b-5e14a96505e9" />


The `extract all` command analyzes the complete layout and creates an extraction (`.ext`) file containing the physical connectivity and parasitic information required for SPICE netlist generation.


<img width="981" height="72" alt="18" src="https://github.com/user-attachments/assets/cea74a67-e5a2-497b-a202-ec54598ece26" />
## Generated SPICE Netlist

After completing the extraction process, the generated SPICE netlist is saved in the **`vsdstdcelldesign`** directory. This netlist represents the extracted circuit along with the parasitic information obtained from the physical layout and serves as the input for subsequent circuit simulation using **NGSpice**.

<img width="827" height="280" alt="19" src="https://github.com/user-attachments/assets/f3aebc4b-3649-4218-a2ef-59076585090d" />
## Updating the SPICE Deck

The extracted SPICE netlist requires a few modifications before it can be simulated in **NGSpice**. These updates ensure that the simulation environment is properly configured by including the required library files, defining the power supply, setting the analysis commands, and specifying the input stimulus.

After making these changes, the SPICE deck is ready for functional and timing analysis.

<img width="742" height="662" alt="20" src="https://github.com/user-attachments/assets/b3a16854-cd0a-4da1-8c51-88a5fc4c6812" />
## Running the Updated SPICE Netlist in NGSpice

After updating the SPICE deck, the design is ready for simulation using **NGSpice**. The simulation verifies the functionality of the CMOS inverter and allows analysis of its transient response.

Launch the simulation using the following command:

```bash
ngspice sky130_inv.spice
```

Once the simulation starts, the transient waveform can be displayed using the following command within the **NGSpice** console:

```tcl
plot y vs time a
```

This command plots the output (`y`) and input (`a`) waveforms with respect to time, enabling verification of the inverter's switching behavior and timing characteristics.


<img width="1732" height="639" alt="21" src="https://github.com/user-attachments/assets/3616b150-f678-4046-bfe1-d66d1fdd2af4" />
# Day 4: Pre-Layout Timing Analysis

## Introduction

Before performing Place and Route (PnR), the physical layout created in Magic must be represented in an abstract form known as the **Library Exchange Format (LEF)**. Unlike the complete GDS layout, the LEF file contains only the essential physical information required by the PnR tool, such as cell dimensions, pin locations, routing blockages, and metal layer definitions.

Using this abstract representation enables OpenLANE to perform efficient cell placement and interconnect routing without processing the entire GDS layout.

---

## Updating the Inverter Cell

Before generating the LEF file, the custom inverter cell must be modified to comply with the routing grid defined by the technology. The **`tracks.info`** file provides information about the routing tracks available for each metal layer and serves as a reference for aligning the standard cell layout.


<img width="233" height="288" alt="22" src="https://github.com/user-attachments/assets/fec0cb85-ba73-42fb-bb11-bebb270c0efe" />

<img width="1632" height="827" alt="23" src="https://github.com/user-attachments/assets/884b79d6-a2d2-48c4-8cfe-bfddb42c9828" />
## Saving the Updated Layout

After making the required layout modifications, save the updated standard cell with a new name to preserve the original design.

Save the layout using the filename:

```text
sky130_vsdinv.mag
```

After saving, reopen the layout in the Magic Layout Editor to verify that all the modifications have been successfully stored and reflected in the updated cell.


<img width="599" height="243" alt="24" src="https://github.com/user-attachments/assets/4bf62642-e0ce-4bd0-88cd-05954596d119" />
## Generating the LEF File

Once the custom inverter layout has been verified, the next step is to generate its **Library Exchange Format (LEF)** file. The LEF file provides an abstract representation of the standard cell, containing essential physical information such as cell dimensions, pin locations, routing blockages, and metal layers. This abstraction enables the cell to be integrated into the OpenLANE physical design flow.

Generate the LEF file by executing the following command in the **tkcon** terminal:

After successful execution, the generated LEF file is created in the **`vsdstdcelldesign`** directory and can be used during placement and routing within OpenLANE.

<img width="1042" height="369" alt="25" src="https://github.com/user-attachments/assets/e6a1828d-c1fb-4ef4-9d72-62649749c613" />
After successfully generating the LEF file, it is saved in the **`vsdstdcelldesign`** directory. This LEF file serves as the abstract representation of the custom inverter cell and will be used to integrate the cell into the OpenLANE physical design flow.


<img width="621" height="252" alt="26" src="https://github.com/user-attachments/assets/a47153d3-d095-4e87-b430-f20702bed827" />
## Copying the Generated LEF File

After generating the custom inverter LEF file, copy **`sky130_vsdinv.lef`** (or the generated LEF file) from the **`vsdstdcelldesign`** directory to the **`src`** directory of the **`picorv32a`** design. This allows OpenLANE to recognize and use the custom standard cell during the implementation flow.

<img width="933" height="94" alt="27" src="https://github.com/user-attachments/assets/46312507-f12d-4b63-9c28-c9643d3dc483" />
## Copying the Liberty Files

To ensure proper timing analysis during synthesis and implementation, copy the following Liberty (`.lib`) files from the **`libs`** directory of **`vsdstdcelldesign`** to the **`src`** directory of the **`picorv32a`** design:

- `sky130_fd_sc_hd__fast.lib`
- `sky130_fd_sc_hd__slow.lib`
- `sky130_fd_sc_hd__typical.lib`

These library files provide timing information for the custom inverter cell under different process conditions.


<img width="1818" height="108" alt="28" src="https://github.com/user-attachments/assets/4207ac88-4891-4f01-83e9-712a42b28f9d" />

## Verifying the Source Directory

After copying the required LEF and Liberty files, verify that the **`src`** directory contains all the necessary files for integrating the custom standard cell into the OpenLANE flow.

Confirm that the generated LEF file and all three Liberty files are present before proceeding with the next implementation stage.

The updated **`src`** directory is shown below.


<img width="999" height="143" alt="29" src="https://github.com/user-attachments/assets/2214e7cd-68fc-4cbc-b0da-a838ededbfd5" />




















