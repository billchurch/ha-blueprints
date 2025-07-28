# 💨 Bath Fan Automation

**Version:** 1.0  
**Domain:** Automation  
**Author:** billchurch  

Smart bathroom exhaust fan control based on humidity levels with manual override support.

<img width="512" height="512" alt="image" src="https://github.com/user-attachments/assets/b0e096cb-31ed-4720-8656-5a1506f2934e" />

## 📥 Installation

[![Open your Home Assistant instance and show the blueprint import dialog with a specific blueprint pre-filled.](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https%3A%2F%2Fraw.githubusercontent.com%2Fbillchurch%2Fha-blueprints%2Frefs%2Fheads%2Fmain%2Fbath_fan.yml)

Click the button above to import this blueprint directly into your Home Assistant instance.

## ✨ Features

- **Automatic Humidity Control**: Fan activates when humidity exceeds threshold
- **Manual Override**: Direct control with configurable timer
- **Smart Comparison**: Monitors bathroom vs house humidity differential
- **Conflict Prevention**: Manual and automatic modes work seamlessly
- **Helper Integration**: Optional dashboard controls via input helpers
- **Flexible Configuration**: Customizable thresholds and timers

## 🔄 How It Works

1. **Humidity Monitoring**: Continuously compares bathroom and house humidity
2. **Smart Activation**: Fan turns on when humidity exceeds threshold + delta
3. **Manual Override**: Direct control temporarily disables automation
4. **Auto Resume**: Returns to automatic mode after override period

## 📋 Prerequisites

1. **Fan Switch**: A controllable switch entity for your exhaust fan
2. **Humidity Sensors**: 
   - Bathroom humidity sensor
   - House/reference humidity sensor
3. **Helpers** (optional but recommended):
   - Input Boolean for override tracking
   - Input Number for timer duration

## ⚙️ Configuration

### Required Inputs

| Input | Description |
|-------|-------------|
| **Fan Switch** | The switch entity controlling your exhaust fan |
| **Bathroom Humidity** | Humidity sensor in the bathroom |
| **Indoor Humidity** | Reference humidity sensor (main house) |

### Optional Inputs

| Input | Description | Default |
|-------|-------------|---------|
| **Manual Override Flag** | Input boolean to track override state | None |
| **Override Duration Helper** | Input number for adjustable timer | None |
| **Humidity On Threshold** | Trigger level for fan activation | 65% |
| **Humidity Off Threshold** | Level to turn fan off | 62% |
| **Humidity Delta** | Difference vs house humidity | 5% |
| **Manual Override Duration** | Default override time | 20 min |

## 🛠️ Setup Instructions

### Step 1: Create Helpers (Optional)

1. **Override Flag Helper**
   - Go to Settings → Devices & Services → Helpers
   - Click "Create Helper" → Toggle
   - Name: "Bath Fan Manual Override"

2. **Duration Helper**
   - Click "Create Helper" → Number
   - Name: "Bath Fan Override Duration"
   - Min: 5, Max: 60, Step: 1
   - Unit: minutes

### Step 2: Import Blueprint

1. Click the import button above
2. Or navigate to Settings → Automations & Scenes → Blueprints
3. Import blueprint and create new automation

### Step 3: Configure Automation

1. Select your fan switch entity
2. Choose bathroom and house humidity sensors
3. Link helpers if created
4. Adjust thresholds to your environment
5. Save with a descriptive name

## 💡 Example Configuration

```yaml
alias: "Master Bath Fan Control"
use_blueprint:
  path: billchurch/bath_fan.yml
  input:
    fan_switch: switch.master_bath_exhaust_fan
    bathroom_humidity: sensor.master_bath_humidity
    indoor_humidity: sensor.hallway_humidity
    manual_override_flag: input_boolean.master_bath_fan_override
    override_duration_helper: input_number.master_bath_override_duration
    humidity_on_threshold: 68
    humidity_off_threshold: 64
    humidity_delta: 8
```

## 📊 Dashboard Card Example

```yaml
type: entities
title: Bath Fan Control
entities:
  - entity: switch.bathroom_exhaust_fan
    name: Fan Switch
  - entity: sensor.bathroom_humidity
    name: Current Humidity
  - type: divider
  - entity: input_boolean.bath_fan_manual_override
    name: Manual Override Active
  - entity: input_number.bath_fan_override_duration
    name: Override Duration (min)
```

## 🎯 Usage Scenarios

### Automatic Operation
- Shower starts → Humidity rises → Fan activates automatically
- Shower ends → Humidity drops → Fan turns off when threshold met

### Manual Control
- Turn on fan directly → Runs for set duration
- Automation pauses during manual period
- Resumes automatic control after timer expires

### Multiple Bathrooms
Create unique helpers for each bathroom:
- `input_boolean.master_bath_fan_override`
- `input_boolean.guest_bath_fan_override`

## ❓ Troubleshooting

### Fan Not Activating
- Verify humidity sensors are reporting values
- Check threshold settings match your environment
- Ensure humidity delta is being exceeded
- Review automation traces for trigger details

### Manual Override Issues
- Confirm helper entities are properly linked
- Check helper entity states in Developer Tools
- Verify override timer configuration

### Frequent Cycling
- Increase gap between on/off thresholds
- Adjust humidity delta for your climate
- Consider longer minimum run times

## 🔍 Debugging

1. **Monitor Sensors**: Developer Tools → States
2. **Check Traces**: Automation → Traces tab
3. **Review History**: Check sensor graphs for patterns
4. **Test Manually**: Use Developer Tools → Services

## 📈 Optimization Tips

- **Winter**: May need lower thresholds due to dry air
- **Summer**: Higher thresholds for humid climates
- **Delta Tuning**: Start with 5% and adjust based on results
- **Timer Settings**: Longer in winter, shorter in summer

## 📚 Resources

- [Humidity Control Best Practices](https://www.home-assistant.io/docs/blueprint/)
- [Creating Input Helpers](https://www.home-assistant.io/docs/blueprint/input/)
- [Switch Integration](https://www.home-assistant.io/integrations/switch/)

## 🐛 Support

For issues or questions:
1. Review the troubleshooting section
2. Check automation traces for errors
3. Verify all entities are available
4. [Report issues on GitHub](https://github.com/billchurch/ha-blueprints/issues)
