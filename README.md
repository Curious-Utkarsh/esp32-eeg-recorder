# ESP32 EEG Recorder — BioAmp EXG Pill, Wi-Fi Dashboard, Python DSP

[![ESP32](https://img.shields.io/badge/MCU-ESP32-E7352C)](https://www.espressif.com/en/products/socs/esp32)
[![Python](https://img.shields.io/badge/Python-SciPy%20%7C%20NumPy-3776AB?logo=python)](https://scipy.org/)
[![WebSocket](https://img.shields.io/badge/stream-WebSocket-555)](#how-it-works)

A single-channel EEG/EXG recorder built from an Upside Down Labs **BioAmp EXG Pill** and an **ESP32** sampling at **250 Hz**. The ESP32 hosts its own Wi-Fi access point with a captive portal: connect a phone or laptop and a live dashboard opens — no router, internet or app needed — where you can watch the signal and record timed sessions to CSV. A Python pipeline adds filtering and frequency-band analysis.

Built in 2026.

## Key features

- **Standalone recorder** — ESP32 soft-AP + DNS captive portal + HTTP + WebSocket; live plot and CSV logging in the browser.
- **Wi-Fi-safe sampling** — input on GPIO39 (ADC1, unaffected by Wi-Fi), 12-bit, sampled by a high-priority task pinned to core 0.
- **v2 firmware with a written post-mortem** — the v1 dashboard froze because a blocking HTTP stream handler starved the event loop and a non-`download` link navigated away from the page. v2 removes the blocking endpoint, sends everything over one WebSocket and builds the CSV in the browser.
- **Signal processing (Python)** — 50 Hz notch + 1–40 Hz Butterworth band-pass, FFT every second, delta/theta/alpha/beta/gamma band powers and a beta/alpha "focus" ratio, raw samples logged for research use.

## How it works

```
BioAmp EXG Pill → ESP32 ADC (250 Hz) → soft-AP + captive portal → browser dashboard (live plot, CSV)
                                     └→ WebSocket → eeg.py (notch, band-pass, FFT, band powers)
```

## Files

| File | What it is |
|---|---|
| `esp32-eeg-standalone-ap-logger.ino` | v1 standalone recorder |
| `esp32-eeg-ap-dashboard-v2.ino` | v2 recorder, non-blocking, with the post-mortem in its header |
| `dashboard.html` | Browser dashboard |
| `eeg.py`, `requirements.txt` | Python filtering and band-power analysis |

## Run

1. Flash `esp32-eeg-ap-dashboard-v2.ino` (Arduino IDE, ESP32 core; WebSockets library).
2. Join the ESP32's Wi-Fi network; the dashboard opens automatically.
3. Optional analysis: `pip install -r requirements.txt && python eeg.py`

Note: `eeg.py` expects a WebSocket client firmware that is not in this repository; the standalone recorder works on its own.

## Related

[High-rate vibration DAQ](https://github.com/Curious-Utkarsh/Teensy_Esp) · [Wearable BLE telemetry](https://github.com/Curious-Utkarsh/Wearable-IOT) — other embedded work from the same period

---
[Kumar Utkarsh](https://github.com/Curious-Utkarsh) · [LinkedIn](https://www.linkedin.com/in/kumar-utkarsh-124aab227/)
