# Hassalarm Wake-Up Light integration for Home Assistant

a complete Home Assistant integration that uses the [Hassalarm Android app](https://github.com/Johboh/hassalarm) to trigger a flashing light automation when your phone alarm goes off - but only when you're actually at home!

## 📋 table of contents

- [overview](#overview)
- [features](#features)
- [requirements](#requirements)
- [installation](#installation)
- [configuration](#configuration)
- [how it works](#how-it-works)
- [troubleshooting](#troubleshooting)
- [credits](#credits)

---

## 🎯 overview

this project creates an intelligent wake-up system that:
- automatically syncs your Android phone alarms with Home Assistant
- triggers a light flashing sequence when the alarm goes off
- only activates when you're at home (detected via WiFi presence)
- provides a dashboard toggle to enable/disable the automation
- shows visual status indicators (green/orange/red) based on your location and settings

**perfect for:**
- people who need a gentle (or not-so-gentle) wake-up with light
- smart home enthusiasts who want their alarms integrated with Home Assistant
- anyone who wants location-aware alarm automations

---

## ✨ features

- **automatic Alarm sync**: your phone alarms automatically sync to Home Assistant via Hassalarm
- **Smart Light Flashing**: light turns OFF/ON every 2 seconds for 60 seconds when alarm triggers
- **presence detection**: only triggers if your phone is connected to home WiFi
- **Dashboard control**: beautiful button card to enable/disable the automation
- **visual status**:
  - 🟢 **green**: you're home & alarm is enabled
  - 🟠 **orange**: alarm enabled but you're away
  - 🔴 **red**: alarm is disabled
- **next Alarm display**: shows when your next alarm is scheduled

---

## 📦 requirements

### hardware/software
- **Home Assistant** 2020.12.1 or later
- **Android phone** with alarm clock app
- **WiFi network** for presence detection
- **smart light** (any light entity controllable by Home Assistant)

### Home Assistant components
- core components (pre-installed):
  - `input_datetime`
  - `input_boolean`
  - `sensor` (time_date)
  - `history` (optional but recommended)
  - `recorder` (optional but recommended)

### HACS frontend components
install these via [HACS](https://hacs.xyz/):
- **[button-card](https://github.com/custom-cards/button-card)** - for the dashboard interface

### mobile apps
- **[Hassalarm](https://play.google.com/store/apps/details?id=com.fjun.hassalarm)** - Android app for alarm sync
- **[Home Assistant companion app](https://companion.home-assistant.io/)** - For phone presence detection

---

## 🚀 installation

### step 1: install Hassalarm on your phone

1. install [Hassalarm from Google Play Store](https://play.google.com/store/apps/details?id=com.fjun.hassalarm)
2. in Home Assistant, go to your **Profile** → **Long-Lived Access Tokens**
3. create a new token and copy it
4. open Hassalarm and configure:
   - **Home Assistant URL**: your HA URL (e.g., `http://192.168.1.100:8123`)
   - **access yoken**: Paste the token you created
   - **entity ID**: `input_datetime.next_alarm`
5. set an alarm in your phone's clock app to test

### step 2: install Home Assistant companion app

1. install the [Home Assistant companion app](https://play.google.com/store/apps/details?id=io.homeassistant.companion.android) on your phone
2. log in and set up location tracking
3. note your device tracker entity name (e.g., `device_tracker.your_phone_name`)

### step 3: install button-card via HACS

1. open **HACS** in Home Assistant
2. go to **Frontend**
3. search for **"button-card"**
4. click **Install**
5. **restart Home Assistant**

---

## ⚙️ configuration

### 1. add to `configuration.yaml`

add the following configuration to your `configuration.yaml` file:

```yaml
# input datetime to store the next alarm time from Hassalarm
input_datetime:
  next_alarm:
    name: Next scheduled alarm
    has_date: true
    has_time: true

# input boolean to enable/disable the alarm light automation
input_boolean:
  alarm_light_enabled:
    name: Alarm Light Enabled
    initial: true
    icon: mdi:lightbulb-alert

# time sensor for triggering automations at specific times
sensor:
  - platform: time_date
    display_options:
      - 'date_time'

# optional but recommended: Enable history and recorder
# this ensures your alarm time persists across HA restarts
### WARNING: if you ennumerate any sensors inside either history 
### or recorder, Home Assistant is going to only persist those 
### sensors and no other sensor data. keep that in mind. 
history:

recorder:
```

**why each component?**
- `input_datetime.next_alarm`: Stores the next alarm time from your phone
- `input_boolean.alarm_light_enabled`: Acts as an on/off switch for the automation
- `sensor.date_time`: Provides current date/time for comparison with alarm time
- `history` & `recorder`: Preserves data across restarts

### 2. create the Light Flash Script

add to `scripts.yaml`:

```yaml
alarm_light_flash:
  alias: "Alarm Light Flash"
  sequence:
    - repeat:
        count: 30  # Flashes for 60 seconds (30 cycles × 2 seconds)
        sequence:
          - action: light.turn_off
            target:
              entity_id: light.dormitor_horia_veioza_pat  # CHANGE THIS to your light entity
          - delay:
              seconds: 1
          - action: light.turn_on
            target:
              entity_id: light.dormitor_horia_veioza_pat  # CHANGE THIS to your light entity
          - delay:
              seconds: 1
```

**important**: replace `light.dormitor_horia_veioza_pat` with your actual light entity ID.

**how it works:**
- repeats 30 times (60 seconds total)
- each cycle: turns light OFF for 1 second, ON for 1 second
- creates a flashing effect to wake you up
- the light remains ON after the flashing

### 3. create the Automation

add to `automations.yaml`:

```yaml
- alias: "Hassalarm wake up light"
  trigger:
    - trigger: template
      value_template: "{{ states('sensor.date_time') == (state_attr('input_datetime.next_alarm', 'timestamp') | int | timestamp_custom('%Y-%m-%d, %H:%M', True)) }}"
  condition:
    - condition: state
      entity_id: device_tracker.shadow  # CHANGE THIS to your device tracker
      state: "home"
    - condition: state
      entity_id: input_boolean.alarm_light_enabled
      state: "on"
  action:
    - action: script.alarm_light_flash
```

**important**: replace `device_tracker.shadow` with your actual device tracker entity ID.

**how it works:**
- **trigger**: Fires when current time matches the alarm time
- **conditions**:
  1. your phone must be home (on WiFi)
  2. the alarm light toggle must be enabled
- **action**: runs the light flashing script

### 4. restart Home Assistant

after editing `configuration.yaml`:
1. go to **Settings** → **System** → **Restart**
2. wait for Home Assistant to restart

for `scripts.yaml` and `automations.yaml`, you can just reload:
1. go to **Developer Tools** → **YAML**
2. click **"Scripts"** and **"Automations"** to reload

### 5. add Dashboard Card

add this to your dashboard (Edit Dashboard → Add Card → Manual):

```yaml
type: custom:button-card
entity: input_boolean.alarm_light_enabled
name: |
  [[[
    return entity.state === 'on' ? 'Alarm Light is ON' : 'Alarm Light is OFF';
  ]]]
icon: mdi:alarm-light
show_state: false
show_label: true
label: |
  [[[
    if (states['input_datetime.next_alarm']) {
      return 'Next: ' + states['input_datetime.next_alarm'].state;
    }
    return 'No alarm set';
  ]]]
tap_action:
  action: toggle
styles:
  icon:
    - width: 35px
    - color: |
        [[[ 
          var phone = states['device_tracker.shadow'];  // CHANGE THIS to your device tracker
          var phoneHome = phone && phone.state === 'home';
          var enabled = entity.state === 'on';
          
          if (phoneHome && enabled) 
            return 'green';
          else if (enabled)
            return 'orange';
          else
            return 'red';
        ]]]
  card:
    - padding: 12px
```

**important**: replace `device_tracker.shadow` with your actual device tracker entity ID.

**Dashboard features:**
- **Title**: shows "Alarm Light is ON" or "Alarm Light is OFF"
- **Label**: displays next alarm time
- **Color coding**:
  - Green = Home & enabled
  - Orange = Away & enabled
  - Red = Disabled
- **click to toggle**: enable/disable the automation

---

## 🔄 How it works

### System flow diagram

```mermaid
graph TD
    A[set alarm on Phone] --> B[Hassalarm detects alarm change]
    B --> C[updates input_datetime.next_alarm in HA]
    C --> D{alarm time reached?}
    D -->|No| C
    D -->|Yes| E{check conditions}
    E --> F{phone at home?}
    F -->|No| G[do nothing]
    F -->|Yes| H{alarm light enabled?}
    H -->|No| G
    H -->|Yes| I[run light flash script]
    I --> J[turn light OFF]
    J --> K[wait 1 second]
    K --> L[turn light ON]
    L --> M[wait 1 second]
    M --> N{repeat 30 times?}
    N -->|No| J
    N -->|Yes| O[end - you are awake!]
    
    style A fill:#e1f5ff
    style I fill:#fff3cd
    style O fill:#d4edda
```

### detailed logic

1. **alarm detection**
   - you set an alarm in any Android alarm app
   - Android broadcasts `ACTION_NEXT_ALARM_CLOCK_CHANGED`
   - Hassalarm detects this and sends the alarm time to Home Assistant
   - `input_datetime.next_alarm` is updated

2. **presence tracking**
   - Home Assistant Companion App tracks your phone's location
   - when connected to home WiFi, `device_tracker` state = "home"
   - when away, state = "away" or "not_home"

3. **automation trigger**
   - every minute, HA compares current time with alarm time
   - when they match, the automation triggers

4. **condition checks**
   - is phone at home? (Check device tracker)
   - is alarm light enabled? (check input_boolean)
   - both must be true to continue

5. **Light Flashing**
   - script runs 30 iterations
   - each iteration: OFF (1s) → N (1s)
   - total duration: 60 seconds

6. **Dashboard Control**
   - toggle button enables/disables automation
   - color indicates status at a glance
   - shows next scheduled alarm time

---

## 🔧 troubleshooting

### Hassalarm not syncing

**problem**: Alarm time not updating in Home Assistant

**solutions**:
1. **check background permissions**:
   - Settings → Apps → Hassalarm → Battery → Unrestricted
   - Disable battery optimization for Hassalarm

2. **use Google Clock**: Some manufacturer alarm apps have bugs
   - install [Google Clock](https://play.google.com/store/apps/details?id=com.google.android.deskclock)
   - set alarms there instead

3. **verify token**: Ensure your long-lived access token is valid
   - check in Hassalarm app settings
   - generate a new token if needed

4. **check entity ID**: must be exactly `input_datetime.next_alarm`

### light not flashing

**problem**: Automation doesn't trigger at alarm time

**solutions**:
1. **check device tracker state**:
   - Developer Tools → States
   - find your device tracker
   - verify it shows "home" (lowercase) when at home
   - update automation if state is capitalized differently

2. **check input_boolean**:
   - make sure it's toggled ON in the dashboard

3. **verify light entity**:
   - Developer Tools → States
   - test manually: `light.turn_on` service
   - replace with correct entity ID in script

4. **check time sensor**:
   - verify `sensor.date_time` exists
   - should update every minute

### Dashboard Card not working

**problem**: button card shows error or doesn't update

**solutions**:
1. **restart after installing button-card**:
   - HACS installations require restart
   - Settings → System → Restart

2. **clear browser cache**:
   - hard refresh: Ctrl+F5 (Windows) or Cmd+Shift+R (Mac)
   - or try incognito mode

3. **verify entity IDs**:
   - check all entity IDs in the card YAML
   - must match your actual entities

4. **check for JavaScript errors**:
   - browser console (F12)
   - look for ButtonCardJSTemplateError

### wrong Alarm Time

**problem**: Alarm time in HA is off by hours

**solutions**:
1. **time zone mismatch**:
   - Settings → System → General
   - set correct time zone

2. **use Google Clock**:
   - known bugs with Samsung and Xiaomi clocks
   - switch to Google Clock app

---

## 🎨 customization Ideas

### change Flash duration

edit the `count` in `scripts.yaml`:
```yaml
count: 60  # 120 seconds (2 minutes)
count: 15  # 30 seconds
```

### change Flash speed

edit the `delay` seconds:
```yaml
- delay:
    seconds: 0.5  # Faster flashing
```

### multiple Lights

add multiple lights in the script:
```yaml
- action: light.turn_on
  target:
    entity_id:
      - light.bedroom_lamp
      - light.bedside_light
      - light.hallway
```

### add Brightness ramp

Instead of OFF/ON, gradually increase brightness:
```yaml
alarm_light_ramp:
  alias: "Alarm light ramp"
  sequence:
    - repeat:
        count: 10
        sequence:
          - action: light.turn_on
            target:
              entity_id: light.bedroom
            data:
              brightness: "{{ repeat.index * 25 }}"
          - delay:
              seconds: 6
```

### pre-Alarm warning

trigger light 5 minutes before alarm:
```yaml
- alias: "Hassalarm pre-wake light"
  trigger:
    - trigger: template
      value_template: "{{ ((as_timestamp(states('sensor.date_time').replace(',','')) | int) + 300) == (state_attr('input_datetime.next_alarm', 'timestamp') | int) }}"
```

---

## 📝 file structure

your Home Assistant configuration should look like this:

```
homeassistant/
├── configuration.yaml    # Main config with input_datetime, sensor, etc.
├── scripts.yaml         # Light flash script
├── automations.yaml     # Wake-up automation
└── ui-lovelace.yaml     # Dashboard (or via UI)
```

---

## 🙏 credits

- **[Hassalarm](https://github.com/Johboh/hassalarm)** by [@Johboh](https://github.com/Johboh) - The Android app that makes this all possible
- **[button-card](https://github.com/custom-cards/button-card)** by [@RomRider](https://github.com/RomRider) - Beautiful custom dashboard cards
- **[Home Assistant](https://www.home-assistant.io/)** - The amazing home automation platform

---

## 📄 license

this project is provided as-is for personal use. Feel free to modify and adapt to your needs.

---

## 🤝 contributing

found a bug or have an improvement? feel free to:
1. open an issue
2. submit a pull request
3. share your customizations!

---

## ⭐ support

If you find this project useful, please:
- ⭐ star this repository
- 📢 share it with other Home Assistant users
- 💬 report issues or suggest improvements

---

**happy waking up! 🌅**
