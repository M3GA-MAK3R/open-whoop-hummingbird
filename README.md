# open-whoop-hummingbird

An open-source, high-performance 65mm FPV micro-whoop platform inspired by the OpenDrone.hw ecosystem. This research and development repository hosts the design schematics, layout source files, and firmware configuration targets to replicate and iterate on the performance benchmarks of the commercial 19.5g Hummingbird V4.1 BNF.

## 📁 Repository Structure

```text
├── .github/
│   └── workflows/          # Automated ERC/DRC check pipelines
├── hardware/
│   ├── components/         # Shared component libraries & footprints
│   ├── config-aio/         # Track A: Integrated 5-in-1 AIO Board Layouts
│   │   ├── schematic/      # EasyEDA / KiCad schematics (F411/RP2350 + 12A ESC)
│   │   └── Gerber/         # Production fabrication packets for JLCPCB
│   └── config-stacked/     # Track B: Modular Stacked Layouts
│       ├── openfc-micro/   # Sub-20mm scaled OpenFC Flight Controller
│       ├── openesc-micro/  # Dedicated standalone 1S micro 4-in-1 ESC
│       └── openrx-micro/   # Sub-miniature ExpressLRS target
├── firmware/
│   ├── betaflight/         # Custom CLI diffs and unified targets
│   └── bluejay/            # ESC ESC firmware configurations (48kHz PWM)
└── docs/
    ├── assembly/           # Stack spacing, weight budgets, and wiring diagrams
    └── bom/                # Shared Bill of Materials (LCSC / JLCPCB parts catalog)
```

---

## 🚀 R&D Flight Tracks

Team members can contribute to either development track depending on fabrication lead times and design constraints.

### Track A: Integrated 5-in-1 AIO
* **Objective:** Merge the MCU, 4-in-1 ESC, ExpressLRS RX, and Analog VTX onto a single 25.5mm x 25.5mm whoop-pattern PCB.
* **Target Weight:** Max 4.8g (populated PCB).
* **Fabrication Route:** Direct EasyEDA to JLCPCB PCBA automation utilizing the shared BOM catalog.
* **Design Rule Note:** Maintain strict trace clearance between the high-current ESC switching FETs and the sensitive 5.8GHz VTX RF traces to avoid video noise.

### Track B: Modular Micro-Stack
* **Objective:** Scale OpenDrone’s larger freestyle components into highly independent, vertically stacked structural layers.
* **Target Layout:** 16mm x 16mm or 20mm x 20mm mounting holes.
* **Assembly Constraint:** Wire routing must utilize lightweight 30AWG silicone wire. Direct solder all connections; eliminate plastic pin headers and JST motor plugs to avoid exceeding the strict weight threshold.

---

## 📊 Target Performance Benchmarks (Hummingbird V4.1 Equivalency)

All design revisions must be vetted against these physical metrics before sending to fabrication:

| Parameter | Specification / Threshold | R&D Testing Notes |
| :--- | :--- | :--- |
| **All-Up Weight (Dry)** | < 19.5 grams | Must keep hardware/frame configurations lean |
| **ESC Current Target** | 12A Burst (1S Input Only) | Sized for high-KV brushless motor transient response |
| **ESC PWM Frequency** | 48kHz (Bluejay or AM32) | Required to maximize micro motor efficiency |
| **RF Telemetry** | ExpressLRS 3.x (SPI or UART) | Built on OpenRX architecture layouts |
| **Video Transmitter** | 5.8GHz Analog (up to 400mW) | Pit-mode capable with smart audio controls |

---

## 🛠 Sourcing & Mechanical Hardware Catalog
To execute a successful bench build, combine the custom PCBs with the following standardized frame components:
* **Frame:** NewBeeDrone Cockroach V3 65mm Ultra-Light Polypropylene Frame.
* **Propulsion:** 0702 V2 Brushless Motors (30,000KV) with 1.0mm shafts.
* **Propellers:** NewBeeDrone Azi Tri-Blade 31mm Propellers.
* **Camera:** NewBeeDrone BeeEye Micro FPV Analog Camera (mounted via custom 3D-printed canopy).
* **Power Delivery:** Solid-pin BT2.0 or NX69 low-sag battery pigtails.

## 🤝 Contribution Guidelines
1. Always run local **Design Rule Checks (DRC)** and **Electrical Rule Checks (ERC)** before pushing to `main`.
2. Keep component layout additions tagged against the unified LCSC parts database located in `/docs/bom/` to ensure JLCPCB pick-and-place compatibility.
3. For software enhancements, submit clean Betaflight CLI diffs referencing specific hardware gyro/accelerometer alignments.
