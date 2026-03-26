# InkTime

An affordable, open-source smartwatch powered by the nRF52840 with an e-paper display, designed for low power consumption and BLE connectivity.

![InkTime assembled](Images/inktime_assembled.png)

## Hardware Diagram

![Block Diagram](Images/block_diagram.png)

The InkTime smartwatch is built around the Nordic nRF52840 microcontroller and features the following subsystems:

```
                          ┌──────────────┐
                          │   USB-C (J4) │
                          └──────┬───────┘
                                 │ VBUS, D+, D-
                          ┌──────▼───────┐
                          │  USBLC6 (D3) │ ESD Protection
                          └──────┬───────┘
                                 │
              ┌──────────────────┼──────────────────┐
              │                  │                   │
       ┌──────▼────────┐  ┌──────▼───────┐   ┌───────▼──────┐
       │  BQ25180 (IC3)│  │              │   │              │
       │  LiPo Charger │  │  nRF52840    │   │  USBLC6 (D3) │
       └──────┬────────┘  │ (NRF52840_QF)│   │  USB Data    │
              │ VBAT      │              │   └──────────────┘
       ┌──────▼────────┐  │  - BLE 5.0   │
       │  RT6160 (IC1) │  │  - GPIO      │
       │  DC/DC        │  │  - I2C       │
       │  Buck-Boost   │  │  - SPI       │
       └──────┬────────┘  │  - USB       │
              │ VREG/3V3  │              │
              └──────────►│              │◄──── 32MHz Crystal (X1)
                          │              │◄──── 32.768kHz Crystal (X2)
                          │              │◄──── Antenna (ANT1)
                          └──┬──┬──┬──┬──┘
                             │  │  │  │
            ┌────────────────┘  │  │  └───────────────┐
            │                   │  │                  │
     ┌──────▼────────┐   ┌──────▼──▼───┐        ┌─────▼────────┐
     │  BMA423 (IC2) │   │ E-Paper     │        │  DRV2605     │
     │  IMU          │   │ Display(J2) │        │  Haptic (IC4)│
     │  Accelerometer│   │ 1.54" SPI   │        └──────────────┘
     └───────────────┘   └─────────────┘
            │                                   ┌──────────────┐
     ┌──────▼───────┐                           │  MAX17048(U1)│
     │  I2C Bus     │◄─────────────────────────►│  Fuel Gauge  │
     │  SDA/SCL     │                           └──────────────┘
     └──────────────┘
                          ┌──────────────┐
                          │  TC2030 (J1) │
                          │  SWD Debug   │
                          └──────────────┘
```

## Bill of Materials (BOM)

| Component | Value/Part Number | Package | Qty | Description | Datasheet |
|-----------|-------------------|---------|-----|-------------|-----------|
| NRF52840_QF | nRF52840 | AQFN-73 | 1 | MCU with BLE 5.0 | [Datasheet](https://infocenter.nordicsemi.com/pdf/nRF52840_PS_v1.1.pdf) |
| IC1 | RT6160AWSC | WL-CSP-15 | 1 | DC/DC Buck-Boost Converter | [Datasheet](https://www.richtek.com/assets/product_file/RT6160A/DS6160A-05.pdf) |
| IC2 | BMA423 | LGA-12 | 1 | Triaxial Accelerometer (IMU) | [Datasheet](https://www.bosch-sensortec.com/products/motion-sensors/accelerometers/bma423/) |
| IC3 | BQ25180YBGR | DSBGA-8 | 1 | LiPo Charger | [Datasheet](https://www.ti.com/lit/ds/symlink/bq25180.pdf) |
| IC4 | DRV2605YZFR | WCSP-9 | 1 | Haptic Driver | [Datasheet](https://www.ti.com/lit/ds/symlink/drv2605.pdf) |
| U1 | MAX17048G+T10 | TDFN-8 | 1 | Fuel Gauge | [Datasheet](https://www.analog.com/media/en/technical-documentation/data-sheets/max17048-max17049.pdf) |
| D3 | USBLC6-2SC6Y | SOT-23-6 | 1 | ESD Protection | [Datasheet](https://www.st.com/resource/en/datasheet/usblc6-2.pdf) |
| ANT1 | 2450AT18B100E | SMD | 1 | 2.4GHz Chip Antenna | [Datasheet](https://www.johansontechnology.com/datasheets/antennas/2450AT18B100.pdf) |
| X1 | 32MHz Crystal | SMD 2016 | 1 | Main Clock | - |
| X2 | 32.768kHz Crystal | SMD 3215 | 1 | RTC Clock | - |
| J1 | TC2030-IDC | Tag-Connect | 1 | SWD Debug Header | [Datasheet](https://www.tag-connect.com/product/tc2030-idc) |
| J2 | 503480-2400 | FPC-24 | 1 | E-Paper FPC Connector | [Datasheet](https://www.molex.com/en-us/part-list/503480-2400) |
| J4 | KH-TYPE-C-16P | SMD | 1 | USB-C Connector | - |
| Q1 | DMG2305UX-7 | SOT-23 | 1 | P-Ch MOSFET (EPD drive) | [Datasheet](https://www.diodes.com/assets/Datasheets/DMG2305UX.pdf) |
| Q3 | SI1308EDL-T1-GE3 | SC-70 | 1 | N-Ch MOSFET (EPD drive) | [Datasheet](https://www.vishay.com/docs/68986/si1308edl.pdf) |
| D2, D4, D5 | MBR0530 | SOD-123 | 3 | Schottky Diode (EPD drive) | - |
| L1 | 3.9nH | SMD 2012 | 1 | RF Matching Network | - |
| L2 | 10µH | SMD 2012 | 1 | nRF Power Inductor | - |
| L3 | 15nH | SMD 2012 | 1 | RF Filter | - |
| L5 | 68µH | SMD 2012 | 1 | EPD Boost Inductor | - |
| L7 | FTC252012SR47MBCA | SMD 2012 | 1 | DC/DC Power Inductor (0.47µH) | [JLCPCB](https://jlcpcb.com/partdetail/6763488-FTC252012SR47MBCA/C5832368) |
| SW_UP, SW_DN, SW_ENT | EVP-AKE31A | SMD | 3 | Tactile Buttons | - |
| SJ1 | Solder Jumper | SMD | 1 | Configuration Jumper | - |
| R1, R2, R5 | 0Ω | 0201 | 3 | Zero-ohm Resistors | - |
| R3, R4 | 3.3kΩ | 0201 | 2 | I2C Pull-up Resistors | - |
| R6, R2_EP_DR, R_PWR_EPD | 10kΩ | 0201 | 3 | Pull-up/Pull-down | - |
| R7, R8, R9 | 10kΩ | 0201 | 3 | Button Pull-down Resistors | - |
| R1_USB, R2_USB | 5.1kΩ | 0201 | 2 | USB-C CC Pull-down | - |
| R_TYPE_SEL | 2.2Ω | 0201 | 1 | EPD Type Select | - |
| R1_EP_DR | 0.47Ω | 0201 | 1 | EPD Current Sense | - |
| C1, C2, C17, C18 | 12pF | 0201 | 4 | Crystal Load Capacitors | - |
| C3, C4 | 1pF | 0201 | 2 | RF Matching | - |
| C9 | 820pF | 0201 | 1 | RF Matching | - |
| C5, C7, C8, C12, C19 | 100nF | 0201 | 5 | Decoupling Capacitors | - |
| C11 | 100pF | 0201 | 1 | Decoupling (DEC5) | - |
| C16 | 47nF | 0201 | 1 | Decoupling | - |
| C15 | 1.0µF | 0402 | 1 | Decoupling | - |
| C6, C14, C20, C21, C43, C2-EP-DR | 4.7µF | 0402 | 6 | Bulk Capacitors | - |
| C24, C39, C1-EP-DR | 10µF | 0402 | 3 | Input/Output Caps | - |
| C25, C33 | 22µF | 0402 | 2 | DC/DC Output Caps | - |
| C23, C27, C34, C42, EPD_C5 | 0.1µF | 0201 | 5 | Decoupling | - |
| C29-C32, C37, C38, EPD_C1-C12 | 1µF | 0402 | 15 | Filter/Bypass Caps | - |
| C10, C13, C22 | N.C. | 0201 | 3 | Do Not Populate | - |
| Test Pads | - | SMD | 14 | TP_GND, TP_3V3, TP_VBAT, etc. | - |

## Hardware Description

### Microcontroller — nRF52840 (NRF52840_QF)
The nRF52840 is the core of the InkTime, providing Bluetooth Low Energy 5.0 connectivity, ARM Cortex-M4F processing at 64MHz, and extensive GPIO. It communicates with peripherals via I2C and SPI buses and is powered by 3.3V from the RT6160 DC/DC converter.

Two crystals provide clock sources: a 32MHz crystal (X1) connected to XC1/XC2 with 12pF load capacitors (C1, C2), and a 32.768kHz crystal (X2) connected to P0.00/XL1 and P0.01/XL2 with 12pF load capacitors (C17, C18).

The RF section uses a 2450AT18B100E chip antenna connected through an impedance matching network (L1=3.9nH, C3=1pF, C4=1pF, C9=820pF) to the ANT pin. The antenna is placed at the edge of the PCB with a copper keepout zone on all layers underneath.

Decoupling capacitors placed close to corresponding power pins: C5 (100nF) on DEC1, C8 (100nF) on DEC3, C7 (100nF) on DEC4_6, C11 (100pF) on DEC5, C15 (1.0µF). Capacitors C10, C13, C22 are N.C. (Do Not Populate) per Nordic's reference design.

### Power Management

**LiPo Charger — BQ25180 (IC3):** Manages battery charging from USB-C (VBUS). Single-cell LiPo battery connected directly to test pads (TP_VBAT, TP_BAT_GND). I2C configurable. Interrupt output (PMIC_INT) to nRF52840. Bypass capacitors C37 (1µF), C39 (10µF). Pull-up R6 (10kΩ) on TS/MR.

**DC/DC Converter — RT6160 (IC1):** Buck-boost converter, VBAT (3.0–4.2V) to VREG/3V3. I2C voltage selection via VSEL pin. Power inductor L7 (0.47µH). Input cap C23 (0.1µF), output caps C24 (10µF), C25 (22µF), C33 (22µF). I2C pull-ups R3, R4 (3.3kΩ).

**Fuel Gauge — MAX17048 (U1):** Battery monitoring via I2C. CELL pin to VBAT, ALERT output to nRF52840. Bypass cap C38 (1µF).

### Display — 1.54" E-Paper (via J2, 503480-2400)
SPI interface (MOSI, SCK, EPD_CS). Drive circuit with Q3 (SI1308EDL), Q1 (DMG2305UX), diodes D2/D4/D5 (MBR0530), inductor L5 (68µH), and capacitors EPD_C1–C12 for high-voltage generation (PREVGH, PREVGL). Control: EPD_RST, EPD_BUSY, EPD_DC.

### IMU — BMA423 (IC2)
Triaxial accelerometer via I2C (SDA, SCL). Two interrupt outputs (IMU_INT1, IMU_INT2) for motion/step/gesture detection. Decoupling C19 (100nF). Zero-ohm resistors R1, R2, R5 for pin configuration.

### Haptic Driver — DRV2605 (IC4)
I2C haptic driver for vibration motor (FIT0774). Enable via HAPTIC_EN GPIO. Decoupling C32 (1µF), C34 (0.1µF).

### USB-C and ESD Protection
KH-TYPE-C-16P connector (J4). USBLC6-2SC6Y (D3) ESD protection. CC pull-downs R1_USB, R2_USB (5.1kΩ). Bypass caps C42 (0.1µF), C43 (4.7µF).

### SWD Debug — TC2030-IDC (J1)
Tag-Connect pogo-pin interface: SWDIO, SWDCLK, SWO, RESET, 3.3V, GND. No permanent connector needed.

## nRF52840 Pin Assignment

| Pin | Signal | Function | Interface |
|-----|--------|----------|-----------|
| P0.00/XL1 | XL1 | 32.768kHz Crystal | Clock |
| P0.01/XL2 | XL2 | 32.768kHz Crystal | Clock |
| P0.04/AIN2 | IMU_INT1 | BMA423 Interrupt 1 | GPIO |
| P0.05/AIN3 | IMU_INT2 | BMA423 Interrupt 2 | GPIO |
| P0.06 | EPD_CS | E-Paper Chip Select | SPI |
| P0.07 | EPD_DC | E-Paper Data/Command | GPIO |
| P0.08 | EPD_RST | E-Paper Reset | GPIO |
| P0.09/NFC1 | PMIC_INT | BQ25180 Interrupt | GPIO |
| P0.11 | SCK | SPI Clock (E-Paper) | SPI |
| P0.12 | MOSI | SPI Data (E-Paper) | SPI |
| P0.13 | EPD_BUSY | E-Paper Busy | GPIO |
| P0.14 | SCL | I2C Clock | I2C |
| P0.18/RESET | RESET | System Reset | Reset |
| P0.19 | HAPTIC_EN | DRV2605 Enable | GPIO |
| P0.26 | SW_UP | Button Up | GPIO |
| P0.27 | SW_DN | Button Down | GPIO |
| P1.00 | SW_ENT | Button Enter | GPIO |
| P1.01 | ALERT | MAX17048 Alert | GPIO |
| P1.09 | SWO | Debug Trace Output | Debug |
| XC1/XC2 | Crystal | 32MHz Oscillator | Clock |
| ANT | RF | Antenna via matching | RF |
| VBUS | USB_VBUS | USB Power (5V) | Power |
| D+/D- | USB Data | USB Data Lines | USB |
| SWDIO | Debug | SWD Data | Debug |
| SWDCLK | Debug | SWD Clock | Debug |

**I2C Bus** shared by: BMA423 (IC2), BQ25180 (IC3), RT6160 (IC1), DRV2605 (IC4), MAX17048 (U1). Pull-ups R3, R4 (3.3kΩ) to 3V3.

## PCB Design Details

### Board Specifications
- **Dimensions:** 46mm × 35mm, rounded corners
- **Layers:** 4 (Top, Route2, Route63, Bottom)
- **Thickness:** 1.0mm
- **Trace widths:** 0.15mm (signals), 0.3mm (power)
- **Components:** All on TOP layer

### Layer Stack
| Layer | Name | Function |
|-------|------|----------|
| 1 | ct Top | Signals + GND polygon |
| 2 | c2 Route2 | Dedicated GND plane |
| 63 | c63 Route63 | Signal routing |
| 64 | cb Bottom | Signals + GND polygon |

### Design Decisions and Accepted Errors
- **Copper Width errors (accepted)**: Power nets (3V3, VREG, VBAT, VBUS, etc.) require 0.3mm trace width per design rules. However, in some dense areas of the board - particularly around small IC packages and tight pad spacing - there is not enough physical space to maintain 0.3mm width for the entire trace length. The resulting DRC errors were accepted.
- **Copper Clearance errors (accepted)**: In highly congested areas, especially around the nRF52840 BGA and the dense passive component clusters, some traces and pads come closer together than the 0.15mm clearance rule allows. These violations occur where component placement leaves no alternative routing path and were accepted after manual verification that no actual shorts exist.
- **Overlapping / Polygon Clearance errors (accepted)**: Some GND polygons on different layers generate clearance warnings where they overlap with same-rank polygons. These are non-functional warnings — the polygons are all on the same net (GND) and do not cause electrical issues.
- **Board Outline Clearance errors (accepted)**: The three tactile buttons (SW_UP, SW_DN, SW_ENT) and the USB-C connector (J4) intentionally extend beyond the PCB edge for mechanical alignment with the enclosure cutouts. This is by design and explicitly permitted in the project specification.
- **N.C. capacitors (C10, C13, C22):** Footprints present but not populated per Nordic reference.
- **Silkscreen:** Labels on IC's, connectors, test pads, capacitors etc.
- **Via stitching**: GND vias are distributed across the board between all ground planes, with higher concentration around the nRF52840 and RF antenna area, to ensure low-impedance ground return paths at 2.4GHz.

## Mechanical

| Component | Dimensions | Notes |
|-----------|-----------|-------|
| Battery (AKYGA LP502030) | 32.5 × 21 × 5.5mm | 3.7V/250mAh, soldered to test pads |
| Display (1.54" e-Paper V2) | 37.32 × 31.80 × 1.05mm | 200×200px, SPI, ultra-low power |
| Vibration Motor (FIT0774) | Ø10 × 2.7mm | 1.5–4.2VDC, 50mA |
| Enclosure | Custom | 3D-printed, cutouts for USB/buttons |

## Repository Structure

```
├── Hardware/
│   ├── InkTime_Schematic.sch
│   └── InkTime_Board.brd
├── Manufacturing/
│   ├── gerbers.zip
│   ├── InkTime_BOM.bom
│   └── InkTime_PnP.cpl
├── Mechanical/
│   ├── InkTime_step.step
│   └── InkTime_complete.f3z
├── Images/
│   ├── TBD
├── LICENSE
└── README.md
```

## License

This project is open source hardware, licensed under [CERN-OHL-P v2](https://ohwr.org/cern_ohl_p_v2.txt).

Distributed as-is; no warranty is given.

---

*Designed as part of the InkTime project — TSC course, Politehnica Bucharest, 2026.*