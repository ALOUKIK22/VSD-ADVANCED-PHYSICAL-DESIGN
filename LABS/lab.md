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
##  Running Floorplan


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
#  Day 3: Designing Standard Cells Using Magic and NGSpice

##  Importing the CMOS Inverter Design Files

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


 #  Day 4: Pre-Layout Timing Analysis

## Introduction

Before performing Place and Route (PnR), the physical layout created in Magic must be represented in an abstract form known as the **Library Exchange Format (LEF)**. Unlike the complete GDS layout, the LEF file contains only the essential physical information required by the PnR tool, such as cell dimensions, pin locations, routing blockages, and metal layer definitions.

Using this abstract representation enables OpenLANE to perform efficient cell placement and interconnect routing without processing the entire GDS layout.

---

## Updating the Inverter Cell

Before generating the LEF file, the custom inverter cell must be modified to comply with the routing grid defined by the technology. The **`tracks.info`** file provides information about the routing tracks available for each metal layer and serves as a reference for aligning the standard cell layout.


<img width="233" height="288" alt="22" src="https://github.com/user-attachments/assets/fec0cb85-ba73-42fb-bb11-bebb270c0efe" />

<img width="1632" height="827" alt="23" src="https://github.com/user-attachments/assets/884b79d6-a2d2-48c4-8cfe-bfddb42c9828" />

<img width="902" height="807" alt="Screenshot 2026-07-12 143934" src="https://github.com/user-attachments/assets/58b1e93c-b967-4487-9362-ade1388504a7" />


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

## Preparing the Design with the Updated Configuration

After incorporating the custom inverter into the design, the OpenLANE flow must be reinitialized using the same run directory. This allows the previously generated run to be overwritten with the updated design files and configuration.

```tcl
prep -design picorv32a -tag 13-08_20-12 -overwrite
```

Here:

- **`-design`** specifies the target design (`picorv32a`).
- **`-tag`** selects the existing run directory to be reused.
- **`-overwrite`** replaces the contents of the selected run with the updated implementation files, ensuring that the custom standard cell and the latest configuration are included in the OpenLANE flow.

## Including the Custom LEF in the OpenLANE Flow

Before proceeding with synthesis, the custom LEF file must be added to the OpenLANE environment so that the physical design flow recognizes the newly created standard cell.

Execute the following commands in the OpenLANE interactive shell:

```tcl
set lefs [glob $::env(DESIGN_DIR)/src/*.lef]
add_lefs -src $lefs
```

The first command searches the **`src`** directory for all LEF files, while the second command imports them into the current OpenLANE session for use during the implementation flow.
## Running Synthesis

After successfully importing the custom LEF files, the design is ready for synthesis.

Execute the following command:

```tcl
run_synthesis
```

This step synthesizes the RTL design using the updated configuration and standard cell library, producing a gate-level netlist that incorporates the custom inverter cell for the subsequent stages of the physical design flow.

<img width="1207" height="894" alt="30" src="https://github.com/user-attachments/assets/f3800167-fc82-4741-98a1-0054ccf28282" />
<img width="312" height="151" alt="31" src="https://github.com/user-attachments/assets/ae9fb402-cd2b-4c7b-aa04-979440e59c84" />
Now, run floorplan using the command run_floorplan followed by placement using run_placement which will place the updated inverter cell in the floorplan. Check the results/placement directory in the runs/13-08_20-12 directory for the generated def file.



Invoke the magic tool to view the placement ready layout using the command below :
## Viewing the Updated Placement Layout in Magic

After completing the placement stage, the generated placement DEF file can be visualized using the **Magic Layout Editor** to verify that the custom standard cell has been successfully integrated into the design.

```bash
magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech \
lef read ../../tmp/merged.lef \
def read picorv32a.placement.def
```

This command loads the Sky130 technology file, the merged LEF, and the generated placement DEF file into Magic, allowing the complete placement layout to be inspected graphically.

## Viewing the Updated Placement Layout in Magic

After completing the placement stage, the generated placement DEF file can be visualized using the **Magic Layout Editor** to verify that the custom standard cell has been successfully integrated into the design.

Execute the following command:

```bash
magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech \
lef read ../../tmp/merged.lef \
def read picorv32a.placement.def
```

This command loads the Sky130 technology file, the merged LEF, and the generated placement DEF file into Magic, allowing the complete placement layout to be inspected graphically.

After locating the cell, press s to select it, type pwd in the tkcon terminal followed by expand command to properly view the placed cell.

<img width="1443" height="784" alt="32" src="https://github.com/user-attachments/assets/e39e0f05-c4ea-4e53-a211-3083ab9bb011" />

# Pre-Layout Static Timing Analysis (STA)

## Introduction

After successfully integrating and placing the custom inverter cell, the next step is to evaluate the timing performance of the design. This is accomplished through **Pre-Layout Static Timing Analysis (STA)** using the **OpenSTA** tool.

Static Timing Analysis verifies whether all timing paths satisfy the specified timing constraints before the routing stage, helping identify potential setup and hold violations early in the physical design flow.

---

## Preparing the Design for STA

Before running OpenSTA, the timing constraint file must be updated with the appropriate design constraints. The existing **`base.sdc`** file is modified to include the required clock definitions, driving cell information, and load capacitance values.

After making the necessary changes, the updated constraint file is saved as **`my_base.sdc`** and copied into the **`src`** directory of the **`picorv32a`** design. This file serves as the input constraint file for the subsequent timing analysis.

<img width="1856" height="675" alt="33" src="https://github.com/user-attachments/assets/2f9fd9e1-1ebe-4814-ace5-26645b3a8189" />

<img width="1822" height="679" alt="34" src="https://github.com/user-attachments/assets/83c7a5d4-e089-4fd8-9aa5-2d2824f9a96a" />

 The inverter driving cell sky130_fd_sc_hd_inv_8 characteristics can be found in the sky130_fd_sc_hd__*.lib liberty files 

 <img width="489" height="730" alt="35" src="https://github.com/user-attachments/assets/d18eb729-8931-46d3-a7b4-68dc180127ab" />

 ## Creating the OpenSTA Configuration Script

Since the updated timing constraint file **`my_base.sdc`** is now available in the **`src`** directory, the next step is to create an **OpenSTA configuration script**. This script serves as the primary input to the OpenSTA tool and specifies all the resources required for timing analysis.

The configuration script includes:

- The synthesized gate-level netlist
- The **`my_base.sdc`** timing constraint file
- Standard cell Liberty (`.lib`) files
- Timing analysis and reporting commands

By providing these inputs, OpenSTA is able to locate the necessary design files, apply the timing constraints, and generate detailed timing reports for the design.

<img width="1852" height="482" alt="36" src="https://github.com/user-attachments/assets/56645b2e-9bab-4d96-9082-e3e152c2df31" />

## Running Static Timing Analysis Using OpenSTA

With the **`pre_sta.conf`** configuration script prepared, the design is ready for timing analysis using **OpenSTA**.

Launch OpenSTA by executing the following command:

```bash
sta pre_sta.conf
```

This command loads the configuration script, which imports the synthesized netlist, Liberty files, timing constraints, and reporting commands required for Static Timing Analysis. OpenSTA then analyzes the design and generates timing reports, allowing verification of setup and hold timing requirements before proceeding to the routing stage.

<img width="812" height="862" alt="37" src="https://github.com/user-attachments/assets/4f11d22b-5d66-4b0a-be94-afddb3c6731e" />

## Observing the Timing Report

Observe the timing reports and identify where the delay is building up.

The two important timing metrics are:

- **WNS (Worst Negative Slack):** It is the worst negative slack and is reported by default.
- **TNS (Total Negative Slack):** It represents the total negative slack across all timing paths.

 enable **`SYNTH_BUFFERING`** by setting it to **1**. Buffer insertion helps reduce wire delays, which can improve the overall timing of the design.


```tcl
set ::env(SYNTH_BUFFERING) 1
```

After enabling buffering, rerun synthesis using:

```tcl
run_synthesis
```

Observe the updated timing report and check whether the **negative slack has been reduced**. Enabling synthesis buffering generally increases the chip area due to the insertion of additional buffers, while improving the timing performance.



<img width="747" height="330" alt="38" src="https://github.com/user-attachments/assets/166c5809-5bc7-4fc4-a94f-85c698a2d0e5" />


Once the synthesis results have been verified, proceed to the floorplanning stage by running:

```tcl
run_floorplan
```

and then proceed with 

```tcl
run_placement
```
during this, overflow should decrease.

<img width="1015" height="857" alt="39" src="https://github.com/user-attachments/assets/c74bf2f0-4a05-4859-81e7-5d836075b373" />


## Verifying the Custom Cell in the PnR Flow

After completing floorplanning and placement, verify that the custom inverter cell has been successfully included in the PnR flow.

Open the placement layout in Magic:

```bash
magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech \
lef read ../../tmp/merged.lef \
def read picorv32a.placement.def
```

Once the layout is loaded:

- Locate the custom cell **`sky130_vsdinv`** in the design.
- Select the cell by hovering over it and pressing **`s`**.


Also verify that the **power (VPWR)** and **ground (VGND)** rails are properly connected to the custom inverter cell.

<img width="1150" height="807" alt="40" src="https://github.com/user-attachments/assets/006ff249-0519-465a-81c2-8a8f4c93c86d" />

We have plugged in our custom cells into the openLANE flow.

Now,  open tkcon and 

```tcl
expand
```

<img width="1141" height="832" alt="41" src="https://github.com/user-attachments/assets/6077db14-58ad-4d04-8049-c74535927ae5" />


Metal 1 is correctly aligned !

# OpenSTA Labs

Execute the following commands:

```tcl
read_lef /openLANE_flow/designs/picorv32a/runs/<run_tag>/tmp/merged.lef

read_liberty -max /openLANE_flow/designs/picorv32a/src/sky130_fd_sc_hd__slow.lib

read_liberty -min /openLANE_flow/designs/picorv32a/src/sky130_fd_sc_hd__fast.lib

read_verilog /openLANE_flow/designs/picorv32a/runs/<run_tag>/results/synthesis/picorv32a.synthesis.v

link_design picorv32a

read_sdc /openLANE_flow/designs/picorv32a/src/my_base.sdc
```

The **slow** Liberty file is used for **setup (maximum delay)** analysis, while the **fast** Liberty file is used for **hold (minimum delay)** analysis.

<img width="681" height="32" alt="42" src="https://github.com/user-attachments/assets/361c6162-282d-4291-aac4-f246acfa5f7e" />

<img width="286" height="72" alt="43" src="https://github.com/user-attachments/assets/88f8f20d-aa96-4319-96c7-dfc12ecd3a36" />

<img width="1418" height="41" alt="44" src="https://github.com/user-attachments/assets/db6f4cbe-225e-4b1c-88e9-cc01b48b1430" />



---

## Timing Analysis Before CTS

At this stage of the flow, the design has **not yet undergone Clock Tree Synthesis (CTS)**. Therefore, only **setup timing analysis** is performed.

Hold analysis is typically carried out **after CTS**, since the clock network is created during that stage. After CTS, a new synthesized netlist is also generated because the insertion of clock buffers modifies the original netlist.


## Fixing Fanout During Synthesis

A large fanout increases the capacitive load on a driver, which leads to increased signal slew and can negatively impact timing.

To limit the maximum fanout during synthesis, set the following OpenLANE environment variable:

```tcl
set ::env(SYNTH_MAX_FANOUT) 4
```

After updating the fanout constraint, rerun synthesis:

```tcl
run_synthesis
```

<img width="957" height="881" alt="45" src="https://github.com/user-attachments/assets/3f3ad78d-882b-4964-bcd2-120b38820c50" />

<img width="750" height="456" alt="46" src="https://github.com/user-attachments/assets/15628236-6538-4643-99aa-14be7c5e2417" />

As Visible, it is driving input of four heavy loads. to solve this chain if buffers is used.

## Analyzing the Critical Timing Path

After generating the timing report, the critical path was analyzed to identify the source of the timing violation. The path report showed that the delay was primarily increasing due to excessive fanout and slow signal transitions (high slew).

<img width="1498" height="418" alt="50" src="https://github.com/user-attachments/assets/e169abd7-9800-4c23-8e6f-2f682c75a473" />

## Inspecting Net Connections

To understand why the delay was increasing, the net with the highest contribution to the critical path was examined.

Execute the following command:

```tcl
report_net -connections _11344_
```

The command reports the **driver pin** and all **load pins** connected to the selected net.

From the report:

- Driver Pin:

```text
_41882_/X (sky130_fd_sc_hd__buf_1)
```

- Load Pins:

```text
_41883_/C  (sky130_fd_sc_hd__nand3_4)
_42245_/A2 (sky130_fd_sc_hd__a21boi_4)
_42554_/A2 (sky130_fd_sc_hd__a21boi_4)
_42874_/A2 (sky130_fd_sc_hd__a21boi_4)
```

It can be observed that a single **BUF_1** is driving multiple loads. As the fanout increases, the capacitive load on the driver also increases, resulting in larger transition delays and degraded timing performance.

<img width="565" height="381" alt="48" src="https://github.com/user-attachments/assets/db915ed5-3f5f-4122-a045-a3b714804749" />

## Replacing the Driver Cell

To improve the drive strength of the critical net, the existing **`sky130_fd_sc_hd__buf_1`** cell is replaced with a stronger **`sky130_fd_sc_hd__buf_4`** cell.

The following command is used:

```tcl
replace_cell _41882_ sky130_fd_sc_hd__buf_4
```

Increasing the drive strength allows the buffer to drive the larger capacitive load more efficiently, thereby reducing signal slew and improving the overall timing of the critical path.

After replacing the cell, regenerate the timing report using:

```tcl
report_checks -fields {net cap slew input_pins} -digits 4
```

<img width="771" height="192" alt="47" src="https://github.com/user-attachments/assets/f753c145-8476-4788-9bcc-b48e9e1f22ea" />

##  Logic

- Generated the timing report using `report_checks` to identify the critical path.
- Used `report_net -connections` to inspect the net responsible for the timing violation.
- Observed that a single **`sky130_fd_sc_hd__buf_1`** cell was driving multiple load pins, resulting in a high fanout.
- A higher fanout increased the net capacitance, leading to a larger output slew.
- The increased slew caused a higher propagation delay, contributing to negative slack.
- Replaced the weak buffer with a stronger **`sky130_fd_sc_hd__buf_4`** using the `replace_cell` command.
- Executed `report_checks -fields {net cap slew input_pins} -digits 4` again to verify the reduction in slew and the improvement in timing.


# Clock Tree Synthesis (CTS)

## Writing the Optimized Netlist

After improving the timing through cell replacement, the modified netlist must be written back to disk so that the subsequent physical design stages use the updated design.

To view the syntax:

```tcl
help write_verilog
```

Write the updated synthesized netlist using:

```tcl
write_verilog /openLANE_flow/designs/picorv32a/runs/<run_tag>/results/synthesis/picorv32a.synthesis.v
```

<img width="543" height="270" alt="52" src="https://github.com/user-attachments/assets/380fadb2-93f8-4a01-8de6-16286ee4df05" />

## Running Clock Tree Synthesis (CTS)

After generating the updated netlist, proceed with **Clock Tree Synthesis (CTS)**.

CTS inserts clock buffers throughout the clock network to distribute the clock signal uniformly and reduce clock skew across all sequential elements.

<img width="1530" height="177" alt="53" src="https://github.com/user-attachments/assets/102c7cf7-338b-4f19-a6e4-b03e403f1830" />

<img width="897" height="268" alt="54" src="https://github.com/user-attachments/assets/1aafada3-d277-44e0-b477-889f0eec5ce2" />

## CTS Configuration

Before running CTS, OpenLANE provides several configurable parameters that control the clock tree implementation.

Some important parameters include:

- **CTS_TARGET_SKEW** – Specifies the target clock skew.
- **CTS_ROOT_BUFFER** – Defines the buffer inserted at the root of the clock tree.
- **CLOCK_TREE_SYNTH** – Enables Clock Tree Synthesis.
- **CTS_TOLERANCE** – Controls the quality-of-results versus runtime trade-off.

<img width="1501" height="171" alt="55" src="https://github.com/user-attachments/assets/f01d46d6-011a-467a-9180-a7d04b045411" />

## Observing the CTS Results

After CTS completes successfully, OpenROAD reports several useful statistics regarding the generated clock tree.

Some key observations include:

- Total clock buffers inserted.
- Number of generated clock nets.
- Minimum and maximum clock path depth.
- Clock tree levels.
- Fanout distribution across the clock network.

<img width="1547" height="796" alt="57" src="https://github.com/user-attachments/assets/81f42822-2c7c-4533-88dd-de91925398cf" />

## Verifying CTS Environment Variables

The environment variables generated after CTS can be verified using the following commands:

```tcl
echo $::env(LIB_SYNTH_COMPLETE)

echo $::env(LIB_TYPICAL)

echo $::env(CURRENT_DEF)

echo $::env(SYNTH_MAX_TRAN)

echo $::env(CTS_MAX_CAP)

echo $::env(CTS_CLK_BUFFER_LIST)

echo $::env(CTS_ROOT_BUFFER)
```

## Loading the CTS Database into OpenSTA

To perform timing analysis after Clock Tree Synthesis, load the generated LEF and DEF files into OpenSTA.

Execute the following commands:

```tcl
read_lef /openLANE_flow/designs/picorv32a/runs/<run_tag>/tmp/merged.lef

read_def /openLANE_flow/designs/picorv32a/runs/<run_tag>/results/cts/picorv32a.cts.def
```

These commands import the physical database generated after CTS.

<img width="1048" height="81" alt="60" src="https://github.com/user-attachments/assets/28bc8b4a-4ba8-49d9-9468-c4dbd77eb573" />

<img width="1541" height="787" alt="61" src="https://github.com/user-attachments/assets/f4881179-80a7-4aba-a9b6-4f6dad503322" />

## Saving the CTS Database

After successfully loading the CTS database into OpenSTA, save the current design database for future analysis.

Execute:

```tcl
write_db pico_cts.db
```

The generated database can later be reloaded without repeating the setup procedure.

## Comparing Timing Before and After CTS

Finally, compare the timing reports before and after Clock Tree Synthesis.

The timing report before CTS showed a slack of approximately:

```text
Slack = -2.17 ns
```

After CTS, the timing improved to:

```text
Slack = -1.23 ns
```

The reduction in negative slack indicates that the insertion of clock buffers improved the clock distribution and reduced timing violations.

<img width="455" height="128" alt="62" src="https://github.com/user-attachments/assets/8faac854-a233-4c99-876b-4c825adeca61" />

<img width="532" height="108" alt="63" src="https://github.com/user-attachments/assets/94d79943-be2b-4c5c-b86e-16c83dd423ad" />

## Saving the CTS Database

After successfully loading the CTS database into OpenSTA, save the current design database for future analysis.

Execute:

```tcl
write_db pico_cts.db
```

The generated database can later be reloaded without repeating the setup procedure.

# Day 5: Routing and Design Rule Check (DRC)

## Introduction

After completing **Floorplanning**, **Placement**, **Clock Tree Synthesis (CTS)**, and **Static Timing Analysis (STA)**, the final stage of the physical design flow is **Routing**.

During routing, all placed standard cells are electrically connected according to the synthesized netlist while adhering to the technology design rules defined by the **Sky130 PDK**. OpenLANE performs both **global routing** and **detailed routing**, ensuring that all signal, power, and ground connections are completed with minimal congestion and timing impact.

The objective of this stage is to generate a fully connected layout that is ready for physical verification, including **Design Rule Checking (DRC)** and **Layout Versus Schematic (LVS)**.

## Design Rule Check (DRC)

During fabrication, the layout is transferred onto the silicon wafer using **optical photolithography**. Since photolithography is limited by the wavelength of light, the layout must satisfy the foundry's design rules to ensure manufacturability.

DRC verifies that the layout complies with these rules by checking parameters such as minimum width, spacing, enclosure, and other geometry constraints defined by the **Sky130 PDK**.

---

## Generating the Power Distribution Network (PDN)

Before routing, the **Power Distribution Network (PDN)** must be generated to provide reliable power (**VPWR**) and ground (**VGND**) connections throughout the design.

Generate the PDN using:

```tcl
gen_pdn
```
<img width="100" height="37" alt="Screenshot 2026-07-15 135822" src="https://github.com/user-attachments/assets/a401b3ec-f0ce-4d05-bd46-1d26f5a7a8cf" />

## Verifying the Current DEF

After PDN generation, verify that the current DEF file has been updated successfully.

Execute:

```tcl
echo $::env(CURRENT_DEF)
```

<img width="760" height="85" alt="Screenshot 2026-07-15 135745" src="https://github.com/user-attachments/assets/2e61de6a-6baf-4120-a955-7f8963ec9094" />


---

## Running Routing

With the PDN successfully generated, proceed to the routing stage by executing:

```tcl
run_routing
```

OpenLANE performs both **global routing** and **detailed routing**, connecting all signal nets while satisfying the routing constraints and design rules specified by the Sky130 technology.

<img width="685" height="275" alt="Screenshot 2026-07-15 141637" src="https://github.com/user-attachments/assets/ffdb317e-a414-49c8-912f-8f456c3260ca" />


## Running Floorplanning

After preparing the design, execute the floorplanning stage using:

```tcl
run_floorplan
```

Once the floorplanning stage is completed, verify the current DEF file generated by OpenLANE:

```tcl
echo $::env(CURRENT_DEF)
```

The output points to the DEF file created during floorplanning, typically located at:

```text
openlane/designs/picorv32a/runs/<run_tag>/tmp/floorplan/pdn.def
```

<img width="698" height="51" alt="Screenshot 2026-07-15 141917" src="https://github.com/user-attachments/assets/4c70bdb8-3071-4da2-aa43-b283e08bfce8" />

## Viewing Routing Configuration Variables

Before starting the routing stage, the routing-related environment variables can be viewed to understand the configuration used by OpenLANE.

Some important routing variables include:

- `ROUTING_STRATEGY` – Specifies the routing optimization strategy.
- `GLB_RT_ADJUSTMENT` – Controls global routing congestion adjustment.
- `GLB_RT_LAYER_ADJUSTMENTS` – Sets layer-wise routing adjustments.
- `RT_MAX_LAYER` – Defines the maximum metal layer used for routing.
- `RT_MIN_LAYER` – Defines the minimum metal layer used for routing.
- `DRT_OPT_ITERS` – Specifies the number of detailed routing optimization iterations.

<img width="1587" height="232" alt="Screenshot 2026-07-15 143141" src="https://github.com/user-attachments/assets/261bac9f-3c27-4687-9b19-af206745f88a" />

## Viewing DRC Violations

Use the **`less`** command to view the generated DRC report and identify the reported violations.

```bash
less <path_to_drc_report>
```

<img width="808" height="195" alt="Screenshot 2026-07-15 182650" src="https://github.com/user-attachments/assets/da846b0c-afe4-4bf0-b074-725998d8b8e1" />

The routing guides are obtained as the output of the **Global Routing** stage. These routing guides provide the preferred routing paths for each net, which are then used during **Detailed Routing**.

The global routing stage in OpenLANE is performed using **FastRoute**.

The generated routing guide file is:

```text
fastroute.guide
```

<img width="1067" height="22" alt="Screenshot 2026-07-15 183042" src="https://github.com/user-attachments/assets/6bb9352d-eb8d-495f-a913-d114b7268e7e" />



















