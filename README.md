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

The inverter is composed of a 1.8V pfet3 and a 1.8V nfet. Like observable above, the pfet's bulk terminal is not showing in the schematic. That is because its connection is defined in its parameters rather than by wire. 
