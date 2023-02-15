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
       - [Optimised placement of cells in our floorplan after using buffers](#optimised-placement-of-cells-in-our-floorplan-after-using-buffers)
       - [Placement is done on two stages](#placement-is-done-on-two-stages)
    - [Cell Design Flow](#cell-design-flow)
    - [Characterization](#characterization)
    - [Timing Characterization](#timing-characterization)
    - [LAB Day 2](#lab-day-2-)
       - [TASK 2: Calculating Die Area](#task-2-calculating-die-area)
* [Day 3 - Design library cell using Magic Layout and ngspice characterization](#day-3---design-library-cell-using-magic-layout-and-ngspice-characterization)
   - [LAB Day 3](#lab-day-3)
   - [Labs for CMOS inverter ngspice simulations](#labs-for-cmos-inverter-ngspice-simulations)
       - [Creating SPICE deck](#creating-spice-deck)
       - [Analysing the inverter](#analysing-the-inverter)
       - [LAB SETUP](#lab-setup)
   - [Inception of Layout Â CMOS fabrication process (16 mask process)](#inception-of-layout-Â-CMOS-fabrication-process-(16-mask-process))
   - [LAB DAY 3 (PART 2)](#labb-day-3(part-2))
   - [LAB DAY 3 (PART 3)](#labb-day-3(part-3))
       - [TASK 3: Calculating Transition & Propogation Delays](#task-3-calculating-transition--propogation-delays)
* [DAY 4 Pre-layout Timing Analysis & Importance of Good Clock Tree](#day-4-pre-layout-timing-analysis--importance-of-good-clock-tree)
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
- The output of GUNA(.lib files) are characterized into :
  - Timing Characterization
  - Power Characterization
  - Noise Characterization
 
 <p align="center">
 <img src="https://github.com/Ren-Ps/PD_RTL2GDS_SKY130_ps/blob/main/Day2/Theory/th20.png">  </p>

### Timing Characterization:
---
The slew timing parameters are listed below. Two inverters are connected in series, called as buffers(circuit is shown above)

- Timing Threshold Definitions:
  - slew_low_rise_thr:  Point towards the lower set of the rising curve of the output. Typically 20% of Vdd.
  - slew_high_rise_thr: Point towards the higher set of the rising curve of the output. Typically 80% of Vdd.

  - slew_low_fall_thr:  Point towards the lower set of the falling curve of the output. Typically 20% of Vdd.
  - slew_high_fall_thr: Point towards the higher set of the falling curve of the output. Typically 80% of Vdd.

  - in_rise_thr:  Defines the point towards the centre of the rising curve of the input. Typically 50% of Vdd.
  - in_fall_thr:  Defines the point towards the centre of the falling curve of the input. Typically 50% of Vdd.

  - out_rise_thr: Defines the point towards the centre of the rising curve of the output. Typically 50% of Vdd.
  - out_fall_thr: Defines the point towards the centre of the falling curve of the output. Typically 50% of Vdd.
 
 #### Propogation Delay
 
 <p align="center">
 <img src="https://github.com/Ren-Ps/PD_RTL2GDS_SKY130_ps/blob/main/Day2/Theory/th22.jpg">  </p>
 
- The formula as, 
<p align="center"> Propogation Delay for_HIGH_to_LOW (tphl) =  (in_rise_thr Time) - (out_fall_thr Time) </p>
<p align="center"> Propogation Delay for_LOW_to_HIGH (tplh)	=  (in_fall_thr Time) - (out_rise_thr Time) </p>
                        
- The delay should always come positive.
- Negative delay is not expected and if negative delay is received then it is due to poor choice of threshold points.
- The designer must select the proper threshold value to create a positive delay. The typical delay threshold is 50%. 

 #### Transition Time
 
  <p align="center">
 <img src="https://github.com/Ren-Ps/PD_RTL2GDS_SKY130_ps/blob/main/Day2/Theory/th21.png">  </p>
 
- The formula as,
<p align="center"> Transition Time (rise) = (Slew_high_rise_thr Time)  -  (Slew_low_rise_thr Time) </p>
<p align="center"> Transition Time (fall) = (Slew_high_fall_thr Time)  -  (Slew_fall_rise_thr Time) </p>

- It's unexpected to see negative propagation delay because the output occurs before the input. So in that case the designer must select the proper threshold value to create a positive delay. 
- The typical delay slew low thresholds is 20% of Vdd and slew high threshold 80% of Vdd.
 <p align="center">
 <img src="https://github.com/Ren-Ps/PD_RTL2GDS_SKY130_ps/blob/main/Day3/Theory/th2.png">  </p>


### LAB DAY 2 : Steps to run and view of Floorplan and Placement using OpenLANE
---
1. **Set configuration variables** 
- The configuration variables or switches must be set up before to starting the floorplan stage.. 
- The configuration variables location is 
  > pssh23@vsd-pd-workshop-05:~/Desktop/work/tools/openlane_working_dir/openlane/configuration$
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
- Useing command in OpenLane,
```
    run_floorplan
```
 <p align="center">
 <img src="https://github.com/Ren-Ps/PD_RTL2GDS_SKY130_ps/blob/main/Day2/Lab/LB4.png">  </p>   
 
3. **Review floorplan files:**
Here basically the ceated files are being checkd using the log files presen in the `log/floorpla/4-ioPlacer.log`. 
 <p align="center">
 <img src="https://github.com/Ren-Ps/PD_RTL2GDS_SKY130_ps/blob/main/Day2/Lab/LB5.png">  </p>   

In case it is not there we can check it using the Magic tool.

 - for floorplan the core utilization is 50%

 - for config.tcl file under the runs folder core utilization is 35%.
    
 - setting the core utilization, verticle and horizontal metal layer by add these three switchs in the config.tcl file

```
set ::env(FP_CORE_UTIL) 65
set ::env(FP_IO_VMETAL) 4
set ::env(FP_IO_HMETAL) 3
```

### TASK 2: Calculating Die Area

The **def(design exchange format)** file, containing the die area and positions which is at location 
> pssh23@vsd-pd-workshop-05:~/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/29-01_11-50/results/floorplan$ vim picorv32a.floorplan.def

<p align="center">
 <img src="https://github.com/Ren-Ps/PD_RTL2GDS_SKY130_ps/blob/main/Day2/Lab/LB7.png">  </p>    


- One micrometre is equal to 1000 database units in this case when the die area is expressed in database units. 

<p align="center">
 <img src="https://github.com/Ren-Ps/PD_RTL2GDS_SKY130_ps/blob/main/Day2/Lab/LB6.png">  </p> 
 
***Calculating the Die Area = (660685 / 1000) x (671405/1000) = 443587.2124 um <sup>2</sup>***


4. **View the floorplan in magic:**

For visualising the layout following a floorplan, utilise the Magic Layout Tool. The following three files are necessary in order to examine a floor layout in Magic
- Technology File `sky130A.tech` (provided by foundary here its provided by pdk of sky130)
- Merged LEF file `merged.lef`(Library Exchange Format files are provided by foundary which contain design rules and abstract information about the standard cells)
- `vim picorv32a.floorplan.def` files (Design Exchange Format files are generated after floorplanning and contains the placement information of macros , standard cells, I/O pins and other physical entities)

To open layout in magic use this command in this location
> pssh23@vsd-pd-workshop-05:~/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/29-01_11-50/results/floorplan$

```
magic -T /home/kunalg123/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.floorplan.def
```
- `-T <address_of_sky130A.tech_file>` where T is for technology file 
- `lef read <address_of_merged.lef_file>` to read the lef files (we use `lef read` because its a standard industry file)
- `def read <address_of_picorv32a.floorplan.def_file>` to read the def files (we use `def read` because its a standard industry file) 
- **NOTE** If address of the required file is at the same working loaction then we just need to provide the required file name.

#### Floorplan looks as,

<p align="center">
 <img src="https://github.com/Ren-Ps/PD_RTL2GDS_SKY130_ps/blob/main/Day2/Lab/LB8-3.png">  </p> 

#### Description of blocks of floorplan

<p align="center">
 <img src="https://github.com/Ren-Ps/PD_RTL2GDS_SKY130_ps/blob/main/Day2/Lab/LB8-2.png">  </p> 
 
 - **Taps Cells** are ment to avoid the lachup condition which occur in the cmos devices, they connect the nwell to th vdd and the substrate to the ground. Here they are diagonaly equdistant which alraedy has ben set in readme file
- Floarplan do not take into considerations the placement of **standard cells**, but standard cells are present at the corner.

5. **Run Placement on OpenLane:** 

```
run_placement
```
<p align="center">
 <img src="https://github.com/Ren-Ps/PD_RTL2GDS_SKY130_ps/blob/main/Day2/Lab/LB9.png">  </p> 

- To run Global Placement in OpenLANE run `% run_placement` command in openlane
- This command is a wrapper which does global placement (performed by RePlace tool),, optimization by Resier's  tool, and ReSer's detailed placement functions (by OpenDP tool). 
- It shows hundreds of iterations with HPWL and OVFL displayed. If the overflow is getting smaller, the algorithm is considered to be converging. It additionally verifies legality.


6. **View the placement in magic:**
For visualising the layout following a placement, utilise the Magic Layout Tool. The following three files are necessary in order to examine a floor layout in Magic
- Technology File `sky130A.tech`
- Merged LEF file `merged.lef` 
- `vim picorv32a.floorplan.def` files

To open layout in magic use this command in this location
> pssh23@vsd-pd-workshop-05:~/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/29-01_11-50/results/floorplan$

```
magic -T /home/kunalg123/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.floorplan.def
```
- `-T <address_of_sky130A.tech_file>`where T is for technology file
- `lef read <address_of_merged.lef_file>` to read the lef files (we use `lef read` because its a standard industry file)
- `def read <address_of_picorv32a.floorplan.def_file>` to read the def files (we use `def read` because its a standard industry file)
- If address of the required file is at the same working loaction then we just need to provide the required file name.

***NOTE***
This `sky130A.tech`(technology), `merged.lef`(layout exchange format) and `picorv32a.placement.def`(design exchange format) files comes allong with the pdk of sky130.
#### Placement looks as,

<p align="center">
 <img src="https://github.com/Ren-Ps/PD_RTL2GDS_SKY130_ps/blob/main/Day2/Lab/lld3.png">  </p> 
 
- So these many standard cells where in the bottom left corner in the initial layout of the floorplan which are now placed in our floorplan.

#### Zoom view of Placement of the Standard Cells
<p align="center">
 <img src="https://github.com/Ren-Ps/PD_RTL2GDS_SKY130_ps/blob/main/Day2/Lab/l4.png">  </p> 

***NOTE*** - This power distribution network gets created during floorplan but in OpenFLOW right now the order is little different, the floorplan does not create this power distribution network, it is done in Post CTS(Clock Tree Synthesis) just before we route it.

 
***placement done..!***
 
 
## Day 3 : Design library cell using Magic Layout and ngspice characterization
---
### Labs for CMOS inverter ngspice simulations
--- 
In this we would be going into depth of one of the cells(inverter cell), we won't build it from scratch rather we would use the github to get the `.mag`(magic) files and from there we will be doing Post Layout simulation in ngspice and post characterizing our sample cell, we would be plugging this cell into a OpenLANE flow, into picorv32a core.
    
**NOTE** - In I/O placement in floorplan on OpenLANE, configurations can be modified while in flight. On OpenLANE, for instance, use  `set ::env(FP_IO_MODE) 2` to make I/O mode not equidistant. On mode 2, the I/O pins won't be evenly spaced out(default of 1). View the `.def` layout for magic by launching floorplan once more with `run floorplan`. The configuration will only be available for the current session if it is changed on the fly; it will not be changed in `runs/config.tcl`, whereas `echo $::env(FP_IO_MODE)`is used to output the variable's most recent value.

First we need to design the library cell.

#### Creating SPICE deck for CMOS Inverter

1. SPICE Deck - It is a connectivity information about a cell. It is a netlist. It has the inputs, tap points, etc.

2. We need to define the component parameter i.e., value for PMS and NMOS. For us value of W/L of PMOS M1 (0.375u/o.25u) and NMOS M2 (0.375u/0.25). Ideally PMOS should be 2 or 3 times wider than NMOS. The load cap is assumed to be 10 fF. 

3. We assume an input supply voltage value (GATE) as 2.5 V and main supply voltage (at drain) as 2.5 V. Generally the supply voltage (GATE) is multiple of length.

4. Now we need to identify the node (those two point in b/w there is a component) and name these nodes.

The SPICE Deck is written below: 

<p align="center">
 <img src="https://github.com/Ren-Ps/PD_RTL2GDS_SKY130_ps/blob/main/Day3/Theory/th3.png"> </p>

- PMOS and NMOS descriptor syntax
    + `[component name] [drain] [gate] [source] [substrate] [transistor type] W=[width] L=[length]`
- Based on nodes and their values, all components are described.
- For load cap connecivity and value `[name] [node1] [node2] [value]'
- Supply voltage `[name] [node1] [node2] [value]`
- Input voltage `[name] [node1] [node2] [value]`
    
**SIMULATION COMMANDS**
- `.op` `.dc Vin 0 2.5 0.05` is the start of SPICE simulation operation where Vin will be sweep from 0 to 2.5 with 0.05 steps
- `tsmc_025um_model.mod` is the model file which contain the technological parameters of the 0.25um NMOS and PMOS Devices.

First invoke the ngspice and then run the following command to simulate:

```
source [filename].cir
run
setplot 
dc1 
plot out vs in 
```

#### Analysing the inverter

* Switching threshold voltage (Vm) - The point where exact transition takes place i.e., Vin = Vout. At this point both the MOS are in saturation and we have a high leakage current (direct current flowing from vdd to ground). If the pull up network is strong the VTC moves towards right (Vm' > Vm) and if pull down network is strong then VTC shifts leftwards (Vm' < Vm).

**Formula for Vm**

<p align="center">
 <img src="https://github.com/Ren-Ps/PD_RTL2GDS_SKY130_ps/blob/main/Day3/Theory/th1.png"> </p>
 
As if needed further, transient analysis can be performed.

### CMOS Fabrication Process (16-Mask CMOS Process):
---
1. <b>Selecting a substrate</b> = Layer where the IC is fabricated. Most commonly used is P-type substrate
2. <b>Creating active region for transistor</b> = Separate the transistor regions using SiO2 as isolation

- Mask 1 = Covers the photoresist layer that must not be etched away (protects the two transistor active regions)
- Photoresist layer = Can be etched away via UV light
- Si3N4 layer = Protection layer to prevent SiO2 layer to grow during oxidation (oxidation furnace)
- SiO2 layer = Grows during oxidation (LOCOS = Local Oxidation of Silicon) and will act as isolation regions between transistors or active regions

<p align="center">
 <img src="https://github.com/Ren-Ps/PD_RTL2GDS_SKY130_ps/blob/main/Day3/Theory/th11.png"> </p>

3. <b>N-Well and P-Well Fabrication</b> = Fabricate the substrate needed by PMOS (N-Well) and NMOS (P-Well)

- Phosporus (5 valence electron) is used to form N-well
- Boron (3 valence electron) is used to form P-Well.
- Mask 2 protects the N-Well (PMOS side) while P-Well (NMOS side) is being fabricated then Mask 3 while N-Well (PMOS side) is being fabricated

<p align="center">
 <img src="https://github.com/Ren-Ps/PD_RTL2GDS_SKY130_ps/blob/main/Day3/Theory/th12.png"> </p>

4. <b>Formation of Gate</b> = Gate fabrication affects threshold voltage. Factors affecting threshold voltage includes:
<p align="center">
 <img src="https://github.com/Ren-Ps/PD_RTL2GDS_SKY130_ps/blob/main/Day3/Theory/th13.png"> </p>

Main parameters are:

- Doping Concentration = Controlled by ion implantation (Mask 4 for Boron implantation in NMOS P-Well and Mask 5 for Arsenic implantation in PMOS N-Well)
- Oxide capacitance = Controlled by oxide thickness (SiO2 layer is removed then rebuilt to the desire thickness)
Mask 6 is for gate formation using polysilicon layer.

<p align="center">
 <img src="https://github.com/Ren-Ps/PD_RTL2GDS_SKY130_ps/blob/main/Day3/Theory/th14.png"> </p>

5. <b>Lightly Doped Drain formation </b>= Before forming the source and drain layer, lightly doped impurity is added:

- Mask 7 for N- implantation (lightly doped N-type) for NMOS
- Mask 8 for P- implantation (lightly doped P-type) for PMOS.
- Heavily doped impurity (N+ for NMOS and P+ for PMOS) is for the actual source and drain but the lightly doped impurity will help maintain spacing between the source and drain and prevent hot electron effect and short channel effect.

<p align="center">
 <img src="https://github.com/Ren-Ps/PD_RTL2GDS_SKY130_ps/blob/main/Day3/Theory/th15.png"> </p>

6. <b>Source and Drain Formation </b>= Mask 9 is for N+ implantation and Mask 10 for P+ implantation

Channeling is when implantations dig too deep into substrate so add screen oxide before implantation
The side-wall spacers maintains the N-/P- while implanting the N+/P+

<p align="center">
 <img src="https://github.com/Ren-Ps/PD_RTL2GDS_SKY130_ps/blob/main/Day3/Theory/th16.png"> </p>

7. <b>Form Contacts and Interconnects </b>= TiN is for local interconnections and also for bringing contacts to the top. TiS2 is for the contact to the actual Drain-Gate-Source. Mask 11 is for etching off the TiN interconnect for the first layer contact.

<p align="center">
 <img src="https://github.com/Ren-Ps/PD_RTL2GDS_SKY130_ps/blob/main/Day3/Theory/th17.png"> </p>

8. <b>Higher Level Metal Formation</b> = We need to planarize first the layer via CMP before adding a metal interconnect. Aluminum contact is used to connect the lower contact to higher metal layer. Process is repeated until the contact reached the outermost layer.

- Mask 12 is for first contact hole
- Mask 13 is for first Aluminum contact layer
- Mask 14 is for second contact hole
- Mask 15 is for second Aluminum contact layer. Mask 16 is for making contact to topmost layer.

<p align="center">
 <img src="https://github.com/Ren-Ps/PD_RTL2GDS_SKY130_ps/blob/main/Day3/Theory/th18.png"> </p>


### Inverter Standard cell Layout & SPICE extraction

- The Magic layout of a CMOS inverter will be used so as to intergate the inverter with the picorv32a design. To do this, inverter magic file is sourced from `vsdstdcelldesign` by cloning it within the `openlane_working_dir/openlane` directory as follows:
```
git clone https://github.com/nickson-jose/vsdstdcelldesign
```
- This creates a vsdstdcelldesign named folder in the openlane directory.

<p align="center">
 <img src="https://github.com/Ren-Ps/PD_RTL2GDS_SKY130_ps/blob/main/Day3/Lab/LB10.png"> </p>
 
- To invoke magic to view the sky130_inv.mag file, the sky130A.tech file must be included in the command along with its path. To ease up the complexity of this command, the tech file can be copied from the magic folder to the vsdstdcelldesign folder.

```
cp sky130A.tech /[target location]

target location here - /home/pssh23/Desktop/work/tools/openlane_working_dir/openlane/vsdstdcelldesign
```

<p align="center">
 <img src="https://github.com/Ren-Ps/PD_RTL2GDS_SKY130_ps/blob/main/Day3/Lab/LB11-1.png"> </p>
 
- The `sky130_inv.mag `file can then be invoked in Magic very easily:

```
magic -T [tech file] [.mag file]
tech file = sky130A.tech .mag file = sky130_inv.mag
```

<p align="center">
 <img src="https://github.com/Ren-Ps/PD_RTL2GDS_SKY130_ps/blob/main/Day3/Lab/LB12.png"> </p>


 #### Layout of the CMOS Inveter in magic
 
 <p align="center">
 <img src="https://github.com/Ren-Ps/PD_RTL2GDS_SKY130_ps/blob/main/Day3/Lab/LB14.png"> </p>

-In a magic layout, to check if there are connections between two parts of the circuit, press "S" button on keyboard 3 times.
-LEF is library exchanged format.
-Functionality of LEF is protecting the IP.
-Using S to identify parts of the layout:

<p align="center">
 <img src="https://github.com/Ren-Ps/PD_RTL2GDS_SKY130_ps/blob/main/Day3/Lab/LB15.png"> </p>
 
-In Sky130 the first layer is called the local interconnect layer or Locali as shown above (blue wave shape like squrs_blcks).
-To verify whether the layout is that of CMOS inverter, verification of P-diffusiona nd N-diffusion regions with Polysilicon can be observed:

 <p align="center">
 <img src="https://github.com/Ren-Ps/PD_RTL2GDS_SKY130_ps/blob/main/Day3/Lab/LB16.png"> </p>

- Refer this [to build and inverter from scratch(workshop reference)](https://github.com/nickson-jose/vsdstdcelldesign)
- [Video reference](https://www.youtube.com/watch?v=RPppaGdjbj0)

***Magic Commands:***
- Left click = lower-left corner of box
- Right click = upper-right corner of box
- "z" = zoom in, "Z" = zoom out, "ctrl + z" = zoom into the box
- Middle click on empty area will turn the box into empty (similar to erasing it)
- "s" three times will select all geometries electrically connected to each other
- `:box` = display parameters of selected box
- `:grid` 0.5um 0.5um = turn on/off and set grid
- `:snap user` = snap based on current grid
- `:help snap` = display help for command
- `:drc style drc(full)` = use all DRC when doing DRC checking
- `:paint poly` = paint "poly" to current box
- `:drc why` = show drc violation inside selected area (white dots are DRC violations )
- `:erase poly` = delete poly inside the box
- `:select area` = select all geometries inside the box
- `:copy n 30` = copy selected geometries to North by 30 grid steps
- `:move n 1` = move selected geometries to North by 1 step ("." to move more, "u" to undo)
- `: select cell _08555_` = select a particular cell instance (e.g. cell _08555_ which can be searched in the DEF file)
- `:cellname allcells` = list all cells in the layout
- `:cellname exists sky130_fd_sc_hd__xor3_4` = check if a cell exists
- `:drc why` = show DRC violation and also the DRC name which can be referenced from [Sky130 PDK Periphery Rules](https://skywater-pdk.readthedocs.io/en/main/rules/periphery.html#rules-periphery--page-root).


<p align="center">
 <img src="https://github.com/Ren-Ps/PD_RTL2GDS_SKY130_ps/blob/main/Day3/Lab/LB17.png"> </p>

<p align="center">
 <img src="https://github.com/Ren-Ps/PD_RTL2GDS_SKY130_ps/blob/main/Day3/Lab/LB19.png"> </p>
 
 Other verification steps are to check drain and source connections. The drains of both PMOS and NMOS must be connected to output port (here, Y) and the sources of both must be connected to power supply VDD (here, VPWR).

Zooming-In to select unit box i.e. Root cell box and get micron & lambada values.
<p align="center">
 <img src="https://github.com/Ren-Ps/PD_RTL2GDS_SKY130_ps/blob/main/Day3/Lab/LB23.png"> </p>

<b>LEF or library exchange format:</b> A format that tells us about cell boundaries, VDD and GND lines. It contains no info about the logic of circuit and is also used to protect the IP.

SPICE extraction: Within the Magic environment, following commands are used in tkcon to achieve .mag to .spice extraction:
``` 
extract all
ext2spice cthresh 0 rethresh 0
ext2spice
 ```
 <p align="center">
 <img src="https://github.com/Ren-Ps/PD_RTL2GDS_SKY130_ps/blob/main/Day3/Lab/LB21.png"> </p>
 
* Viewing the spice file. 

```
vim sky130_inv.spice
```
 <p align="center">
 <img src="https://github.com/Ren-Ps/PD_RTL2GDS_SKY130_ps/blob/main/Day3/Lab/LB22.png"> </p>
 
 * Add further spice commands as shown, 
 <p align="center">
 <img src="https://github.com/Ren-Ps/PD_RTL2GDS_SKY130_ps/blob/main/Day3/Lab/LB25.png"> </p>
 
  <p align="center">
 <img src="https://github.com/Ren-Ps/PD_RTL2GDS_SKY130_ps/blob/main/Day3/Lab/LB24.png"> </p>
 
 * Movie to direcotry as shown for viewing files as
   ``` - pshort.lib ```
   ``` - nshort.lib ```
       
 * Viewing the lib file.
 ```
 vim pshort.lib
 ```  
  <p align="center">
 <img src="https://github.com/Ren-Ps/PD_RTL2GDS_SKY130_ps/blob/main/Day3/Lab/LB26.png"> </p>
 
 * Keep scrolling in to find the device name which we are going to use in spice deck.
  * Here the p-mos model name is ``` pshort_model ```.
  * The same way, for n-mos  model name is ``` nshort_model ```.
  <p align="center">
 <img src="https://github.com/Ren-Ps/PD_RTL2GDS_SKY130_ps/blob/main/Day3/Lab/LB27.png"> </p>
 
 * Update the device model names in spice dec. Also as if needed include library path/.lib .
 
  <p align="center">
 <img src="https://github.com/Ren-Ps/PD_RTL2GDS_SKY130_ps/blob/main/Day3/Lab/LB28.png"> </p>
 
 * now on terminal runthe spice code
 ```ngspice sky130_inv.spice```
 
  <p align="center">
 <img src="https://github.com/Ren-Ps/PD_RTL2GDS_SKY130_ps/blob/main/Day3/Lab/LB29.png"> </p>
 
 * On succesful i.e. error free simulation, a waveform of interest (which is mentioned in spice code) will apear as,
 
  <p align="center">
 <img src="https://github.com/Ren-Ps/PD_RTL2GDS_SKY130_ps/blob/main/Day3/Lab/LB30.png"> </p>
 
 ### TASK 3: Calculating Transition & Propogation Delays
 ---
 
 * Using mouse pointer click the points of interest. Its relevent x and y axis (i.e. here would be Voltage Vs Time) points will apear on terminal. Note those points to get timing analysis.
 
  <p align="center">
 <img src="https://github.com/Ren-Ps/PD_RTL2GDS_SKY130_ps/blob/main/Day3/Lab/LB31-3.png"> </p>
 
 * Rise time (tr) is the time, during transition, when output switches from 20% to 80% of the maximum value. 
   * Tr = (2.19898e-09) - (2.15306e-09) = ***0.04592 nSec.***
 
 * Fall time (tf) is the time, during transition, when output switches from 80% to 20% of the maximum value.
   * Tf = (4.07653e-09) - (4.04082e-09) = ***0.03571 nSec.***


  <p align="center">
 <img src="https://github.com/Ren-Ps/PD_RTL2GDS_SKY130_ps/blob/main/Day3/Lab/LB32-1.png"> </p>
 
 * The propagation delay high to low (tpHL) is the delay when output switches from high-to-low, after input switches from low-to-high. The delay is usually calculated at 50% point of input-output switching.
   * Tphl = (2.18367e-09) - (2.18367e-09) = ***0.03061 nSec.***
   * Tplh = (4.05278e-09) - (4.04969e-09) = ***0.00309 nSec.***

Next objective is to use this layout of inverter to create a lef file. Using this lef in openlane and plugging this cell we will make a custom cell. We will plug this in picorv32a.

**DRC Rules & Analysis**

* Technology files have all the technology related file. It consists all information about the layer, pattern, electrical connectivity, GDS generation rule, DRC rule, all other kind of rules, etc. Tnformation about the technology files can be found on [MAGIC_VLSI](http://opencircuitdesign.com/magic/index.html). 

* cif - caltech intermediate formate - It is used interchangably with gds in magic tech file and documentation. For [DRC rules](https://skywater-pdk.readthedocs.io/en/main/rules/periphery.html#rules-periphery--page-root). The basic DRC rules are called edge based rules. 

* Download the required DRC_test files using the command.

```
wget http://opencircuitdesign.com/open_pdks/archive/drc_test.tgz
```

* Upon extraction we find that there are .mag files and sky130A.tech file.

* Now we can use magic to analyse the DRC rule and fix it if it's violated.
* Magic may invoked here by, ``` magic -d XR ```
 
 
# DAY 4 Pre-layout timing analysis and importance of good clock tree
---

OpenLANE is a Place & Route flow and for placement of any cell may not require the entire 'mag file' information.  PnR tool does not need all informations from the .mag file like the logic part but only PnR boundaries, power/ground ports, and input/output ports.  Here 'lef files' come into picture. lef files has only these information. It protects our IP. So the next step is to extract the LEF file from Magic. But first, we need to follow guidelines of the PnR tool for the standard cells:

- The input and output ports lies on the intersection of the horizontal and vertical tracks (ensure the routes can reach that ports).
The width of the standard cell must be odd multiple of the tracks horizontal pitch and height must be odd multiples of tracks vertical pitch

- The width of the standard cell must be odd multiple of the tracks horizontal pitch and height must be odd multiples of tracks vertical pitch.

#### Extracting the LEF File:

- Objective is to extract the lef file from .mag file and then try to plug the lef file to picorv32a. (that is instead of std cell we will use our own design)
- To check these guidelines, we need to change the grid of Magic to match the actual metal tracks. The ``` pdks/sky130A/libs.tech/openlane/sky130_fd_sc_hd/tracks .info``` contains those metal informations.
- Then do `` less tracks.info ``` for viewing.

<p align="center">
 <img src="https://github.com/Ren-Ps/PD_RTL2GDS_SKY130_ps/blob/main/Day4/LAB/LB3.png"> </p>
Horizontal track pitch = 0.46, verticle track pitch = 0.34, horizontal offset = 0.23, verticle offset = 0.17

Tracks are used during routing. route can usually go above the track which are the layers. So, route are basically metal traces. PnR is automated process so we need to specify where do we want our route can go and this information is given by the tracks. Hence tracks are guide to route. Horizontal and verticle track pitches are mentioned. 

***The ports (in and o/p) are in li1 metal layer. So we need to ensure these ports are on intersection or on li1 horizontal and verticle layer.***

We now werify the guideline using magic. Pressing `g` make the grid visible. We will converge the grid with track value so that we can verify that our ports are actually on the intesection of horizontal and verticle li1 or not. So we try to take track file as reference and verify our file by getting grid information from tkcon window.

From track file we can get x pitch, y pitch, verticle offset and horizontal offset. Let's make a grid according to the track information.

command inside vsdstdcelldesign

```
magic -T sky130A.tech sky130_inv.mag &
```
   
<p align="center">
 <img src="https://github.com/Ren-Ps/PD_RTL2GDS_SKY130_ps/blob/main/Day4/LAB/LB4.png"> </p>
 
 We can observe horizontal and verticle crossing also we can observe the grid spacing is changed.
 
 <p align="center">
 <img src="https://github.com/Ren-Ps/PD_RTL2GDS_SKY130_ps/blob/main/Day4/LAB/LB5-1.png"> </p>
 
The width of the std cell in x direction (x pitch) should be odd multiple of the x pitch and height of the std cell y direction should be odd multiple of the y pitch. We find that the grids are as per our conditions.
 
 **LEF file extraction**

Ports doesn't mean anything to magic. Port definations are required while we want to extract the lef files. After extraction ports are converted in pins. The LEF file contains the cell size, port definitions, and properties which aid the placer and router tool. With that, the ports definition, port class, and port use must be set first. The instructions to set these definitions via Magic are on the [vsdstdcelldesign repo](https://github.com/nickson-jose/vsdstdcelldesign#create-port-definition).

 <p align="center">
 <img src="https://github.com/Ren-Ps/PD_RTL2GDS_SKY130_ps/blob/main/Day4/LAB/LB6.png"> </p>

Once we have defined the ports. Our next step is to define the purpose of the ports. For that we do port class and port use. Refer to [vsdstdcelldesign repo](https://github.com/nickson-jose/vsdstdcelldesign#create-port-definition).

After setting the parameters we are ready to extract lef file from our mag. We give the cell a custom name - `save sky130_vsdinv.mag` (command in the tkcon tab).

Then open our new inverter mag. 

```
magic -T sky130A.tech sky130_vsdinv.mag
```

Command to create the lef file (run in tkcon window)

```
lef write [name optional]
```

 <p align="center">
 <img src="https://github.com/Ren-Ps/PD_RTL2GDS_SKY130_ps/blob/main/Day4/LAB/LB7.png"> </p>
 
Setting a layer as port create a PIN in the macro. Now our lef file is ready.
 
 **Introduction to timing libs and steps to include new cell in synthesis*

* First copy the newly created lef to src under picorv32a:

```
cp sky130_vsdinv.lef /home/pssh23/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/src
```

We need to have a library which has our cell defination for synthesis so that abc can map it. (inside vsdstdcelldesign -> libs). We have different library file for different PVT and of different speed. 

* We will require fast slow and typical for STA analysis. 
* Let's have a look into one of it. by using ```less [filename]``` or ```vim [filename]```.

<p align="center">
 <img src="https://github.com/Ren-Ps/PD_RTL2GDS_SKY130_ps/blob/main/Day4/LAB/LB8.png"> </p>

* Now our objective is that the tool should map the vsd cell during the synthesis flow. We will copy the library (from vsdstdcelldesign -> libs) files to src folder under picorv32a.

```
cp sky130_fd_sc_hd__*  /home/pssh23/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/src
```

<p align="center">
 <img src="https://github.com/Ren-Ps/PD_RTL2GDS_SKY130_ps/blob/main/Day4/LAB/LB9.png"> </p>
 
 * Then ```less config.tcl``` under the picorv32a for viewing and editing.

<p align="center">
 <img src="https://github.com/Ren-Ps/PD_RTL2GDS_SKY130_ps/blob/main/Day4/LAB/LB10.png"> </p>
 
 * Update the ```config.tcl``` as follows

<p align="center">
 <img src="https://github.com/Ren-Ps/PD_RTL2GDS_SKY130_ps/blob/main/Day4/LAB/LB11.png"> </p>
 
 * After editing the config file run the full flow from start. Open fresh terminal opened and go through commands as: 

```
1. cd work/tools/openlane_working_dir/openlane
2. docker
3.  ./flow.tcl -interactive
4. package require openlane 0.9
5. prep -design picorv32a -tag [file_name (29-01_16-28)] -overwrite
```
<p align="center">
 <img src="https://github.com/Ren-Ps/PD_RTL2GDS_SKY130_ps/blob/main/Day4/LAB/LB12.png"> </p>
 
 * Now to add additional lefs, run following after finishing above prep step.

```
set lefs [glob $::env(DESIGN_DIR)/src/*.lef]
add_lefs -src $lefs
```
<p align="center">
 <img src="https://github.com/Ren-Ps/PD_RTL2GDS_SKY130_ps/blob/main/Day4/LAB/LB13.png"> </p>
 
 * Then run synthesis
***Before it make sure delete the the old synthesis file to change the slack while changing the attributes/variables/switches****

```
run_synthesis
```

While synthesis is runnning the terminal log moving, The our designed inverter is being used.

<p align="center">
 <img src="https://github.com/Ren-Ps/PD_RTL2GDS_SKY130_ps/blob/main/Day4/LAB/LB14-1.png"> </p>
 
***Notice the number of our designed inverter is being used. As well as the Chip Area.***

<p align="center">
 <img src="https://github.com/Ren-Ps/PD_RTL2GDS_SKY130_ps/blob/main/Day4/LAB/LB15.png"> </p>

and Synthesis with modified/ add-on cell is done.

## Delay Table:

Problem:

1. The capacitance or the load at the output node of each and every buffer in the complete clock tree is varying. 

2. Also if the load is varying the input transition is varying.

To avoid large skew between endpoints of a clock tree (happening due to signal arrives at different point in time):

* After splitting the buffers. 

* Buffers on the same level must have same capacitive load to ensure same timing delay or latency on the same level. It means that each buffer at the same level is having same load.

* Buffers on the same level must also be the same size (different buffer sizes -> different W/L ratio -> different resistance -> different RC constant -> different delay). It means that the buffer at same level should be of same size. 

<p align="center">
 <img src="https://github.com/Ren-Ps/PD_RTL2GDS_SKY130_ps/blob/main/Day4/Theory/th2.png"> </p>

Solution:

Delay tables are the solution. Delay tables are 2D table. Delay of a component is characterised and summaries in a table.

The timing model of each cell is recorded and is summarised in delay tables, which are part of the liberty file. The output slew is the main cause of delay. Capacitive load and input slew are also factors that affect output slew. The input slew has its own transition delay table and is a function of the previous buffer's output cap load and input slew.

<p align="center">
 <img src="https://github.com/Ren-Ps/PD_RTL2GDS_SKY130_ps/blob/main/Day4/Theory/th3.png"> </p>
 
 Notice how skew is zero since delay for both clock path is x9'+y15.

### Fix Negative Slack:

1. Let us change some variables to minimize the negative slack. We will now change the variables "on the flight". Use echo $::env(SYNTH_STRATEGY) to view the current value of the variables before changing it:

```
% echo $::env(SYNTH_STRATEGY)
AREA 0
% set ::env(SYNTH_STRATEGY) "DELAY 0"
% echo $::env(SYNTH_BUFFERING)
1
% echo $::env(SYNTH_SIZING)
0
% set ::env(SYNTH_SIZING) 1
% echo $::env(SYNTH_DRIVING_CELL)
sky130_fd_sc_hd__inv_2
```

<p align="center">
 <img src="https://github.com/Ren-Ps/PD_RTL2GDS_SKY130_ps/blob/main/Day4/LAB/LB16.png"> </p>

With SYNTH_STRATEGY of Delay 0, the tool will focus more on optimizing/minimizing the delay, index can be 0 to 3 where 3 is the most optimized for timing (sacrificing more area). SYNTH_BUFFERING of 1 ensures cell buffer will be used on high fanout cells to reduce delay due to high capacitance load. SYNTH_SIZING of 1 will enable cell sizing where cell will be upsize or downsized as needed to meet timing. SYNTH_DRIVING_CELL is the cell used to drive the input ports and is vital for cells with a lot of fan-outs since it needs higher drive strength (larger driving cell needed).

2. Below is the log report for slack.

<p align="center">
 <img src="https://github.com/Ren-Ps/PD_RTL2GDS_SKY130_ps/blob/main/Day4/LAB/LB15-1.png"> </p>

<p align="center">
 <img src="https://github.com/Ren-Ps/PD_RTL2GDS_SKY130_ps/blob/main/Day4/LAB/LB15-2.png"> </p>

3. Next run floor plan by executing the following codes one by one:

```
init_floorplan
place_io
global_placement_or
detailed_placement
tap_decap_or
detailed_placement
gen_pdn
run_cts
```

Then check the file which is created. Go to the placements folder under reults and then invoke the magic tool and load the def file. 
<p align="center">
 <img src="https://github.com/Ren-Ps/PD_RTL2GDS_SKY130_ps/blob/main/Day4/LAB/LB17-1.png"> </p>

The command is:

```
magic -T /home/pssh23/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.placement.def
```

We can see our sky130_vsdinv file in the merged.lef file inside the tmp folder. The macro is present.

<p align="center">
 <img src="https://github.com/Ren-Ps/PD_RTL2GDS_SKY130_ps/blob/main/Day4/LAB/LB22.png"> </p>
 
We can also see the sky130_vsdinv inside the layout also:
 
<p align="center">
 <img src="https://github.com/Ren-Ps/PD_RTL2GDS_SKY130_ps/blob/main/Day4/LAB/LB23.png"> </p>
 
### Timing Analysis (Pre-Layout STA using Ideal Clocks):
 
 First we take the ideal clock (clock tree is not yet build) and do the timing analysis with it. After that we will do with real clock.

### Pre-layout timing analysis (using ideal clock) 

Pre-layout STA will not yet include effects of clock buffers and net-delay due to RC parasitics (wire delay will be derived from PDK library wire model).

* SETUP TIMING ANALYSIS.
Specifications Clock frequency = 1GHz and period of 1ns.

We have a launch flop and capture flop and in between we the the combinational logic. We have ideal clock network i.e., clock tree is not yet built. Hence we do not have any buffer in the clock path. This is a typical scenario for hold time and setup time calculation. We send the 1st riseing clock to the launch flop (t=0ns) and the 2nd rising to the capture flop (t=1ns).  

<p align="center">
 <img src="https://github.com/Ren-Ps/PD_RTL2GDS_SKY130_ps/blob/main/Day4/Theory/th4.png"> </p>

Setup timing analysis equation is:
```
Θ < T - S - SU
```
- Θ = Combinational delay which includes clk to Q delay of launch flop and internal propagation delay of all gates between launch and capture flop
- T = Time period, also called the required time
- S = Setup time. As demonstrated below, signal must settle on the middle (input of Mux 2) before clock tansists to 1 so the delay due to Mux 1 must be considered, this delay is the setup time.

<p align="center">
 <img src="https://github.com/Ren-Ps/PD_RTL2GDS_SKY130_ps/blob/main/Day4/Theory/th5.png"> </p>

- SU = Setup uncertainty due to jitter which is temporary variation of clock period. This is due to non-idealities of PLL/clock source.

### Pre-Layout STA with OpenSTA:

In cts we try to change the netlist by making clock tree.
The below files can be found in th extras folder in vsdstdcelldesign.
Making the pre_sta.conf and save it in the openlane folder.

```
set_cmd_units -time ns -capacitance pF -current mA -voltage V -resistance kOhm -distance um
read_liberty -max /home/pssh23/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/src/sky130_fd_sc_hd__slow.lib
read_liberty -min /home/pssh23/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/src/sky130_fd_sc_hd__fast.lib
read_verilog /home/pssh23/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/30-01_04-42/results/synthesis/picorv32a.synthesis.v
link_design picorv32a
read_sdc /home/pssh23/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/src/picorv32a.sdc
report_checks -path_delay min_max -fields {slew trans net cap input_pin}
```

After cts new .v files start getting created.

Creating picorv32a.sdc and save this file in the src folder of picorv32a folder.
```
set ::env(CLOCK_PORT) clk
set ::env(CLOCK_PERIOD) 12.000
set ::env(SYNTH_DRIVING_CELL) sky130_fd_sc_hd__inv_8
set ::env(SYNTH_DRIVING_CELL_PIN) Y
set ::env(SYNTH_CAP_LOAD) 17.65
create_clock [get_ports $::env(CLOCK_PORT)]  -name $::env(CLOCK_PORT)  -period $::env(CLOCK_PERIOD)
set IO_PCT  0.2
set input_delay_value [expr $::env(CLOCK_PERIOD) * $IO_PCT]
set output_delay_value [expr $::env(CLOCK_PERIOD) * $IO_PCT]
puts "\[INFO\]: Setting output delay to: $output_delay_value"
puts "\[INFO\]: Setting input delay to: $input_delay_value"


set clk_indx [lsearch [all_inputs] [get_port $::env(CLOCK_PORT)]]
#set rst_indx [lsearch [all_inputs] [get_port resetn]]
set all_inputs_wo_clk [lreplace [all_inputs] $clk_indx $clk_indx]
#set all_inputs_wo_clk_rst [lreplace $all_inputs_wo_clk $rst_indx $rst_indx]
set all_inputs_wo_clk_rst $all_inputs_wo_clk


# correct resetn
set_input_delay $input_delay_value  -clock [get_clocks $::env(CLOCK_PORT)] $all_inputs_wo_clk_rst
#set_input_delay 0.0 -clock [get_clocks $::env(CLOCK_PORT)] {resetn}
set_output_delay $output_delay_value  -clock [get_clocks $::env(CLOCK_PORT)] [all_outputs]

# TODO set this as parameter
set_driving_cell -lib_cell $::env(SYNTH_DRIVING_CELL) -pin $::env(SYNTH_DRIVING_CELL_PIN) [all_inputs]
set cap_load [expr $::env(SYNTH_CAP_LOAD) / 1000.0]
puts "\[INFO\]: Setting load to: $cap_load"
set_load  $cap_load [all_outputs]
```

This is replicating the same results as we had after run synthesis stage. pre_sta.conf will be the fill on which we will be doing our STA analysis.

To perform pre STA run the command below by opening the terminal in openlane folder which is inside the openlane_working_dir.
```
sta [file_name] // (our case = pre_sta.conf)
```

As we haven't done CTS hold time doesn't hold any significance. The delay of any cell is function of input slew and output load. We can play with these data and can get slack as positive. So we can also play with some of some of these parameters.

Command to check what a particular cell is driving:
```
report_net -connections _[cell number/net number]_

```
To replace the buffer (from buf 1 to buf 4) we use the following command.

replace_cell _23732_ sky130_fd_sc_hd__buf_4

report check will report the worst path, by default it is the max setup slack
```
report_checks -field {net cap slew input_pins} -digits 4
```
Upsizing the buffer will change the cell. We can replace cell to bring donw the slack. It will slightly increase the area,
```
1. First find the cell you want to replace. 
2. Then echo its net details by the following command 
report_net -connections _[cell number/net number]_ 
3. Then use the replace command to upsize it.
replace_cell _[net to be replaced]_ [new net name]

```
 
## Clock Tree Synthesis

There are three parameters that we need to consider when building a clock tree:

* Clock Skew = In order to have minimum skew between clock endpoints, clock tree is used. This results in equal wirelength (thus equal latency/delay) for every path of the clock.
* Clock Slew = Due to wire resistance and capacitance of the clock nets, there will be slew in signal at the clock endpoint where signal is not the same with the original input clock signal anymore. This can be solved by clock buffers. Clock buffer differs in regular cell buffers since clock buffers has equal rise and fall time.
* Crosstalk = Clock shielding prevents crosstalk to nearby nets by breaking the coupling capacitance between the victim (clock net) and aggresor (nets near the clock net), the shield might be connected to VDD or ground since those will not switch. Shileding can also be done on critical data nets.


### LAB DAY 4 (PART 4)
 
 In the terminal in which we run the run_cts command there only go to openroad. Type the following command in the terminal.
```
openroad
```

This will open the open road. Our objective to do the analysis of the entire circut where clock tree has been build now. Now we will open OpenSTA here. For timing alnalysis.

1. We first create a db `
2. db is create using lef and def file. In our analysis we use these db. (It is a one time process. Whenever lef changes we have to change the db)
3. To create a db

// first read lef (it is inside the tmp folder (merged.lef)
read_lef [location] {my case = read_lef /openLANE_flow/designs/picorv32a/runs/30-01_04-42/tmp/merged.lef}

// secondly read def (it is present inside cts folder present under the results folder/cts)
read_def [location] {my case = /openLANE_flow/designs/picorv32a/runs/30-01_04-42/results/cts/picorv32a.cts.def}

// creating db
write_db [name] // my case = pico_cts.db (created under the openlane folder)

// reading db 
read_db [name] // my case = pico_cts.db

//  reading verilog (it is present inside cts folder present under the results/synthesis/picorv32a.synthesis_cts.v)
read_verilog [location] // {my case = /openLANE_flow/designs/picorv32a/runs/30-01_04-42/results/synthesis/picorv32a.synthesis_cts.v}

// reading library (max)
read_liberty -max $::env(LIB_FASTEST)

// reading library (min)
read_liberty -min $::env(LIB_SLOWEST)

// reading sdc
read_sdc [location] {my case = /openLANE/designs/picorv32a/src/my_base.sdc}

// now the clock has been generated 
set_propagated_clock [all_clocks]

// report
report_checks -path_delay min_max -format full_clock_expanded -digits 4

All the loaction should be after /openLANE_flow/.....
```
run_cts
```
The CTS run adds clock buffers in therefore buffer delays come into picture and our analysis from here on deals with real clocks. Setup and hold time slacks may now be analysed in the post-CTS STA anlysis in OpenROAD within the openLANE flow:
```
openroad
write_db pico_cts.db
read_db pico_cts.db
read_verilog /openLANE_flow/designs/picorv32a/runs/30-01_04-42/results/synthesis/picorv32a.synthesis_cts.v
read_liberty $::env(LIB_SYNTH_COMPLETE)
link_design picorv32a
read_sdc /openLANE_flow/designs/picorv32a/src/my_base.sdc
set_propagated_clock (all_clocks)
report_checks -path_delay min_max -format full_clock_expanded -digits 4
```

## DAY 5: Final Steps for RTL2GDS using TritonRoute and OpenSTA
 
 ### Maze Routing:
 One simple routing algorithm is Maze Routing or Lee's routing:

The shortest path is one that follows a steady increment of one (1-to-9 on the example below). There might be multiple path like this but the best path that the tool will choose is one with less bends. The route should not be diagonal and must not overlap an obstruction such as macros.
This algorithm however has high run time and consume a lot of memory thus more optimized routing algorithm is preferred (but the principles stays the same where route with shortest path and less bends is preferred)
 
 ![Screenshot 2023-01-30 at 6 27 15 PM](https://github.com/Ren-Ps/PD_RTL2GDS_SKY130_ps/blob/main/Day5/Theory/th1.png)

 ### DRC Cleaning:
DRC cleaning is the next step after routing. DRC cleaning is done to ensure the routes can be fabricated and printed in silicon faithfully. Most DRC is due to the constraints of the photolitographic machine for chip fabrication where the wavelength of light used is limited. There are thousands of DRC and some DRC are:

- Minimum wire width
- Minimum wire pitch (center to center spacing)
- Minimum wire spacing (edge to edge spacing)
- Signal short = this can be solved my moving the route to next layer using vias. This results in more DRC (Via width, Via Spacing, etc.). Higher metal layer must be wider than lower metal layer and this is another DRC.
 
 ### Power Distribution Network:
 
This is just a review on PDN. The power and ground rails has a pitch of 2.72um thus the reason why the customized inverter cell has a height of 2.72 or else the power and ground rails will not be able to power up the cell. Looking at the LEF file runs/[date]/tmp/merged.nom.lef, you will notice that all cells are of height 2.72um and only width differs.

As shown below, power and ground flows from power/ground pads -> power/ground ring-> power/ground straps -> power/ground rails.
 
 ![image](https://github.com/Ren-Ps/PD_RTL2GDS_SKY130_ps/blob/main/Day5/Theory/th2.png)

 ### Routing Stage:
 
 Power Distribution Network generation
 
Unlike the general ASIC flow, Power Distribution Network generation is not a part of floorplan run in OpenLANE. PDN must be generated after CTS and post-CTS STA analysis:
 
 gen_pdn
 
 'run_routing' 
 
 A DEF file will be formed runs/[date]/results/routing/picorv32.def Open the DEF file output of routing stage in Magic.
 
 Similar to what we did when we plugged in the custom inverter cell, look for sky130_myinverter at the DEF file then search that cell instance in magic.
 
 Done!

# REFERENCES

[Kunal Ghosh - Co-founder of VSD](https://www.udemy.com/user/anagha/)
[Nickson Jose - Workshop Instructor](https://www.udemy.com/user/nickson-jose/)
[OpenLANE-Sky130-Physical-Design-Workshop](https://github.com/AngeloJacobo/OpenLANE-Sky130-Physical-Design-Workshop#floorplan-stage)
