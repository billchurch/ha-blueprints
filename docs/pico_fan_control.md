# 🎛️ Lutron Pico Fan & Light Control

**Version:** 1.1  
**Domain:** Automation  
**Author:** billchurch  

Control ceiling fans and optional lights using a Lutron Pico 5-button remote (model PJ2-3BRL-GXX-F01).

![Pico Remote](https://github.com/user-attachments/assets/cf32fe42-f5b9-4c31-a494-aec0eef93773)

## 📥 Installation

[![Open your Home Assistant instance and show the blueprint import dialog with a specific blueprint pre-filled.](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https%3A%2F%2Fraw.githubusercontent.com%2Fbillchurch%2Fha-blueprints%2Frefs%2Fheads%2Fmain%2Fpico_fan_5_simple.yml)

Click the button above to import this blueprint directly into your Home Assistant instance.

## ✨ Features

- **Single Press Controls**: Control fan speed with dedicated buttons
- **Long Press Controls**: Continuous light dimming (300ms+ hold)
- **Smart Toggle**: Middle button toggles light or fan based on configuration
- **Customizable Settings**: Adjust brightness steps and dimming rate
- **Speed Cycling**: Intelligent fan speed control with up/down buttons

## 🎮 Button Layout

| Button | Single Press | Long Press |
|--------|-------------|------------|
| **ON** | Turn fan on | - |
| **UP** | Increase fan speed | Brighten light continuously |
| **MIDDLE** | Toggle light (or fan if no light) | - |
| **DOWN** | Decrease fan speed | Dim light continuously |
| **OFF** | Turn fan off | - |

## 📋 Prerequisites

1. **Lutron Caseta Integration**: Must be installed and configured
2. **Pico Remote**: Model PJ2-3BRL-GXX-F01 paired with your Caseta hub
3. **Fan Entity**: A controllable fan entity in Home Assistant
4. **Light Entity** (optional): For dimming functionality

## ⚙️ Configuration

### Required Inputs

| Input | Description |
|-------|-------------|
| **Lutron Pico** | Select your paired Pico remote device |
| **Fan** | Choose the fan entity to control |

### Optional Inputs

| Input | Description | Default |
|-------|-------------|---------|
| **Light** | Light entity for dimming control | None |
| **Brightness Step** | Percentage change per dimming step | 10% |
| **Dimming Rate** | Speed of continuous dimming | 200ms |

## 🛠️ Setup Instructions

1. **Import the Blueprint**
   - Click the import button above
   - Or manually import via Settings → Automations & Scenes → Blueprints

2. **Create Automation**
   - Go to Settings → Automations & Scenes
   - Click "Create Automation" → "Use Blueprint"
   - Select "Pico Fan Simple 5-Button with Long Press v1.1"

3. **Configure Inputs**
   - Select your Pico remote from the device list
   - Choose your fan entity
   - Optionally select a light entity
   - Adjust brightness and dimming settings

4. **Save & Test**
   - Give your automation a descriptive name
   - Save and test each button function

## 💡 Example Configuration

```yaml
alias: "Bedroom Fan Control"
use_blueprint:
  path: billchurch/pico_fan_5_simple.yml
  input:
    pico_remote: 5f1fee7421ad0638ec5fca2c0ab451b7
    fan_entity: fan.bedroom_ceiling_fan
    light_entity: light.bedroom_fan_light
    brightness_step: 15
    dimming_rate: 150
```

## 📊 Dashboard Card Example

```yaml
type: vertical-stack
cards:
  - type: tile
    entity: fan.bedroom_ceiling_fan
    features:
      - type: fan-speed
  - type: light
    entity: light.bedroom_fan_light
    features:
      - type: brightness
```

## 🔧 Advanced Configuration

### Adjusting Long Press Threshold

The default 300ms threshold can be modified in the blueprint YAML:

```yaml
timeout: "00:00:00.3"  # Change to desired duration
```

### Brightness Limits

- **Maximum**: Stops at ~250 brightness
- **Minimum**: Stops at ~5 brightness

Modify these values in the UP/DOWN button conditions if needed.

## ❓ Troubleshooting

### Remote Not Working
- Verify Pico is paired with Caseta hub
- Check device ID in automation traces
- Ensure Lutron integration is functioning

### Light Dimming Issues
- Confirm light supports brightness control
- Check light entity attributes in Developer Tools
- Verify light entity is properly selected

### Fan Speed Problems
- Ensure fan supports speed control
- Check fan.increase_speed/decrease_speed service availability
- Review automation traces for errors

## 🔍 Debugging

1. **Check Entity States**: Developer Tools → States
2. **Review Traces**: Settings → Automations → [Your Automation] → Traces
3. **Monitor Logs**: Settings → System → Logs

## 📚 Resources

- [Community Forum Thread](https://community.home-assistant.io/t/pico-fan-simple-5-button-remote-for-lutron-caseta-haiku-or-any-fan/901507)
- [Lutron Caseta Integration](https://www.home-assistant.io/integrations/lutron_caseta/)
- [Blueprint Documentation](https://www.home-assistant.io/docs/blueprint/)

## 🐛 Support

For issues or questions:
1. Check the troubleshooting section above
2. Review automation traces for specific errors
3. Post in the [Community Forum](https://community.home-assistant.io/)
4. [Report issues on GitHub](https://github.com/billchurch/ha-blueprints/issues)