# ESPHOME Danfoss BD35 Controller

This system enables smart, efficient cooling control for a Danfoss BD35 compressor-based refrigerator,
integrating Home Assistant and an ESP32.

## Features

1. Hybrid Adaptive Cooling (Inside + Outside Temp-Based)
   - Adjusts compressor speed dynamically based on:
     - Fridge temperature relative to configurable thresholds
     - Ambient temperature compensation
   - Includes hysteresis protection:
     - Minimum 60-second delay between power changes
     - 5% minimum change threshold to prevent micro-adjustments
     - Safe fallback to 50% power if sensors fail

2. Configurable Temperature Thresholds
   - High temperature threshold (6-15°C)
   - Normal temperature threshold (3-8°C)
   - Low temperature threshold (0-5°C)
   - All adjustable via Home Assistant

3. Performance Monitoring
   - Compressor runtime tracking (hours)
   - Cycle counting
   - Current cooling power display (%)

4. Operating Modes
   - Turbo Mode (30-minute full power boost)
   - Manual Override Mode
   - Automatic fallback to adaptive cooling

5. Hardware Control
   - PWM Compressor Speed Control (ESP32 → BD35 "C" Terminal)
     - Dynamic speed control (2,000-3,500 RPM)
     - Minimum 12% duty cycle safety limit
   - Optocoupler-based ON/OFF Control (ESP32 → BD35 "T" Terminal)

6. Network Connectivity
   - Primary: Ethernet (LAN8720)
   - Fallback: WiFi with Access Point mode
   - Home Assistant integration with encryption

7. Power Optimization
   - Intelligent speed control based on cooling demand
   - Hysteresis protection prevents unnecessary cycling
   - Ambient temperature compensation

## Hardware Requirements

| Category | Component | Purpose |
|----------|-----------|---------|
| Main Controller | ESP32 Development Board | Controls cooling logic, runs ESPHome |
| Fridge Sensors | Ruuvi BLE Temp Sensor (Inside fridge) | Reads internal fridge temperature |
| | Ambient Temp Sensor (Ruuvi, DHT22, or BME280) | Reads outside temperature |
| Compressor Speed Control | ESP32 GPIO14 (PWM Output) → BD35 C Terminal | Controls compressor speed dynamically |
| | 100kΩ Resistor (BD35 C Terminal → GND) | Stabilizes PWM signal |
| Compressor ON/OFF Control | Optocoupler on GPIO5 → BD35 T Terminal | Isolated control for BD35 T Terminal |
