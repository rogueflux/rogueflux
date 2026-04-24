Also, I notice you uploaded the **PI7C9X2G608GP** datasheet — that's the Gen2 predecessor in the same family. Let me give you a full rundown on the Gen3 variant you're asking about.

---

## PI7C9X3G808GP — Full Overview

Made by **Diodes Incorporated** (formerly Pericom Semiconductor), this is a high-performance PCIe Gen3 packet switch.

---

### Core Identity

The PI7C9X3G808GP is a PCIe Gen3 packet switch that supports 8 lanes of Gen3 SERDES in flexible 2-port, 3-port, 4-port, 5-port, and 8-port configurations. The architecture allows flexible port configuration by allocating variable lane widths for each port. It can be configured to have different port types such as upstream port, downstream ports, and Cross-Domain End-Point (CDEP) ports to support applications including port fan-out and dual-host connectivity. Multiple DMA channels are embedded to facilitate data communication more efficiently among hosts and endpoints. [Diodes Incorporated](https://www.diodes.com/part/view/PI7C9X3G808GP)

---

### Key Features

Cut-through and store-and-forward modes can be utilized with a packet forwarding latency of less than 150ns typical. Under full load conditions and 80°C junction temperature, the PI7C9X3G808GP draws only 2.9W of power. [Business Wire](https://www.businesswire.com/news/home/20210331005039/en/8-Port-PCIe-3.0-Packet-Switch-from-Diodes-Incorporated-Provides-Design-Flexibility-and-Power-Savings)

The device supports PCIe low-power link states and sub-states, and can turn off any empty hot-pluggable ports until they are needed. It features a unique built-in PCIe 3.0 clock buffer supporting common, separate-reference no-spread (SRNS), and separate-reference independent-spread (SRIS) clock architectures. It is specified over the industrial temperature range of -40°C to 85°C. [Diodes Incorporated](https://www.diodes.com/design/support/technical-articles/advanced-pcie-features-for-power-conscious-and-mission-critical-applications)

The switch supports provisions for surprise hot removal of a connected device, downstream port containment (DPC) for handling uncorrectable errors, and hot-plug support for upstream/downstream ports with serial/parallel hot-plug types. [Diodes Incorporated](https://www.diodes.com/design/support/technical-articles/advanced-pcie-features-for-power-conscious-and-mission-critical-applications)

---

### Package & Pricing

The PI7C9X3G808GP is supplied in a high-performance flip-chip package with a 196-ball BGA format measuring 15mm × 15mm. [Business Wire](https://www.businesswire.com/news/home/20210331005039/en/8-Port-PCIe-3.0-Packet-Switch-from-Diodes-Incorporated-Provides-Design-Flexibility-and-Power-Savings)

---

### Relation to Your Uploaded Document (PI7C9X2G608GP)

The two devices are from the same Pericom/Diodes family and share a very similar register map and pin philosophy:

| Feature | PI7C9X**2G608**GP (your PDF) | PI7C9X**3G808**GP |
|---------|-------------------------------|-------------------|
| PCIe Gen | Gen 2 (5 GT/s) | Gen 3 (8 GT/s) |
| Lanes | 8 | 8 |
| Max ports | 6 | 8 |
| Package | FC-BGA | HFCBGA-196 (15×15 mm) |
| Power (full load) | ~2W | 2.9W |
| CDEP support | Yes | Yes |
| Clock buffer | External needed | Built-in |

The Gen3 variant adds the integrated REFCLK buffer (no external ZL40262 needed unlike in the PCI11400C EVB), higher throughput per lane, and two additional configurable ports.

---

### Port Configuration Options

```
8 lanes can be split as:

2-port:  [x4 UP] + [x4 DOWN]
3-port:  [x4 UP] + [x2 DOWN] + [x2 DOWN]
4-port:  [x4 UP] + [x2 DOWN] + [x1 DOWN] + [x1 DOWN]
5-port:  [x4 UP] + [x1 DOWN] × 4
8-port:  [x1 UP] + [x1 DOWN] × 7  (all single-lane)
```

For your specific use case (PCIe x4 upstream + Ethernet + USB + PCIe x1 downstream), a **4-port or 5-port configuration** of the 3G808GP would be a viable alternative to the PCI11400C — though you'd still need a separate USB host controller (unlike PCI11400C which integrates xHCI).

---

### Power Rails Required

| Rail | Voltage | Purpose |
|------|---------|---------|
| VDDC | 0.95V | PCIe SerDes core, PLL |
| VDDIO | 1.8V | I/O ring, sideband signals |
| VDDA | 1.8V | Analog/PLL supply |

These are the two primary rails (0.95V and 1.8V), which is why power is measured under the conditions of 0.95V/1.8V with PCIe3 devices usage on all downstream ports and full data traffic operation on all endpoints. [Diodes Incorporated](https://www.diodes.com/assets/product-showcases/8-Port-PCIe-3.0-Packet-Switch-Provides-Design-Flexibility-and-Power-Savings.pdf)

Suggested regulators: TPS62130 (0.95V, 3A) for VDDC and MIC23201 (1.8V) for VDDIO/VDDA — both need tight load transient response given Gen3 SerDes sensitivity.

---

### Compared to PCI11400C for Your Design

If you want to use the 3G808GP instead of PCI11400C as your PCIe switch, the tradeoff is:

- **Pro:** True Gen3 switch, more port flexibility, lower forwarding latency, built-in REFCLK buffer
- **Con:** No integrated USB host controller (need separate xHCI IC like ASM3142 or Renesas uPD720202), no integrated UART/GPIO, more complex BGA routing (196-ball, 0.8mm pitch)

For your stated design (PCIe x4 in → Ethernet + USB3 + USB2 + PCIe x1 out), the **PCI11400C remains the simpler choice** since it integrates the xHCI controller. The 3G808GP shines in pure PCIe fan-out applications like NAS, storage arrays, or multi-GPU boards.
