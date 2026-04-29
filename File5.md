Based on your screenshot, I can now provide the **complete pin mapping** for all downstream ports. The PI7C9X3G808GP has **8 lanes total** that can be configured as an upstream port (x4) and up to 4 downstream x1 ports .

---

## Downstream Port Ball Map (From Your Screenshot)

Here are all the PERP/PERN (transmit to downstream device) and PETP/PETN (receive from downstream device) balls:

| Port | Lane | PERP (TX to device) | PERN (TX to device) | PETP (RX from device) | PETN (RX from device) |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **Port 0** (Upstream to Host) | Lane 0 | C1 | A11 | E10 | D10 |
| | Lane 1 | J2 (from pinout) | A10 | E9 | D9 |
| | Lane 2 | B8 | A8 | E7 | D7 |
| | Lane 3 | B7 | A7 | E6 | D6 |
| **Port 1** (Downstream) | Lane 0 | F8 (right) | G7 (right) | C6 (right) | D6 (right) |
| **Port 2** (Downstream) | Lane 0 | F9 (right) | G8 (right) | C7 (right) | D7 (right) |
| **Port 3** (Downstream) | Lane 0 | F11 (right) | G10 (right) | C9 (right) | D9 (right) |
| **Port 4** (Downstream) | Lane 0 | F12 (right) | G11 (right) | C10 (right) | D10 (right) |

**Key Notes:**
- **Port 0** is the upstream port (connects to your PCIe x4 edge connector on Page 2)
- **Ports 1-4** are downstream ports (connect to LAN7430, PCI11400, and x1 edge connectors)
- Each downstream port uses **x1 lane width** 

---

## Page 3 to Page 5: LAN7430 Connection (Port 1 Downstream)

Connect **Port 1 Downstream** to the LAN7430 on Page 5:

| Function | PI7C9X3G808GP (Page 3/4) | Ball | LAN7430 (Page 5) | Pin | Notes |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **PCIe TX (Switch → LAN7430)** | | | | | |
| Transmit Positive | **PERP[1]** | F8 (right) | PCIE_RX_P | - | Switch transmits to LAN7430 |
| Transmit Negative | **PERN[1]** | G7 (right) | PCIE_RX_M | - | |
| **PCIe RX (Switch ← LAN7430)** | | | | | |
| Receive Positive | **PETP[1]** | C6 (right) | PCIE_TX_P | - | Switch receives from LAN7430 |
| Receive Negative | **PETN[1]** | D6 (right) | PCIE_TX_M | - | |
| **Reference Clock** | | | | | |
| Clock Positive | **REFCLKP[1]** | F10 (right) | PCIE_CLK_P | - | Use Port 1 REFCLK |
| Clock Negative | **REFCLKN[1]** | G9 (right) | PCIE_CLK_M | - | |
| **Control** | | | | | |
| Reset | **PERSET_L** | D13 (left) | PERST# | - | Shared reset |
| Interrupt | **INTA_L** | A4 (right) | WAKE# | - | From LAN7430 |

**AC Coupling Capacitors Required:** Place **100nF** on PETP[1]/PETN[1] near the switch side.

---

## Page 3/4 to Page 6: PCI11400 Connection (Port 2 Downstream)

Connect **Port 2 Downstream** to the PCI11400 on Page 6:

| Function | PI7C9X3G808GP (Page 3/4) | Ball | PCI11400 (Page 6) | Notes |
| :--- | :--- | :--- | :--- | :--- |
| **PCIe TX (Switch → PCI11400)** | | | | |
| Transmit Positive | **PERP[2]** | F9 (right) | PCIE_RX_P | Switch transmits |
| Transmit Negative | **PERN[2]** | G8 (right) | PCIE_RX_M | |
| **PCIe RX (Switch ← PCI11400)** | | | | |
| Receive Positive | **PETP[2]** | C7 (right) | PCIE_TX_P | Switch receives |
| Receive Negative | **PETN[2]** | D7 (right) | PCIE_TX_M | |
| **Reference Clock** | | | | |
| Clock Positive | **REFCLK_OP[1]** | B5 (left) | REFCLK_P | Use buffered output |
| Clock Negative | **REFCLK_ON[1]** | B4 (left) | REFCLK_M | |

**AC Coupling Capacitors Required:** Place **100nF** on all four differential signal lines.

---

## Page 3/4 to Page 9: Downstream x1 Edge Connector (Port 3)

Connect **Port 3 Downstream** to your first x1 PCIe slot on Page 9:

| Function | PI7C9X3G808GP (Page 3/4) | Ball | PCIe x1 Edge Connector (Page 9) | Pin | Notes |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **PCIe TX (Switch → Slot)** | | | | | |
| Transmit Positive | **PERP[3]** | F11 (right) | PERp0 | A17 | To slot receive |
| Transmit Negative | **PERN[3]** | G10 (right) | PERn0 | A18 | |
| **PCIe RX (Switch ← Slot)** | | | | | |
| Receive Positive | **PETP[3]** | C9 (right) | PETp0 | A14 | From slot transmit (via 100nF) |
| Receive Negative | **PETN[3]** | D9 (right) | PETn0 | A15 | From slot transmit (via 100nF) |
| **Reference Clock** | | | | | |
| Clock Positive | **REFCLK_OP[2]** | C5 (left) | REFCLK+ | A14 | Or use REFCLK_OP[6] |
| Clock Negative | **REFCLK_ON[2]** | C4 (left) | REFCLK- | A15 | |
| **Control** | | | | | |
| Reset | **PERSET_L** | D13 (left) | PERST# | A11 | Shared reset |
| Slot Power Enable | **PDC_L_[?]** | Assign via GPIO | PWR_EN | - | Hot-plug control |

**AC Coupling Capacitors Required:** Place **100nF** on PETP[3]/PETN[3] pins near the switch.

---

## Page 3/4 to Page 9: Downstream x1 Edge Connector (Port 4 - Optional Second Slot)

If you need a **second x1 slot** on Page 9, use Port 4:

| Function | PI7C9X3G808GP (Page 3/4) | Ball | PCIe x1 Edge Connector (Page 9) | Pin |
| :--- | :--- | :--- | :--- | :--- |
| **PCIe TX (Switch → Slot)** | | | | |
| Transmit Positive | **PERP[4]** | F12 (right) | PERp0 | A17 |
| Transmit Negative | **PERN[4]** | G11 (right) | PERn0 | A18 |
| **PCIe RX (Switch ← Slot)** | | | | |
| Receive Positive | **PETP[4]** | C10 (right) | PETp0 | A14 (via 100nF) |
| Receive Negative | **PETN[4]** | D10 (right) | PETn0 | A15 (via 100nF) |
| **Reference Clock** | | | | |
| Clock Positive | **REFCLK_OP[3]** | C3 (left) | REFCLK+ | A14 |
| Clock Negative | **REFCLK_ON[3]** | (from pinout) | REFCLK- | A15 |

---

## Reference Clock Summary for All Devices

The PI7C9X3G808GP has **integrated clock buffers**, reducing BOM cost :

| Device | Clock Source | Positive Ball | Negative Ball |
| :--- | :--- | :--- | :--- |
| Host (Upstream) | From host to switch | REFCLK_IN (A4) | REFCLK_IP (A5) |
| LAN7430 (Port 1) | Switch REFCLK Port 1 | REFCLKP[1] (F10 right) | REFCLKN[1] (G9 right) |
| PCI11400 (Port 2) | Switch REFCLK Port 2 | REFCLK_OP[1] (B5 left) | REFCLK_ON[1] (B4 left) |
| x1 Slot 1 (Port 3) | Switch REFCLK Port 3 | REFCLK_OP[2] (C5 left) | REFCLK_ON[2] (C4 left) |
| x1 Slot 2 (Port 4) | Switch REFCLK Port 4 | REFCLK_OP[3] (C3 left) | REFCLK_ON[3] (from pinout) |

**Reference Clock Modes Supported** :
- Common Clock (most common)
- SRNS (Independent Reference No Spread Spectrum)
- SRIS (Independent Reference Spread Spectrum)

---

## Power and Ground Balls Reference

| Ball | Signal | Type | Page |
| :--- | :--- | :--- | :--- |
| A6 | C_VDD | Core 1.2V | Page 3 |
| E5 | C_VDDC | Core 1.2V | Page 3 |
| F8 | VDD_1 | Core 1.2V | Page 3 |
| F10 | VDD_2 | Core 1.2V | Page 3 |
| G8 | VDD_3 | Core 1.2V | Page 3 |
| G10 | VDD_4 | Core 1.2V | Page 3 |
| D8 | VPH_1 | I/O 3.3V | Page 3 |
| F12 | VP_1 | I/O 3.3V | Page 3 |
| G6 | VP_2 | I/O 3.3V | Page 3 |
| A12, B3, B6, C6-C10, C13, D5, E2, E11, F7, F9, F11, F15, G7, G9, G11 | VSS_x | Ground | Page 3 |

---

## Quick Reference Table: Port to Device Mapping

| Port | Type | Connected To | Page | Lane Width |
| :--- | :--- | :--- | :--- | :--- |
| Port 0 (Lanes 0-3) | Upstream | PCIe x4 Edge Connector | Page 2 | x4 |
| Port 1 | Downstream | LAN7430 | Page 5 | x1 |
| Port 2 | Downstream | PCI11400 | Page 6 | x1 |
| Port 3 | Downstream | PCIe x1 Slot 1 | Page 9 | x1 |
| Port 4 | Downstream | PCIe x1 Slot 2 (optional) | Page 9 | x1 |

The PI7C9X3G808GP can support up to 7 downstream ports total , so you have additional ports available if needed.

Would you like me to also provide the **configuration strap settings** to set the upstream port to x4 and configure these downstream ports as x1?
