# Kobra S1 Klipper Configuration & Macros

A collection of custom Klipper macros and configuration tweaks for the **Anycubic Kobra S1** running [Rinkhals custom firmware](https://github.com/Rinkhals/Kobra_S1).

> ⚠️ **Important:** These macros and configurations require the Rinkhals custom firmware to be installed on your Kobra S1. They will not work with stock firmware.

---

## 📋 Table of Contents

- [Prerequisites](#prerequisites)
- [Installation](#installation)
- [Macros](#macros)
  - [PREHEAT30](#preheat30)
  - [COOLDOWN30](#cooldown30)
  - [ZPLUS](#zplus)
  - [ZMINUS](#zminus)
  - [CLEAN_NOZZLE](#clean_nozzle)
- [Configuration Changes](#configuration-changes)
  - [TMC2209 Extruder Current](#tmc2209-extruder-current)
  - [Bed Mesh Settings](#bed-mesh-settings)
- [Contributing](#contributing)

---

## Prerequisites

- Anycubic Kobra S1 3D Printer
- [Rinkhals Custom Firmware](https://github.com/Rinkhals/Kobra_S1) installed
- Basic understanding of Klipper configuration

---

## Installation

1. Open up Mainsail or Fluidd for your printer(enter printers IP in a web-browser)
2. Navigate to the config file called `printer.custom.cfg`
3. Copy and paste the sections you want from this page into this document
4. Save and restart the printer fully (unlike normal klipper, you need to do a full system reboot after saving for the config changes to take effect)

---

## Macros

### PREHEAT30

**Description:** Preheats the printer chamber for 30 minutes using the heated bed and auxiliary fan. Automatically disables after 30 minutes and turns off motors.

**Use Case:** Ideal for preparing the chamber before printing with materials that require a warm environment (ABS, ASA, etc.)

```ini
[gcode_macro PREHEAT30]
gcode:
  G90
  G28
  G0 X125 Y125 Z10 F3600
  SET_HEATER_TEMPERATURE HEATER=heater_bed TARGET=110
  SET_FAN_SPEED FAN=box_fan SPEED=1
  G4 S1800
  SET_HEATER_TEMPERATURE HEATER=heater_bed TARGET=0
  SET_FAN_SPEED FAN=box_fan SPEED=0
  M84
```

> 📝 **Note:** This macro will home the printer and move to the center position. The bed will heat to 110°C for 30 minutes.

---

### COOLDOWN30

**Description:** Activates the chamber exhaust fan for 30 minutes to cool down and ventilate the chamber. Automatically disables after 30 minutes.

**Use Case:** Perfect for clearing fumes after printing with materials like ABS or ASA.

```ini
[gcode_macro COOLDOWN30]
gcode:
  TURN_OFF_HEATERS
  SET_FAN_SPEED FAN=air_filter_fan SPEED=1
  G4 S1800
  SET_FAN_SPEED FAN=air_filter_fan SPEED=0
```

> ⚠️ **Warning:** This macro will turn off all heaters immediately.

---

### ZPLUS

**Description:** Increases the Z offset by 0.05mm while printing.

**Use Case:** Use this if your nozzle is too close to the bed during a print. Can be called multiple times for larger adjustments.

```ini
[gcode_macro ZPLUS]
gcode:
   SET_GCODE_OFFSET Z_ADJUST=+0.05
```

> 📝 **Note:** This adjustment is temporary and will reset after the print completes. Make permanent changes in your slicer.

---

### ZMINUS

**Description:** Decreases the Z offset by 0.05mm while printing.

**Use Case:** Use this if your nozzle is too far from the bed during a print. Can be called multiple times for larger adjustments.

```ini
[gcode_macro ZMINUS]
gcode:
   SET_GCODE_OFFSET Z_ADJUST=-0.05
```

> 📝 **Note:** This adjustment is temporary and will reset after the print completes. Make permanent changes in your slicer.

---

### CLEAN_NOZZLE

**Description:** Automated nozzle cleaning routine. Heats the nozzle to 250°C, homes the printer, then performs a wiping cycle on the brush/wiper area.

**Use Case:** Clean the nozzle if doing maintenance of manual filament swaps.

```ini
[gcode_macro CLEAN_NOZZLE]
gcode:
  SET_HEATER_TEMPERATURE HEATER=extruder TARGET=250
  G28
  G4 S5
  G90
  G1 F36000
  G1 Y250
  G1 F8000
  G1 X81
  G1 Y273
  G4 S2
  G1 F8000
  G1 X96
  G1 X81
  G1 F8000
  G1 X96
  G1 X81
  G1 F8000
  G1 X96
  G1 X81
  G1 F8000
  G1 X96
  G1 X81
  G1 F8000
  G1 X96
  G1 X81
  G1 F8000
  G1 X96
  G1 X81
  G1 F8000
  G1 X96
  G1 X81
  G1 F8000
  G1 X96
  G1 X81
  G1 F8000
  G1 X96
  G1 X81
  G1 F8000
  G1 X96
  G1 X81
  G4 S2
  G1 F8000
  G1 X83
  G1 Y250
  G1 F12000
  G1 Y250
  G1 X47 
  G1 F12000
  G1 Y276
  SET_HEATER_TEMPERATURE HEATER=extruder TARGET=0
  M84
```

> ⚠️ **Warning:** This macro will turn off the extruder heater and disable motors after completion. Remove the last two lines if you want to keep them active.

---

## Configuration Changes

### TMC2209 Extruder Current

**Description:** Increases the extruder stepper motor run current to 0.8A for improved performance and reduced skipping.

**Use Case:** Recommended for better extrusion consistency, especially with high-flow printing or flexible filaments.

```ini
[tmc2209 extruder]
run_current: 0.8
```

> ⚠️ **Warning:** Higher current may cause the motor to run warmer. Monitor temperatures during initial use.

---

### Bed Mesh Settings

**Description:** Optimized bed mesh leveling configuration with 7x7 probe points and bicubic interpolation for improved first layer adhesion.

**Use Case:** Provides more accurate bed leveling across the entire print surface.

```ini
[bed_mesh]
speed: 400
probe_count: 7,7
algorithm: bicubic
mesh_min: 15,15
mesh_max: 235,235
```

> 📝 **Note:** The 7x7 probe grid will take longer than a 5x5 grid but provides better accuracy. Adjust `probe_count` if you prefer faster leveling.

---

## Contributing

Found a bug or have an improvement? Feel free to let me know!

---

## License

These configurations are provided as-is for the Rinkhals community. Use at your own risk.

---

**Happy Printing! 🖨️**
