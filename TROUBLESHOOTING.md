# Troubleshooting Guide

Common issues and solutions for the Hassalarm Wake-Up Light Integration.

## Table of Contents

1. [Hassalarm App Issues](#hassalarm-app-issues)
2. [Automation Not Triggering](#automation-not-triggering)
3. [Device Tracker Problems](#device-tracker-problems)
4. [Dashboard Card Issues](#dashboard-card-issues)
5. [Light Not Flashing](#light-not-flashing)
6. [Time Zone Problems](#time-zone-problems)
7. [Performance Issues](#performance-issues)

---

## Hassalarm App Issues

### Problem: Alarm not syncing to Home Assistant

**Symptoms:**
- `input_datetime.next_alarm` shows "unknown" or doesn't update
- Alarm set on phone but not reflected in HA

**Solutions:**

1. **Check Background Permissions** (Most Common Issue)
   ```
   Android Settings → Apps → Hassalarm → Battery
   - Set to "Unrestricted"
   - Disable battery optimization
   ```

2. **Verify App Configuration**
   - Open Hassalarm app
   - Check Home Assistant URL is correct (include `http://` or `https://`)
   - Verify long-lived access token is valid
   - Entity ID must be exactly: `input_datetime.next_alarm`

3. **Test Connectivity**
   - Open Home Assistant in your phone's browser
   - If it doesn't load, check:
     - WiFi connection
     - Home Assistant is running
     - Firewall settings

4. **Check Hassalarm Service**
   ```
   Android Settings → Apps → Hassalarm → Storage
   - Clear Cache (not Clear Data)
   - Force Stop app
   - Open app again
   ```

5. **Use Google Clock App**
   - Some manufacturer alarm apps have bugs
   - Known issues with:
     - Samsung Clock
     - Xiaomi Clock
     - OnePlus Clock
   - Install [Google Clock](https://play.google.com/store/apps/details?id=com.google.android.deskclock)

### Problem: Hassalarm constantly shows "Syncing" or errors

**Symptoms:**
- App shows sync error
- "Failed to update" message

**Solutions:**

1. **Regenerate Access Token**
   ```
   Home Assistant → Profile → Long-Lived Access Tokens
   - Delete old token
   - Create new token
   - Copy and paste into Hassalarm
   ```

2. **Check Home Assistant Logs**
   ```
   Settings → System → Logs
   - Look for authentication errors
   - Look for API errors related to input_datetime
   ```

3. **Verify Entity Exists**
   ```
   Developer Tools → States
   - Search for: input_datetime.next_alarm
   - If not found, check configuration.yaml
   ```

---

## Automation Not Triggering

### Problem: Light doesn't flash when alarm goes off

**Symptoms:**
- Alarm rings on phone
- Light doesn't flash
- Automation shows "Last triggered: never"

**Diagnostic Steps:**

1. **Check Automation State**
   ```
   Settings → Automations & Scenes → Hassalarm Wake Up Light
   - Ensure it's enabled (toggle on)
   - Check last triggered time
   ```

2. **Verify Trigger**
   ```
   Developer Tools → States
   - Check sensor.date_time (updates every minute?)
   - Check input_datetime.next_alarm (shows correct alarm time?)
   ```

3. **Test Conditions Manually**
   ```
   Developer Tools → States
   - device_tracker.your_phone: Should be "home" (lowercase)
   - input_boolean.alarm_light_enabled: Should be "on"
   ```

4. **Force Trigger Test**
   ```
   Developer Tools → Services
   Service: script.alarm_light_flash
   Click "Call Service"
   - Does light flash? If not, see "Light Not Flashing" section
   ```

### Problem: Automation triggers at wrong time

**Symptoms:**
- Light flashes hours before/after alarm
- Time is off

**Solutions:**

1. **Check Time Zone**
   ```
   Settings → System → General → Time Zone
   - Set to your correct time zone
   - Restart Home Assistant
   ```

2. **Verify Alarm Time in HA**
   ```
   Developer Tools → States → input_datetime.next_alarm
   - Compare with phone alarm time
   - Should match exactly
   ```

3. **Check Time Sensor**
   ```
   Developer Tools → States → sensor.date_time
   - Should match current time
   - Format: YYYY-MM-DD, HH:MM
   ```

---

## Device Tracker Problems

### Problem: Device tracker always shows "away"

**Symptoms:**
- Automation never triggers because "not home" condition
- Dashboard shows red or orange icon permanently

**Solutions:**

1. **Check Home Assistant Companion App**
   ```
   Mobile App → Settings → Companion App
   - Location Tracking: Enabled
   - Background Refresh: Enabled
   ```

2. **Verify Tracker State**
   ```
   Developer Tools → States → device_tracker.your_phone
   - Current state: Should be "home" or "away"
   - Check state history (click entity)
   ```

3. **Check Home Zone**
   ```
   Settings → Areas & Zones → Home
   - Ensure home location is correct
   - Radius should cover your WiFi range
   ```

4. **Force Update**
   ```
   Home Assistant App on Phone
   - Pull down to refresh
   - Or: Settings → Companion App → Force Update Location
   ```

### Problem: Wrong device tracker entity

**Symptoms:**
- Can't find `device_tracker.your_phone`
- Entity ID doesn't match documentation

**Solutions:**

1. **Find Correct Entity**
   ```
   Developer Tools → States
   - Filter by "device_tracker"
   - Look for your phone name
   - Common formats:
     - device_tracker.phone_name
     - device_tracker.your_name_phone
     - device_tracker.pixel_8
   ```

2. **Update All Configurations**
   - Replace in `automations.yaml`
   - Replace in dashboard YAML
   - Reload automations and refresh dashboard

3. **Check State Format**
   ```
   Some trackers use:
   - "home" / "away" (lowercase)
   - "Home" / "Away" (capitalized)
   - "not_home"
   
   Update automation condition to match exact state
   ```

---

## Dashboard Card Issues

### Problem: Button card shows error or doesn't load

**Symptoms:**
- Card shows "Custom element doesn't exist"
- JavaScript errors
- Card appears blank

**Solutions:**

1. **Verify button-card Installation**
   ```
   HACS → Frontend → Search "button-card"
   - Should show as installed
   - Check version (latest is best)
   ```

2. **Restart Home Assistant**
   ```
   Settings → System → Restart
   - Required after installing HACS components
   ```

3. **Clear Browser Cache**
   ```
   Hard Refresh:
   - Windows: Ctrl + F5
   - Mac: Cmd + Shift + R
   - Or try Incognito/Private mode
   ```

4. **Check Browser Console**
   ```
   Press F12 → Console tab
   - Look for red errors
   - Common: "ButtonCardJSTemplateError"
   ```

### Problem: Card shows wrong color or doesn't update

**Symptoms:**
- Icon stays blue/gray
- Color doesn't change with status
- Shows wrong state

**Solutions:**

1. **Verify Entity IDs in Card YAML**
   ```yaml
   # These must match your actual entities:
   device_tracker.your_phone  # Change to your tracker
   ```

2. **Check Entity States**
   ```
   Developer Tools → States
   - Verify all entities exist
   - Check their current states
   - States are case-sensitive!
   ```

3. **Fix JavaScript Template**
   ```yaml
   # State comparison must match exactly:
   phone.state === 'home'  # Lowercase
   # vs
   phone.state === 'Home'  # Capitalized
   ```

4. **Hard Refresh After Changes**
   - Save card
   - Ctrl + F5 (Windows) or Cmd + Shift + R (Mac)

---

## Light Not Flashing

### Problem: Script doesn't control light

**Symptoms:**
- Script runs but light doesn't respond
- Light turns on but doesn't turn off
- Light state stuck

**Solutions:**

1. **Test Light Manually**
   ```
   Developer Tools → Services
   Service: light.turn_on
   Entity: light.your_light
   Click "Call Service"
   - Does it work? If not, light entity issue
   ```

2. **Verify Light Entity ID**
   ```
   Developer Tools → States
   - Search for your light
   - Copy exact entity ID
   - Update in scripts.yaml
   ```

3. **Check Light Supports On/Off**
   ```
   Some lights require brightness parameter:
   
   action: light.turn_on
   target:
     entity_id: light.bedroom
   data:
     brightness: 255  # Add this
   ```

4. **Check Script is Running**
   ```
   Developer Tools → States
   - Search: script.alarm_light_flash
   - State should change to "on" when triggered
   ```

### Problem: Light flashes once then stops

**Symptoms:**
- Light turns on/off once
- Script stops early
- Only 1-2 flashes

**Solutions:**

1. **Check Script Syntax**
   ```yaml
   # Ensure proper indentation
   - repeat:
       count: 30
       sequence:  # This must be indented correctly
         - action: light.turn_on
   ```

2. **Check Home Assistant Logs**
   ```
   Settings → System → Logs
   - Look for script errors
   - Look for light entity errors
   ```

3. **Reduce Count for Testing**
   ```yaml
   - repeat:
       count: 3  # Test with just 3 cycles
   ```

---

## Time Zone Problems

### Problem: Times don't match between phone and HA

**Symptoms:**
- Alarm shows correct time on phone
- Shows different time in Home Assistant
- Off by exactly N hours

**Solutions:**

1. **Set Home Assistant Time Zone**
   ```
   Settings → System → General → Time Zone
   - Select your correct time zone
   - Restart required
   ```

2. **Check Server Time**
   ```
   Developer Tools → Template
   {{ now() }}
   - Should show correct current time
   ```

3. **Verify Phone Time Zone**
   ```
   Phone Settings → Date & Time
   - Automatic time zone: ON
   - Matches Home Assistant setting
   ```

4. **Check Input Datetime**
   ```
   Developer Tools → States → input_datetime.next_alarm
   Attributes → timestamp
   - Compare with expected alarm time
   ```

---

## Performance Issues

### Problem: Home Assistant becomes slow

**Symptoms:**
- Dashboard loads slowly
- Automations delayed
- High CPU usage

**Solutions:**

1. **Reduce Recorder Data**
   ```yaml
   # configuration.yaml
   recorder:
     purge_keep_days: 3  # Reduce from 7
     exclude:
       domains:
         - automation
         - script
   ```

2. **Limit History**
   ```yaml
   # configuration.yaml
   history:
     exclude:
       domains:
         - automation
         - script
   ```

3. **Check Database Size**
   ```
   File Manager → /config/home-assistant_v2.db
   - If > 1GB, consider purging old data
   ```

4. **Reload Instead of Restart**
   ```
   Developer Tools → YAML
   - Reload Automations (not full restart)
   - Reload Scripts
   ```

---

## Getting More Help

If you're still experiencing issues:

1. **Check Home Assistant Logs**
   ```
   Settings → System → Logs
   - Copy relevant error messages
   ```

2. **Enable Debug Logging**
   ```yaml
   # configuration.yaml
   logger:
     default: info
     logs:
       homeassistant.components.automation: debug
       homeassistant.components.script: debug
   ```

3. **Test in Isolation**
   - Disable other automations temporarily
   - Test each component individually
   - Verify basic functionality first

4. **Open GitHub Issue**
   - Include Home Assistant version
   - Include relevant logs
   - Describe steps to reproduce
   - Link to this troubleshooting guide

5. **Community Support**
   - [Home Assistant Community](https://community.home-assistant.io/)
   - [Hassalarm GitHub Issues](https://github.com/Johboh/hassalarm/issues)
   - [Home Assistant Discord](https://discord.gg/home-assistant)

---

## Quick Reference Checklist

Before asking for help, verify:

- [ ] Home Assistant version 2020.12.1 or later
- [ ] Hassalarm app installed and configured
- [ ] button-card installed from HACS
- [ ] Home Assistant Companion app installed
- [ ] All entity IDs updated in configurations
- [ ] Home Assistant restarted after config changes
- [ ] Browser cache cleared
- [ ] Time zones match
- [ ] Automations enabled
- [ ] Device tracker shows correct state
- [ ] Light entity responds to manual commands
- [ ] No errors in Home Assistant logs