# Smart Battery Management System — Design Package
**Version:** 1.0 | **Date:** 2026 | **Classification:** Engineering Reference

## Package Contents

```
BMS_Design_Package/
├── README.md                          ← This file
├── Schematic/
│   ├── BMS_Schematic_Rev1.0.pdf       ← Full schematic (PDF, print-ready)
│   └── BMS_Schematic_Rev1.0.png       ← Schematic image (high-res)
├── PCB_Layout/
│   ├── BMS_PCB_Layout_Rev1.0.pdf      ← PCB layout top+bottom views (PDF)
│   └── BMS_PCB_Layout_Rev1.0.png      ← PCB layout image (high-res)
├── Gerbers/
│   ├── Top_Copper/    BMS-F_Cu.gbr    ← L1: Signal + Component pads
│   ├── Inner_Layers/  BMS-In1_Cu.gbr  ← L2: Ground plane
│   │                  BMS-In2_Cu.gbr  ← L3: Power distribution
│   ├── Bottom_Copper/ BMS-B_Cu.gbr    ← L4: High-current traces
│   ├── Silkscreen/    BMS-F_SilkS.gbr ← Component labels
│   │                  BMS-B_SilkS.gbr
│   ├── Soldermask/    BMS-F_Mask.gbr  ← Solder mask openings
│   │                  BMS-B_Mask.gbr
│   ├── Paste/         BMS-F_Paste.gbr ← Stencil openings (SMD pads)
│   └── Drill/         BMS-PTH.drl     ← Plated through-hole drill file
│                      BMS-NPTH.drl    ← Non-plated drill (mounting holes)
├── BOM/
│   └── BMS_BOM_Rev1.0.csv             ← Full Bill of Materials (CSV)
├── Netlist/
│   └── BMS_Netlist.net                ← Netlist (KiCad format)
└── KiCad_Project/
    ├── BMS.kicad_pro                  ← KiCad project file
    ├── BMS.kicad_sch                  ← KiCad schematic stub
    └── BMS.kicad_pcb                  ← KiCad PCB stub
```

## PCB Specifications

| Parameter | Value |
|-----------|-------|
| Board dimensions | 160 mm × 110 mm |
| Layer count | 4 |
| Substrate | FR4, Tg 150°C |
| Board thickness | 1.6 mm |
| Copper weights | L1: 1 oz / L2: 1 oz / L3: 2 oz / L4: 3 oz |
| Min trace/space | 0.15 mm / 0.15 mm (signal), 8 mm (power) |
| Min drill | 0.3 mm (vias), 3.2 mm (mounting holes) |
| Surface finish | ENIG (Electroless Nickel Immersion Gold) |
| Solder mask | Green LPI, both sides |
| Silkscreen | White epoxy, both sides |
| Impedance control | 50 Ω (CAN differential pair) |
| Max working voltage | 60 V |
| Max continuous current | 70 A (main path, L4 traces) |

## Design Rules Summary

### High-Current Path (L4 Bottom Copper - 3oz)
- Main power traces: 8 mm wide minimum
- Current rating: ≥85 A at 20°C rise (IPC-2221)
- Via stitching: 20× vias minimum through all layers for current transitions

### Signal Path (L1 Top Copper - 1oz)  
- Signal traces: 0.2 mm standard, 0.15 mm minimum
- CAN differential pair: 0.25 mm, matched length ±0.5 mm
- I2C: 0.2 mm, with 100 pF distributed capacitance limit

### Thermal Management
- MOSFET (Q1-Q8): Thermal vias to L3 copper pour, 4 mm grid, 0.8 mm drill
- AFE IC (U1): Exposed pad soldered to L2 GND via, 6× 0.5 mm vias
- LDO (U7): Thermal slug on bottom, stitched to GND pour

## Ordering Instructions (JLCPCB / PCBWay)

1. Upload Gerbers ZIP → Select **4-layer**
2. Dimensions: 160 × 110 mm
3. Quantity: 5–10 (prototype)
4. Layer stackup: JLC04161H-7628 (or equivalent)
5. Cu thickness: Specify 1/1/2/3 oz (non-standard — confirm with fab)
6. Surface finish: **ENIG**
7. Solder mask: Green
8. Request impedance test coupon (50 Ω)

## Critical Notes

⚠️ **High voltage warning**: Pack voltage up to 26.1V (6S), max 60V transient
⚠️ **High current**: 70A continuous, 300A short-circuit. Use proper fusing.
⚠️ **ESD sensitive**: U1, U2, U3 are ESD sensitive. Handle with ESD precautions.
⚠️ **Pre-charge**: Always use pre-charge circuit before connecting capacitive loads.
