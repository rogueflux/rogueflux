Based on the pinout you shared and the available technical documentation, I will now provide the **specific pin-to-pin connections** between the PI7C9X3G808GP switch and both the **LAN7430** (Gigabit Ethernet) and **PCI11400** (USB 3.2 Host Controller).

---

## Overview of Connection Strategy

Based on the PI7C9X3G808GP documentation:

- **Upstream Port (to host)**: Port 0, configured as x4 lanes to the PCIe x4 edge connector 
- **Downstream Ports**:
    - **Port 1** (x1 lane) → LAN7430 (Ethernet)
    - **Port 2** (x1 lane) → PCI11400 (USB 3.2)
    - **Ports 3-7** → Available for other x1 downstream edge connectors or spare 

This matches the switch's capability of supporting up to 7 downstream ports in various lane width configurations .

---

## Page 3 to Page 5 Connections: LAN7430

### Assigned Port Configuration
- **Switch Downstream Port**: Port 1 (using PETN[0]/PETP[0] and PERN[0]/PERP[0])
- **Lane Width**: x1 (single lane)
- **Reference Clock**: REFCLKP[0]/REFCLKN[0] (Port 0 clock output)

### Detailed Pin Mapping

| Signal Function | PI7C9X3G808GP (Page 3) | LAN7430 (Page 5) | Notes |
| :--- | :--- | :--- | :--- |
| **PCIe TX (Switch → LAN7430)** | | | |
| Transmit Positive | **PETP[0]** (E10) | PCIE_RX_P | Switch transmits, LAN7430 receives |
| Transmit Negative | **PETN[0]** (D10) | PCIE_RX_M | |
| **PCIe RX (Switch ← LAN7430)** | | | |
| Receive Positive | **PERP[0]** (C1) | PCIE_TX_P | Switch receives, LAN7430 transmits |
| Receive Negative | **PERN[0]** (A11) | PCIE_TX_M | |
| **Reference Clock (100MHz)** | | | |
| Clock Positive | **REFCLKP[0]** (B9) | PCIE_CLK_P | Common clock architecture  |
| Clock Negative | **REFCLKN[0]** (A9) | PCIE_CLK_M | |
| **Control & Management** | | | |
| Reset Output | **PERSET_L** (D13) | PERST# | Active low reset |
| Interrupt Input | **INTA_L** (A4 right) | WAKE# | Wake/Interrupt from LAN7430 |
| SMBus Clock | **SCL_I2C** (D14 right) | (Optional SMBus) | For configuration access |
| SMBus Data | **SDA_I2C** (D13 right) | (Optional SMBus) | |
| **Power** | | | |
| Core Power | C_VDD (A6), C_VDDC (E5) | VDD_CORE | 1.2V (match voltage) |
| I/O Power | VPH_1 (D8), VP_1 (F12) | VDD_IO | 3.3V |
| Ground | Any VSS pin | VSS | Common ground |

### AC Coupling Capacitors Required
Place **100nF capacitors in series** on the following lines from the LAN7430 to the switch:
- **PETP[0] ← PCIE_RX_P** (capacitor near LAN7430)
- **PETN[0] ← PCIE_RX_M** (capacitor near LAN7430)
- **PERP[0] → PCIE_TX_P** (capacitor near switch side)
- **PERN[0] → PCIE_TX_M** (capacitor near switch side)

---

## Page 3/4 to Page 6 Connections: PCI11400

### Assigned Port Configuration
- **Switch Downstream Port**: Port 2 (using PETN[1]/PETP[1] and PERN[1]/PERP[1])
- **Lane Width**: x1 (single lane)
- **Reference Clock**: REFCLK output from Port 1 (use REFCLK_OP[1]/REFCLK_ON[1] or buffer)

### Detailed Pin Mapping

| Signal Function | PI7C9X3G808GP (Page 3/4) | PCI11400 (Page 6) | Notes |
| :--- | :--- | :--- | :--- |
| **PCIe TX (Switch → PCI11400)** | | | |
| Transmit Positive | **PETP[1]** (E9) | PCIE_RX_P | Switch transmits |
| Transmit Negative | **PETN[1]** (D9) | PCIE_RX_M | |
| **PCIe RX (Switch ← PCI11400)** | | | |
| Receive Positive | **PERP[1]** (J2 - from your pinout) | PCIE_TX_P | Switch receives |
| Receive Negative | **PERN[1]** (A10) | PCIE_TX_M | |
| **Reference Clock (100MHz)** | | | |
| Clock Positive | **REFCLK_OP[1]** (B5) | REFCLK_P | Use switch's built-in clock buffer  |
| Clock Negative | **REFCLK_ON[1]** (B4) | REFCLK_M | |
| **Control & Management** | | | |
| Reset Output | **PERSET_L** (D13) | PERST# | Shared with LAN7430 |
| Interrupt Input | **GPIO[?]** (assign) | INT# | Use available GPIO |
| SMBus Clock | **SCL_I2C** (D14 right) | SCL | Shared bus |
| SMBus Data | **SDA_I2C** (D13 right) | SDA | Shared bus |
| **Power** | | | |
| Core Power | C_VDD (A6), C_VDDC (E5) | VDD_CORE | 1.2V or 1.8V (check datasheet) |
| I/O Power | VPH_1 (D8), VP_1 (F12) | VDD_IO | 3.3V |

### AC Coupling Capacitors Required
Place **100nF capacitors** on all four differential signal lines between the switch and PCI11400.

---

## Page 3 to Page 2 Connections: Upstream x4 Edge Connector

### Port 0 Configuration (x4 lanes to host)

| Switch Ball (Page 3) | Signal | Direction | Edge Connector Pin (Page 2) |
| :--- | :--- | :--- | :--- |
| **Lane 0** | | | |
| PERP[0] (C1) | To Host (TX) | Output | A17 (PERp0) |
| PERN[0] (A11) | To Host (TX) | Output | A18 (PERn0) |
| PETP[0] (E10) | From Host (RX) | Input | A14 (PETp0) via 100nF |
| PETN[0] (D10) | From Host (RX) | Input | A15 (PETn0) via 100nF |
| **Lane 1** | | | |
| PERP[1] (from pinout) | To Host (TX) | Output | A23 (PERp1) |
| PERN[1] (A10) | To Host (TX) | Output | A24 (PERn1) |
| PETP[1] (E9) | From Host (RX) | Input | A19 (PETp1) via 100nF |
| PETN[1] (D9) | From Host (RX) | Input | A20 (PETn1) via 100nF |
| **Lane 2** | | | |
| PERP[2] (B8) | To Host (TX) | Output | A27 (PERp2) |
| PERN[2] (A8) | To Host (TX) | Output | A28 (PERn2) |
| PETP[2] (E7) | From Host (RX) | Input | A23? Verify mapping |
| PETN[2] (D7) | From Host (RX) | Input | A24? Verify mapping |
| **Lane 3** | | | |
| PERP[3] (B7) | To Host (TX) | Output | A30 (PERp3) |
| PERN[3] (A7) | To Host (TX) | Output | A31 (PERn3) |
| PETP[3] (E6) | From Host (RX) | Input | A27? Verify mapping |
| PETN[3] (D6) | From Host (RX) | Input | A28? Verify mapping |
| **Reference Clock (100MHz from host)** | | | |
| REFCLK_IN (A4) | From host | Input | A14 (REFCLK+) |
| REFCLK_IP (A5) | From host | Input | A15 (REFCLK-) |
| **Control Signals** | | | |
| PERSET_L (D13) | To host | Output | A11 (PERST#) |
| INTA_L (A4 right) | From host | Input | A11 (WAKE#?) |

### Important Notes
1. **AC coupling capacitors** (100nF-200nF) must be placed on all **PETp/n** lines from the host (signals entering the switch) 
2. The switch can be configured as **x4 upstream** via PORT_CFG strapping pins 
3. All high-speed pairs require **85Ω differential impedance**

---

## Page 3 to Page 9 Connections: Downstream x1 Edge Connector

You can connect additional x1 slots using available downstream ports:

| Switch Port | Lane | Switch Balls | Edge Connector Pins |
| :--- | :--- | :--- | :--- |
| **Port 3 (x1)** | 0 | PERP[4], PERN[4], PETP[4], PETN[4] | x1 slot signals |
| **Port 4 (x1)** | 0 | PERP[5], PERN[5], PETP[5], PETN[5] | x1 slot signals |
| **Port 5 (x1)** | 0 | PERP[6], PERN[6], PETP[6], PETN[6] | x1 slot signals |
| **Port 6 (x1)** | 0 | PERP[7], PERN[7], PETP[7], PETN[7] | x1 slot signals |

**Reference Clock**: Use REFCLK_OP[6] and REFCLK_ON[6] for Port 6, etc.

---

## Page 1 Power Rail Requirements Summary

Based on the PI7C9X3G808GP specifications :

| Voltage Rail | Components | Current (Typical) | Notes |
| :--- | :--- | :--- | :--- |
| **+3.3V** | All I/O, PCIe connectors, VPH pins | ~3-5A | For switch I/O and downstream devices |
| **+3.3VAUX** | Wake-up logic | ~100mA | For standby power |
| **+1.2V Core** | PI7C9X3G808GP (C_VDD, C_VDDC, VDD_x) | ~2.5A | Switch core voltage |
| **+1.8V** | LAN7430 core, PCI11400 core | ~1A | Depending on device requirements |
| **+1.0V** | PCI11400 core (check datasheet) | TBD | Verify from PCI11400 datasheet |

**Power Sequencing for PI7C9X3G808GP**:
1. Apply core voltage (1.2V) first
2. Apply I/O voltage (3.3V)
3. Assert PERST# after all rails are stable

---

## Configuration Straps Summary

| Strap Pin | Page | Balls | Recommended Setting |
| :--- | :--- | :--- | :--- |
| **PORT_CFG[1:0]** | Page 3 | C11, D12 | Set for x4 upstream / x4 downstream configuration |
| **CK_MODE** | Page 3 | E12 | Pull-up: Common Clock mode |
| **CHIP_MODE[0:1]** | Page 3/4 | F13, G12 | Set for normal operation |
| **SMBUS_EN_L** | Page 4 | C8 right | Pull-down: Enable SMBus |
| **TEST** | Page 4 | B5 right | Pull-down to GND (disable test mode) |

---

Let me know if you need the precise ball numbers for PERP[4-7] and PERN[4-7] from your pinout to complete the downstream x1 connector mapping!
