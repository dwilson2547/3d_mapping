# Bill of Materials

Prices are approximate USD at time of writing. Sourcing notes included where relevant.

---

## Sensors

| Item | Model | Qty | Unit Price | Total | Notes |
|---|---|---|---|---|---|
| LiDAR | Livox Horizon | 1 | — | — | Already owned |
| Primary camera | FLIR Blackfly S BFS-U3-120S4C | 1 | $500 | $500 | 12.3MP, Sony IMX253, global reset, USB3 |
| Depth + IMU | [Intel RealSense D435i](https://www.amazon.com/dp/B07MWR2YJB?ref=ppx_yo2ov_dt_b_fed_asin_title) | 1 | $380 | $380 | Active IR stereo + BMI055 IMU, factory calibrated |
| Flanking cameras | [Arducam OV9281 USB (global shutter, hardware trigger variant)](https://www.amazon.com/dp/B096M5DKY6?ref=ppx_yo2ov_dt_b_fed_asin_title&th=1) | 2 | $55 | $110 | Confirm hardware trigger pin on listing before ordering |
| Supplemental IMU | [Witmotion WT901C](https://www.amazon.com/dp/B07YYTD9GR?ref=ppx_yo2ov_dt_b_fed_asin_title) | 1 | $60 | $60 | USB or UART, ROS2 driver [available](https://witmotion-sensor.com/products/9-axis-inclinometer-high-performance-acceleration-wt901c-compass-triaxial-mpu9250-tilt-sensor) |

**Sensor subtotal: ~$1,050**

---

## Optics

| Item | Qty | Unit Price | Total | Notes |
|---|---|---|---|---|
| M12 lens 6mm (matched set for OV9281s) | 2 | $20 | $40 | Buy matched pair from same batch for consistent distortion |
| C-mount lens for FLIR (6–12mm prime) | 1 | $80–150 | $80–150 | Computar or Kowa recommended |

**Optics subtotal: ~$120–190**

---

## Structure & Mechanical

| Item | Qty | Unit Price | Total | Notes |
|---|---|---|---|---|
| CF round tube 12mm OD / 10mm ID | ~400mm | — | — | Already ordered — handle |
| CF square tube 12mm OD / 10mm ID | ~300mm total | — | — | Already ordered — camera arms |
| Structural epoxy | 1 | $25–40 | $25–40 | Loctite EA 9492 or 3M DP420 |
| Self-amalgamating tape | 1 roll | $8 | $8 | For plug fit adjustment |
| M3 hex socket cap screws assortment | 1 | $12 | $12 | For split clamps |
| M4 hex socket cap screws assortment | 1 | $12 | $12 | For Livox bracket |
| M3 heat-set nut-serts | 50 | $8 | $8 | For printed parts |
| M4 heat-set nut-serts | 20 | $6 | $6 | For printed parts |

**Mechanical subtotal: ~$70–90**

---

## Electrical & Data

| Item | Qty | Unit Price | Total | Notes |
|---|---|---|---|---|
| Powered USB 3.0 hub (7-port industrial) | 1 | $40 | $40 | Pluggable or Monoprice industrial series |
| ESP32 dev board | 1 | $10 | $10 | Camera sync pulse controller |
| USB-A to USB-C cables (0.5m) | 4 | $8 | $32 | Short runs from sensors to hub |
| USB-C to USB-A cable (1m) | 1 | $8 | $8 | Hub to workstation |
| Ethernet cable (1m) | 1 | $6 | $6 | Livox Horizon to workstation |
| Velcro cable ties | 1 pack | $8 | $8 | Cable management on rig |

**Electrical subtotal: ~$104**

---

## Consumables & Printing

| Item | Qty | Unit Price | Total | Notes |
|---|---|---|---|---|
| ASA filament | 1kg | $28 | $28 | Final structural prints |
| PLA filament | 0.5kg | $12 | $12 | Test prints only |
| 220 grit sandpaper | 1 sheet | $2 | $2 | CF surface prep before bonding |
| Isopropyl alcohol 99% | 250ml | $8 | $8 | Surface prep before bonding |
| N95 respirator | 1 | $5 | $5 | Required when cutting/drilling CF |
| Nitrile gloves | 1 box | $10 | $10 | Epoxy work |

**Consumables subtotal: ~$65**

---

## Calibration Targets

| Item | Qty | Unit Price | Total | Notes |
|---|---|---|---|---|
| ChArUco board (printed A3, laminated) | 1 | $5 | $5 | Camera intrinsic + extrinsic calibration |
| AprilTag grid target (known dimensions) | 1 | $5 | $5 | Scale reference and LiDAR/camera extrinsic calibration |

**Calibration subtotal: ~$10**

---

## Total Estimated Cost

| Category | Subtotal |
|---|---|
| Sensors | ~$1,050 |
| Optics | ~$120–190 |
| Structure & Mechanical | ~$70–90 |
| Electrical & Data | ~$104 |
| Consumables & Printing | ~$65 |
| Calibration | ~$10 |
| **Total** | **~$1,420–1,510** |

Excludes the Livox Horizon (already owned), the CF tubing (already ordered), and the workstation/GPU.

---

## Future Upgrades

| Item | Purpose | Approx Cost |
|---|---|---|
| FLIR Blackfly S ×2 (additional) | Replace OV9281 flanking cameras | ~$1,000 |
| Zivid Two M130 | Close-range structured light depth | ~$15–20k |
| Photoneo PhoXi 3D Scanner S | Dedicated small object / PCB scanning station | ~$20k |
| u-blox F9P + antenna | RTK GPS for outdoor metric scale | ~$250 |
| Structured light IR projector | Active texture on featureless surfaces | ~$80–200 |
| Polarizing filter (FLIR C-mount) | Reduce specular highlights on shiny surfaces | ~$30 |
| High CRI LED ring (5600K, dimmable) | Controlled illumination for small object scanning | ~$60–120 |