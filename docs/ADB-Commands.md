---
title: ADB Commands
nav_order: 3
description: "Essential ADB commands and system configurations for Android customization."
---

# ADB Commands & Configurations

Essential ADB commands and system configurations for Android customization and optimization.

## 📱 Prerequisites

Before using these commands, ensure you have:
- **ADB Tools** installed on your computer
- **Developer Options** enabled on your Android device
- **USB Debugging** enabled
- Device connected via USB or wireless ADB

### Installing ADB

#### Linux
```bash
# Debian/Ubuntu
sudo apt install android-tools-adb

# Arch Linux
sudo pacman -S android-tools
```

#### macOS
```bash
brew install android-platform-tools
```

#### Windows
Download from [Android Developer Site](https://developer.android.com/tools/releases/platform-tools)

---

## 🔧 System Settings Commands

These commands modify Android system settings using the `settings` command via ADB shell.

### Display & UI Settings

#### Enable AOD with Super Wallpaper
```bash
adb shell settings put system aod_using_super_wallpaper 1
```
**Description:** Enables Always-On Display (AOD) using the super wallpaper feature (MIUI/HyperOS).

### Performance Settings

#### Enable GPU Tuner
```bash
adb shell settings put global GPUTUNER_SWITCH true
```
**Description:** Enables GPU tuning for better graphics performance.

### Security Settings

#### Allow Non-Market App Installation
```bash
adb shell settings put secure install_non_market_apps 1
```
**Description:** Permits installation of apps from sources other than the Play Store.

### Network Settings

#### Disable WiFi Scanning When Off
```bash
adb shell settings put global wifi_scan_always_enabled 0
```
**Description:** Prevents WiFi scanning when WiFi is turned off, saving battery.

---

## 📋 Settings Command Reference

### Command Structure

The `settings` command follows this pattern:
```bash
adb shell settings put <namespace> <key> <value>
```

### Namespaces

| Namespace | Description | Persistence |
|-----------|-------------|-------------|
| `system` | User-level settings (display, sound, UI) | User-specific |
| `secure` | Secure settings (security, privacy) | User-specific |
| `global` | System-wide settings (network, performance) | Device-wide |

### Common Operations

#### Read a Setting
```bash
adb shell settings get <namespace> <key>
```

#### List All Settings
```bash
# List all system settings
adb shell settings list system

# List all secure settings
adb shell settings list secure

# List all global settings
adb shell settings list global
```

#### Delete a Setting
```bash
adb shell settings delete <namespace> <key>
```

#### Reset to Defaults
```bash
adb shell settings reset <namespace>
```

---

## 🛠️ Package Management

### Grant Permissions
```bash
# Grant a specific permission
adb shell pm grant <package> <permission>

# Example: Grant write secure settings to SetBox
adb shell pm grant com.yn.setbox.plugin android.permission.WRITE_SECURE_SETTINGS
```

### Disable System Apps
```bash
# Disable an app (without uninstalling)
adb shell pm disable-user --user 0 <package>

# Enable an app
adb shell pm enable --user 0 <package>
```

### Uninstall Apps
```bash
# Uninstall for current user (keeps system app)
adb shell pm uninstall --user 0 <package>

# Full uninstall (requires root)
adb shell pm uninstall <package>
```

---

## 🔌 Wireless ADB

### Enable Wireless ADB

1. **Connect via USB first:**
   ```bash
   adb tcpip 5555
   ```

2. **Find device IP address:**
   ```bash
   adb shell ip addr show wlan0
   ```

3. **Connect wirelessly:**
   ```bash
   adb connect <device-ip>:5555
   ```

4. **Disconnect USB and verify:**
   ```bash
   adb devices
   ```

### Disable Wireless ADB
```bash
adb usb
```

---

## 🎯 Useful Commands

### Device Information
```bash
# Device model
adb shell getprop ro.product.model

# Android version
adb shell getprop ro.build.version.release

# Device serial number
adb shell getprop ro.serialno

# Battery info
adb shell dumpsys battery
```

### Screen Control
```bash
# Take screenshot
adb shell screencap /sdcard/screenshot.png
adb pull /sdcard/screenshot.png

# Record screen
adb shell screenrecord /sdcard/recording.mp4

# Turn screen off
adb shell input keyevent 26

# Unlock screen
adb shell input keyevent 82
```

### Input Simulation
```bash
# Send text
adb shell input text "Hello World"

# Press home button
adb shell input keyevent 3

# Press back button
adb shell input keyevent 4

# Tap at coordinates
adb shell input tap 500 1000

# Swipe gesture
adb shell input swipe 300 1000 300 300 100
```

---

## ⚠️ Important Notes

### Safety Considerations

1. **Backup First**: Always backup your data before modifying system settings
2. **Know What You're Doing**: Incorrect settings can cause system instability
3. **Test One at a Time**: Apply settings individually to identify issues
4. **Factory Reset Option**: Be prepared to factory reset if something goes wrong

### Persistence

- Settings may reset after:
  - System updates
  - Factory reset
  - ROM changes
  - Some OEM optimizations

### Automation

Consider using tools like [Automate](https://llamalab.com/automate) or Tasker to automatically reapply settings after reboot.

---

## 🔍 Troubleshooting

### ADB Not Recognized
```bash
# Kill and restart ADB server
adb kill-server
adb start-server
```

### Device Not Found
```bash
# Check connected devices
adb devices

# Verify USB drivers (Windows)
# Reinstall platform-tools
```

### Permission Denied
```bash
# Restart ADB with root
adb root

# Or use Shizuku apps as an alternative
```

---

## 📚 Additional Resources

- [Android Debug Bridge Documentation](https://developer.android.com/tools/adb)
- [Settings Command Reference](https://developer.android.com/reference/android/provider/Settings)
- [ADB Commands Cheatsheet](https://gist.github.com/Pulimet/5013acf2cd5b28e55036c82c91bd56d8)

---

## 🔙 Navigation

- [← Back to Home](index.md)
- [← Android Apps](Android-Apps.md)
- [Chromium Browser →](Chromium-Browser.md)
