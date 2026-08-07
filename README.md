# Home Assistant Blueprints

A collection of automation blueprints for Home Assistant
to simplify common smart home scenarios.

## 📋 Available Blueprints

| Blueprint | Version | Description | Quick Install | Documentation |
| --- | --- | --- | --- | --- |
| **🎛️ Lutron Pico Fan Control** | v1.0 | Control ceiling fans and lights with a Lutron Pico 5-button remote. Features single press fan control and long press light dimming. | [![Install](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https%3A%2F%2Fraw.githubusercontent.com%2Fbillchurch%2Fha-blueprints%2Frefs%2Fheads%2Fmain%2Fpico_fan_5_simple.yml) | [View Docs](docs/pico_fan_control.md) |
| **💨 Bath Fan Automation** | v1.0 | Smart bathroom fan control based on humidity levels with manual override support. Automatically manages ventilation to prevent moisture buildup. | [![Install](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https%3A%2F%2Fraw.githubusercontent.com%2Fbillchurch%2Fha-blueprints%2Frefs%2Fheads%2Fmain%2Fbath_fan.yml) | [View Docs](docs/bath_fan_automation.md) |
| **💧 Moisture Sensor Notifications** | v1.2 | Critical water leak detection system with multi-channel alerts including iOS notifications, TTS announcements, and persistent UI warnings. | [![Install](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https%3A%2F%2Fraw.githubusercontent.com%2Fbillchurch%2Fha-blueprints%2Frefs%2Fheads%2Fmain%2Fmoisture_sensor_notifications.yml) | [View Docs](docs/moisture_sensor_notifications.md) |
| **🪟 IKEA Shortcut Shade Control** | v1.0 | Control shades, blinds, or any cover with an IKEA Shortcut button. Works with Zigbee2MQTT, ZHA, and deCONZ integrations. | [![Install](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https%3A%2F%2Fraw.githubusercontent.com%2Fbillchurch%2Fha-blueprints%2Frefs%2Fheads%2Fmain%2Fikea_shortcut_shade_control.yml) | [View Docs](docs/ikea_shortcut_shade_control.md) |
| **🪟 Aqara Double Rocker Shade Control** | v1.0 | Control shades, blinds, or any cover with one rocker of an Aqara WXKG07LM double rocker remote via Zigbee2MQTT. Leaves the other rocker free for a separate automation. | [![Install](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https%3A%2F%2Fraw.githubusercontent.com%2Fbillchurch%2Fha-blueprints%2Frefs%2Fheads%2Fmain%2Faqara_wxkg07lm_shade_control.yml) | [View Docs](docs/aqara_wxkg07lm_shade_control.md) |
| **🌡️ Temperature Sensor Notifications** | v1.0 | Temperature threshold monitoring with multi-channel alerts including iOS notifications, TTS announcements, and persistent UI warnings. Configurable repeat alerts while threshold is exceeded. | [![Install](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https%3A%2F%2Fraw.githubusercontent.com%2Fbillchurch%2Fha-blueprints%2Frefs%2Fheads%2Fmain%2Ftemperature_sensor_notifications.yml) | [View Docs](docs/temperature_sensor_notifications.md) |

## 🚀 Quick Start

1. Click on any documentation link above to learn more about each blueprint
2. Use the **Import Blueprint** button in each doc to add it to your Home Assistant
3. Configure the automation using the detailed setup instructions provided

## 🤝 Contributing

Feel free to submit issues, fork the repository,
and create pull requests for any improvements.

## 📄 License

These blueprints are provided as-is for use with Home
Assistant. Feel free to modify and adapt them to your
needs.

## 🔗 Resources

- [Home Assistant Blueprint Documentation](https://www.home-assistant.io/docs/blueprint/)
- [Home Assistant Community Forum](https://community.home-assistant.io/)
- [Report Issues](https://github.com/billchurch/ha-blueprints/issues)
