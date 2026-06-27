# LocalWeatherStation

> ** Project Status: In Development **

A compact, affordable, and portable weather station designed to collect real-time atmospheric data and provide hyperlocal weather predictions through a web dashboard. Built from scratch as a solo engineering project combining custom PCB design, 3D printed mechanical components, and a data-driven prediction platform.

<img width="767" height="997" alt="image" src="https://github.com/user-attachments/assets/3a06db81-0fcb-465f-92de-22e3ab3d9033" />
<img width="693" height="677" alt="image" src="https://github.com/user-attachments/assets/bdf0b80a-53eb-4dbd-8e90-f0e1d0d6e11f" />

---

##  Project Overview

OpenWeatherNode is a self-contained weather monitoring system housed in a custom-designed circular enclosure inspired by the traditional Stevenson Screen. It collects atmospheric data through multiple sensors and transmits readings to a web dashboard capable of short-term weather prediction (0–6 hours) based on pressure trend analysis.

This project sits at the intersection of **engineering physics** and **aerospace atmospheric sensing** — two fields where accurate, affordable, and deployable sensor systems are increasingly relevant.

---

##  Current Progress

- [x] Enclosure design — circular louvered Stevenson Screen style
- [x] BME280 sensor mount — suspended tripod platform with airflow exposure
- [x] Anemometer housing — dual bearing cup anemometer with rain roof
- [x] Full assembly concept — louvered enclosure, transition piece, anemometer on top
- [x] Cup anemometer arms and cups — adjusted design to compensate wind strength and replacements
- [x] Wind direction vane — in progress
- [ ] Rain gauge — planned
- [ ] PCB design in KiCad — in progress
- [ ] Breadboard prototype — planned
- [ ] Web dashboard — planned
- [ ] Weather prediction algorithm — planned
- [ ] Solar power integration — planned

---

## 🔩 Hardware

### Sensors
<img width="453" height="350" alt="image" src="https://github.com/user-attachments/assets/ccd955d1-3bc3-4fcb-b115-38a0c3cdf8c2" />
<img width="946" height="749" alt="image" src="https://github.com/user-attachments/assets/496d28f8-6b39-464c-b630-c9e934755678" />

| Sensor | Measurement | Interface |
|--------|-------------|-----------|
| BME280 | Temperature, Humidity, Pressure | I2C |
| VEML6075 | UV Index / Solar Radiation | I2C |
| Hall Effect Sensor | Wind Speed (pulse counting) | Digital |
| AS5600 | Wind Direction | I2C |
| HX711 + Load Cell | Rainfall (weight based gauge) | Digital |

### Processing
- **Prototyping** — Raspberry Pi 5 8GB
- **Final Design** — ESP32 bare chip on custom PCB

### Power
- Lithium battery with charge controller
- Solar panel — size TBD based on power budget analysis

---

##  Repository Structure

```
OpenWeatherNode/
│
├── CAD/
│   ├── enclosure/          # Main Stevenson Screen enclosure files
│   ├── anemometer/         # Cup anemometer assembly files
│   ├── sensor_mount/       # BME280 tripod suspension platform
│   ├── rain_gauge/         # Weight based rain gauge files
│   └── renders/            # Screenshots and render images
│
├── PCB/
│   ├── schematic/          # KiCad schematic files
│   ├── layout/             # PCB layout files
│   ├── gerbers/            # Manufacturing files for JLCPCB
│   └── BOM.csv             # Bill of materials
│
├── firmware/
│   ├── prototype/          # Raspberry Pi Python prototype code
│   │   ├── sensors/        # Individual sensor reading scripts
│   │   ├── logging/        # Data logging to CSV and database
│   │   └── main.py         # Main prototype entry point
│   └── production/         # ESP32 final firmware
│       └── src/            # Production source files
│
├── dashboard/
│   ├── grafana/            # Grafana dashboard config files
│   ├── node-red/           # Node-RED flow files
│   └── prediction/         # Weather prediction algorithm
│       └── pressure_trend.py
│
├── docs/
│   ├── design_decisions.md # Engineering decisions and reasoning
│   ├── calibration.md      # Sensor calibration methodology
│   ├── BOM.md              # Full bill of materials with pricing
│   └── references.md       # Sources and research references
│
└── README.md
```

---

##  Sensor Architecture

All PCB mounted sensors communicate via **I2C** on a shared bus:
- Minimizes wiring complexity
- Allows multiple sensors on two wires
- BME280, VEML6075, and AS5600 all I2C compatible

External mechanical sensors connect via digital GPIO:
- Hall effect sensor — pulse counting for wind speed
- HX711 load cell amplifier — weight based rainfall

---

##  Weather Prediction Approach

Predictions are based on **pressure trend analysis** over rolling time windows:

| Pressure Change | Timeframe | Prediction |
|----------------|-----------|------------|
| Drop > 3 hPa/hr | 1 hour | Storm warning |
| Drop > 1 hPa/hr | 3 hours | Rain likely |
| Rising steadily | 3-6 hours | Clearing conditions |
| Stable | — | Current conditions continuing |

Prediction window: **0–6 hours** (nowcasting)

---

##  Enclosure Design

- **Shape** — Circular, inspired by Stevenson Screen
- **Material** — ASA filament (UV and temperature resistant)
- **Ventilation** — Angled louvered sides at 30 degrees
- **Sensor isolation** — BME280 on suspended tripod platform
- **Thermal management** — PCB sealed in upper chamber with active fan ventilation
- **Dimensions** — Approximately 17 x 17 x 18 cm main enclosure

---

##  Data Transmission

- **Prototyping** — Raspberry Pi WiFi to local network
- **Final design** — ESP32 WiFi with future LoRa capability planned

---

##  Roadmap

**Phase 1 — Mechanical (Current)**
- Finalize all CAD designs
- 3D print and assemble prototype enclosure
- Test structural integrity

**Phase 2 — Electronics**
- Breadboard prototype with all sensors on Raspberry Pi
- Validate all sensor readings
- Design custom PCB in KiCad
- Order PCB from JLCPCB

**Phase 3 — Software**
- Python sensor reading scripts
- Data logging pipeline
- Grafana dashboard setup
- Pressure trend prediction algorithm

**Phase 4 — Integration**
- Full system assembly
- Calibration against Environment Canada reference data
- Solar power integration
- Outdoor deployment testing

**Phase 5 — Future**
- Weather Underground API submission
- CWOP citizen science network integration
- LoRa mesh networking capability

---

##  References & Acknowledgements

- Bosch BME280 Datasheet
- WMO Weather Station Standards
- Stevenson Screen Design Principles — modern-physics.org
- Murata Ultrasonic Transducer Documentation

---

##  License

This project is open source under the MIT License. Feel free to use, modify, and build upon this work with attribution.

---

*Solo project by a McMaster Engineering student. Built from scratch — hardware, firmware, and software.*
