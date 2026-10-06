# InkTime -- Smartwatch PCB Design

## Overview

InkTime is an open-source, low-power smartwatch design targeting a battery life of at least 30 days, using an e-paper display and an energy-efficient hardware architecture.

The system is built around the nRF52840 microcontroller and integrates:
- an e-paper display
- BLE notifications
- an accelerometer for step counting
- haptic feedback
- efficient power management

---

## Block Diagram

```
           [ USB Type-C ]                              [ LiPo Battery ]
                 |                                            |
                 v                                            |
          +---------------+                                   |
          |   BQ25180     |<----------------------------------+
          |   Charger     |                                   |
          +---------------+                                   v
                 | VSYS                             +---------------+
                 v                                  |  MAX17048     |
          +---------------+                         | Fuel Gauge    |
          |   RT6160      |                         +---------------+
          | Buck-Boost    |                                |
          +---------------+                                |
                 | 3.3V                                   |
                 |                                        |
        +------------------- nRF52840 MCU ------------------+
        |                                                   |
        | I2C ---> BMA421                                  |
        | I2C ---> DRV2605L ---> Motor                     |
        | I2C ---> MAX17048                               |
        | I2C ---> BQ25180, RT6160                        |
        |                                                   |
        | SPI ---> E-paper Display                         |
        | GPIO ---> Buttons                                |
        | GPIO ---> PFET (Display Power)                   |
        +---------------------------------------------------+
```

---

## Bill of Materials (BOM)

| Function | Component | Package | JLC Code | Datasheet |
|--------|-----------|--------|----------|----------|
| MCU | nRF52840-QIAA-R | aQFN73 | C190794 | Nordic |
| Charger | BQ25180YBGR | DSBGA-8 | C3682423 | TI |
| Regulator | RT6160AWSC | WLCSP-15 | C7065276 | Richtek |
| Fuel Gauge | MAX17048G+T10 | DFN-8 | C2682616 | Maxim |
| Accelerometer | BMA421 | LGA-12 | C5242966 | Bosch |
| Haptic Driver | DRV2605LDGSR | VSSOP-10 | C527464 | TI |
| Motor | LCM1027B3605F | Wire | C7528806 | ERM |
| PFET | SI2301CDS | SOT-23 | C10487 | Vishay |
| USB-C | KH-TYPE-C-16P | SMD | - | Generic |
| Debug | TC2030 | Footprint | - | Tag-Connect |

---

## Hardware Functionality

### Power
- USB-C 5V input
- BQ25180 charger with power-path management
- RT6160 for the 3.3 V supply
- MAX17048 for battery monitoring

### MCU
- nRF52840 (BLE + system control)
- RTC for time updates

### Display
- E-paper 1.54”
- periodic updates
- PFET-controlled power supply

### Sensors and Haptics
- BMA421 for step counting
- DRV2605L for vibration feedback

### Input
- 3 buttons (active low)

---

## nRF52840 Pin Mapping

### I2C
- P0.06 SDA
- P0.07 SCL

### SPI
- P0.02 SCK
- P0.03 MOSI
- P0.05 CS
- P0.15 DC
- P0.16 RST
- P0.17 BUSY

### GPIO
- P1.01 PFET
- P0.12 Haptic EN

### Interrupts
- P0.08 IMU INT1
- P1.08 IMU INT2
- P0.10 Fuel gauge ALERT
- P0.11 Charger INT

### Buttons
- P0.13 UP
- P0.14 DOWN
- P1.00 ENTER

---

## Design Decisions

- components placed only on the top layer
- ground planes on the top and bottom layers
- via stitching in the RF area
- decoupling capacitors close to the pins
- keepout beneath the antenna
- battery connection without a JST connector

---

## DRC and Validation

- ERC checked
- DRC performed
- exceptions:
  - Only INPUT pins on NET ID
  - mechanical overlap of USB/buttons

---

## Conclusion

The design is optimized for low power consumption, compact integration and mass production.
