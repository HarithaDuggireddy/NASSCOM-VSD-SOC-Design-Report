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

Here, we can see that pins are placed on the left and the right side of the die. We need a least resistance path to the clock connection.

<img width = "200" alt="Screenshot 2024-03-19 at 3 52 04 PM" src="https://github.com/HarithaDuggireddy/NASSCOM-VSD-SOC-Design-Report/assets/163351500/658c49c2-9fd5-4213-824e-fd46ff2ca464">

To block placing any other cells at pin placement we use blockages. 

<img width = "200" alt= "Screenshot 2024-03-19 at 3 52 43 PM" src="https://github.com/HarithaDuggireddy/NASSCOM-VSD-SOC-Design-Report/assets/163351500/094b0b39-568e-4349-b796-558a9df44cd7">


### LIBRARY BINDING AND PLACEMENT

**Netlist Binding and Initial place design:**
**Placement and Routing:**

1) Binding the Physical cells with the netlist
2) Usually, in the real world, every cell is rectangular or square with some height and width. Not in the shape that we see in the books.
3) From the circuit we have taken in the pin placement stage, We take all the cells as square or rectangular shapes, as shown below.

<img width = "200" alt="Screenshot 2024-03-19 at 10 10 16 PM" src="https://github.com/HarithaDuggireddy/NASSCOM-VSD-SOC-Design-Report/assets/163351500/5d30d188-59ad-4568-b151-08cd9f13bed4">

4) All cell information will be stored in the **Library**.
5) The **library** will have different-sized cells with different values. Bigger-sized cells will have less resistance and faster performance. We can pick up what we want depending on our design. 

<img width = "200" alt="Screenshot 2024-03-19 at 10 27 41 PM" src="https://github.com/HarithaDuggireddy/NASSCOM-VSD-SOC-Design-Report/assets/163351500/9179defa-b993-4bcf-a22f-e9c169441f01">

6) We have all the required cells and floorplan; now, we need to place them on the floor plan.

<img width = "200" alt="Screenshot 2024-03-19 at 10 33 02 PM" src="https://github.com/HarithaDuggireddy/NASSCOM-VSD-SOC-Design-Report/assets/163351500/69fbe55a-435c-4177-bedb-3e4ecce956fc">

7) We will do this step at the floorplan stage. We place all the blocks as per their connection with respect to the Input and Output pins.
