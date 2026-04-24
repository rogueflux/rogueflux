Now I have enough information to give a detailed pin-by-pin connection guide. The connections between these three chips are all PCIe — the 3G808GP is the central switch, with PCI11400C and LAN7430 each connecting to a downstream port.

---

## System Architecture Assumed

```
[Host PCIe x4] ──► PI7C9X3G808GP (Port 0, upstream x4)
                        │
                        ├── Port 1 (downstream x1 or x4) ──► PCI11400C
                        │
                        └── Port 2 (downstream x1) ──────► LAN7430
```

---

## 1. PI7C9X3G808GP ↔ LAN7430 (PCIe x1)

The LAN7430 is a PCIe x1 endpoint. From the EVB-LAN7430 schematic, its PCIe pins are:

| PI7C9X3G808GP Signal | Direction | LAN7430 Pin | LAN7430 Signal | Notes |
|---|---|---|---|---|
| TXP[4] | → | Pin 19 | PCIE_TX_P | Switch TX → LAN7430 RX |
| TXN[4] | → | Pin 21 | PCIE_TX_M | Switch TX → LAN7430 RX |
| RXP[4] | ← | Pin 16 | PCIE_RX_P | LAN7430 TX → Switch RX |
| RXN[4] | ← | Pin 17 | PCIE_RX_M | LAN7430 TX → Switch RX |
| REFCLKOP[2] | → | Pin 25 | PCIE_CLK_P | 100 MHz diff clock out |
| REFCKON[2] | → | Pin 26 | PCIE_CLK_M | 100 MHz diff clock out |
| PERST[2]# | → | Pin 44 | PERST# | Active-low reset to LAN7430 |
| CLKREQ[2]# | ← | Pin 42 | CLKREQ# | Open-drain, pulled to 3.3V |
| WAKE[2]# | ← | Pin 43 | WAKE# | Open-drain, pulled to 3.3V |

> **AC Coupling:** Place 0.22 µF capacitors in series on TXP[4]/TXN[4] on the switch side (near 3G808GP). LAN7430 handles its own RX side internally.

---

## 2. PI7C9X3G808GP ↔ PCI11400C (PCIe x1 or x4)

From the EVB-PCI11400C schematics, PCI11400C's upstream PCIe port pins are:

### If using PCIe x1 connection (simplest, uses 1 lane)

| PI7C9X3G808GP Signal | Direction | PCI11400C Pin | PCI11400C Signal | Notes |
|---|---|---|---|---|
| TXP[5] | → | Pin 112 | PCIE_RXP_L0_P1 | Switch TX → PCI11400C RX |
| TXN[5] | → | Pin 113 | PCIE_RXM_L0_P1 | Switch TX → PCI11400C RX |
| RXP[5] | ← | Pin 116 | PCIE_TXP_L0_P1 | PCI11400C TX → Switch RX |
| RXN[5] | ← | Pin 117 | PCIE_TXM_L0_P1 | PCI11400C TX → Switch RX |
| REFCLKOP[1] | → | Pin 121 | PCIE_REFCLK_IN_P | 100 MHz diff REFCLK |
| REFCKON[1] | → | Pin 122 | PCIE_REFCLK_IN_M | 100 MHz diff REFCLK |
| PERST[1]# | → | Pin 104 | PCIE_PERST_N | Active-low reset |
| CLKREQ[1]# | ← | Pin — | PCIE_CLKREQ0_N | Open-drain, 10kΩ to 3.3V |
| WAKE[1]# | ← | Pin — | PCIE_WAKE_P0_N | Open-drain, 10kΩ to 3.3V |

### If using PCIe x4 connection (full bandwidth — uses lanes 4–7)

| PI7C9X3G808GP | Direction | PCI11400C Signal | PCI11400C Pin |
|---|---|---|---|
| TXP[4]/TXN[4] | → | PCIE_RXP/M_L0_P0 | Pins 11/12 |
| TXP[5]/TXN[5] | → | PCIE_RXP/M_L1_P0 | Pins 26/25 |
| TXP[6]/TXN[6] | → | PCIE_RXP/M_L2_P0 | Pins 42/43 |
| TXP[7]/TXN[7] | → | PCIE_RXP/M_L3_P0 | Pins 57/56 |
| RXP[4]/RXN[4] | ← | PCIE_TXP/M_L0_P0 | Pins 15/16 |
| RXP[5]/RXN[5] | ← | PCIE_TXP/M_L1_P0 | Pins 22/21 |
| RXP[6]/RXN[6] | ← | PCIE_TXP/M_L2_P0 | Pins 46/47 |
| RXP[7]/RXN[7] | ← | PCIE_TXP/M_L3_P0 | Pins 53/52 |
| REFCLKOP[1]/N | → | PCIE_REFCLK_IN_P/M | Pins 121/122 |
| PERST[1]# | → | PCIE_PERST_N | Pin 104 |
| CLKREQ[1]# | ← | PCIE_CLKREQ0_N | — |
| WAKE[1]# | ← | PCIE_WAKE_P0_N | — |

---

## 3. PI7C9X3G808GP Upstream ↔ Host PCIe x4 Edge

| 3G808GP Signal | Direction | Host Edge Signal | Notes |
|---|---|---|---|
| TXP[0:3] / TXN[0:3] | → | PETp/n[0:3] | Switch TX to host RX |
| RXP[0:3] / RXN[0:3] | ← | PERp/n[0:3] | Host TX to switch RX |
| REFCLKP / REFCLKN | ← | REFCLKp/n | 100 MHz from host |
| PERST# | ← | nPERST | From host, active low |
| CLKREQ# | → | nCLKREQ | Switch requests clock |
| WAKE# | → | nWAKE | Switch wakeup signal |

---

## 4. PI7C9X3G808GP Sideband & Config Pins

These must also be connected for proper operation:

| Signal | Connection | Purpose |
|---|---|---|
| MODE[2:0] | Strap resistors to 0.95V/GND | Sets port/lane configuration (e.g., 4-port mode) |
| EECS / EECK / EEDI / EEDO | To I²C/SPI EEPROM | Loads configuration at boot |
| SMDAT / SMCLK | To management bus or pull-ups | SMBus access for register config |
| JTAG (TDI/TDO/TMS/TCK) | Header or NC | Debug/boundary scan |
| RSTB# | RC circuit or supervisor IC | Power-on reset, hold low until rails stable |
| VDDC (0.95V) | Decoupling caps on all pins | SerDes core, PLL — most noise-sensitive rail |
| VDDIO (1.8V) | Decoupling caps on all pins | I/O ring, sideband signals |
| GND | All ground balls | Solid unbroken ground plane required under BGA |

---

## 5. Complete Lane Allocation Summary

```
PI7C9X3G808GP — 8 Lanes Total
──────────────────────────────────────────────────
Port 0 (Upstream)   : Lanes 0–3  (x4) → Host PCIe x4 edge
Port 1 (Downstream) : Lanes 4–7  (x4) → PCI11400C upstream
                   OR Lanes 4–4  (x1) → PCI11400C (if x1 sufficient)
Port 2 (Downstream) : Lane  5    (x1) → LAN7430 (PCIe x1 endpoint)
──────────────────────────────────────────────────
Recommended mode strap: 4-port or 3-port configuration
```

> **Important note on REFCLK:** The 3G808GP has an integrated clock buffer that can distribute REFCLK to all downstream ports. You do **not** need an external ZL40262 clock buffer — unlike the PCI11400C EVB design. Simply connect the host-provided 100 MHz REFCLK to the 3G808GP's REFCLKP/N input, and route the on-chip REFCLKOP[n]/REFCKON[n] outputs to each downstream device (LAN7430 and PCI11400C).
