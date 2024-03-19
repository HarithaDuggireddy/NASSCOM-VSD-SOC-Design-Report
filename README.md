# NASSCOM-VSD-SOC-Design-Report 

SOC(System-on-Chip): Integrating all the hardware components onto the single chip.

***DIGITAL VLSI SOC DESIGN AND PLANNING***   &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;   

We have many Architectures, Here we are mainly focusing on **RISCV ARCHITECTURE** **(RISCV Based CPU Core - picorv32)**

[Sky130 Day 1 - Inception of open-source EDA, OpenLANE and Sky130 PDK](#section-1)

[Sky130 Day 2 - Good floorplan vs bad floorplan and introduction to library cells](#section-2)

Sky130 Day 3 - Design library cell using Magic Layout and ngspice characterization

Sky130 Day 4 - Pre-layout timing analysis and importance of good clock tree

Sky130 Day 5 - Final steps for RTL2GDS using tritonRoute and openSTA

<a name="section-1"></a>
## Day 1 

Getting Knowledge on opensource EDA Tools, OpenLANE, SKY130nm PDk, QFN48 Package, chip, pads, core, die, IP's, RISC-V and Softaware to HArdware Applications.
       
Let's Take an AURDUINO LEONARD Board. In that we have one processor chip. we have many interfaces around the processor they are 
JTAG-UART FTDI, QSPI1-FLASH, I2C0-EEPROM, VCC/GND, ADc(QSP3), MUXED with GPIOS, SDRAM chip, DIrect I2C, direct QSPI, GPIO0-7, PWM0-3, GPIO8-14, PWM4-5

If we see inside the processor        
 <img width="300" alt="Screenshot 2024-03-15 at 16 06 05" src="https://github.com/HarithaDuggireddy/NASSCOM-VSD-SOC-Design-Report/assets/163351500/409ee6de-9f78-4165-a0ee-9044921f61c7">
  
  The above picture shows the package named __QFN48__ (Quad Flat No-leads), this is a __48 pin__ package with __7mmx7mm area__.
  The pin locations of this package are designed as per the arduino board that need to drive
  
  The __chip__ will be some where at the center of the package, So that the I/O pins can be easily routable.
 <img width="300" alt="Screenshot 2024-03-15 at 16 23 07" src="https://github.com/HarithaDuggireddy/NASSCOM-VSD-SOC-Design-Report/assets/163351500/5ad50161-9349-4945-99c5-f604a458d189">

If we open up the chip, chip will have many components like __I/O PADS__ (to send the signal inside or outside the chip), The __Core__ (where the Logic cells have), __Die__ is surrounded by the core which is getting manufactured on the silicon wafer (also this we can say the size of the entire chip)  
  <img width="400" alt="Screenshot 2024-03-15 at 16 52 41" src="https://github.com/HarithaDuggireddy/NASSCOM-VSD-SOC-Design-Report/assets/163351500/7a47714c-716e-49de-88ce-daea2dc8bb19">

**Core AREA**  
  <img width="400" alt="Screenshot 2024-03-15 at 17 02 01" src="https://github.com/HarithaDuggireddy/NASSCOM-VSD-SOC-Design-Report/assets/163351500/319c91bc-2e6c-4934-bbee-15317f149d68">

The typical chip consists of SoC, SRAM, ADC, DAC, PLL, SPI and couple other simple programming or logical components

__Foundry IP's__:  IP's are reusable. Here IP(Intellectual Property)'s include SRAM, PLL, ADC, and DAC. Every design is almost dependent on the foundry rules. we as a SOC design Engineer or any VLSI chip desigining engineers should be continuosly comuunicate with the foundry.

__Macro's__: Macros are basically an Instructions that can vary from chip to chip. 

Now If we take __(RISCV SoC)__ we can see in the above chip inside the core area. RISCV is an Open Source Instruction Set Architecture (ISA). It uses fewer instructions than other ISA's and less complex. So, It reduces the size and the power consumption of the chip.

### RISCV (INSTRUCTION SET ARCHITECTURE)
  This nothing but a language of the computers, this is how we talk to the computers.
   
 &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; **EX:** we have C program, to run that C program on a comuter Hardware or any chip, we need to compile C-program in It's assembly language program which is nothing but the RISCV assembly language program here, 
 this will convert into machine level language program which is nothing but the binary-level language program. Finally this Binary bits( 0 and 1 ) will be executed in the layout.  

The interface between the RISCV and Computer hardware chip is a Hardware Description Language (HDL) to communicate. we need to implement perticular ISA's speciafications into RTL, and from RTL to Layout it's a standard layout flow.

       ** FLOW ** : Architecture spec -> RTL implementation -> Layout 

### From Software Applications to Hardware
To run all the applications that we are using in our day-to-day life on hardware (which basically a computer) we need **System Software** as interface between them, that converts into a Binary Language (Machine Undertandable Language).

**System Software Flow**:
   
        OS(operating System) -> Compiler -> Assembler

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**OS** Handles Memories, IO operations, Low level system functions. OS gives output as small functions as C, C++, VB, JAVA..etc., and gives to **Compiler**, 
compiler will be converting this funtions to Instructions and stores in **.exe file** (Instructions will be different for different hardware, for example: CISC, RISC, X86, ARM etc...). **ASSEMBLER** 
will take this instructions and converts to Binary Language. This Binary values are given as input to the Hardware or Computer to get an output.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Instructions acts as an Abstract Interface (ISA) between the C language (all High Level Languages) and the Hardware (Any electronic Device). Instructions are the way how a user communicates with the computer.

### SOC DESIGN AND OPENLANE
**All Components Of Open-Source ASIC Design**:  
Important things to have for implementing any Digital ASIC Design are **RTL Description, EDA Tools, and PDK Data**

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;There are many Open Sources Available for RTL Designs, EDA Tools and for PDK DATA. In that some are:

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;1)librarycores.org, opencores.org, github.com for RTL Designs

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;2)OpenLANE, OpenROAD, Qflow for EDA Tools

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;3) Google Released First PDK Source Sky130nm

PDK's are generally a "lambda" based rules to seperate the design from technology. This is like an Interface between the Process Engineers and the design Engineers to make a manufaturable Layout.

PDK stands for Process Design Kit. It include different files like Process Design Rules: (DRC, LVS, PEX , Device Models), Digital Standard Cell Libraries, I/O Libraries, Technology files, Process Design Rule Manuals, Design Kits for EDA Tools,Documentation and Guidelines

>Still the 130nm chips has >6% of sales in the present market. As this has good performance that is good for the perticular application and less cost.
> Intel: P4EE @ 3.46GHZ

**RTL to GDS FLOW**:

<img width="300" alt="Screenshot 2024-03-16 at 02 49 34" src="https://github.com/HarithaDuggireddy/NASSCOM-VSD-SOC-Design-Report/assets/163351500/ed6e87c4-2c78-4660-a7dc-dd93a4a3fff3">

&nbsp;&nbsp;&nbsp;&nbsp;**Synthesis**: Converts RTL Code into circuit out of components from the standard cell linbrary (that has regular Layouts of the gates).

&nbsp;&nbsp;&nbsp;&nbsp;**Floor and Power Planning**: 

&nbsp;&nbsp;&nbsp;&nbsp;1)Main thing here is to plan the silicon area and to create robust power distribution 
network to power the cells.

&nbsp;&nbsp;&nbsp;&nbsp;2)Partitioning the chip die between different system building blocks and place the IO PADS

&nbsp;&nbsp;&nbsp;&nbsp;<img width="300" alt="Screenshot 2024-03-16 at 03 31 19" src="https://github.com/HarithaDuggireddy/NASSCOM-VSD-SOC-Design-Report/assets/163351500/803bcea0-c7bd-445d-a134-2aebff26cdba">

&nbsp;&nbsp;&nbsp;&nbsp;3)Macro floor - Planning: We define the Macro Dimensions, Pin Locations, Rows and Routing Tracks (which will be used in the Placement &nbsp;&nbsp;&nbsp;&nbsp;and Routing Stage)

<img width="210" alt="Screenshot 2024-03-16 at 03 45 15" src="https://github.com/HarithaDuggireddy/NASSCOM-VSD-SOC-Design-Report/assets/163351500/d5ead563-32de-4330-9a34-76d9c2f7ede0">

&nbsp;&nbsp;&nbsp;&nbsp;4) In **Power Planning**, The chip is powered by Multiple VDD and GND Pins.

&nbsp;&nbsp;&nbsp;&nbsp;5) The Power Pins are connected to all components through rings, and Vertical & Horizontal Straps (such parallel structures are build to &nbsp;&nbsp;&nbsp;&nbsp;reduce the resistance, Hence the IR Drop and to address the electromigration problem).

&nbsp;&nbsp;&nbsp;&nbsp;6)Typically upper metal layers are used for the Power routing, because they are thicker and have lower resistance

&nbsp;&nbsp;&nbsp;&nbsp;<img width="348" alt="Screenshot 2024-03-16 at 04 05 24" src="https://github.com/HarithaDuggireddy/NASSCOM-VSD-SOC-Design-Report/assets/163351500/5f09377a-0e12-42a1-8cf1-9af5167fa7ae">

&nbsp;&nbsp;&nbsp;&nbsp;**Placement**:

&nbsp;&nbsp;&nbsp;&nbsp;1) Determining the locations of the cells. It is an important stage in the design flow, because it affects routabil- ity, performance, heat &nbsp;&nbsp;&nbsp;&nbsp;distribution, and to a less extent, power consumption of a design.

&nbsp;&nbsp;&nbsp;&nbsp;2) Typically Placement is done in two steps: **Global** and followed by **Detailed** Placement

&nbsp;&nbsp;&nbsp;&nbsp;3)Global Placement try's to find the optimal positions of the cells, But those positions are not final and some may have overlaps. 

&nbsp;&nbsp;&nbsp;&nbsp;<img width="180" alt="Screenshot 2024-03-16 at 14 34 04" src="https://github.com/HarithaDuggireddy/NASSCOM-VSD-SOC-Design-Report/assets/163351500/8819b1b6-69bf-466d-9707-fe2a50bcdabb">


&nbsp;&nbsp;&nbsp;&nbsp;4)Detail Placement will alter those placements to be perfect without overlaps and easily routable.

&nbsp;&nbsp;&nbsp;&nbsp;<img width="180" alt="Screenshot 2024-03-16 at 14 34 54" src="https://github.com/HarithaDuggireddy/NASSCOM-VSD-SOC-Design-Report/assets/163351500/c16035e7-3cba-4d73-9b17-bd735e9b8a2c">

&nbsp;&nbsp;&nbsp;&nbsp;**Clock Tree Synthesis**:

&nbsp;&nbsp;&nbsp;&nbsp;1) Before Routing the signals, we need to route the clock.

&nbsp;&nbsp;&nbsp;&nbsp;2) We create a Clock districution network to connect clock to the all sequential elements with minimum skew and minimum Latency.

&nbsp;&nbsp;&nbsp;&nbsp;3) Some clock Network algorithms are H-Tree, X-Tree, Method of Mean and Median, Geometric Matching Algorithms 

&nbsp;&nbsp;&nbsp;&nbsp;4) The below Image shows the H-Tree shaped Clock Network

&nbsp;&nbsp;&nbsp;&nbsp;<img width="180" alt="Screenshot 2024-03-16 at 14 39 07" src="https://github.com/HarithaDuggireddy/NASSCOM-VSD-SOC-Design-Report/assets/163351500/ac0bd281-9dfe-4825-8044-eb13256432e3">

&nbsp;&nbsp;&nbsp;&nbsp;**Signal Routing**:

&nbsp;&nbsp;&nbsp;&nbsp;1) Implementing the Connctions using available metal layers.

&nbsp;&nbsp;&nbsp;&nbsp;2) For each metal layer the PDK defines the thickness, pitch, Tracks and the minimum width, and the via's and contact sizes.

&nbsp;&nbsp;&nbsp;&nbsp;<img width="329" alt="Screenshot 2024-03-16 at 15 07 30" src="https://github.com/HarithaDuggireddy/NASSCOM-VSD-SOC-Design-Report/assets/163351500/be7783dc-4de0-4474-a677-c158e270e0df">

>The Sky 130nm has total 6 layers of metals.

&nbsp;&nbsp;&nbsp;&nbsp;3) The Lowest Layer is used for Local Interconnection (Titanium Nitrade Layer), Only used for short-distance conncetions.

&nbsp;&nbsp;&nbsp;&nbsp;4) All other Layers are aluminium layers.

&nbsp;&nbsp;&nbsp;&nbsp;<img width="300" alt="Screenshot 2024-03-16 at 15 06 03" src="https://github.com/HarithaDuggireddy/NASSCOM-VSD-SOC-Design-Report/assets/163351500/4bb34438-7a7e-4f08-8ad8-9abbd9ed50f0">

&nbsp;&nbsp;&nbsp;&nbsp;5) Global Routing generated the Routing Grids, and Detailed Routing uses the routing grids to implement the actual wiring.

After completing the Routing we can contnue with the signoff process by verifying the layout. Two types of verification to caryy out are Physical and Timing Verification. Physical Verification involves DRC (Design Rule Checks), LVS(Layout Vs, Schematic), and Timing verfication Involves STA (Static Timing Analysis - Set-Up and Hold Checks) 

<img width="150" alt="Screenshot 2024-03-16 at 15 28 20" src="https://github.com/HarithaDuggireddy/NASSCOM-VSD-SOC-Design-Report/assets/163351500/2547ae0f-62c0-4700-bcfb-ab06d9cc45d7">

1) openLANE is a Tool used to perform entire RTL to GDSII flow (Openlane is Integrated with Many open sources like OPENROAD, yosis, Qflow, MAGIC VLSI Layout Tool, Fault, ABC, K_Layout)

2) Tuned for Sky 130nm PDK but also supports XFAB180 and GF130G

3) Can be used for both Macros and Chip Layouts

4) Two Modes of operation: Autonomous or Iterative

5) we can also use openLANE for Regression Testing

6) Physical Implementation (Automatic Place and Route is carried out using OPENROAD App)

7) openLANE ASIC flow is shown in below image with the open sources involve in each step to perform the flow.

<img width="300" alt="Screenshot 2024-03-16 at 15 40 47" src="https://github.com/HarithaDuggireddy/NASSCOM-VSD-SOC-Design-Report/assets/163351500/093ed014-6759-46b1-bba4-4a86c5aa2cb1">

**Design For Test** :

This is an optional step to perform after the Synthesis. This involves Scan Insertion, Automatic Test Pattern Generation (ATPG), Test Patterns Compaction, Fault Coverage and Fault Simulation.

**Antenna Rule Violations**:

During the Long Metal Routes, There is chance of reactive ion etching, that causes charge to accumulate on the wire. Due to this generated charges chances of breaking the transistor gates are high. To avoid this we have two solution.

 &nbsp;&nbsp;&nbsp;&nbsp;1) **Bridging**: This attaches higher metal layers in between (Requires Router Awareness)

&nbsp;&nbsp;&nbsp;&nbsp;<img width="383" alt="Screenshot 2024-03-16 at 16 28 39" src="https://github.com/HarithaDuggireddy/NASSCOM-VSD-SOC-Design-Report/assets/163351500/a6b7ba44-99b4-40f4-8b04-25cf8c789d6b">

 &nbsp;&nbsp;&nbsp;&nbsp;2) **Antenna Diodes**: Adding Antenna diodes can leak away the accumulated charges (Antenna Diodes are provided by the standard Cell &nbsp;&nbsp;&nbsp;&nbsp; Library file)

&nbsp;&nbsp;&nbsp;&nbsp;<img width="144" alt="Screenshot 2024-03-16 at 16 29 11" src="https://github.com/HarithaDuggireddy/NASSCOM-VSD-SOC-Design-Report/assets/163351500/f473f3b2-4bae-4617-9009-d504c3dcfa16">

&nbsp;&nbsp;&nbsp;&nbsp;3) Preventive approach to add antenna diodes:  Add a fake antenna diode next to every cell input after placement, Run the antenna &nbsp;&nbsp;&nbsp;&nbsp; checker on the routed layout, If that reports a violation on the cell input pin, replace fake diode by real diode. 

&nbsp;&nbsp;&nbsp;&nbsp;<img width="229" alt="Screenshot 2024-03-16 at 16 47 02" src="https://github.com/HarithaDuggireddy/NASSCOM-VSD-SOC-Design-Report/assets/163351500/17be0b61-95e4-42e0-a5a6-98780f6d2917">

>RC EXTRACTION: DEF to SPEF


## GETTING INTO OPEN SOURCE EDA TOOL (UBUNTU VIRTUAL MACHINE)**:
libs.tech is specific to the tool
libs.ref is specific to the technology



<a name="section-2"></a>
## DAY2

### CHIP FLOORPLAN AND CONSIDERATIONS

**Utilization factor and aspect ratio:**

1) The first step of the PD Flow is to define the width and height of the Core and Die.
&nbsp;&nbsp;&nbsp;&nbsp;<img width="200" alt="Screenshot 2024-03-16 at 23 52 07" src="https://github.com/HarithaDuggireddy/NASSCOM-VSD-SOC-Design-Report/assets/163351500/ea4138a1-daea-49bf-a8c6-d49f43135409">

2) Here, we need to find the rough dimensions of the Cells. Here, we start with basic netilist (Defines the connectivity of an electronic design). This will take cells from the synthesized netlist to find the rough dimensions of the cell.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<img width="200" alt="Screenshot 2024-03-16 at 23 59 19" src="https://github.com/HarithaDuggireddy/NASSCOM-VSD-SOC-Design-Report/assets/163351500/d14c7241-8959-48b3-b8a2-e94a69bd2be6">
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<img width="200" alt="Screenshot 2024-03-17 at 00 01 41" src="https://github.com/HarithaDuggireddy/NASSCOM-VSD-SOC-Design-Report/assets/163351500/f0bd4a99-de3e-4fee-906f-43e946544799">
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<img width="200" alt="Screenshot 2024-03-17 at 00 03 24" src="https://github.com/HarithaDuggireddy/NASSCOM-VSD-SOC-Design-Report/assets/163351500/87fef589-6ddc-4585-9890-4d177a3acfb2">

3) **Utilization Factor:** Ratio of the Area Occupied by the Netlist to the Total Area of the Core.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<img width="200" alt="Screenshot 2024-03-17 at 00 20 23" src="https://github.com/HarithaDuggireddy/NASSCOM-VSD-SOC-Design-Report/assets/163351500/009c1d95-70e7-499d-a965-6d9358a30961">
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<img width="200" alt="Screenshot 2024-03-17 at 00 26 03" src="https://github.com/HarithaDuggireddy/NASSCOM-VSD-SOC-Design-Report/assets/163351500/ba08fed5-6bfe-48b2-935c-4141ef21062c">
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<img width="200" alt="Screenshot 2024-03-17 at 00 27 46" src="https://github.com/HarithaDuggireddy/NASSCOM-VSD-SOC-Design-Report/assets/163351500/3ccb121f-5a6a-4651-9a01-92520b7c7502">

4) If we place cells inside the core area by utilizing 100% space as shown in the above picture, then It is not possible to route the signals and add any additional cells.
5) If the Utilization factor = 1, It means we have utilized the 100% area of the core and no extra space.
6) So we should have a utilization factor <1. So, that we can place additional logic if needed and for routing. 
7) **Aspect Ratio:** Ratio of the Height of the Core or Die to the Width of the Core or Die.
8) So, whenever the aspect ratio is 1. It means the chip is in a square shape. If it is any number other than 1, It means the chip is in a rectangular shape.

**Pre-placed Cells:**

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<img width="200" alt="Screenshot 2024-03-17 at 00 51 29" src="https://github.com/HarithaDuggireddy/NASSCOM-VSD-SOC-Design-Report/assets/163351500/8c96c89a-9423-4506-b4f2-609781d274c8">
&nbsp;&nbsp;&nbsp;&nbsp;<img width="200" alt="Screenshot 2024-03-17 at 00 53 44" src="https://github.com/HarithaDuggireddy/NASSCOM-VSD-SOC-Design-Report/assets/163351500/838346e2-557c-48be-a3de-4e3ab7007205">
&nbsp;&nbsp;&nbsp;&nbsp;<img width="200" alt="Screenshot 2024-03-17 at 00 54 46" src="https://github.com/HarithaDuggireddy/NASSCOM-VSD-SOC-Design-Report/assets/163351500/31c755dd-35ce-4c86-9eff-770e45118d33">

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<img width="200" alt="image" src="https://github.com/HarithaDuggireddy/NASSCOM-VSD-SOC-Design-Report/assets/163351500/4209883c-e9b2-4c34-a776-f7e226051934">
&nbsp;&nbsp;&nbsp;&nbsp;<img width="200" alt="Screenshot 2024-03-17 at 01 02 26" src="https://github.com/HarithaDuggireddy/NASSCOM-VSD-SOC-Design-Report/assets/163351500/52a0a396-8316-44cb-918b-ae50af7cc304">

The user will define the locations of some of the cells before starting the APR. The pre-placed cell locations should be defined perfectly, as after that, we can't move them in the whole design cycle. Mostly, these particular blocks are communicating with the input pins. So, we place them close to the input pins side.

**Decoupling Capacitors:**

&nbsp;&nbsp;&nbsp;<img width="200" alt="image" src="https://github.com/HarithaDuggireddy/NASSCOM-VSD-SOC-Design-Report/assets/163351500/5841985f-13da-44f5-9ab0-2d64c061faa0">
&nbsp;&nbsp;&nbsp;<img width="200" alt="Screenshot 2024-03-17 at 01 56 21" src="https://github.com/HarithaDuggireddy/NASSCOM-VSD-SOC-Design-Report/assets/163351500/67807aee-0717-4f06-8a95-793b30961382">
&nbsp;&nbsp;&nbsp;&nbsp;<img width="200" alt="Screenshot 2024-03-17 at 01 56 43" src="https://github.com/HarithaDuggireddy/NASSCOM-VSD-SOC-Design-Report/assets/163351500/978e8090-b76b-405c-974c-e26320616aaa">

&nbsp;&nbsp;&nbsp;&nbsp;<img width="200" alt="Screenshot 2024-03-17 at 02 29 23" src="https://github.com/HarithaDuggireddy/NASSCOM-VSD-SOC-Design-Report/assets/163351500/2d72c44c-e722-44ac-98f2-dec0a8761686">

1) By using de-coupling capacitors, we can reduce the problem of IR Drop (Voltage Drop). So, whenever there is insufficient power to the cells, decoupling capacitors can send the required amount of power immediately with the stored charge.
2) The name itself says that this is de-coupled from the main power supply. So, There won't be any switching activity missed, and there won't be cross-talk issues.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<img width="200" alt="Screenshot 2024-03-17 at 02 33 45" src="https://github.com/HarithaDuggireddy/NASSCOM-VSD-SOC-Design-Report/assets/163351500/d2632ff8-2655-4c2f-966a-97c5a5d8a8a3">

**Power Planning:**

1) The main objective of Power planning is to ensure reliable and efficient power delivery to all the components.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<img width="200" alt="Screenshot 2024-03-18 at 5 49 08 PM" src="https://github.com/HarithaDuggireddy/NASSCOM-VSD-SOC-Design-Report/assets/163351500/34410c7b-a2f1-47da-8702-5ef53a6d8b88"> Assume the wire connected between the macros in red color is a 16-bit bus. When the driver is sending a signal from logic 1 to 0. The complete signal should remain as same until it reaches the load. We can't connect decoupling capacitors (connected only for some critical blocks) for all the blocks. We will create a Power Distribution Network all over the chip to maintain the signal strength.

As we see below, there is an inverter at the load side. So, as we pass this particular 16-bit logic to the inverter, all the capacitors charged to 1 will discharge to 0, and all the 0's to 1. 

<img width = "200" alt="Screenshot 2024-03-18 at 6 09 41 PM" src="https://github.com/HarithaDuggireddy/NASSCOM-VSD-SOC-Design-Report/assets/163351500/2aafd122-0b4f-4875-99fc-824fcf8ed995">


We see  when all the charged capacitances are getting discharged at a time, and we connect only a single ground line to all the capacitors. We see the capacitors, which are supposed to be logic 0, there is a bump formed due to insufficient power to all the lines at the same time. This we call **Ground Bounce**. If the size of the bump exceeds the Noise margin level, it may enter into an undefined state.

<img width = "200" alt="Screenshot 2024-03-18 at 6 11 40 PM" src="https://github.com/HarithaDuggireddy/NASSCOM-VSD-SOC-Design-Report/assets/163351500/702538a0-61e0-42ae-b371-cee36487991b">

In the same way, when all capacitances are getting charged at the same time that is connected to the single power supply. There is a chance of **voltage droop** (lowering of voltage) due to insufficient power supply to all the capacitors at the same time. As long as that value inside the noise margin level is not an issue, if it exceeds the NM level, outputs will be unpredictable.

<img width = "200" alt="Screenshot 2024-03-18 at 6 12 11 P" src="https://github.com/HarithaDuggireddy/NASSCOM-VSD-SOC-Design-Report/assets/163351500/c436ccd6-c293-46f0-9885-33509f71cf27">

Building a good power distribution network can solve the **Voltage droop** and **ground Bounce** issues. If we see the below image, the power supply is coming from one source point. 

<img width = "200" alt="Screenshot 2024-03-18 at 6 13 00 PM" src="https://github.com/HarithaDuggireddy/NASSCOM-VSD-SOC-Design-Report/assets/163351500/9471fcc9-b9a0-49d4-98c2-f2cfc22b6f72">

Here, we can see that we have multiple power and gnd supplies. So that each time when we need power and gnd supply we can take from the nearest source. 

<img width = "200" alt="Screenshot 2024-03-18 at 6 13 19 PM" src="https://github.com/HarithaDuggireddy/NASSCOM-VSD-SOC-Design-Report/assets/163351500/dc8680eb-9ae3-4f16-a4d0-6afdd0f136aa">

The picture below shows how the Power Mesh looks like. 

<img width = "200" alt="Screenshot 2024-03-18 at 6 14 03 PM" src="https://github.com/HarithaDuggireddy/NASSCOM-VSD-SOC-Design-Report/assets/163351500/baffcc7f-8880-4b8e-9653-84d83014a9a4">

&nbsp;
&nbsp;
**Pin Placement and Logical Cell placement Blockage:**

Picture A : <img width = "200" alt="Screenshot 2024-03-19 at 3 49 05 PM" src="https://github.com/HarithaDuggireddy/NASSCOM-VSD-SOC-Design-Report/assets/163351500/8f97358a-9d7f-4c86-8ecd-af8c1f235a02">

Picture B : <img width = "200" alt="Screenshot 2024-03-19 at 3 50 57 PM" src="https://github.com/HarithaDuggireddy/NASSCOM-VSD-SOC-Design-Report/assets/163351500/ed5522cd-153d-4a9a-8b28-7378725ed66e">

This is the complete circuit by combining A and B pictures:
<img width = "200" alt="Screenshot 2024-03-19 at 3 51 18 PM" src="https://github.com/HarithaDuggireddy/NASSCOM-VSD-SOC-Design-Report/assets/163351500/eddc93d5-1e16-417a-bc13-3f864de8aec9">

From the above picture, we can see that there are only two clocks, Clk1 and Clk2, that are going to all flip-flops. So we can connect Clk1 and Clk2 as shown below. Now we can see here we have Din1,2,3,4 and Clk1,2 at the input side and Dout1,2,3,4 and Clkout at the output side. We can Place Input and output pins at the top and bottom (or) at the left and right (or) at the corners inside the Die as per the design. But we need to make sure the blocks that are connected to input pins are close to the input pin side and the same for the output. So that the routing will be easier. The **netlist**, which is in the Verilog/VHDL language, will have all the connection-related information. 

<img width = "200" alt="Screenshot 2024-03-19 at 3 51 41 PM" src="https://github.com/HarithaDuggireddy/NASSCOM-VSD-SOC-Design-Report/assets/163351500/e748010a-9ebf-466f-85c4-5085d8c6398b"> .<br>

Here, we can see that pins are placed on the left and the right side of the die.

<img width = "200" alt="Screenshot 2024-03-19 at 3 52 04 PM" src="https://github.com/HarithaDuggireddy/NASSCOM-VSD-SOC-Design-Report/assets/163351500/658c49c2-9fd5-4213-824e-fd46ff2ca464">

To block placing any other cells at pin placement we use blockages. 

<img width = "200" alt= "Screenshot 2024-03-19 at 3 52 43 PM" src="https://github.com/HarithaDuggireddy/NASSCOM-VSD-SOC-Design-Report/assets/163351500/094b0b39-568e-4349-b796-558a9df44cd7">

























