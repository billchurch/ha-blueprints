# 🪟 IKEA Shortcut Button Shade Control

**Version:** 1.1  
**Domain:** Automation  
**Author:** billchurch  

Control shades, blinds, curtains, or any cover entity using an IKEA Shortcut button (model E1812).

<img width="384" height="384" alt="shortcutbutton-med" src="https://github.com/user-attachments/assets/61d3abea-7a66-48d9-9371-a77b1144095a" />

## 📥 Installation

[![Open your Home Assistant instance and show the blueprint import dialog with a specific blueprint pre-filled.](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https%3A%2F%2Fraw.githubusercontent.com%2Fbillchurch%2Fha-blueprints%2Frefs%2Fheads%2Fmain%2Fikea_shortcut_shade_control.yml)

Click the button above to import this blueprint directly into your Home Assistant instance.

## ✨ Features

- **Multi-Integration Support**: Works with Zigbee2MQTT, ZHA, and deCONZ
- **Simple Control**: One button controls all shade functions
- **Universal Compatibility**: Works with any Home Assistant cover entity
- **No Configuration Required**: Fixed actions for consistent operation

## 🎮 Button Actions

| Action | Function | Description |
|--------|----------|-------------|
| **Single Press** | Open Cover | Opens the shade/blind |
| **Double Press** | Close Cover | Closes the shade/blind |
| **Long Press** | Set Position | Sets to configured position (default 100%) |
| **Hold Release** | Stop | Stops cover movement (can be disabled) |

## 📋 Prerequisites

1. **IKEA Shortcut Button**: Model E1812 (TRÅDFRI shortcut button)
2. **Zigbee Integration**: One of the following configured and working:
   - Zigbee2MQTT
   - ZHA (Zigbee Home Automation)
   - deCONZ
3. **Cover Entity**: A configured shade, blind, or cover entity in Home Assistant

## ⚙️ Configuration

### Required Inputs

| Input | Description |
|-------|-------------|
| **IKEA Shortcut Button** | Select your paired E1812 button device |
| **Cover** | Choose the cover entity to control |

### Optional Inputs

| Input | Description | Default |
|-------|-------------|---------|
| **Long Press Position** | Position to set when long pressing (0-100%) | 100% |
| **Ignore Release Action** | Disable stop on button release | Off |

## 🛠️ Setup Instructions

1. **Pair Your Button**
   - Follow your Zigbee integration's pairing process
   - The button should appear as a device in Home Assistant

2. **Import the Blueprint**
   - Click the import button above
   - Or manually import via Settings → Automations & Scenes → Blueprints

3. **Create Automation**
   - Go to Settings → Automations & Scenes
   - Click "Create Automation" → "Use Blueprint"
   - Select "IKEA Shortcut Button Shade Control v1.1"

4. **Configure**
   - Select your IKEA button from the device list
   - Choose your cover entity
   - Save with a descriptive name

## 🔍 Integration-Specific Notes

### Zigbee2MQTT

- Button appears as an MQTT device
- Actions are received as MQTT device triggers
- Ensure MQTT integration is properly configured

### ZHA

- Button appears as a ZHA device
- Uses native device triggers
- May show as "TRADFRI SHORTCUT Button"

### deCONZ

- Requires ConBee/RaspBee coordinator
- Button appears as a deCONZ device
- Actions mapped to remote button events

## ❓ Troubleshooting

### Button Not Appearing

- Ensure button is properly paired
- Check your Zigbee integration logs
- Try re-pairing the button
- Verify integration is running

### Actions Not Working

- Confirm button battery level
- Test cover entity manually
- Check automation traces for errors
- Verify correct integration selected

### Cover Not Responding

- Test cover controls in Developer Tools
- Ensure cover supports required services:
  - `cover.open_cover`
  - `cover.close_cover`
  - `cover.set_cover_position`
  - `cover.stop_cover`

## 💡 Tips

- **Battery Life**: IKEA buttons have excellent battery life (2+ years typical)
- **Response Time**: Actions trigger immediately on button press
- **Multiple Buttons**: Create separate automations for each button/cover pair
- **Custom Positions**: Set your preferred position for long press (e.g., 50% for partial shade)
- **Hold Behavior**: Enable "Ignore Release Action" to set position without manual stop

## 🔧 Advanced Usage

### Position Presets

The long press position is now configurable during blueprint setup. Common uses:

- **100%**: Fully open (default)
- **50%**: Half open for partial shade
- **25%**: Privacy position
- **0%**: Fully closed

### Hold-to-Position Mode

When "Ignore Release Action" is enabled:

- Long press sets the cover to your preset position
- No need to hold until the cover reaches position
- Release action is ignored (won't stop the cover)
- Useful for consistent positioning without timing

### Multiple Covers

Create separate automations for each button/cover combination, or modify the automation to control cover groups.

## 📚 Resources

- [IKEA TRÅDFRI Documentation](https://www.ikea.com/us/en/p/tradfri-shortcut-button-white-20356382/)
- [Home Assistant Cover Integration](https://www.home-assistant.io/integrations/cover/)
- [Blueprint Documentation](https://www.home-assistant.io/docs/blueprint/)

## 🐛 Support

For issues or questions:

1. Check the troubleshooting section above
2. Review automation traces for specific errors
3. Post in the [Community Forum](https://community.home-assistant.io/)
4. [Report issues on GitHub](https://github.com/billchurch/ha-blueprints/issues)
