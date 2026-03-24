# 🍓 Smart Greenhouse — IoT Controller (Raspberry Pi)

<div align="center">

![Python](https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white)
![Raspberry Pi](https://img.shields.io/badge/Raspberry%20Pi-A22846?style=for-the-badge&logo=raspberry-pi&logoColor=white)
![InfluxDB](https://img.shields.io/badge/InfluxDB-22ADF6?style=for-the-badge&logo=influxdb&logoColor=white)
![Firebase](https://img.shields.io/badge/Firebase-FFCA28?style=for-the-badge&logo=firebase&logoColor=black)

Part of the [🌿 Smart Greenhouse System](https://github.com/Wassef-Ben-Rgaya/smart-greenhouse)

</div>

---

## 📖 Description

Python scripts running on **Raspberry Pi 4** for reading environmental sensors, controlling actuators via GPIO relays, storing time-series data in InfluxDB, and syncing to Firebase cloud. This is the core IoT layer of the Smart Greenhouse System.

---

## ⚙️ Hardware Managed

| Sensor / Actuator | Interface | Purpose |
|-------------------|-----------|---------|
| DHT22 | GPIO (1-wire) | Temperature & air humidity |
| TSL2561 | I2C | Light intensity (lux) |
| Soil moisture sensor | ADC | Irrigation decision making |
| DS3231 RTC | I2C | Real-time clock synchronization |
| 6-Relay module | GPIO | Switch all actuators |
| Water pump | Relay 1 | Automated irrigation |
| 30W LED grow light | Relay 2 | Plant lighting |
| Fan #1 | Relay 3 | Ventilation |
| Fan #2 | Relay 4 | Ventilation |
| Thermostat fan | Relay 5–6 | Heating control |

---

## 📁 Project Structure

```
smart-greenhouse-iot/
├── sensors/             # Sensor reading modules
│   ├── dht22.py         # Temperature & humidity
│   ├── tsl2561.py       # Light intensity
│   └── soil.py          # Soil moisture
├── actuators/           # Actuator control modules
│   ├── pump.py          # Water pump
│   ├── led.py           # LED grow light
│   └── fans.py          # Fans & heater
├── database/            # Data storage
│   ├── influxdb.py      # Local InfluxDB writes
│   └── firebase.py      # Cloud Firebase sync
├── config.py            # Thresholds & settings
├── main.py              # Main control loop
├── requirements.txt     # Python dependencies
└── .gitignore
```

---

## 🚀 Installation

### Prerequisites
- Raspberry Pi 4 (Raspberry Pi OS)
- Python >= 3.8
- InfluxDB installed locally
- Firebase credentials

### Steps

```bash
# 1. Clone on your Raspberry Pi
git clone https://github.com/Wassef-Ben-Rgaya/smart-greenhouse-iot.git
cd smart-greenhouse-iot

# 2. Install dependencies
pip install -r requirements.txt

# 3. Configure settings
cp config.example.py config.py
# Edit config.py with your thresholds and credentials

# 4. Run manually
python main.py

# 5. Run as system service (auto-start on boot)
sudo cp greenhouse.service /etc/systemd/system/
sudo systemctl enable greenhouse.service
sudo systemctl start greenhouse.service
```

---

## 📊 Data Pipeline

```
Sensors (every 1 second)
    │
    ├──► InfluxDB (local storage — always on)
    │
    ├──► Firebase Realtime DB (cloud sync — every 5 min)
    │
    └──► Local CSV backup (if offline)
              └──► Auto-sync on reconnect
```

---

## 🌡️ Automation Logic

```python
# Threshold configuration example (config.py)
THRESHOLDS = {
    "temperature": {"min": 15, "max": 25},    # °C
    "humidity":    {"min": 50, "max": 80},    # %
    "light":       {"min": 500, "max": 3000}, # lux
    "soil":        {"min": 30, "max": 70},    # %
}

# If temperature > max → activate fans
# If temperature < min → activate heater
# If soil < min        → activate pump
# If light < min       → activate LED
```

---

## 🔗 Related Repositories

| Repo | Description |
|------|-------------|
| [smart-greenhouse-backend](https://github.com/Wassef-Ben-Rgaya/smart-greenhouse-backend) | Node.js REST API |
| [smart-greenhouse-mobile](https://github.com/Wassef-Ben-Rgaya/smart-greenhouse-mobile) | Flutter mobile app |
| [smart-greenhouse-plant-prediction](https://github.com/Wassef-Ben-Rgaya/smart-greenhouse-plant-prediction) | AI/ML models & Flask API |

---

## 👨‍💻 Author

**Wassef BEN RGAYA** — [LinkedIn](https://www.linkedin.com/in/wassef-ben-rgaya-600817188) · [GitHub](https://github.com/Wassef-Ben-Rgaya)

© 2025 — Polytech Tunis Final Year Project
