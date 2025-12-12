# 📡 ESP32 Mini Dev Board
### Custom Hardware & PCB Design

A compact, USB-C powered ESP32 development board designed from scratch using KiCad. Features USB-UART programming, auto-boot circuitry, onboard 3.3V regulation, ESD protection, and optimized RF layout following Espressif design guidelines.

**Purpose:** Learning embedded hardware design, portfolio demonstration, and real-world PCB manufacturing.

---

## 🚀 Key Features

### 🔌 Power & USB Interface
- **USB-C connector** with proper CC pull-down resistors
- **Polyfuse** (500mA) for over-current protection
- **CH224K** USB-C power negotiation IC (5V sink)
- **Dual USB ESD protection** using SOT-23-6 TVS diode array
- **AP2112K-3.3** LDO regulator with input/output filtering

### 🖥️ Programming & Debug
- **CP2102N** USB-UART bridge (no external FTDI needed)
- **Auto-reset circuitry** using dual NPN transistors
- **One-click firmware upload** via USB

### 📡 MCU Core
- **ESP32-WROOM-32E** module with integrated antenna
- RF keep-out zone and antenna clearance maintained
- Complete decoupling network per Espressif guidelines
- **EN pin pull-up** and **IO0 boot switch**

### 🎛️ User Controls
- Reset button
- Boot/Flash button  
- Power LED indicator

---

## 📁 Repository Structure

```
├── hardware/
│   ├── esp32-mini.kicad_pro       # KiCad project
│   ├── esp32-mini.kicad_sch       # Schematic
│   ├── esp32-mini.kicad_pcb       # PCB layout
│   ├── gerbers/                   # Manufacturing files
│   ├── bom.csv                    # Bill of materials
│   └── renders/                   # 3D visualizations
├── docs/
│   ├── design-notes.md            # Design decisions
│   ├── esp32-reference.pdf        # Espressif guidelines
│   └── usb-c-integration.md       # USB-C implementation notes
└── README.md
```

---

## 🧩 System Architecture

```
┌─────────┐    ┌──────────┐    ┌─────────┐    ┌──────┐    ┌────────────┐
│ USB-C   │───▶│ Polyfuse │───▶│   TVS   │───▶│CH224K│───▶│ AP2112-3.3 │───▶ 3.3V
└─────────┘    └──────────┘    └─────────┘    └──────┘    └────────────┘
     │                                                              │
     └──────▶ CP2102N ───▶ Auto-reset Circuit ───▶ ESP32 ◀────────┘
                               (Q1/Q2)              (TX/RX/EN/IO0)
```

---

## 🛠️ Design Tools

- **KiCad 8.x** — Schematic capture & PCB layout
- **Espressif ESP32 Hardware Design Guidelines** — RF best practices
- **Silicon Labs CP2102N Reference Design** — USB-UART implementation
- **CH224K Application Notes** — USB-C power delivery
- **Manufacturing:** JLCPCB, OSH Park, PCBWay

---

## 🧱 Circuit Breakdown

### 1️⃣ Power Stage
- USB-C VBUS → Polyfuse → CH224K (5V sink)
- AP2112-3.3 LDO with 4.7µF input/output capacitors
- Power LED with current-limiting resistor

### 2️⃣ USB-UART Bridge
- CP2102N (QFN-20 package)
- 5.1kΩ CC resistors for USB-C default power
- TVS diode for D+/D– line protection
- Differential pair routing maintained

### 3️⃣ ESP32 Core
- Decoupling capacitors positioned directly under module pads
- 10kΩ pull-up on EN (enable) pin
- Boot button: IO0 → GND (momentary)
- Reset button: EN → GND (momentary)

### 4️⃣ Auto-Reset Circuit
- Dual NPN transistor configuration (Q1/Q2)
- Resistor network (R6–R10) for DTR/RTS control
- Replicates ESP32 DevKitC auto-programming behavior

---

## 📐 PCB Layout Highlights

✅ **15mm RF keep-out area** in front of ESP32 antenna  
✅ **Short differential routing** for USB D+/D–  
✅ **Ground plane stitching** with perimeter vias  
✅ **Proximity placement:** LDO caps near regulator pins  
✅ **Auto-reset transistors** placed under CP2102N for compact routing  
✅ **ESD protection** at USB connector entry point  

---

## 📦 Manufacturing Guide

### Step 1: Generate Gerbers
1. Open `esp32-mini.kicad_pro` in KiCad
2. Navigate to **File → Plot**
3. Select **Gerber** format and plot all layers
4. Generate **drill files**

### Step 2: Order PCBs
Upload Gerber ZIP to:
- **JLCPCB** (economical, fast turnaround)
- **OSH Park** (premium quality)
- **PCBWay** (flexible options)

**Recommended specs:**
- 2-layer PCB
- 1.6mm thickness
- HASL or ENIG finish
- 1oz copper

### Step 3: Component Assembly
- Review `bom.csv` for parts list
- Use solder paste + hotplate/hot air station for QFN packages
- Hand-solder through-hole components (buttons, USB-C)

---

## 🔌 Programming & Usage

### Initial Setup
1. Connect board via USB-C cable
2. CP2102N enumerates as a virtual COM port
3. Install CH340/CP210x drivers if needed (usually automatic)

### Firmware Upload
**Using Arduino IDE:**
```cpp
// Select: Tools → Board → ESP32 Dev Module
// Select: Tools → Port → COMx (your port)
// Click Upload
```

**Using ESP-IDF:**
```bash
idf.py flash monitor
```

### Manual Flash Mode
If auto-reset fails:
1. Hold **BOOT** button
2. Press **RESET** button
3. Release **RESET**, then release **BOOT**
4. Upload firmware

---

## 📸 Gallery

*Include your images here:*

| Schematic | PCB Layout | 3D Render |
|-----------|------------|-----------|
| ![](hardware/renders/schematic.png) | ![](hardware/renders/layout.png) | ![](hardware/renders/3d.png) |

---

## 🔮 Future Enhancements

- [ ] **Battery charging circuit** (TP4056 or MCP73831)
- [ ] **Onboard RGB LED** with WS2812B
- [ ] **Qwiic/STEMMA connector** for I²C peripherals
- [ ] **4-layer PCB** for improved RF performance
- [ ] **LiPo fuel gauge** (MAX17048)
- [ ] **USB-C PD trigger** for 9V/12V operation

---

## 🏆 License

**MIT License**  
Feel free to modify, redistribute, and use this design in personal or commercial projects.

---

## ❤️ Acknowledgments

Special thanks to:
- **Espressif Systems** — Comprehensive ESP32 documentation
- **Silicon Labs** — CP2102N reference circuits
- **WCH** — CH224K USB-C sink IC application notes
- **KiCad Development Team** — Best open-source EDA software

---

## 📬 Contact & Contributions

Found a bug? Have an improvement? Open an issue or submit a pull request!

**Project maintained by:** [Your Name]  
**Email:** your.email@example.com  
**GitHub:** [@yourusername](https://github.com/yourusername)

---

<div align="center">

**⭐ Star this repo if you found it useful! ⭐**

Made with ❤️ and lots of ☕

</div>
