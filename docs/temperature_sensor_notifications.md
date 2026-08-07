# 🌡️ Temperature Sensor Notifications

**Version:** 1.2
**Domain:** Automation
**Author:** billchurch

Comprehensive temperature monitoring with multi-channel
alerts including iOS notifications, TTS announcements,
and persistent warnings for high and low threshold
breaches.

## 📥 Installation

[![Open your Home Assistant instance and show the blueprint import dialog with a specific blueprint pre-filled.](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https%3A%2F%2Fraw.githubusercontent.com%2Fbillchurch%2Fha-blueprints%2Frefs%2Fheads%2Fmain%2Ftemperature_sensor_notifications.yml)

Click the button above to import this blueprint directly
into your Home Assistant instance.

## ✨ Features

- **Multi-Sensor Support**: Monitor unlimited temperature sensors
- **Dual Thresholds**: Independent high and low temperature triggers
- **Critical iOS Alerts**: Bypass Do Not Disturb and Focus modes
- **Text-to-Speech**: Announce temperature alerts through smart speakers
- **Persistent UI Alerts**: Dashboard notifications that stick
- **Directional Detection**: Knows whether temperature went too high or too low
- **Repeat Notifications**: Continue alerting until
  temperature returns to safe range
- **Action Buttons**: Quick acknowledge and view options
- **Location Aware**: Notifications include sensor location

## 🔔 Alert Channels

### Mobile Notifications

- **iOS**: Critical alerts with custom interruption levels
- **Android**: High priority sticky notifications
- **Actions**: Acknowledge and View Sensor buttons
- **Smart Navigation**: Tap to go to sensor location

### Text-to-Speech

- All Home Assistant TTS providers supported
- Multiple speaker targeting
- Customizable announcement messages
- Location-aware announcements

### UI Notifications

- Persistent dashboard alerts
- Unique IDs prevent duplicates
- Clear identification of affected sensors

## 📋 Prerequisites

1. **Temperature Sensors**: Sensors with `temperature` device class
2. **Mobile App**: Home Assistant companion app installed
3. **TTS** (optional): Configured TTS service
4. **Media Players** (optional): For announcements

## ⚙️ Configuration

### Required Inputs

| Input | Description |
| --- | --- |
| **Temperature Sensors** | Select all temperature sensors to monitor |
| **Notification Devices** | Mobile devices for alerts |

### Optional Inputs

| Input | Description | Default |
| --- | --- | --- |
| **Enable High Threshold** | Enable high temp alerts | No |
| **High Threshold** | Temp ceiling value | 80° |
| **Enable Low Threshold** | Enable low temp alerts | No |
| **Low Threshold** | Temp floor value | 32° |
| **Notification Title** | Alert title text | "Temperature Alert!" |
| **Notification Message** | Alert message template | Dynamic |
| **Enable Actions** | Add notification buttons | No |
| **Repeat Notifications** | Continue alerting while threshold exceeded | No |
| **Repeat Interval** | Minutes between repeats | 30 |
| **iOS Interruption Level** | Alert priority | Active |
| **Enable UI Notification** | Dashboard alerts | No |
| **Enable TTS** | Voice announcements | No |
| **TTS Service** | TTS provider | None |
| **TTS Media Players** | Speaker targets | None |
| **TTS Message** | Announcement text | Dynamic |

## 🛠️ Setup Instructions

### Step 1: Sensor Preparation

Ensure your temperature sensors have:

- Proper device class (`temperature`)
- Meaningful names
- Assigned areas/locations

### Step 2: Import Blueprint

1. Click the import button above
2. Review the blueprint configuration
3. Create new automation from blueprint

### Step 3: Basic Configuration

1. **Select Sensors**: Choose all temperature sensors
2. **Add Devices**: Select mobile devices for notifications
3. **Enable Thresholds**: Turn on high and/or low threshold monitoring
4. **Set Thresholds**: Configure temperature limits
5. **Save**: Give automation a descriptive name

### Step 4: Advanced Features (Optional)

#### Enable iOS Critical Alerts

1. Set interruption level to "Critical"
2. Grant critical alert permission in iOS settings

#### Configure TTS

1. Enable TTS in blueprint
2. Select TTS service provider
3. Choose media players
4. Customize announcement message

#### Setup Repeat Notifications

1. Enable repeat notifications
2. Set repeat interval
3. Notifications continue until temperature returns to safe range

## 💡 Example Configurations

### Basic Setup

```yaml
alias: "Temperature Alert - Basic"
use_blueprint:
  path: billchurch/temperature_sensor_notifications.yml
  input:
    temperature_sensors:
      - sensor.living_room_temperature
      - sensor.bedroom_temperature
    notify_devices:
      - abcd1234efgh5678
    enable_high_threshold: true
    high_threshold: 85
```

### Freeze Prevention

```yaml
alias: "Freeze Warning"
use_blueprint:
  path: billchurch/temperature_sensor_notifications.yml
  input:
    temperature_sensors:
      - sensor.garage_temperature
      - sensor.basement_temperature
      - sensor.attic_temperature
    notify_devices:
      - abcd1234efgh5678
    enable_low_threshold: true
    low_threshold: 35
    notification_title: "🥶 Freeze Warning!"
    interruption_level: critical
    repeat_enabled: true
    repeat_interval: 15
```

### Fridge/Freezer Monitoring

```yaml
alias: "Fridge & Freezer Monitor"
use_blueprint:
  path: billchurch/temperature_sensor_notifications.yml
  input:
    temperature_sensors:
      - sensor.refrigerator_temperature
      - sensor.freezer_temperature
    notify_devices:
      - abcd1234efgh5678
    enable_high_threshold: true
    high_threshold: 45
    notification_title: "🧊 Appliance Temperature Alert!"
    repeat_enabled: true
    repeat_interval: 10
    enable_ui_notification: true
```

### Full Featured Setup

```yaml
alias: "Temperature Alert - Advanced"
use_blueprint:
  path: billchurch/temperature_sensor_notifications.yml
  input:
    temperature_sensors:
      - sensor.living_room_temperature
      - sensor.garage_temperature
      - sensor.attic_temperature
      - sensor.basement_temperature
    notify_devices:
      - abcd1234efgh5678
      - ijkl9012mnop3456
    enable_high_threshold: true
    high_threshold: 90
    enable_low_threshold: true
    low_threshold: 32
    notification_title: "🌡️ TEMPERATURE ALERT!"
    interruption_level: critical
    enable_actions: true
    repeat_enabled: true
    repeat_interval: 10
    enable_ui_notification: true
    enable_tts: true
    tts_service: tts.google_cloud
    tts_targets:
      - media_player.kitchen_speaker
      - media_player.living_room_speaker
    tts_message: >-
      Alert! Temperature
      {{ 'above' if trigger.id == 'high' else 'below' }}
      threshold at {{ trigger.to_state.state }} degrees
      in {{ area_name(trigger.entity_id) }}!
```

## 📊 Dashboard Integration

### Temperature Alert Card

```yaml
type: entity-filter
title: Temperature Sensors
entities:
  - sensor.living_room_temperature
  - sensor.garage_temperature
  - sensor.attic_temperature
  - sensor.basement_temperature
state_filter:
  operator: ">"
  value: 80
show_empty: false
card:
  type: entities
  title: ⚠️ High Temperature
```

### Sensor Overview

```yaml
type: glance
title: Temperature Sensors
entities:
  - sensor.living_room_temperature
  - sensor.garage_temperature
  - sensor.attic_temperature
  - sensor.basement_temperature
show_state: true
```

## 🎯 Message Variables

Use these in notification and TTS messages:

- `{{ trigger.to_state.name }}` - Sensor friendly name
- `{{ trigger.entity_id }}` - Sensor entity ID
- `{{ trigger.to_state.state }}` - Current temperature value
- `{{ trigger.id }}` - Trigger direction (`high` or `low`)
- `{{ area_name(trigger.entity_id) }}` - Sensor location

## ❓ Troubleshooting

### No Notifications Received

- Verify mobile app is installed and logged in
- Check notification permissions in phone settings
- Ensure at least one threshold (high or low) is enabled
- Ensure sensors are properly reporting temperature values
- Review automation traces

### iOS Critical Alerts Not Working

1. iOS Settings → Home Assistant
2. Enable "Critical Alerts" permission
3. Test with interruption level set to "critical"

### TTS Not Playing

- Verify TTS service is configured
- Check media players are online
- Test TTS service in Developer Tools
- Ensure TTS is enabled in automation

### Sensors Not Triggering

- Check sensor states in Developer Tools
- Verify device_class is "temperature"
- Confirm thresholds are set to appropriate values for your unit system
- Review sensor history graphs

## 🔍 Testing

1. **Simulate Temperature Change**:
   - Developer Tools → States
   - Set sensor to a value above or below your threshold
   - Monitor automation traces

2. **Test Notifications**:
   - Use different interruption levels
   - Verify all devices receive alerts
   - Check action buttons work

3. **Test TTS**:
   - Manually trigger with test state
   - Verify all speakers announce
   - Check volume levels

## 📚 Resources

- [Mobile App Notifications](https://companion.home-assistant.io/docs/notifications/notifications-basic/)
- [iOS Critical Alerts](https://companion.home-assistant.io/docs/notifications/critical-notifications/)
- [TTS Configuration](https://www.home-assistant.io/integrations/#text-to-speech)
- [Temperature Sensor Docs](https://www.home-assistant.io/integrations/sensor/)

## 🐛 Support

For issues or questions:

1. Check the troubleshooting guide above
2. Review automation traces for errors
3. Verify entity availability
4. [Report issues on GitHub](https://github.com/billchurch/ha-blueprints/issues)
