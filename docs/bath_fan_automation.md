# 💨 Bath Fan Automation

**Version:** 2.4  
**Domain:** Automation  
**Author:** billchurch  

Smart bathroom exhaust fan control based on humidity levels with manual override support.

<img width="512" height="512" alt="image" src="https://github.com/user-attachments/assets/b0e096cb-31ed-4720-8656-5a1506f2934e" />

## 📥 Installation

[![Open your Home Assistant instance and show the blueprint import dialog with a specific blueprint pre-filled.](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https%3A%2F%2Fraw.githubusercontent.com%2Fbillchurch%2Fha-blueprints%2Frefs%2Fheads%2Fmain%2Fbath_fan.yml)

Click the button above to import this blueprint directly into your Home Assistant instance.

## ⚠️ Upgrading from v1.x

**v2.0 is a breaking change.** The manual override is now tracked by a **Timer**
helper instead of an input boolean plus an in-automation `delay`.

Three bugs drove the rewrite:

- The automation could not tell a manual press apart from *its own* turn-on.
  When humidity turned the fan on, that counted as a manual override, blocking
  the automatic turn-off for the full override period.
- `mode: restart` meant any humidity reading aborted an in-flight override.
- The override lived in a `delay`, so a Home Assistant restart dropped it and
  left the fan running with nothing scheduled to turn it off.

To migrate:

1. Create a Timer helper (Settings → Devices & Services → Helpers → Create
   Helper → Timer). Any name works; leave its duration at `0:00:00`, since the
   blueprint sets the duration when it starts the timer.
2. Re-import the blueprint and edit your automation.
3. Select the new timer in the **Manual Override Timer** field.
4. Leave **Manual Override Flag** alone. It is now ignored, and is kept only so
   automations created with v1.x still load after re-import. Once every
   automation using this blueprint is migrated, you can delete the input
   boolean helper it pointed at.

## ✨ Features

- **Automatic Humidity Control**: Fan activates when humidity exceeds threshold
- **Manual Override**: Timer-backed, and survives Home Assistant restarts
- **True Manual Detection**: Uses the event context to distinguish a real
  button press or UI tap from the automation's own turn-on
- **Smart Comparison**: Monitors bathroom vs house humidity differential
- **Conflict Prevention**: Manual and automatic modes work seamlessly
- **Flexible Configuration**: Customizable thresholds and timers
- **Optional Run-On**: Keep the fan running a few extra minutes after humidity
  clears, instead of stopping the moment the threshold is crossed
- **Stale Sensor Guard** (v2.3): When the override timer ends, the fan turns
  off if the humidity reading is unavailable, not a number, or older than the
  Stale Reading Limit, so a dead sensor can't leave the fan running
- **Relative Turn-Off** (v2.4): The fan turns off once the bathroom is within
  the Humidity Off Delta of the house, or below the Humidity Off Threshold,
  whichever comes first. On a humid day the whole house can sit above the off
  threshold; before v2.4 the fan then ran until someone turned it off
- **Relative Turn-On** (v2.4): The fan also turns on whenever the bathroom
  rises more than the Humidity Delta above the house, even if it never
  crosses the Humidity On Threshold. Before v2.4 it turned on only at that
  crossing, so a bathroom already above the threshold, or one that reached
  the delta a little later, left the fan off. A manual override still blocks
  it, and a stale bathroom reading can't start it this way

## 🔄 How It Works

1. **Humidity Monitoring**: Continuously compares bathroom and house humidity
2. **Smart Activation**: Fan turns on when the bathroom crosses the on
   threshold while more than the delta above the house, or (v2.4) whenever it
   rises more than the delta above the house, threshold or not
3. **Manual Override**: Turning the fan on by hand starts the override timer,
   which suspends humidity control while it runs
4. **Early Cancel**: Turning the fan off by hand cancels the override
   immediately, so humidity control resumes right away
5. **Auto Resume**: When the timer finishes, the fan turns off if humidity has
   recovered; otherwise humidity control simply takes over again.
   "Recovered" means below the Humidity Off Threshold, or less than the
   Humidity Off Delta above the house, whichever comes first. The same rule
   applies when humidity drops and when the run-on countdown starts.
6. **Optional Run-On**: If a Fan Run-On Timer is configured, the fan keeps
   running for the configured duration after humidity clears, instead of
   stopping immediately. A humidity rebound or a manual turn-off during the
   countdown cancels it.

The override state is `timer.your_timer == 'active'` — one source of truth you
can inspect in Developer Tools → States at any time. The run-on timer works
the same way.

## 📋 Prerequisites

1. **Fan Switch**: A controllable switch entity for your exhaust fan
2. **Humidity Sensors**:
   - Bathroom humidity sensor
   - House/reference humidity sensor
3. **Timer Helper** (required): tracks the manual override
4. **Input Number** (optional): user-adjustable override duration

## ⚙️ Configuration

### Required Inputs

| Input | Description |
| --- | --- |
| **Fan Switch** | The switch entity controlling your exhaust fan |
| **Bathroom Humidity** | Humidity sensor in the bathroom |
| **Indoor Humidity** | Reference humidity sensor (main house) |
| **Manual Override Timer** | Timer helper tracking the manual override |

### Optional Inputs

| Input | Description | Default |
| --- | --- | --- |
| **Humidity On Threshold** | Trigger level for fan activation | 65% |
| **Humidity Off Threshold** | Level to turn fan off | 62% |
| **Humidity Delta** | The fan turns on once the bathroom is more than this far above the house, threshold or not | 5% |
| **Humidity Off Delta** | The fan also turns off once the bathroom is less than this far above the house; 0 turns the rule off | 2% |
| **Manual Override Duration** | Default override time | 20 min |
| **Override Duration Helper** | Input number for adjustable timer; overrides the above | None |
| **Manual Override Flag** | Deprecated and ignored — see the upgrade notes | None |
| **Fan Run-On Timer** | Timer helper; when set, enables the run-on behavior below | None |
| **Run-On Duration** | Minutes to keep the fan running after humidity clears, when the timer above is set | 10 min |
| **Stale Reading Limit** | A humidity reading older than this counts as stale: the fan turns off when the override timer ends, and the reading can't turn the fan on through the delta rule | 180 min |

## 🛠️ Setup Instructions

### Step 1: Create Helpers

1. **Override Timer** (required)
   - Go to Settings → Devices & Services → Helpers
   - Click "Create Helper" → Timer
   - Name: "Bath Fan Manual Override"
   - Duration: leave at `0:00:00` — the blueprint sets it at runtime

2. **Duration Helper** (optional)
   - Click "Create Helper" → Number
   - Name: "Bath Fan Override Duration"
   - Min: 5, Max: 60, Step: 1
   - Unit: minutes

3. **Run-On Timer** (optional)
   - Go to Settings → Devices & Services → Helpers
   - Click "Create Helper" → Timer
   - Name: "Bath Fan Run-On"
   - Duration: leave at `0:00:00` — the blueprint sets it at runtime
   - Select it as the **Fan Run-On Timer** input. Leave it unset to keep the
     fan turning off immediately when humidity clears.

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
    override_timer: timer.master_bath_fan_override
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
  - entity: timer.bath_fan_manual_override
    name: Manual Override
  - entity: input_number.bath_fan_override_duration
    name: Override Duration (min)
  - entity: timer.bath_fan_run_on
    name: Run-On Timer
```

## 🎯 Usage Scenarios

### Automatic Operation
- Shower starts → Humidity rises past the on threshold, or more than the
  delta above the house → Fan activates automatically
- Shower ends → Humidity drops below the off threshold, or to within the off
  delta of the house → Fan turns off (or after
  the run-on duration, if a Fan Run-On Timer is configured)

### Manual Control
- Turn on fan directly → Override timer starts, fan runs for the set duration
- Humidity control is suspended while the timer is active
- Turn the fan off by hand → Override is cancelled, humidity control resumes
- Timer finishes → Fan turns off if humidity has recovered

### Multiple Bathrooms
Create a separate timer for each bathroom:
- `timer.master_bath_fan_override`
- `timer.guest_bath_fan_override`

## ❓ Troubleshooting

### Fan Not Activating
- Verify humidity sensors are reporting values
- Check threshold settings match your environment
- Ensure humidity delta is being exceeded
- If the bathroom is humid but the fan stays off, check the bathroom sensor
  has reported within the Stale Reading Limit; an old reading can't turn the
  fan on through the delta rule
- Before v2.4 the fan turned on only when the bathroom crossed the on
  threshold, so a room already above it stayed off. Re-import the blueprint
- Review automation traces for trigger details

### Manual Override Issues
- Check the timer entity in Developer Tools → States. `active` means an
  override is in effect; `idle` means humidity control is running
- If the override never starts, confirm the fan is being switched by hand or
  from the UI. Changes made by *another* automation are deliberately not
  treated as manual and will not start an override
- If the override never ends, check that the timer's `remaining` is counting
  down and that `timer.finished` appears in Developer Tools → Events
- If the fan turns off at the end of an override while the room is still
  humid, the sensor may be reporting less often than the Stale Reading Limit.
  Raise the limit above its longest gap between reports

### Fan Runs for Hours on Humid Days
- Before v2.4 the fan turned off only below the Humidity Off Threshold. When
  the whole house is above it, re-import the blueprint to get the Humidity Off
  Delta rule, and check the delta isn't set to 0

### Frequent Cycling
- Keep the Humidity Off Delta well below the Humidity Delta
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
