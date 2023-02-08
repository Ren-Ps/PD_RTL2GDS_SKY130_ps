# Advance Physical Design RTL-to-GDSII using SKY130 technology and OpenLANE for EDA

# PD_RTL2GDS_SKY130_ps
This repo contents learning info and steps followed during the workshop of Advanced Physical Design Using OpenLANE/SKY130. It focuses on the complete ASIC flow approach from RTL2GDS using opensource EDA tools and the PICORV32A which is RISC-V architecture is followed for designing the core.

# Table of Content
  * [About RTL to GDSII Flow](#about-rtl-to-gdsii-flow)
  * [SKYWater130 PDK](#skywater130-pdk)
  * [OpenLANE](#openlane)
  * [Tools Used](#tools-used)
  * [Day 1 - Inception of open-source EDA, OpenLANE and Sky130 PDK](#day-1---inception-of-open-source-eda-openlane-and-sky130-pdk)
    - [How to talk to computers](#how-to-talk-to-computers)
      - [IC Terminologies](#ic-terminologies)
      - [Introduction to RISC-V](#introduction-to-risc-v)
        * [RISC-V Characterstics](#risc-v-characterstics)
      - [Software to Hardware](#software-to-hardware)
        * [What happens when we run a program?](#what-happens-when-we-run-a-program)
        * [How does an application run on a computer?](#how-does-an-application-run-on-a-computer)
    - [SoC design and OpenLane](#soc-design-and-openlane)
       - [Introduction to Digital Design](#introduction-to-digital-design)
         * [What is a PDK?](what-is-a-pdk?)
         * [Environment Setup](#environment-setup)
       - [Simplified RTL to GDSII Flow](#simplified-rtl-to-gdsii-flow)
       - [About OpenLANE](#about-openlane)
    - [Getting familier to open-source EDA tools](#getting-familier-to-open-source-eda-tools)
       - [Contents of the OpenLANE Directory](#contents-of-the-openlane-directory)
       - [LAB Day 1](#lab-day-1)
       - [TASK 1: Finding the d flip flop ratio](#TASK-1-finding-the-d-flip-flop-ratio)
* [Day 2 - Good floorplan vs bad floorplan and introduction to library cells](#day-2---good-floorplan-vs-bad-floorplan-and-introduction-to-library-cells
)
    - [Stages of Floorplanning:](#stages-of-floorplanning)
       - [The height and width of core and die](#the-height-and-width-of-core-and-die)
       - [The location of Preplaced Cell](#the-location-of-preplaced-cell)
       - [Surround preplaced cells with decoupling capacitors](#surround-preplaced-cells-with-decoupling-capacitors)
       - [Power Planning](#power-planning)
       - [Pin Placement](#pin-placement)
       - [Logical Cell Placement Blockage](#logical-cell-placement-blockage)
    - [Placement and routing](#placement-and-routing)
       - [Introduction to Placement](#introduction-to-placement)
       - [Initial placement of cells in our floorplan](#initial-placement-of-cells-in-our-floorplan)
       - [Optimised placement of cells in our floorplan after using buffers] (#optimised-placement-of-cells-in-our-floorplan-after-using-buffers)
       - [Placement is done on two stages](#Placement-is-done-on-two-stages)
    - [LAB Day 2](#lab-day-2)
       - [TASK 2: Calculating area](#task-2-calculating-area)
* [Day 3 - Design library cell using Magic Layout and ngspice characterization](#day-3---design-library-cell-using-magic-layout-and-ngspice-characterization)
   - [LAB Day 3](#lab-day-3)
   - [Labs for CMOS inverter ngspice simulations](#labs-for-cmos-inverter-ngspice-simulations)
       - [Creating SPICE deck](#creating-spice-deck)
       - [Analysing the inverter](#analysing-the-inverter)
       - [LAB SETUP](#lab-setup)
   - [Inception of Layout Â CMOS fabrication process (16 mask process)](#inception-of-layout-Â-CMOS-fabrication-process-(16-mask-process))
   - [LAB DAY 3 (PART 2)](#labb-day-3(part-2))
   - [LAB DAY 3 (PART 3)](#labb-day-3(part-3))
       - [TASK 3: calculating delays and fall time](#task-3-calculating-delays-and-fall-time)
* [DAY 4 Pre-layout timing analysis and importance of good clock tree](#day-4-pre---layout-timing-analysis-and-importance-of-good-clock-tree)
   - [Pre-layout timing analysis and importance of good clock tree](#pre-layout-timing-analysis-and-importance-of-good-clock-tree)
* [DAY 5 - Final step for RTL2GDS]
       
       
# About RTL to GDSII Flow
RTL (Register tranfer level) to GDSII (Graphic Data Stream) flow consists of the complete set of steps required to create a file which could be sent for tapeout. The RTL code is synthesized and optimised. After sysnthesis of the code, PnR, floor and power planning is done while keeping in check the timing constraints. At the end GDSII file is written out.
The complete flow consists of following steps:
* Writting RTL
* Synthesis
* STA (Static Timing Analysis)
* DFT (Design for Testability) 
* Floorplanning
* Placement
* CTS (Clock Tree Synthesis)
* Routing
* GDSII Streaming

# SKYWater130 PDK
It is a Open source PDK (Process Design Kit) which is released by the collabration of Google and SkyWater Technologies foundary. Currently this technology has a target node of 130 nm. It is open to everyone and can be accessed at [SkyWater Open Source PDK](https://github.com/google/skywater-pdk). This PDK is extremely flexible as it provides many optional featurs as standard features. Hence it povide designers with wide range of design choice. 

# OpenLANE
It is an open-source VLSI flow created using open source tools. Basically it is collection of various scripts which invoke and execute these tools in right sequence, modifies inputs and outputs and gives an organised results.

# Tools Used
 | Tool | Used for |
 | ----- | ----- |
 | [Yosys](https://github.com/YosysHQ/yosys) | Synthesis of RTL Design |
  | ABC | Mapping of Netlist |
  | [OpenSTA](https://github.com/The-OpenROAD-Project/OpenSTA) | Static Timing Analysis |
  | [OpenROAD](https://github.com/The-OpenROAD-Project/OpenROAD) | Floorplanning, Placement, CTS, Optimization, Routing |
  | [TritonRoute](https://github.com/The-OpenROAD-Project/TritonRoute) | Detailed Routing |
  | [Magic VLSI](http://opencircuitdesign.com/magic/) | Layout Tool |
  | [NGSPICE](https://github.com/imr/ngspice) | SPICE Extraction and Simulation |
  | SPEF_EXTRACTOR | Generation of SPEF file from DEF file |
  
# Day 1 - Inception of open-source EDA, OpenLANE and Sky130 PDK
  ## How to talk to computers
  ### IC Terminologies
  In the complete flow to this RTL2GDS physical designing there are lot of terminologies one comes across. Some of these terms are described below.
  * Package - ICs are basically presents as packages. These packages are materials which contains the semiconductor device. These packages protect the device from damage. these are of various kind. An example of QFN-48 (Quad Falt No-Leads) with 48 pins is taken here.
  
<p align="center">
 <img src="https://github.com/Ren-Ps/PD_RTL2GDS_SKY130_ps/blob/main/Day%201/Theory/th1.png">

* Chip - It sits in the centre of the package. The chip is connected to the package pins using **wire bond**. Inside the chip we have various components such as pad, core, interconnects, etc.
  * Pads - These are the itermediate structure through which the internal signals from the core of IC is connected to the external pins of the chip. These pads are organised as Pad Frame. There are different kind of pads for input, output, power supply and ground.
  * Core - It is the place where all the logic units (gates, muxs, etc) are presnet inside the chip. These are able to execute the set of instructions given to the chip and produce an output.
  * Die - It is the block which consists of semiconducting material and it can be used to build certain functional cuircuit which can be further sent for fabrication. It is the entire size of the chip.
  
<p align="center">
 <img src="https://github.com/Ren-Ps/PD_RTL2GDS_SKY130_ps/blob/main/Day%201/Theory/th2.png">
 
  ### Introduction to RISC-V
  **[RISC-V](https://riscv.org/technical/specifications/)** is an open instruction set architechture rooted on **reduced instruction set computer principles**. It is an open source ISA used for processor design. 
  #### RISC-V Characterstics
  * It uses one clock cycle per instruction.
  * It follows the th RISC Princples. 
  * It has both 32-bit and 64-bit varients. It also support floating point instruction.
  * It avoids micro-architechture or technology dependent features.
  * It accelerates the time for design to reach the market as it uses open-source IP.
  
  ### Software to Hardware 
  The flow shows how the high level language (at software end) gets converted to machine language (at hardware end) and then gets executed on the package.
  #### What happens when we run a program?
  Suppose a C program needs to run on a hardware. So we nned to pass this C program to the hardware. So firstly the C program is compiled into assembly language (RISC-V assembly language program). Now this assmebly language is converted into the machine language program (basically 1's and 0's). Now this 1's and 0's are understanable by the hardware.

#### How does an application run on a computer?
1. The application software enters the system software (major component of it are OS, Compiler and Assembler). 
 * The OS handles I/O operations, memories and many low level functions.
 * then the program passes to Compiler which changes the program to Assembly language (compiled into instructions depends upon the hardware).
 * Now the instruction set goes to Assembler. Assembler converts the instruction set to machine language (binary numbers). 
2. The system software converts the apllication software into binary language. 
3. Now these binary numbers enter our chip layout and according the function is performed.

![SoftHard](https://github.com/Ren-Ps/PD_RTL2GDS_SKY130_ps/blob/main/Day%201/Theory/th3.png)

## SoC design and OpenLane
### Introduction to Digital design
For designing Digital ASIC ICs we require following components and some of it's opensource resources are also mentioned. 
 * RTL models (old IP's) {github.com, librecores.org, etc}
 * EDA tool {OpenROAD, OpenLANE, etc}
 * PDK Data {SKYWater 130} 

In the workshop every component is used from sources which are open soucre. The following image gives an idea about each component as an open source resource.

<p align="center">
 <img src="https://github.com/Ren-Ps/PD_RTL2GDS_SKY130_ps/blob/main/Day%201/Theory/th4.png">

#### What is a PDK? 
PDK stands for Process Design Kit, it is provided by foundaries and it consists of library or set of building blocks which are used to build ICs. Each component in the library is seperate building bolck and ae made following certain foundary rules.

PDKs acts as an inteface between the FABs and the designeers. PDKs have collection of files whcih are used to model a fabrication process for the EDA tools used to design an IC. PDK consists of tecnology node information, Process Design Rules (to verify DRC, LVC, PEX, etc), device model, I/O libraries, Standard cell libraries, macros files, lef files, etc.

**Google along with SKYWater made the laters PDK opensource (130 nm node).** The PDK only need data information for successful implementation.

#### Environment Setup
The OpenLANE flow requires various open source tools as well as their supporting tools to be installed for the complete Physical design flow. Installing this tools one by one is tedious as well as one can get lost in the steps. Installation can be done easily using some set of scripts present in following repositories [VSDFlow](https://github.com/kunalg123/vsdflow) (for installing Yosys, OpenSTA, Magic, OpenTimer, netgent, etc)  and [OpenLANE Build Scripts](https://github.com/nickson-jose/openlane_build_script).

### Simplified RTL to GDSII Flow

<p align="center">
 <img src="https://github.com/Ren-Ps/PD_RTL2GDS_SKY130_ps/blob/main/Day%201/Theory/th5.png">

The flow starts from the HDL code i.e.RTL model and ends with GDSII file. The major implimenation steps are:
   * Synthesis - During synthesis the HDL design is translated into circuits,  which are made up of components present in the standard cell library. The resultant circuit is described in HDL and its referred as gate level netlist which is functional equivalent of RTL code. The library building block of cell have regular layouts, cell layout is enclosed by fixed height (rectanglar in shape) whereas the width is variable an is discrete i.e., integer multiple of unit call side width.

<p align="center">
 <img src="https://github.com/Ren-Ps/PD_RTL2GDS_SKY130_ps/blob/main/Day%201/Theory/th6.png">

* Floor Planning - In Floor planning the chip area is being planned which in turn creates a robust Power distribution to power the circuits. The die is partitioned into different building blocks or components, also the I?O pads are distributed. During macro floor planning macro dimensions, it's pin locations and row definations i.e, rows and routing plan.

<p align="center">
 <img src="https://github.com/Ren-Ps/PD_RTL2GDS_SKY130_ps/blob/main/Day%201/Theory/th7.png">

* Power Planning - The power network is constructed typically for a chip was it has to power multiple VDD and ground pins. The power pin are connected to all component through rings and multiple horizontal and vertical strips. Sach parallel structure is meant to reduce the resistance.

<p align="center">
 <img src="https://github.com/Ren-Ps/PD_RTL2GDS_SKY130_ps/blob/main/Day%201/Theory/th8.png">

* Placements - For macros we place the GATE level netlist cell on vertical rows. To reduce the interconnect delay conical cells are placed very close to each other and this is also done to enable successful routing afterwards. Placement is done in two ways Global placement and detailed placement. Global placement provide optimal result and these may or may not be legal where as the detail placement is always legal.

<p align="center">
 <img src="https://github.com/Ren-Ps/PD_RTL2GDS_SKY130_ps/blob/main/Day%201/Theory/th9.png">

 * Clock Tree Synthesis (CTS) - Before signal routing clock routing is done so that the clock distribution is done to every sequential block. Clock distribution network delivers the clock to each of the sequential block. It is done so that there is minimum skew and latency. It usually follows a shape i.e., H-tree, X-tree, etc.

<p align="center">
 <img src="https://github.com/Ren-Ps/PD_RTL2GDS_SKY130_ps/blob/main/Day%201/Theory/th10.png">

* Routing - The signal routing is done using metal layers. It is essential to find valid pattern of horizontal and verticle wires to implement the nets that connects the cells together. Router uses the available metal layers as defined by the PDK. For each metal layer the PDK defines the thickness, width, pitch and vias. Vias are used to connect two metal wires. SkyWater 130nm has 6 metal layers.

<p align="center">
 <img src="https://github.com/Ren-Ps/PD_RTL2GDS_SKY130_ps/blob/main/Day%201/Theory/th11.png">

* Verification and Sign-offs - After PnR and CTS we perform verifications, to check whether our layout is valid or not. These verifications consists of Physical verification such as DRC and LVS. Design Rules Checking (DRC) ensures that the layout follows the design rules and Layout Vs Schematic ensures that the final layout is as per the synthesised gate level netlist or not. Finally Static Timing Analysis is done (STA) to make sure that all the timing constraints are met by the circuit.
   
### About OpenLANE
[OpenLANE](https://openlane.readthedocs.io/en/latest/) is a flow which uses various open source tools for the RTL to GDSII flow. It has the striVe family of open everything SoCs (Open PDK, Open EDA, Open RTL). The various tools it uses are Yosys, OpenROAD, Magic, Netgen, SPEF_Extraction, etc.  

* Two Modes of Operations: Autonomus and Interactive
* It is tuned for SKYWater 130nm open PDK.
* OpenLANE ASIC flow is shown as,

![OPENLANE](https://github.com/Ren-Ps/PD_RTL2GDS_SKY130_ps/blob/main/Day%201/Theory/th12.png)

* The flow starts with RTL Synthesis. RTL is fed to Yosys with the design constraints. Yosys translates the RTL into a logic circuit using generic components. 
* the circuit can be optimized and then mapped with standard cell library usin the tool abc. There are [abc scrript](http://people.eecs.berkeley.edu/~alanmi/abc/) to guide the optimization. OpenLANE has several abc scripts which has different synthesis statergies (least area, least power consumption, etc). The synthesis exploration utility is for statergy exploration and report generation.
* OpenLANE has design exploration utility which can be used to sweep the design configurations (16 in total) and it genrates reports which has different design matrix and also shows the number of violations in layout. It is used for regression testing and to find the best configuration of our design. 
* OpenSTA performs the Static timing analysis on the netlist which is generated during synthesis.
* Now after synthesis, the testing part starts (DFT) i.e., scan insertion, Automatic Test Pattern Generation (ATPG), Test Pattern Compaction, Fault Coverage and Fault Simulation. This step is optional.
* Nest step is Physical implementation. This part is done with the help of OpenROAD application. It performs PnR which consists of FP+PP, Placement (Global and Detailed), Optimization, CTS and routing (Global and Detailed). TritonRoute is used for detailed routing.
* Logic equivalence checking (LEC) is performed as the circuit changes due to optimization process as compare to the one generated during synthesis. This is done using Yosys tool to make sure the functionality is equivalent. 
* During insertion there is a special step that is fake antenna insertion. It is required to address the antenna rule violations. The concept of fake antenna is something like we have already considered the antenna so that on later stage we do not have any antenna violations. Hence we add fake antenna diode next to every cell input after placement. Then antenna checker is run from the Magic tool against the layout. 
* Fake antenna diode cell is created and added to standard cell library. 

/*hence a long wire is simulated it acts as an antenna but as a conductor it collects charges which can damage the transistor gates connected to the wire during fab. So the length of wire connected to transistor gate must be limited. This is done by the help of Router.*/

* The sign off include STA, DRC and LVS. It also involves interconnect RC extraxtion from the routed layout followed by STA using OpenSTA.
* Physical signoffs include DRC and LVS. DRC and LVS is performed using Magic tool. Circuit extraction is done NetGen.

## Getting familier to open-source EDA tools
### Contents of the OpenLANE Directory

The following content is specific to the workshop. There are lot of other files present in the directory too.
 ![Openlane](https://github.com/Ren-Ps/PD_RTL2GDS_SKY130_ps/blob/main/Day%201/LAB/LB1.png)

1. **OpenLane folder** - It contains all the tools and the file that need to be invoked during the flow.
 ![inOpenLane](https://github.com/Ren-Ps/PD_RTL2GDS_SKY130_ps/blob/main/Day%201/LAB/LB6.png)
 
2. **Designs** - This folder consists of all the designs requried during the flow (picorv32a is the design used in this workshop)
![Designs](https://github.com/Ren-Ps/PD_RTL2GDS_SKY130_ps/blob/main/Day%201/LAB/LB8.png)

3. **PDK's** - This folder contains all the pdk related files as well as information. (open pdk, Sky130, Skywater pdk).
* open pdk consists of the scripts.
* sky130A pdk consists of the libs.ref (has files specific to process such as timing, lef-both tech and cell) 
 ![ref](https://github.com/Ren-Ps/PD_RTL2GDS_SKY130_ps/blob/main/Day%201/LAB/LB2.png)
 and libs.tech (has all the files specific to the tool) files.
 ![tech](https://github.com/Ren-Ps/PD_RTL2GDS_SKY130_ps/blob/main/Day%201/LAB/LB3.png)
* skywater pdk consists of skywater 130 nm pdks.
 ![sky](https://github.com/Ren-Ps/PD_RTL2GDS_SKY130_ps/blob/main/Day%201/LAB/LB4.png)
**NOTE:** - Here **sky130_fd_sc_hd libs.tech** is being used.

4. **config files** - It bypasses any configuration that has already been done i.e., many of the switches use default value that is already present in the OpenLane flow.
 An example within config file as,
 ![inConfig](https://github.com/Ren-Ps/PD_RTL2GDS_SKY130_ps/blob/main/Day%201/LAB/LB9.png)

 **The precedence order of Openlane settings are:**
* sky130_xyz_config.tcl
* config.tcl
* Default value (already set in OpenLane)

### LAB DAY 1:


**Step 1:** Starting OpenLane
* Go to openlane folder.

```
cd work/tools/openlane_working_dir/openlane
```

* Then run the **docker** command.

```
docker
```

* Now run the **flow.tcl** file with interactive mode.

```
./flow.tcl -interactive
```

 ![dockr](https://github.com/Ren-Ps/PD_RTL2GDS_SKY130_ps/blob/main/Day%201/LAB/LB7.png)
 
* Now import packages

```
package require openlane 0.9
```
![image](https://github.com/Ren-Ps/PD_RTL2GDS_SKY130_ps/blob/main/Day%201/LAB/LB10-1.png)

* Now we are good to go to execute our commands.

**NOTE** - The above commands are to be run everytime we use OpenLANE for RTL2GDSII flow.

**Step 2:** Design Preperation
* Knowing the contents of our design (picorv32a) folder.

 ![pico](https://github.com/Ren-Ps/PD_RTL2GDS_SKY130_ps/blob/main/Day%201/LAB/LB8.png)

Checking our config.tcl file values by running the below command in picorv32a folder (it has clock period of 5 unit) 

```
less config.tcl
```

![configure](https://github.com/Ren-Ps/PD_RTL2GDS_SKY130_ps/blob/main/Day%201/LAB/LB9.png)

* Creating file for our design i.e., setting up the design. It merges the cell LEF files and the technology LEF files generating merged.lef which is present in the temp folder.

```
prep -design picorv32a
```

![prep](https://github.com/Ren-Ps/PD_RTL2GDS_SKY130_ps/blob/main/Day%201/LAB/LB10.png)

This marks the creation of new folder inside picorv32a named as runs folder which consists of new folder whose name is the date on which the command is run. The following folder has results, reports, command logs, PDK Sources, etc files.

![runs](https://github.com/Ren-Ps/PD_RTL2GDS_SKY130_ps/blob/main/Day%201/LAB/LB11.png)

**Step 3:** Running Synthesis

Yosys synthesis is run when the command for synthesis is entered. Along with it abc scripts are also run and OpenSTA is also run.

```
run_synthesis
```

 ![sysnth](https://github.com/Ren-Ps/PD_RTL2GDS_SKY130_ps/blob/main/Day%201/LAB/LB18.png)
 
After running systhesis logs, reports and results are created.

Also a netlis file is created in the results --> symthesis folder named **picorv32a.synthesis.v**

 ![sysntheLoc](https://github.com/Ren-Ps/PD_RTL2GDS_SKY130_ps/blob/main/Day%201/LAB/LB22.png)
 
 In **picorv32a.synthesis.v** file using command ``` less picorv32a.synthesis.v ```
 
 ![lesssys](https://github.com/Ren-Ps/PD_RTL2GDS_SKY130_ps/blob/main/Day%201/LAB/LB23.png)
 
### TASK 1: Finding the d flip flop ratio

Count of d flip flop (sky130_fd_sc_hd_dfxtp_2) = 1613 

![DFFs](https://github.com/Ren-Ps/PD_RTL2GDS_SKY130_ps/blob/main/Day%201/LAB/LB20.png)

Number of cells = 14876 

![NosCell](https://github.com/Ren-Ps/PD_RTL2GDS_SKY130_ps/blob/main/Day%201/LAB/LB21.png)

###So: **flop ratio = count of d flip flops / number of cells = 1613/14876 = 0.108429 (10.8429 %)**

The synthesis statisttics report can bee seen above image. 
 
 
## Day 2: Good Floorplan vs Bad Floorplan and Introduction to library cells
---
### Stages of Floorplanning:
---
The placement of logical blocks, library cells, and pins on a silicon chip is known as chip floorplanning. It ensures that every module has been given the proper area and aspect ratio, that every pin of the module is connected to another module or the chip's edge, and that modules are placed so that they take up the least amount of space on a chip.

1. **The height and width of core and die**
- The core, which is located in the middle of the die, is where the logic blocks are put. The dimensions of each standard cell on the netlist determine the width and height. 
- **Utilization Factor** is defined as the ratio of area of occupancy by the netlist(for hard macros and standard cells or soft macro cells) to total area of the core. Utilization factor in a realistic situation is between 0.5 and 0.6. Only this space is used for the netlist and the rest space is used for routing and more extra cells. 
- **Aspect Ratio** is defined as the ratio between height and the width of core.
 
<p align="center">
 <img src="https://github.com/Ren-Ps/PD_RTL2GDS_SKY130_ps/blob/main/Day2/Theory/th1.png"> </p>


2. **The location of Preplaced Cell**
- **Preplaced Cell** are complex logic blocks that are previously implemented but can be reused, such as memory, clock-gating cells, MUX's, comparator, etc. Prior to placement and routing, the user-defined placement on the core must be completed (thus preplaced cells). 
- This needs to be very well described because the automated place and route tools won't be able to touch or move these preplaced cells.
- These pre-placement cell need to be surrounded by decoupling capacitors.

<p align="center">
 <img src="https://github.com/Ren-Ps/PD_RTL2GDS_SKY130_ps/blob/main/Day2/Theory/th2.png">  </p>

 
3. **Surround preplaced cells with decoupling capacitors**
- The complex preplaced logic block needs a lot of current from the power supply to switch the current. However, due to the resistance and inductance of the wire, there will be a voltage drop because of the distance between the main power supply and the logic block. As a result, the voltage at the logic block might no longer fall within the noise margin range (logic become unstable).
-  Utilizing **decoupling capacitors** which are hudge bunch of capacitor completely filled with charges, close to the logic block will provide the necessary current for the logic block to switch inside the desired noise margin range.

   Decoupling capacitors surrounding the preplaced blocks
<p align="center">
 <img src="https://github.com/Ren-Ps/PD_RTL2GDS_SKY130_ps/blob/main/Day2/Theory/th3.png">  </p>
 
4. **Power Planning**
- It is not possible to apply a decoupling capacitor for sourcing logic blocks with enough current throughout the entire chip, only on the important components (preplaced complex logicblocks). 
- Due to the large amount of current that must be sinked simultaneously when a large number of elements switch from logic 1 to logic 0, this could result in **ground bounce**, and switching from logic 0 to logic 1 could result in **voltage droop** because there is not enough current from the power source to source the needed current for all elements. The increase or decrease in voltage may not be within the noise margin range due to voltage droop and ground bounce.
-  The reason for problem of voltage droop and ground bounce is because the supply has been provided only from one point so we use multiple power source taps (power mesh) are the solution, allowing components to source current from the closest VDD tap and sink current to the closest VSS tap. The majority of processors include several powersource pins because of this.

<p align="center">
 <img src="https://github.com/Ren-Ps/PD_RTL2GDS_SKY130_ps/blob/main/Day2/Theory/th4.png">  </p>

         ####  Power Planning showing multiple power source taps
 
<p align="center">
 <img src="https://github.com/Ren-Ps/PD_RTL2GDS_SKY130_ps/blob/main/Day2/Theory/th5.png"> </p>


5. **Pin Placement**
- The area between the core and the die is where the input and output ports are located.
- The positions of the ports depend on the placements of the cells that are connected with them on the core. 
- Since this clock must be able to drive the entire chip so the clock pin is thicker (lowest resistance route) than data ports.

<p align="center">
 <img src="https://github.com/Ren-Ps/PD_RTL2GDS_SKY130_ps/blob/main/Day2/Theory/th6.png">  </p>
 
6. **Logical Cell Placement Blockage**
- This ensures that no cells are placed by the automated placement and routing tool on the die's pin locations.

<p align="center">
 <img src="https://github.com/Ren-Ps/PD_RTL2GDS_SKY130_ps/blob/main/Day2/Theory/th7.png">  </p>
 
**Inputs for floorplan:**

- Netlist (.v)

- Technology file (techlef)

- Timing Library files (.lib)

- Physical library (.lef)

- Synopsys design constraints (.sdc)

<p align="center">
 <img src="https://github.com/Ren-Ps/PD_RTL2GDS_SKY130_ps/blob/main/Day2/Theory/th23.png">  </p>

We are done with Floor and Power Planning.!
 
### Placement in OpenLANE
---
- **Library** consists of shape and size of all the standard cells(the different sizes of same standard cells is known as **drive strength**, lower size standard cell has lower driving strength), various functionality  of the same cells, timing and delay information of all the cells.
- We also keep decap cells, macros and IPs in Library.
#### Introduction to Placement 
- After floorplanning, next comes placement, it determines location of each of the components on the die. The standard cells that are present in the generated netlist are not the only cells that are placed. Placement enhances the design, removing any timing violations brought created  by the relative placement on the die.
- In placement we bind the netlist to a real-size physical cell. The physical standard cell will be taken from a library that offers various alternatives for the identical cells, shapes, dimensions, and delay.
- They are place it in the floorplaning(which have properly positioned input and output ports that are well designed) according to our netlist. To minimise timing delay, the flip flops must be positioned as close to the input and output pins as practicable.
- **The main goal of placement is to make sure Standard Cells are correctly placed in Standard Cells roots**

#### Initial placement of cells in our floorplan
![Iniplmnt](https://github.com/Ren-Ps/PD_RTL2GDS_SKY130_ps/blob/main/Day2/Theory/th8.png)

- To keep the **signal integrity**(Signal integrity or SI is a set of measures of the quality of an electrical signal. In digital electronics, a stream of binary values is represented by a voltage or current waveform.) we optimise placement, for that we calculate the wirelength and capacitance (C=εA/d) and then add **repeaters** and **buffers** based on those values. The wirelength will cause a capacitance and a resistance that will result in unwanted voltage drops and slew rates(slew is inversly proportional to capacitance) that might not be allowed for logic gates that switch current quickly. Inserting buffers for lengthy lines that serve as intermediaries and divide a single long wire into multiple ones would reduce resistance and capacitance. 
- If it needs to operate at a high frequency(2GHz), we occasionally also do **abutment**, in which logic cells are put very close to one another (virtually with zero delay). 
- Route crisscrossing is a common occurrence in PnR because the crisscrossed path can be implemented utilising a different metal layer (vias).

#### Optimised placement of cells in our floorplan after using buffers
![optimzd](https://github.com/Ren-Ps/PD_RTL2GDS_SKY130_ps/blob/main/Day2/Theory/th9.png)

- After placement optimization, timing analysis will be set up using an ideal clock, which has no wire delays and no clock buffer-related delays because CTS has not yet been completed.

Placement is now focused on **congestion rather than timing**. Standard cells are also not placed on the floorplan stage; rather, they are placed on the placement stage. The cells that are placed on the floorplan stage are macros or preplaced cells. 

#### Placement is done on two stages:
- **Global Placement** is placement without legalisations with the intention of cutting down on wirelength.The main function of Global Placement is to reduce wirelengthand in OpenLANE use the concept of of the HPWL (Half Perimeter Wirelength) reduction concept.
- **Detailed Placement** is placement with legalisation(legalisation is more required for timing point of view), where the standard cells must be adjacent, in standard rows, and without overlaps.


### Cell Design Flow
---

In IC design flow a library is a place where we keep all our standard cells, buffers, decap cell, etc. The library does not only have different cells with different functionality but it also have same cell with different sizes, threshold voltage, delays, etc.

- In Cell design we will look at how a standard cell is designed in the library
#### Inputs to Cell Design Flow

<p align="center">
 <img src="https://github.com/Ren-Ps/PD_RTL2GDS_SKY130_ps/blob/main/Day2/Theory/th10.png">  </p>

<p align="center">
 <img src="https://github.com/Ren-Ps/PD_RTL2GDS_SKY130_ps/blob/main/Day2/Theory/th11.png">  </p>

**Library and user Defined Specs**
- **Cell height** has been defined as the seperation between power and the ground rail and it is the responsibilty of the cell develepor that cell height is mantained. Cell height depends on the timing information(if the cell height is high then it would be able to drive more longer wire, that is called higher drive strength cells)
- The standard cells has to operate at a certain **Supply Voltage** which is beign provided by the top level designer and accordingly the library devloper has to take that supply voltage and design the library cell such that it specifies supply voltage.
- **Metal Layer**, **Pin Locations**, **Drawn Gate Length** requirments has to be decided by the library devloper.

<p align="center">
 <img src="https://github.com/Ren-Ps/PD_RTL2GDS_SKY130_ps/blob/main/Day2/Theory/th12.png">  </p>
 
 <p align="center">
 <img src="https://github.com/Ren-Ps/PD_RTL2GDS_SKY130_ps/blob/main/Day2/Theory/th13.png">  </p>

#### Design steps Outputs of Cell Design Flow

 <p align="center">
 <img src="https://github.com/Ren-Ps/PD_RTL2GDS_SKY130_ps/blob/main/Day2/Theory/th14.png">  </p>
 
- **Circuit Design** step is mostly based on spice simulations.
- From the circuit design step we get output called as **CDL(circuit discription language) file**
- So from circuit design step onse we known about the W/L ratios of nmos and pmos, then we need to implement it in layout design.
- **Art of Layout is Eular's path and stick diagram**

 <p align="center">
 <img src="https://github.com/Ren-Ps/PD_RTL2GDS_SKY130_ps/blob/main/Day2/Theory/th15.png">  </p>
 
- In **Layout Design** first step is to get the logic implemented with the help of nmos and pmos transistors and get a nmos and pmos nework graph out of our design and obtain the Eular's path(path wich has been traced only once) and then go for stick diagram out of it. 

 <p align="center">
 <img src="https://github.com/Ren-Ps/PD_RTL2GDS_SKY130_ps/blob/main/Day2/Theory/th16.png">  </p>

- Later we convert this stick diagram into a proper layout according to the rules which we have got from the input.
- Final step is to extrace the paracitcs out of the final layout and characterize it in terms of timing.
- The output of the layout will be **GDSII(Graphic Design System II) file**, **LEF(Library Exchange Format ) file**(define width and heigt of the cell) and the **extraced spice netlist**(define resistance and capacitance of all the nodes)


### Characterization 
---
- Next step after we get the extraced step netlist and layout is characterization(*that's what this course is all about*)
- **Characterization** helps us to get timing, noise and power information.
- The output of characterization is **timing, noise, power.lib files** and the **functionality** of this circuit

#### Characterization Flow
1. Read in the models files
2. Read the extreaced spice netlist
3. Recognize the behaviour of the buffer
4. Read the sub circuits of the inverter
5. Attach the necessary power sources 
6. Apply the stimulus
7. Provide the necessary output capacitances 
8. Prove the necessary simulation commands
#### Characterization Setup

 <p align="center">
 <img src="https://github.com/Ren-Ps/PD_RTL2GDS_SKY130_ps/blob/main/Day2/Theory/th18.png">  </p>
 
  <p align="center">
 <img src="https://github.com/Ren-Ps/PD_RTL2GDS_SKY130_ps/blob/main/Day2/Theory/th19.png">  </p>

- Next step is to feed in all these inputs from 1 to 8 as in form of a configuration file to the characterization software called as **GUNA** and this software will generate timing, noise and power models.

 <p align="center">
 <img src="https://github.com/Ren-Ps/PD_RTL2GDS_SKY130_ps/blob/main/Day2/Theory/th20.png">  </p>

- The output of GUNA(.lib files) are characterized into :
1. Timing Characterization
2. Power Characterization
3. Noise Characterization
  
### Timing Characterization:
---
The slew timing parameters are listed below. Two inverters are connected in series, called as buffers(circuit is shown above)
#### Propogation Delay and Transition Time

 <p align="center">
 <img src="https://github.com/Ren-Ps/PD_RTL2GDS_SKY130_ps/blob/main/Day2/Theory/th21.png">  </p>
    
- The timing parameters for propagation delay are listed below.
    
 <p align="center">
 <img src="https://github.com/Ren-Ps/PD_RTL2GDS_SKY130_ps/blob/main/Day2/Theory/th22.png">  </p>
    
- **Propogation Delay** is defined as, time(out_thr)-time(in_thr)
- **Transition Time** is defined as, time(slew_high_rise_thr)-time(slew_low_rise_thr)  **OR**  time(slew_high_fall_thr)-time(slew_low_fall_thr).
- 
- It's unexpected to see negative propagation delay because the output occurs before the input. So in that case the designer must select the proper threshold value to create a positive delay. The typical delay threshold is 50% and slew low thresholds is 20% of Vdd and slew high threshold 80% of Vdd.

### LAB DAY 2 :

### Steps to run and view floorplan using OpenLANE
---
1. **Set configuration variables** 
- The configuration variables or switches must be set up before to starting the floorplan stage.. 
- The configuration variables location is 
  > pssh23@vsd-pd-workshop-01:~/Desktop/work/tools/openlane_working_dir/openlane/configuration$
```
.
├── README.md      
├── floorplan.tcl 
├── cts.tcl
├── checkers.tcl
├── general.tcl
├── lvs.tcl
├── synthesis.tcl 
├── routing.tcl
└── placement.tcl
```

The default OpenLANE settings are contained in the `.tcl` files, and the `README.md` defines every configuration variable for every stage. 
    
#### `README.md` file contains,

 <p align="center">
 <img src="https://github.com/Ren-Ps/PD_RTL2GDS_SKY130_ps/blob/main/Day2/Lab/LB1.png">  </p>   
   
Here we find every configuration that the current run has approved. This could originate from (in order of priority):
- PDK specific configuration `sky130A_sky130_fd_sc_hd_config.tcl` inside the `openlane/design/picorv32a` folder
- `config.tcl` inside the `openlane/designs/picorv32a` folder
- System default settings inside `openlane/configurations`

#### `floorplan.tcl` file contains,

 <p align="center">
 <img src="https://github.com/Ren-Ps/PD_RTL2GDS_SKY130_ps/blob/main/Day2/Lab/LB2.png">  </p>   

2. **Run floorplan on OpenLane:** 
- Use this command OpenLane
```
    run_floorplan
```
 <p align="center">
 <img src="https://github.com/Ren-Ps/PD_RTL2GDS_SKY130_ps/blob/main/Day2/Lab/LB4.png">  </p>   
 
 3.**Review floorplan files:**
Here basically the ceated files are being checkd using the log files presen in the `log/floorpla/4-ioPlacer.log`. 
 <p align="center">
 <img src="https://github.com/Ren-Ps/PD_RTL2GDS_SKY130_ps/blob/main/Day2/Lab/LB5.png">  </p>   

In case it is not there we can check it using the Magic tool.

 - for floorplan the core utilization is 50%

 - for config.tcl file under the runs folder core utilization is 35%.
    
 - setting the core utilization, verticle and horizontal metal layer by add these three switchs in the config.tcl file

```
set ::env(FP_CORE_UTIL) 50
set ::env(FP_IO_VMETAL) 3
set ::env(FP_IO_HMETAL) 4
```

#### Calculations of Die Area

The **def(design exchange format)** file, containing the die area and positions which is at location 
> pssh23@vsd-pd-workshop-01:~/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/29-01_11-50/results/floorplan$ vim picorv32a.floorplan.def

<p align="center">
 <img src="https://github.com/Ren-Ps/PD_RTL2GDS_SKY130_ps/blob/main/Day2/Lab/LB7.png">  </p>    

- One micrometre is equal to 1000 database units in this case when the die area is expressed in database units. 
- The **die's surface area is therefore 443587 microns squared**, or (660685/1000)microns*(671405/1000)microns.

<p align="center">
 <img src="https://github.com/Ren-Ps/PD_RTL2GDS_SKY130_ps/blob/main/Day2/Lab/LB6.png">  </p> 
