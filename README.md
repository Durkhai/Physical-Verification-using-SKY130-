# Physical Verification using SKY130

![Workshop_Image](https://user-images.githubusercontent.com/102425944/195231772-acf036f3-4dea-4d8e-b267-666d2dc2c023.jpeg)


This repository documents the work done during the 5 day [workshop](https://www.vlsisystemdesign.com/physical-verification-using-sky130/) conducted by VSD_IAT from 10th to 14th October 2022. All the work done is separeted by the respective days. Day 4 is not documented, since it consisted of a demonstration of Openlane.

## Table of contents

* Day 1 - Introduction to SkyWater PDKs and open-source EDA tools
  + Creating an inverter schematic in Xschem
  + Creating a symbol from schematic
  + Importing schematic to layout in Magic
  + Final DRC/LVS checks and post layout simulations
* Day 2 - DRC/LVS Introduction
  + GDS read
  + Ports
  + Abstract view
  + Basic Extraction
  + Setup for DRC
  + Setup for LVS
  + Setup for XOR




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

### Importing schematic to layout in Magic

The netlist was then imported into magic and the devices were routed to form a layout.

![image](https://user-images.githubusercontent.com/102425944/195642208-040b8b87-4689-41b2-87eb-398862f335f4.png)

### Final DRC/LVS checks and post layout simulations

Magic tells us that there are no DRC violations, but to check for LVS ones we need to use Netgen. After running it with the original netlist against the layout netlist we get the following output.

![image](https://user-images.githubusercontent.com/102425944/195643849-d971da23-e33c-4ac6-9850-a94447646a92.png)

This tells us that there was a problem with creating the nets on the layout.


## Day 2 - DRC/LVS Introduction

### GDS read

Firstly, the GDS files for the skywater library cells were read into magic with the istyle sky130(). The cell and2_1 was loaded.

![image](https://user-images.githubusercontent.com/102425944/195670235-d991cc6d-d053-4d67-b5a4-607f8d748812.png)

Afterwards, the istyle was changed to sky130(vendor) and the files were read again.

![image](https://user-images.githubusercontent.com/102425944/195670471-26df79aa-7275-4e1a-92e8-5a0961d46481.png)

This time, the cells that were read as text are now identified as pins (tags went from yellow to dark blue).

### Ports

Only GDS file was read into magic:

![image](https://user-images.githubusercontent.com/102425944/195672399-28fdad1f-aded-4b75-9c43-d865805d46d4.png)

After LEF file was read:

![image](https://user-images.githubusercontent.com/102425944/195672856-58b73820-a6c8-4ca4-bc37-91eadac1dbf4.png)

After spice netlist was read:

![image](https://user-images.githubusercontent.com/102425944/195673358-920e893b-4e72-4fb0-8cfd-7d554e571a82.png)


### Abstract view

This time, we read just the LEF files and loaded the same and2_1 cell.

![image](https://user-images.githubusercontent.com/102425944/195671189-ffa868db-22ba-425a-8e52-dc78f026a0b0.png)

As expected, the port order is incorrect since this metadata is not found in the LEF files.

![image](https://user-images.githubusercontent.com/102425944/195674291-67c7bb4a-0154-4674-9036-428e556f7f5f.png)

However, after loading the netlist this information is added.

![image](https://user-images.githubusercontent.com/102425944/195674616-3eb29b00-ca2b-4a79-8204-985e43ab5cff.png)

### Basic Extraction

Basic extraction:

![image](https://user-images.githubusercontent.com/102425944/195676474-484a0a31-9fc1-4dcd-a3c2-37ebb50b077e.png)

Extraction with capacitances:

![image](https://user-images.githubusercontent.com/102425944/195677109-d85fc409-b3f4-4bc7-a97c-bd577c8db78b.png)

Extraction with capacitances and resistances:

![image](https://user-images.githubusercontent.com/102425944/195678227-8e1231a0-8631-47f3-b357-b9c8edbf0169.png)

### Setup for DRC

Style was changed to drc(full), the DRC check was run on and2_1 cell.

![image](https://user-images.githubusercontent.com/102425944/195679854-cd4ca096-50f3-4339-806d-69eab05a87cb.png)

After adding and properly aligning a tap cell, the DRC errors go away.

![image](https://user-images.githubusercontent.com/102425944/195680747-d50a6472-d1d6-4135-a2c1-b2841ee2242b.png)

However, if we descend into the and2_1 cell, we see that the errors in that context are still there, they just dont exist in the top level layout.

![image](https://user-images.githubusercontent.com/102425944/195680938-ac166529-c552-474d-b04b-d269ba7a0800.png)

### Setup for LVS

Results of LVS from PDK netlist and magic outputed netlist of the and2_1 cell.

![image](https://user-images.githubusercontent.com/102425944/195683923-7aea3763-a617-4a0d-8820-f745d53b26f5.png)

### Setup for XOR

The and2_1 cell was copied into another layout named "altered". Then, a piece of the locxal interconnect was removed.

![image](https://user-images.githubusercontent.com/102425944/195686084-95523123-1b41-4ae3-a994-2d84e5a94136.png)

After running a XOR test between these 2 cells we get the differences in geometry between them.

![image](https://user-images.githubusercontent.com/102425944/195686661-de479838-21ae-4d1f-b951-a7cf27ade3df.png)

Results of the XOR test after shifting a cell:

![image](https://user-images.githubusercontent.com/102425944/195687756-384f152e-fe41-4231-8abc-fbb6f2872afe.png)

## Day 3 - Design Rule Checking

To get started, the lab files were obtained from a github repository.

![image](https://user-images.githubusercontent.com/102425944/195848639-eb7b94b3-cd5e-44d7-ae97-6575715d765f.png)

### Width rule and spacing rule

Magic can tell us what a particular DRC error is by selecting it with the box and doing a DRC report. In this case, the width of the metal was smaller than the minnimum.

![image](https://user-images.githubusercontent.com/102425944/195849407-9cf05123-939a-44c7-803d-34c5e5f685ac.png)

We can paint the layout using the graphic enviornment or the console.

![image](https://user-images.githubusercontent.com/102425944/195850536-4edb1577-092c-4179-af32-617803b875af.png)

### Wide spacing and notch rule

![image](https://user-images.githubusercontent.com/102425944/195852696-ec8434ff-1fe0-4ef2-a9cb-fad812c4564f.png)

![image](https://user-images.githubusercontent.com/102425944/195852754-c6ec4f0e-c0c5-42bd-864c-7747503aacb0.png)

### Vias

![image](https://user-images.githubusercontent.com/102425944/195854416-45448148-63e6-4a3c-920c-7fbcef0791c3.png)

![image](https://user-images.githubusercontent.com/102425944/195854775-fc88ec9a-2e39-4601-8c36-7d9e8ad4fbfd.png)

![image](https://user-images.githubusercontent.com/102425944/195855090-e59b8142-b71b-4929-a7ef-d32e3eefb23c.png)

![image](https://user-images.githubusercontent.com/102425944/195857152-01d804eb-65d8-4efd-bc76-756546aeb8ec.png)

![image](https://user-images.githubusercontent.com/102425944/195858050-48c3e9d8-59c2-496c-99d9-b6666e62ae33.png)

### Minimum area and minimum hole rules

![image](https://user-images.githubusercontent.com/102425944/195859600-89bbd38e-a4fd-4e0e-bbc5-98ba8e44015f.png)

![image](https://user-images.githubusercontent.com/102425944/195859986-3a7b836d-0f05-46f4-a956-659a6d0b5042.png)

### Wells and deep n-well

![image](https://user-images.githubusercontent.com/102425944/195867683-b3b1a26e-9026-46ac-9ba2-03fad32591a9.png)

![image](https://user-images.githubusercontent.com/102425944/195870715-c5dbb3c2-56de-44c1-9185-8ccc97940888.png)

![image](https://user-images.githubusercontent.com/102425944/195951632-907df90f-1877-49a9-9e16-7c82747e3126.png)

### Derived layers

![image](https://user-images.githubusercontent.com/102425944/195952375-8a2a8405-b273-4e2c-9e9a-e2fb588b39a4.png)

![image](https://user-images.githubusercontent.com/102425944/195952865-5e7c6fd3-e135-44d4-993e-c209df8d37ed.png)

![image](https://user-images.githubusercontent.com/102425944/195953017-a23584a4-c3ef-4ba9-b110-df274f9c47b4.png)

### Parameterized and PDK devices

![image](https://user-images.githubusercontent.com/102425944/195953369-4d0e2a56-e04e-4170-9111-f0a7e2cdfcc0.png)

![image](https://user-images.githubusercontent.com/102425944/195955502-09e10ad6-74f5-4f15-9f3d-a872e3191be5.png)

### Angle error and overlap rule

![image](https://user-images.githubusercontent.com/102425944/195956147-99646d00-658b-4c5e-9e78-e70a998d51dc.png)

![image](https://user-images.githubusercontent.com/102425944/195956531-6d01d05f-22a4-477f-9836-91aa80f1edda.png)

![image](https://user-images.githubusercontent.com/102425944/195956553-88eef484-9400-4b93-ab42-fa1deca5fd36.png)

![image](https://user-images.githubusercontent.com/102425944/195956938-494649aa-8471-4181-b409-db8be789601a.png)

### Latch up and antenna rules

![image](https://user-images.githubusercontent.com/102425944/195958439-1634eb6d-1706-4818-86c8-4e9235a44d93.png)

![image](https://user-images.githubusercontent.com/102425944/195958693-cee074ab-3e4a-4bf7-894c-1a80450d2609.png)

![image](https://user-images.githubusercontent.com/102425944/195958815-df5a40dd-f195-4008-ac1b-cb0abf0fec1e.png)

### Density rules

![image](https://user-images.githubusercontent.com/102425944/195959281-9c1cbe7a-ea43-44f4-b7e4-990cfa0ae312.png)

![image](https://user-images.githubusercontent.com/102425944/195959418-b33dbbc2-4adf-474c-ac69-3677d0bc9629.png)

![image](https://user-images.githubusercontent.com/102425944/195959521-9104c413-4ad2-4603-a52e-f3376586a983.png)



