# Tiny-Thinker
Barebones STM32 board I made to learn PCB design. I followed [this tutorial!](https://www.youtube.com/watch?v=aVUqaB0IMh4)

## Pictures

### Schematic
![Schematic](./docs/Schematic.png)

### Routing
![Routing](./docs/Routing.png)

### CAD
![CAD](./docs/CAD.png)

## Notes

- [BOM and CPL Reference](https://jlcpcb.com/help/article/How-to-generate-the-BOM-and-Centroid-file-from-KiCAD
)

- [Adding JLCPCB to KiCAD](https://www.youtube.com/watch?v=Bf6XzcvVBs4)

Create a folder called `manufacturing` to put the BOM, CPL, and Gerbers in

- [JLCPCB Parts](https://jlcpcb.com/parts)

### STM32 Application Notes
- [AN4879 Using USB](https://www.st.com/resource/en/application_note/an4879-introduction-to-usb-hardware-and-pcb-guidelines-using-stm32-mcus-stmicroelectronics.pdf)
- [STM32F446RET6 Datasheet](https://www.st.com/resource/en/datasheet/stm32f446mc.pdf)
> **NOTE** Make sure to properly calculate VCAP_1 and things like that!

### BOMs
When using JLCPCB, use this order:
>**NOTE** You could use DNP, or you could just delete all entries you don't want assembled

| Comment | Designator | Footprint | JLCPCB Part # | Qty | DNP |
| :---: | :---: | :---: | :---: | :---: | :---: |
| 100nF | C1 | Capacitor_SMD:C_0402_1005Metric | C1525 | 5 | DNP | 

KiCAD is going to have this format:
>**NOTE** You can change the order of the columns, just not the names of the default ones

>**NOTE** Change `Value` to `Comment` and `Reference` to `Designator`

| Value (`Comment`) | Reference (`Designator`) | Footprint | JLCPCB Part # | Qty | DNP |
| :---: | :---: | :---: | :---: | :---: | :---: |
| 100nF | C1 | Capacitor_SMD:C_0402_1005Metric | C1525 | 5 | `X`

### CPL / Pick&Place Files
To start exporting the CPL files, open the `PCB Editor` and click `File` then `Fabrication Output` then `Component Placements (.pos, .gbr)...`

```
|-- PCB Editor
|   |-- File
|   |   |-- Fabrication Output
|   |   |   |-- Component Placements (.pos, .gbr)...
```

Make sure it:

- Format: `CSV`
- Units: `Millimeters`
- Check `Generate single file with both front and back positions`

Then click `Generate Position File`

Open the file, should be `<project>-all-pos.csv`

Change the following:

- `Ref` -> `Designator`
- `PosX` -> `Mid X`
- `PosY` -> `Mid Y`
- `Rot` -> `Rotation`
- `Side` -> `Layer`

| Ref (`Designator`) | Val | Package| PosX (`Mid X`) | PosY (`Mid Y`)| Rot (`Rotation`) | Side (`Layer`) |
| :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| C1 | 4.7u | Capacitor_SMD:C_0402_1005Metric | 120.2 | -102 | 180 | top |

### Gerber Files

Go to : 

```
|-- PCB Editor
|   |-- File
|   |   |-- Fabrication Output
|   |   |   |-- Gerbers (.gbr)...
```

Make sure all the included layers are correct

Click `Generate Drill Files...`

Tick `PTH and NPTH in single file`

Click `Generate`

Click `Close`

Click `Plot`

Put all the files that just got created into a `.zip` archive

### Ordering on JLCPCB

- Surface Finish: `LeadFree HASL` 