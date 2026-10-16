# FLIGHT CONTROLLER - STM32F405

A custom 4 layer flight controller built from scratch for YSWS Pixl. This follows a hybrid approach of 50mm*50mm which is not normal for actual FCs on drone. This is only a **paper-design**. This has not been manufactured yet.

![PCB](images/FINAL/3D_PCB_HERO.png)

# Description 
This flight controller is unique in a way, unlike most of the FC's which skip Fuses to save space, this contains 3 fuses, making the room for human errors tiny.

The main MCU is STM32F405RGT6, this tiny thing has 1MB flash and 192KB ram and can easily run betaflight at full speed. With betaflight it can also do blackbox (memory) logging, IMU (gyroscope) and OSD (share details like altitude etc) simultaneously.

IMU I used for this project is ICM-42688-P which runs at 24 MHZ! It's a 6 axis gyroscope. I chose this because it is low noise (doesn't give false readings due to electrical noise) and gives reading in high resolution.

Then comes the blackbox which logs the data which is useful when the drone crashes or something so it is easier for us to point out errors. The exact module is W25Q128JVS,
~4 minutes of 2 kHz blackbox logging, or ~30 minutes at 500 Hz.

Watchdog-safe boot BOOT0 pull-down (R21) + BOOT1 pull-down (R25) MCU always boots from flash unless user says so. Sometimes when drone crashes MCU might repeatedly boot into USB mode and face continuous resets, but this mechanism ensures that, this aint going to happen.

No OSD: OSD is a chip that transmits data like altitude etc into the video, but most of the modern day digital cameras have this inbuilt. So I removed it.

No barometer: Same as OSD, most modern day GPS have barometer built in so there is no need of one on board.

2S - 6S input power, max 28V. And over current protection in built.

Reverse polarity protection,due to a P-FET on board even if the user connects + and - in reverse, power ain't gonna reach other parts,it's gonna stop at the P-Fet thus protecting rest of the FC.

USB ORing protection, with schkotty diode it ensures that USB and battery power won't fight or push power back.

5 independent UARTs, enough for connections GPS, Camera, etc with one spare.

4 pins for motors, with current and voltage measurement with dividers (a divider is a junction of Resistors which steps down the voltage/current and MCU multiplies it with a specific number,here 11 to get voltage and current readings).

Addressable RGB LED and two onboard LEDs indicating 5V and 3.3V with buzzer connections headers for audible output.

What makes it unique ? I built this board such that it is easy to debug, yes no board can work perfectly first time, this instead of saving space, i tried to make it more debuggable by adding multiple Testpoints. 

# Overview

This FC contains minimal architecture (as i am a rookie), it does not contain in built barometer or OSD.

# Architecture

```
Flight_Controller
├── PCB
│   ├── FlightController_Drone.csv
│   ├── FlightController_Drone.kicad_pcb
│   ├── FlightController_Drone.kicad_pro
│   ├── FlightController_Drone.kicad_sch
│   └── gerbers.zip
├── README.md
└── images
    ├── DRAFT
    │   ├── S10_2.png
    │   ├── S11_2.png
    │   ├── S12_2.png
    │   ├── S13_2.png
    │   ├── S1_1.png
    │   ├── S2_1.png
    │   ├── S3_1.png
    │   ├── S4_2.png
    │   ├── S4_P.png
    │   ├── S5_2.png
    │   ├── S6_2.png
    │   ├── S7_2.png
    │   ├── S8_2.png
    │   ├── S9_2.png
    │   └── SFull_2.png
    └── FINAL
        ├── 3D_PCB.png
        ├── 3D_PCB_B.png
        ├── 3D_PCB_HERO.png
        ├── Annotation 2026-10-11 182149.png
        ├── B_CU_PCB.png
        ├── DRC_DUMB.png
        ├── F_CU_PCB.png
        ├── F_PCB.png
        ├── In1_CU.png
        ├── In2_CU.png
        ├── S1.png
        ├── S10.png
        ├── S11.png
        ├── S12.png
        ├── S13.png
        ├── S2.png
        ├── S3.png
        ├── S4.png
        ├── S5.png
        ├── S6.png
        ├── S7.png
        ├── S8.png
        └── S9.png
```

# BOM
refer the .csv file in this repo under PCB.

# Gallery

![PCB](images/FINAL/3D_PCB.png)
![PCB](images/FINAL/3D_PCB_B.png)
![PCB](images/FINAL/B_CU_PCB.png)
![PCB](images/FINAL/F_CU_PCB.png)

**FOR MORE IMAGES PLEASE CHECK images FOLDER**  

# Note

DRC is giving weird errors about footprints touching each other which is fine until pads touch, so I moved them into warnings.

# AI usage
Ai was used only in one or two instances to verify netlist, this makes AI usage less than 5% for this project.

# License

MIT License and you are free to manufacture this board (ik no one will) but in case it dont work, inform me :)
