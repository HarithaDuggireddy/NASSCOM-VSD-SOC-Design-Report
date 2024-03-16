# NASSCOM-VSD-SOC-Design-Report 

SOC(System-on-Chip) : Integrating all the hardware components on to the single chip.

***DIGITAL VLSI SOC DESIGN AND PLANNING***   &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;   

We have many Architectures, Here we are mainly focusing on **RISCV ARCHITECTURE** **(RISCV Based CPU Core - picorv32)**

[Sky130 Day 1 - Inception of open-source EDA, OpenLANE and Sky130 PDK](#section-1)

Sky130 Day 2 - Good floorplan vs bad floorplan and introduction to library cells

Sky130 Day 3 - Design library cell using Magic Layout and ngspice characterization

Sky130 Day 4 - Pre-layout timing analysis and importance of good clock tree

Sky130 Day 5 - Final steps for RTL2GDS using tritonRoute and openSTA

<a name="section-1"></a>
## Day 1 

Getting Knowledge on opensource EDA Tools, OpenLANE, SKY130nm PDk, QFN48 Package, chip, pads, core, die, IP's, RISC-V and Softaware to HArdware Applications.
       
Let's Take an AURDUINO LEONARD Board. In that we have one processor chip. we have many interfaces around the processor they are 
JTAG-UART FTDI, QSPI1-FLASH, I2C0-EEPROM, VCC/GND, ADc(QSP3), MUXED with GPIOS, SDRAM chip, DIrect I2C, direct QSPI, GPIO0-7, PWM0-3, GPIO8-14, PWM4-5

If we go inside the processor        
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









