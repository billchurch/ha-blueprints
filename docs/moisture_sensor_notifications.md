# 💧 Moisture Sensor Notifications

**Version:** 1.2  
**Domain:** Automation  
**Author:** billchurch  

Comprehensive water leak detection with multi-channel critical alerts including iOS notifications, TTS announcements, and persistent warnings.

<img width="512" height="512" alt="image" src="https://github.com/user-attachments/assets/70c21669-444d-4108-84b1-5dcf8a767dbb" />

## 📥 Installation

[![Open your Home Assistant instance and show the blueprint import dialog with a specific blueprint pre-filled.](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https%3A%2F%2Fraw.githubusercontent.com%2Fbillchurch%2Fha-blueprints%2Frefs%2Fheads%2Fmain%2Fmoisture_sensor_notifications.yml)

Click the button above to import this blueprint directly into your Home Assistant instance.

## ✨ Features

- **Multi-Sensor Support**: Monitor unlimited moisture/leak sensors
- **Critical iOS Alerts**: Bypass Do Not Disturb and Focus modes
- **Text-to-Speech**: Announce leaks through smart speakers
- **Persistent UI Alerts**: Dashboard notifications that stick
- **Smart Detection**: Works with binary and numeric sensors
- **Repeat Notifications**: Continue alerting until resolved
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

1. **Moisture Sensors**: Binary or numeric water leak sensors
2. **Mobile App**: Home Assistant companion app installed
3. **TTS** (optional): Configured TTS service
4. **Media Players** (optional): For announcements

## ⚙️ Configuration

### Required Inputs

| Input | Description |
|-------|-------------|
| **Moisture Sensors** | Select all sensors to monitor |
| **Notification Devices** | Mobile devices for alerts |

### Optional Inputs

| Input | Description | Default |
|-------|-------------|---------|
| **Notification Title** | Alert title text | "Moisture Detected!" |
| **Notification Message** | Alert message template | Dynamic with location |
| **Moisture Threshold** | Numeric sensor trigger % | 0 (any moisture) |
| **Enable Actions** | Add notification buttons | No |
| **Repeat Notifications** | Continue alerting | No |
| **Repeat Interval** | Minutes between repeats | 30 |
| **iOS Interruption Level** | Alert priority | Active |
| **Enable UI Notification** | Dashboard alerts | No |
| **Enable TTS** | Voice announcements | No |
| **TTS Service** | TTS provider | None |
| **TTS Media Players** | Speaker targets | None |
| **TTS Message** | Announcement text | Dynamic |

## 🛠️ Setup Instructions

### Step 1: Sensor Preparation

Ensure your moisture sensors have:
- Proper device class (`moisture`)
- Meaningful names
- Assigned areas/locations

### Step 2: Import Blueprint

1. Click the import button above
2. Review the blueprint configuration
3. Create new automation from blueprint

### Step 3: Basic Configuration

1. **Select Sensors**: Choose all moisture sensors
2. **Add Devices**: Select mobile devices for notifications
3. **Set Title**: Customize notification title
4. **Save**: Give automation a descriptive name

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
3. Notifications continue until moisture cleared

## 💡 Example Configurations

### Basic Setup
```yaml
alias: "Water Leak Detection - Basic"
use_blueprint:
  path: billchurch/moisture_sensor_notifications.yml
  input:
    moisture_sensors:
      - binary_sensor.kitchen_sink_leak
      - binary_sensor.washing_machine_leak
    notify_devices:
      - abcd1234efgh5678
```

### Full Featured Setup
```yaml
alias: "Water Leak Detection - Advanced"
use_blueprint:
  path: billchurch/moisture_sensor_notifications.yml
  input:
    moisture_sensors:
      - binary_sensor.kitchen_sink_leak
      - binary_sensor.bathroom_leak
      - binary_sensor.water_heater_leak
      - binary_sensor.washing_machine_leak
    notify_devices:
      - abcd1234efgh5678
      - ijkl9012mnop3456
    notification_title: "🚨 WATER LEAK ALERT!"
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
    tts_message: "Urgent! Water detected in {{ area_name(trigger.entity_id) }}!"
```

## 📊 Dashboard Integration

### Leak Status Card
```yaml
type: entity-filter
title: Water Leak Sensors
entities:
  - binary_sensor.kitchen_sink_leak
  - binary_sensor.bathroom_leak
  - binary_sensor.water_heater_leak
state_filter:
  - "on"
  - problem
show_empty: false
card:
  type: entities
  title: ⚠️ Active Leaks
```

### Sensor Overview
```yaml
type: glance
title: Moisture Sensors
entities:
  - binary_sensor.kitchen_sink_leak
  - binary_sensor.bathroom_leak
  - binary_sensor.water_heater_leak
  - binary_sensor.washing_machine_leak
show_state: true
```

## 🎯 Message Variables

Use these in notification and TTS messages:
- `{{ trigger.to_state.name }}` - Sensor friendly name
- `{{ trigger.entity_id }}` - Sensor entity ID
- `{{ trigger.to_state.state }}` - Current state
- `{{ area_name(trigger.entity_id) }}` - Sensor location

## ❓ Troubleshooting

### No Notifications Received
- Verify mobile app is installed and logged in
- Check notification permissions in phone settings
- Ensure sensors are properly triggering
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
- Verify device_class is "moisture"
- Test sensors with water
- Review sensor history graphs

## 🔍 Testing

1. **Simulate Leak**: 
   - Developer Tools → States
   - Set sensor to "on" or "wet"
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
- [Binary Sensor Docs](https://www.home-assistant.io/integrations/binary_sensor/)

## 🐛 Support

For issues or questions:
1. Check the troubleshooting guide above
2. Review automation traces for errors
3. Verify entity availability
4. [Report issues on GitHub](https://github.com/billchurch/ha-blueprints/issues)
