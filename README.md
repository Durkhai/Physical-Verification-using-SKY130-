# Physical Verification using SKY130

![Workshop_Image](https://user-images.githubusercontent.com/102425944/195231772-acf036f3-4dea-4d8e-b267-666d2dc2c023.jpeg)


This repository documents the work done during the 5 day [workshop](https://www.vlsisystemdesign.com/physical-verification-using-sky130/) conducted by VSD_IAT from 10th to 14th October 2022. All the work done is separeted by the respective days. Day 4 is not documented, since it consisted of a demonstration of Openlane.

## Table of contents

* Day 1 - Introduction to SkyWater PDKs and open-source EDA tools
  + Creating an inverter schematic in Xschem
  + Creating a symbol from schematic
  + Importing schematic to layout in Magic
  + Final DRC/LVS checks and post layout simulations





## Day 1 - Introduction to SkyWater PDKs and open-source EDA tools

The lab exercises of the first day gave an introduction into the design flow of an analog design using open-source tools. In this case, a simple inverter circuit was developed and tested for errors.

### Creating an inverter schematic in Xschem

The flow starts with making the schematic, which can be seen below.

![image](https://user-images.githubusercontent.com/102425944/195458809-040f7b31-dd1f-4466-bd87-3b9267d28370.png)

The inverter is composed of a 1.8V pfet3 and a 1.8V nfet. Like observable above, the pfet3's bulk terminal is not showing in the schematic. That is because the "3" variants' bulk connection is defined in its parameters rather than by wire. It's usually good practice to use non "3" variants of these components.

### Creating a symbol from schematic

To properly test the schematic, a testbench was made. First, a symbol was created from the previous schematic. After that, voltage sources were added and connected ending up with the following circuit.

![image](https://user-images.githubusercontent.com/102425944/195461906-6909d1b0-810f-4a37-8758-783edfc7acf3.png)

After simulating the circuit and plotting the results, ngspice outputed a graph with the input and output voltage curves.

![image](https://user-images.githubusercontent.com/102425944/195463723-1f5650a4-5117-4c54-993e-6daf9b94b601.png)

From it we can see that the output voltage stays at 1.8V when the input is below acertain threshold (+-0.6V) and is 0V when the input voltage is above +-0.8V. This is the behavior expected from an inverter.


