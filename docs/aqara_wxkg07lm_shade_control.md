# 🪟 Aqara Double Rocker Shade Control

**Version:** 1.1  
**Domain:** Automation  
**Author:** billchurch  

Control shades, blinds, curtains, or any cover entity using one rocker of an
Aqara WXKG07LM (D1 double rocker wireless remote switch) via Zigbee2MQTT.

## 📥 Installation

[![Open your Home Assistant instance and show the blueprint import dialog with a specific blueprint pre-filled.](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https%3A%2F%2Fraw.githubusercontent.com%2Fbillchurch%2Fha-blueprints%2Frefs%2Fheads%2Fmain%2Faqara_wxkg07lm_shade_control.yml)

Click the button above to import this blueprint directly into your Home Assistant instance.

## ✨ Features

- **One Rocker, One Cover**: Uses a single rocker so the other stays free for another automation
- **Rocker Selection**: Left, right, or both-pressed-together, chosen at setup
- **Universal Compatibility**: Works with any Home Assistant cover entity
- **Configurable Hold Position**: Pick the position the hold action applies

## 🎮 Button Actions

All actions apply to the rocker you select during setup.

| Action | Z2M Event | Function |
| --- | --- | --- |
| **Single Press** | `single_<rocker>` | Opens the shade/blind |
| **Double Press** | `double_<rocker>` | Closes the shade/blind |
| **Hold** | `hold_<rocker>` | Sets to configured position (default 100%) |

The WXKG07LM emits no release event, so this blueprint has no stop action —
unlike the IKEA Shortcut blueprint, whose stop came from the E1812's
`brightness_stop` release.

## 📋 Prerequisites

1. **Aqara WXKG07LM**: D1 double rocker wireless remote switch
2. **Zigbee2MQTT**: Paired and running, with the MQTT integration configured in
   Home Assistant
3. **Cover Entity**: A configured shade, blind, or cover entity
4. **Triggers discovered**: See the warning below — press each action once
   before creating the automation

## ⚠️ Discover the Triggers First

Zigbee2MQTT's Home Assistant guide states that *"the MQTT device triggers are
discovered by Zigbee2MQTT once the event is triggered on the device at least
once."*

That means Home Assistant does not know a trigger exists until you have used
it. **Before creating the automation**, on your chosen rocker:

1. Press it once (fires `single_<rocker>`)
2. Double-press it (fires `double_<rocker>`)
3. Press and hold it (fires `hold_<rocker>`)

If you skip this, the automation will fail to save with an "unknown device
trigger" style error. You can confirm discovery under **Settings → Devices &
Services → MQTT → your device**, where the actions appear in the automation
trigger list.

## ⚙️ Configuration

### Required Inputs

| Input | Description |
| --- | --- |
| **Aqara Double Rocker Button** | Your paired WXKG07LM, under its Z2M name |
| **Cover** | Choose the cover entity to control |

### Optional Inputs

| Input | Description | Default |
| --- | --- | --- |
| **Rocker** | Which rocker controls the cover: Left, Right, or Both | Left |
| **Hold Position** | Position to set when holding (0-100%) | 100% |

## 🛠️ Setup Instructions

1. **Pair Your Button**
   - Pair the WXKG07LM with Zigbee2MQTT
   - The button should appear as a device in Home Assistant

2. **Fire Each Action Once**
   - See the discovery warning above — do this before step 4

3. **Import the Blueprint**
   - Click the import button above
   - Or manually import via Settings → Automations & Scenes → Blueprints

4. **Create Automation**
   - Go to Settings → Automations & Scenes
   - Click "Create Automation" → "Use Blueprint"
   - Select "Aqara Double Rocker Shade Control v1.0"

5. **Configure**
   - Select your Aqara button from the device list
   - Choose the rocker and your cover entity
   - Save with a descriptive name

## 🔀 Using Both Rockers

This blueprint deliberately handles one rocker so the other stays available.
To control two covers from one remote, create **two automations** from this
blueprint — one with **Rocker: Left**, one with **Rocker: Right** — each
pointing at a different cover.

The **Both** option responds only when the two rockers are pressed together,
which Zigbee2MQTT reports as a distinct `*_both` event. That leaves the
individual left and right rockers untouched, so a third automation can use
them.

## ❓ Troubleshooting

### Device Not Appearing in the Picker

The picker lists **every** Aqara/Xiaomi device that arrived over MQTT, not just
WXKG07LM remotes. Look for whatever name you gave the remote in Zigbee2MQTT.

This is deliberate. Zigbee2MQTT does not put the `WXKG07LM` code in the field
HA's `model:` filter reads. Its MQTT discovery payload is built from the
device converter like so:

```text
manufacturer  <- converter vendor        "Aqara"
model         <- converter description   "Wireless remote switch D1 (double rocker)"
model_id      <- converter model         "WXKG07LM"
```

Older Zigbee2MQTT releases packed both into one string
(`"Wireless remote switch D1 (double rocker) (WXKG07LM)"`), and `model_id`
only exists on Zigbee2MQTT 1.39+ with Home Assistant 2024.8+. Filtering on
`model: WXKG07LM` matched none of those shapes, which is why blueprint v1.0
showed an empty picker. v1.1 filters on manufacturer alone so the remote is
listed on every combination of versions.

If the list is empty entirely, the remote is not paired with Zigbee2MQTT.
This blueprint is Zigbee2MQTT-only — ZHA and deCONZ use a different integration
and a different device-trigger scheme, so its triggers cannot match them.

### "Unknown Device Trigger" When Saving

The action has not been discovered yet. See the discovery warning above.

### Only Single Press Works

Double and hold events are separate MQTT device triggers and each needs its own
first activation. Fire all three on your chosen rocker, then re-save the
automation.

### Actions Not Working

- Confirm button battery level
- Test the cover entity manually in Developer Tools
- Check the automation trace — if the automation ran but did nothing, the
  `trigger.id` did not match the selected rocker, meaning you pressed the other
  one
- Verify the cover supports `cover.open_cover`, `cover.close_cover`, and
  `cover.set_cover_position`

## 💡 Tips

- **Position Presets**: Common hold positions are 100% (fully open), 50%
  (partial shade), 25% (privacy), 0% (fully closed)
- **Multiple Covers**: Create separate automations for each rocker/cover pair,
  or point the cover input at a cover group
- **Response Time**: Single press acts immediately; double press and hold carry
  the device's own detection delay

## 📚 Resources

- [Zigbee2MQTT WXKG07LM Device Page](https://www.zigbee2mqtt.io/devices/WXKG07LM.html)
- [Zigbee2MQTT Home Assistant Integration](https://www.zigbee2mqtt.io/guide/usage/integrations/home_assistant.html)
- [Home Assistant Cover Integration](https://www.home-assistant.io/integrations/cover/)
- [Blueprint Documentation](https://www.home-assistant.io/docs/blueprint/)

## 🐛 Support

For issues or questions:

1. Check the troubleshooting section above
2. Review automation traces for specific errors
3. Post in the [Community Forum](https://community.home-assistant.io/)
4. [Report issues on GitHub](https://github.com/billchurch/ha-blueprints/issues)
