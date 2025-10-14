# Quick Start Guide

Get your Hassalarm Wake-Up Light integration running in 15 minutes!

## 🎯 What You'll Need

- ✅ Home Assistant (2020.12.1 or later)
- ✅ Android phone
- ✅ Smart light connected to Home Assistant
- ✅ 15 minutes of your time

---

## Step 1: Install Required Apps (5 minutes)

### On Your Phone:

1. **Install Hassalarm**
   - Open [Google Play Store](https://play.google.com/store/apps/details?id=com.fjun.hassalarm)
   - Install "Hassalarm"
   - Don't open it yet!

2. **Install Home Assistant Companion**
   - Open [Google Play Store](https://play.google.com/store/apps/details?id=io.homeassistant.companion.android)
   - Install "Home Assistant"
   - Log in with your HA credentials
   - Allow location permissions when prompted

### In Home Assistant:

3. **Install button-card**
   - Open **HACS** (install it first if you don't have it)
   - Go to **Frontend**
   - Click **Explore & Download Repositories**
   - Search "**button-card**"
   - Click **Download**
   - **Restart Home Assistant** (important!)

---

## Step 2: Configure Home Assistant (5 minutes)

### 2.1: Edit configuration.yaml

Add this to your `configuration.yaml`:

```yaml
input_datetime:
  next_alarm:
    name: Next scheduled alarm
    has_date: true
    has_time: true

input_boolean:
  alarm_light_enabled:
    name: Alarm Light Enabled
    initial: true
    icon: mdi:lightbulb-alert

sensor:
  - platform: time_date
    display_options:
      - 'date_time'

history:
recorder:
```

**Save** and **Restart Home Assistant**

### 2.2: Create the Light Flash Script

Create or edit `scripts.yaml`:

```yaml
alarm_light_flash:
  alias: "Alarm Light Flash"
  sequence:
    - repeat:
        count: 30
        sequence:
          - action: light.turn_on
            target:
              entity_id: light.YOUR_LIGHT_HERE  # ← CHANGE THIS!
          - delay:
              seconds: 1
          - action: light.turn_off
            target:
              entity_id: light.YOUR_LIGHT_HERE  # ← CHANGE THIS!
          - delay:
              seconds: 1
```

**Important:** Replace `light.YOUR_LIGHT_HERE` with your actual light entity!

To find your light entity:
1. Go to **Developer Tools → States**
2. Search for "light."
3. Find your bedroom light
4. Copy the entity ID (e.g., `light.bedroom_lamp`)

**Save** and reload scripts:
- **Developer Tools → YAML → Scripts**

### 2.3: Create the Automation

Create or edit `automations.yaml`:

```yaml
- alias: "Hassalarm Wake Up Light"
  trigger:
    - trigger: template
      value_template: "{{ states('sensor.date_time') == (state_attr('input_datetime.next_alarm', 'timestamp') | int | timestamp_custom('%Y-%m-%d, %H:%M', True)) }}"
  condition:
    - condition: state
      entity_id: device_tracker.YOUR_PHONE_HERE  # ← CHANGE THIS!
      state: "home"
    - condition: state
      entity_id: input_boolean.alarm_light_enabled
      state: "on"
  action:
    - action: script.alarm_light_flash
```

**Important:** Find your device tracker:
1. Go to **Developer Tools → States**
2. Search for "device_tracker"
3. Find your phone (look for your phone's name)
4. Copy the entity ID (e.g., `device_tracker.pixel_8`)
5. Also note if the state says "home" or "Home" (case matters!)

**Save** and reload automations:
- **Developer Tools → YAML → Automations**

---

## Step 3: Connect Hassalarm (3 minutes)

### 3.1: Create Access Token

1. In Home Assistant, click your **Profile** (bottom left)
2. Scroll down to **Long-Lived Access Tokens**
3. Click **Create Token**
4. Name it "Hassalarm"
5. **Copy the token** (you can't see it again!)

### 3.2: Configure Hassalarm App

1. Open **Hassalarm** on your phone
2. Tap **"Configure"** or the settings icon
3. Enter your **Home Assistant URL**
   - Example: `http://192.168.1.100:8123`
   - Or: `https://your-domain.duckdns.org`
4. Paste the **access token** you copied
5. Set **Entity ID** to: `input_datetime.next_alarm`
6. Tap **Save**

### 3.3: Set Phone Permissions

**Critical step!** Otherwise Hassalarm won't work in background:

1. Go to **Android Settings → Apps → Hassalarm**
2. Tap **Battery**
3. Select **"Unrestricted"**
4. Or disable **"Battery optimization"**

---

## Step 4: Add Dashboard Card (2 minutes)

1. Go to your **Home Assistant Dashboard**
2. Click **Edit Dashboard** (three dots, top right)
3. Click **+ Add Card**
4. Scroll down and click **Manual** (at the bottom)
5. Paste this code:

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
          var phone = states['device_tracker.YOUR_PHONE_HERE'];
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

6. **Replace** `device_tracker.YOUR_PHONE_HERE` with your device tracker from Step 2.3
7. Click **Save**
8. Click **Done**

---

## Step 5: Test Everything! (2 minutes)

### Test 1: Check Hassalarm Connection

1. Open any **alarm clock app** on your phone
2. Set an alarm for 2 minutes from now
3. Wait 1-2 minutes
4. In Home Assistant, go to **Developer Tools → States**
5. Search for `input_datetime.next_alarm`
6. It should show your alarm time! ✅

If it doesn't appear:
- Check phone battery settings (Step 3.3)
- Check Hassalarm app shows "Synced"
- See [Troubleshooting Guide](TROUBLESHOOTING.md)

### Test 2: Test Light Flash

1. Go to **Developer Tools → Services**
2. Service: `script.alarm_light_flash`
3. Click **Call Service**
4. Your light should flash on/off! ✅

If it doesn't:
- Check your light entity ID in scripts.yaml
- Test light manually: `light.turn_on` service

### Test 3: Test Dashboard

1. Go to your dashboard
2. Find the Alarm Light card
3. Icon should be:
   - 🟢 Green (if you're home and it's enabled)
   - 🟠 Orange (if enabled but away)
   - 🔴 Red (if disabled)
4. Click the card - it should toggle on/off ✅

### Test 4: Wait for Real Alarm

1. Keep the alarm you set for 2 minutes
2. Don't cancel it!
3. When your phone alarm rings...
4. Your light should start flashing! ✅

---

## 🎉 Success!

If all tests passed, you're done! Your wake-up light system is ready.

### What happens now:

1. Set any alarm on your phone (any alarm app)
2. Hassalarm automatically syncs to Home Assistant
3. When alarm time arrives:
   - ✅ If you're home → Light flashes for 60 seconds
   - ❌ If you're away → Nothing happens
4. Use dashboard button to enable/disable

---

## 🔧 Customization

### Change flash duration:
```yaml
# scripts.yaml
count: 60  # 120 seconds instead of 60
```

### Change flash speed:
```yaml
# scripts.yaml
delay:
  seconds: 0.5  # Faster flashing
```

### Multiple lights:
```yaml
# scripts.yaml
entity_id:
  - light.bedroom_lamp
  - light.hallway_light
```

See [README.md](README.md) for more customization ideas!

---

## ❌ Something Not Working?

1. **Check Home Assistant Logs**
   - Settings → System → Logs

2. **Verify Entity IDs**
   - Developer Tools → States
   - All entities must exist

3. **Read Troubleshooting Guide**
   - [TROUBLESHOOTING.md](TROUBLESHOOTING.md)

4. **Ask for Help**
   - [Open GitHub Issue](https://github.com/your-repo/issues)
   - Include: HA version, error logs, what you tried

---

## 📚 Next Steps

- Read [README.md](README.md) for detailed documentation
- Explore [example configurations](examples/)
- Check [TROUBLESHOOTING.md](TROUBLESHOOTING.md) for common issues
- Customize your wake-up experience!

**Enjoy your intelligent wake-up light system!** 🌅