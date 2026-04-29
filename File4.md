Excellent! Thank you for sharing the PI7C9X3G808GP pinout. Now I can provide you with the **exact pin mapping** for Pages 3 and 4.

Based on your request to split the switch across 2 pages:
- **Page 3 (PCIe Switch 1A)** = Parts 1-4 of the pinout (A1 through G14 on the left side, plus corresponding right side balls)
- **Page 4 (PCIe Switch 1B)** = Parts 5-7 of the pinout (remaining balls)

Here is the organized pinout for each page:

---

## Page 3: PCIe Switch 1A (First 4 Parts)

### Ball A Side (Left Column) - Group 1

| Ball | Signal | Connection Target |
| :--- | :--- | :--- |
| A1 | H1 | **Leave NC** (test point) |
| A2 | REFCLK_ON[0] | To PCIe x4 connector A14 (100MHz out) |
| A3 | REFCLK_OP[0] | To PCIe x4 connector A15 (100MHz out) |
| A4 | REFCLK_IN | **From** PCIe x4 connector REFCLK+ (optional) |
| A5 | REFCLK_IP | **From** PCIe x4 connector REFCLK- (optional) |
| A6 | C_VDD | 1.2V core supply (with decoupling) |
| A7 | PERN[3] | **From** PCIe x4 connector PETn3 (via 100nF) |
| A8 | PERN[2] | **From** PCIe x4 connector PETn2 (via 100nF) |
| A9 | REFCLKN[0] | REFCLKN for port 0 (to downstream devices) |
| A10 | PERN[1] | **From** PCIe x4 connector PETn1 (via 100nF) |
| A11 | PERN[0] | **From** PCIe x4 connector PETn0 (via 100nF) |
| A12 | VSS_1 | GND |
| A13 | GPIO[1] | Configuration strap (pull-up/down) |
| A14 | GPIO[0] | Configuration strap (pull-up/down) |

### Ball B Side (Left Column) - Group 2

| Ball | Signal | Connection Target |
| :--- | :--- | :--- |
| B1 | JTAG_SEL_L | Pull-up to 3.3V (enable JTAG) |
| B2 | REFCLK_OP[4] | REFCLK output to LAN7430 |
| B3 | VSS_2 | GND |
| B4 | REFCLK_ON[1] | Secondary reference clock output |
| B5 | REFCLK_OP[1] | Secondary reference clock output |
| B6 | VSS_3 | GND |
| B7 | PERP[3] | **To** PCIe x4 connector PERn3 (to host) |
| B8 | PERP[2] | **To** PCIe x4 connector PERn2 (to host) |
| B9 | REFCLKP[0] | REFCLKP for port 0 (to downstream devices) |
| C1 | PERP[1] | **To** PCIe x4 connector PERn1 (to host) |
| C2 | REFCLK_ON[4] | REFCLK output option |
| C3 | REFCLK_OP[3] | REFCLK output option |
| C4 | REFCLK_ON[2] | REFCLK output option |
| C5 | REFCLK_OP[2] | REFCLK output option |
| C6 | VSS_5 | GND |

### Ball C Side (Left Column) - Group 3

| Ball | Signal | Connection Target |
| :--- | :--- | :--- |
| C7 | VSS_6 | GND |
| C8 | VSS_7 | GND |
| C9 | VSS_8 | GND |
| C10 | VSS_9 | GND |
| C11 | PORT_CFG[1] | Configuration (port width, strapped to 3.3V/GND) |
| C12 | GPIO[5] | General purpose I/O or strap |
| C13 | VSS_10 | GND |
| C14 | GPIO[4] | General purpose I/O or strap |
| D1 | REFCLK_OP[6] | REFCLK output (to downstream device) |
| D2 | REFCLK_ON[5] | REFCLK output |
| D3 | REFCLK_OP[5] | REFCLK output |
| D4 | VDDR_1 | 1.8V or 3.3V (reference voltage) |
| D5 | VSS_11 | GND |

### Ball D Side (Left Column) - Group 4

| Ball | Signal | Connection Target |
| :--- | :--- | :--- |
| D6 | PETN[3] | **To** LAN7430 or PCI11400 RXn (via 100nF) |
| D7 | PETN[2] | **To** LAN7430 or PCI11400 RXn (via 100nF) |
| D8 | VPH_1 | 3.3V I/O power |
| D9 | PETN[1] | **To** LAN7430 or PCI11400 RXn (via 100nF) |
| D10 | PETN[0] | **To** LAN7430 or PCI11400 RXn (via 100nF) |
| D11 | VDDR_2 | Reference voltage |
| D12 | PORT_CFG[2] | Configuration strapping |
| D13 | PERSET_L | **To** PERST# of all downstream devices |
| D14 | GPIO[6] | GPIO or strap |
| E1 | REFCLK_ON[6] | REFCLK output |
| E2 | VSS_12 | GND |
| E3 | GPIO[31] | GPIO or strap |
| E4 | GPIO[30] | GPIO or strap |
| E5 | C_VDDC | 1.2V core (critical, good decoupling) |
| E6 | PETP[3] | **From** LAN7430 or PCI11400 TXp |
| E7 | PETP[2] | **From** LAN7430 or PCI11400 TXp |
| E8 | RESREF | 200Ω (±1%) to GND (reference resistor) |
| E9 | PETP[1] | **From** LAN7430 or PCI11400 TXp |
| E10 | PETP[0] | **From** LAN7430 or PCI11400 TXp |
| E11 | VSS_13 | GND |
| E12 | CK_MODE | Clock mode select (pull-up/down) |
| F1 | PORT_GOOD_L[0] | Hot-plug indicator LED (active low) |
| F2 | PORT_GOOD_L[1] | Hot-plug indicator LED (active low) |
| F3 | REFCLK_ON[7] | REFCLK output |
| F4 | REFCLK_OP[7] | REFCLK output |
| F5 | GPIO[28] | GPIO or strap |
| F6 | GPIO[29] | GPIO or strap |
| F7 | VSS_14 | GND |
| F8 | VDD_1 | 1.2V core power |
| F9 | VSS_15 | GND |
| F10 | VDD_2 | 1.2V core power |
| F11 | VSS_16 | GND |
| F12 | VP_1 | 3.3V I/O power |
| F13 | CHIP_MODE_[0] | Chip operation mode (strap) |
| F14 | PORT_GOOD_L[3] | Hot-plug indicator (active low) |
| F15 | VSS_17 | GND |

### Ball G Side (Left Column) - Group 4 continued

| Ball | Signal | Connection Target |
| :--- | :--- | :--- |
| G1 | PORT_GOOD_L[2] | Hot-plug indicator (active low) |
| G2 | GPIO[25] | GPIO or strap |
| G3 | GPIO[26] | GPIO or strap |
| G4 | GPIO[27] | GPIO or strap |
| G5 | FATAL_ERR_L | Error indicator (connect to LED if desired) |
| G6 | VP_2 | 3.3V I/O power |
| G7 | VSS_18 | GND |
| G8 | VDD_3 | 1.2V core power |
| G9 | VSS_19 | GND |
| G10 | VDD_4 | 1.2V core power |
| G11 | VSS_20 | GND |
| G12 | CHIP_MODE_[1] | Chip operation mode (strap) |
| G13 | PORT_GOOD_L[5] | Hot-plug indicator |
| G14 | PORT_GOOD_L[6] | Hot-plug indicator |
| - | PORT_GOOD_L[4] | Hot-plug indicator (not listed above G14) |

---

## Page 4: PCIe Switch 1B (Parts 5-7)

### Right Side Balls (First Column) - Group 5

| Ball | Signal | Connection Target |
| :--- | :--- | :--- |
| A1 (right) | GPIO[23] | GPIO or strap |
| A2 (right) | VSS_21 | GND |
| A3 (right) | GPIO[24] | GPIO or strap |
| A4 (right) | INTA_L | Interrupt output (to host) |
| A5 (right) | VSS_22 | GND |
| A6 (right) | VDD_5 | 1.2V core power |
| A7 (right) | VSS_23 | GND |
| A8 (right) | VDD_6 | 1.2V core power |
| A9 (right) | VSS_24 | GND |
| A10 (right) | VP_3 | 3.3V I/O power |
| A11 (right) | TRST_L | JTAG reset (pull-up to 3.3V) |
| A12 (right) | TMS | JTAG TMS (to debug header) |
| A13 (right) | TDO | JTAG TDO (to debug header) |
| B1 (right) | PORT_GOOD_L[7] | Hot-plug indicator LED (active low) |
| B2 (right) | GPIO[22] | GPIO or strap |

### Right Side Balls (Second Column) - Group 6

| Ball | Signal | Connection Target |
| :--- | :--- | :--- |
| B3 (right) | GPIO[20] | GPIO or strap |
| B4 (right) | GPIO[21] | GPIO or strap |
| B5 (right) | TEST | **Tie to GND** (test mode disable) |
| B6 (right) | VP_4 | 3.3V I/O power |
| B7 (right) | VSS_25 | GND |
| B8 (right) | VDD_7 | 1.2V core power |
| B9 (right) | VSS_26 | GND |
| C1 (right) | VDD_8 | 1.2V core power |
| C2 (right) | VSS_27 | GND |
| C3 (right) | TCK | JTAG TCK (to debug header) |
| C4 (right) | TDI | JTAG TDI (to debug header) |
| C5 (right) | VSS_28 | GND |
| C6 (right) | PETP[4] | **From** downstream device TX (LAN7430/PCI11400) |
| C7 (right) | PETP[5] | **From** downstream device TX |
| C8 (right) | SMBUS_EN_L | **Tie low** to enable SMBus |
| C9 (right) | PETP[6] | **From** downstream device TX |
| C10 (right) | PETP[7] | **From** downstream device TX |
| C11 (right) | VSS_30 | GND |
| C12 (right) | EEDI | SPI EEPROM data in |
| C13 (right) | EECK | SPI EEPROM clock |
| C14 (right) | EECS_L | SPI EEPROM chip select (active low) |
| D1 (right) | GPIO[15] | GPIO or strap |
| D2 (right) | VSS_31 | GND |
| D3 (right) | CLKBUF_INPUT_SEL | Clock buffer input select (pull-up/down) |
| D4 (right) | VDDR_3 | Reference voltage |
| D5 (right) | VSS_32 | GND |
| D6 (right) | PETN[4] | **To** downstream device RXn (via 100nF) |
| D7 (right) | PETN[5] | **To** downstream device RXn (via 100nF) |
| D8 (right) | VPH_2 | 3.3V I/O power |
| D9 (right) | PETN[6] | **To** downstream device RXn (via 100nF) |
| D10 (right) | PETN[7] | **To** downstream device RXn (via 100nF) |
| D11 (right) | VDDR_4 | Reference voltage |
| D12 (right) | I2C_ADDR_[0] | I2C address select (pull-up/down) |
| D13 (right) | SDA_I2C | I2C data (to EEPROM/host) |
| D14 (right) | SCL_I2C | I2C clock (to EEPROM/host) |

### Right Side Balls (Third Column) - Group 7

| Ball | Signal | Connection Target |
| :--- | :--- | :--- |
| E1 (right) | GPIO[14] | GPIO or strap |
| E2 (right) | GPIO[12] | GPIO or strap |
| E3 (right) | GPIO[13] | GPIO or strap |
| E4 (right) | GPIO[11] | GPIO or strap |
| E5 (right) | GPIO[10] | GPIO or strap |
| E6 (right) | VSS_33 | GND |
| E7 (right) | VSS_34 | GND |
| E8 (right) | VSS_35 | GND |
| E9 (right) | VSS_36 | GND |
| E10 (right) | VSS_37 | GND |
| E11 (right) | PDC_L_[6] | Port disable control (active low, pull-up) |
| E12 (right) | I2C_ADDR_[2] | I2C address select |
| F1 (right) | VSS_38 | GND |
| F2 (right) | I2C_ADDR_[1] | I2C address select |
| F3 (right) | GPIO[9] | GPIO or strap |
| F4 (right) | GPIO[7] | GPIO or strap |
| F5 (right) | GPIO[8] | GPIO or strap |
| F6 (right) | PDC_L_[7] | Port disable control |
| F7 (right) | VSS_39 | GND |
| F8 (right) | PERP[4] | **To** downstream device TX (if using port 4) |
| F9 (right) | PERP[5] | **To** downstream device TX |
| F10 (right) | REFCLKP[1] | REFCLK output for port 1 |
| F11 (right) | PERP[6] | **To** downstream device TX |
| F12 (right) | PERP[7] | **To** downstream device TX |
| F13 (right) | VSS_40 | GND |
| F14 (right) | PDC_L_[5] | Port disable control |
| F15 (right) | PDC_L_[4] | Port disable control |
| G1 (right) | PDC_L_[3] | Port disable control |
| G2 (right) | SHPCINT_L | Shared PCIe interrupt (to host) |
| G3 (right) | VSS_41 | GND |
| G4 (right) | SHDA_I2C | Shared I2C data (to host) |
| G5 (right) | SHCL_I2C | Shared I2C clock (to host) |
| G6 (right) | VSS_42 | GND |
| G7 (right) | PERN[4] | **From** downstream device RX (via 100nF) |
| G8 (right) | PERN[5] | **From** downstream device RX |
| G9 (right) | REFCLKN[1] | REFCLK output for port 1 |
| G10 (right) | PERN[6] | **From** downstream device RX |
| G11 (right) | PERN[7] | **From** downstream device RX |
| G12 (right) | VSS_43 | GND |
| G13 (right) | PDC_L_[2] | Port disable control |
| G14 (right) | PDC_L_[0] | Port disable control |

---

## Quick Reference: Critical Connections Summary

| Function | Page 3 Balls | Page 4 Balls |
| :--- | :--- | :--- |
| **Upstream (x4 host)** | A7-A8, A10-A11 (PERN), B7-B8, C1 (PERP) | None |
| **Downstream devices** | D6-D7, D9-D10 (PETN), E6-E7, E9-E10 (PETP) | C6-C7, C9-C10 (PETP), D6-D7, D9-D10 (PETN), F8-F9, F11-F12 (PERP), G7-G8, G10-G11 (PERN) |
| **Reference clocks** | A2-A3, B2, B4-B5, C2-C5, D1-D3, F3-F4 | F10, G9 |
| **Power** | A6, C_VDD; D8, F12, G6 (VPH); E5 (C_VDDC); F8, F10, G8, G10 (VDD) | A6, A8, B8, C1 (VDD); A10, B6, D8 (VPH) |
| **GND** | A12, B3, B6, C6-C10, C13, D5, E2, E11, F7, F9, F11, F15, G7, G9, G11 | A2, A5, A7, A9, B7, B9, C2, C5, C11, D2, D5, E6-E10, F1, F7, F13, G3, G6, G12 |
| **Configuration** | A13-A14, C11, C12, C14, D12, D14, E3-E4, E12, F5-F6, F13, G2-G4, G12 | C8, C12-C14, D12-D14, E11-E12, F2, F5, F14-F15, G1, G4-G5, G13-G14 |
| **Status LEDs** | F1-F2, F14, G1, G13-G14 | B1 |
| **JTAG** | B1 | A11-A13, C3-C4 |
| **I2C/SMBus** | None | C8, D13-D14, G4-G5 |

---

## Recommended Strapping for Operation

| Strap Ball | Recommended Value | Function |
| :--- | :--- | :--- |
| CK_MODE (E12) | Pull-up to 3.3V | Common Clock mode |
| SMBUS_EN_L (C8 right) | Pull-down to GND | Enable SMBus |
| CHIP_MODE_[0] (F13) | Pull-up | Normal operation |
| CHIP_MODE_[1] (G12) | Pull-down | Normal operation |
| TEST (B5 right) | Pull-down | Disable test mode |
| JTAG_SEL_L (B1) | Pull-up | Enable JTAG |

Would you like me to also provide the specific connections between this switch and the LAN7430/PCI11400 using these ball assignments?
