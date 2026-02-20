# Chromium Browser for Android

Custom Chromium browser development for Android with performance optimizations and privacy enhancements.

## 🎯 Project Goal

The current plan is to fork [Brave Browser](https://github.com/brave/brave-browser) and apply custom patches for enhanced performance and features on Android devices.

---

## 🚀 Base Project

### Brave Browser
**Repository:** [brave/brave-browser](https://github.com/brave/brave-browser)

**Why Brave?**
- Built on Chromium with privacy-first approach
- Active development and maintenance
- Built-in ad/tracker blocking
- Strong Android support
- Open source with permissive license

---

## 🔧 Planned Patches

### Performance Optimizations

#### 1. Skia Scale Patch
**Source:** [thorium/skia_scale.patch](https://github.com/Alex313031/thorium/blob/main/other/skia_scale.patch)

**Purpose:** Optimizes Skia graphics rendering for better performance and reduced memory usage on mobile devices.

**Benefits:**
- Improved scrolling performance
- Better canvas rendering
- Reduced GPU memory consumption
- Smoother animations

---

## 📚 Reference Projects

These projects serve as inspiration and reference for custom Chromium builds:

### Core References

| Project | Description | Key Features | Link |
|---------|-------------|--------------|------|
| **Brave Core** | Brave's core functionality | Privacy features, crypto wallet, rewards | [GitHub](https://github.com/brave/brave-core) |
| **Cromite** | Privacy-focused Chromium fork | Enhanced security, debloated | [GitHub](https://github.com/uazo/cromite) |
| **Thorium** | Performance-optimized Chromium | Speed optimizations, patches | [GitHub](https://github.com/Alex313031/thorium) |

### Additional References

| Project | Focus Area | Link |
|---------|------------|------|
| **Ultimatum** | Custom Chromium build | [GitHub](https://github.com/gonzazoid/Ultimatum) |
| **Trivalent** | Secure Chromium variant | [GitHub](https://github.com/secureblue/Trivalent) |
| **Helium Browser** | Android-optimized browser | [GitHub](https://github.com/jqssun/android-helium-browser) |

---

## 🛠️ Development Setup

### Prerequisites

```bash
# Install depot_tools
git clone https://chromium.googlesource.com/chromium/tools/depot_tools.git
export PATH="$PATH:/path/to/depot_tools"

# System requirements
# - Ubuntu 20.04+ or Debian 11+
# - 64GB+ RAM recommended
# - 200GB+ free disk space
# - Python 3.8+
```

### Clone Brave Browser

```bash
# Clone the repository
git clone https://github.com/brave/brave-browser.git
cd brave-browser

# Initialize and sync
npm install
npm run init

# Sync Chromium and dependencies
npm run sync
```

### Apply Custom Patches

```bash
# Create patches directory
mkdir -p patches/custom

# Download Skia scale patch
curl -o patches/custom/skia_scale.patch \
  https://raw.githubusercontent.com/Alex313031/thorium/main/other/skia_scale.patch

# Apply patch
cd src
git apply ../patches/custom/skia_scale.patch
```

---

## 🎨 Planned Features

### Privacy Enhancements
- ✅ Built-in ad/tracker blocking (from Brave)
- ⏳ Enhanced fingerprinting protection
- ⏳ Cookie auto-deletion
- ⏳ Advanced privacy settings

### Performance Optimizations
- ⏳ Skia rendering improvements
- ⏳ Memory usage optimization
- ⏳ Faster JavaScript execution
- ⏳ Reduced startup time

### Android-Specific
- ⏳ Better battery optimization
- ⏳ Improved gesture controls
- ⏳ Enhanced mobile UI/UX
- ⏳ Better support for low-end devices

---

## 📦 Build Process

### Building for Android

```bash
# Set up Android build environment
npm run build -- --target_os=android --target_arch=arm64

# Configure build
gn args out/android_arm64

# Build flags (add to args.gn)
target_os = "android"
target_cpu = "arm64"
is_official_build = true
is_debug = false

# Build
ninja -C out/android_arm64 chrome_public_apk

# Output APK location
# out/android_arm64/apks/BraveMonoPublic.apk
```

### Build Variants

| Variant | Target | Description |
|---------|--------|-------------|
| `arm` | 32-bit ARM | Older devices |
| `arm64` | 64-bit ARM | Modern devices (recommended) |
| `x86` | 32-bit Intel | Emulators |
| `x86_64` | 64-bit Intel | Emulators, Chrome OS |

---

## 🔬 Testing

### Installation

```bash
# Install APK via ADB
adb install -r out/android_arm64/apks/BraveMonoPublic.apk

# Or install on emulator
adb -e install -r out/android_arm64/apks/BraveMonoPublic.apk
```

### Performance Testing

```bash
# Monitor performance
adb shell dumpsys meminfo com.brave.browser
adb shell dumpsys gfxinfo com.brave.browser

# Capture logs
adb logcat | grep -i chromium
```

---

## 📋 Patch Management

### Creating Patches

```bash
# Make changes in src/
cd src
git add .
git commit -m "Description of changes"

# Create patch file
git format-patch HEAD~1 --stdout > ../patches/custom/my_patch.patch
```

### Applying Multiple Patches

```bash
# Apply all patches in directory
for patch in patches/custom/*.patch; do
    git apply "$patch"
done
```

---

## 🔗 Useful Resources

### Documentation
- [Chromium Development](https://www.chromium.org/developers/)
- [Building Chromium for Android](https://chromium.googlesource.com/chromium/src/+/main/docs/android_build_instructions.md)
- [Brave Browser Wiki](https://github.com/brave/brave-browser/wiki)

### Patch Repositories
- [Thorium Patches](https://github.com/Alex313031/thorium/tree/main/other)
- [Cromite Patches](https://github.com/uazo/cromite/tree/master/build/patches)
- [Ungoogled Chromium Patches](https://github.com/ungoogled-software/ungoogled-chromium)

### Communities
- [Brave Community](https://community.brave.com/)
- [Chromium Developers](https://groups.google.com/a/chromium.org/g/chromium-dev)
- [XDA Developers](https://forum.xda-developers.com/)

---

## 🚧 Development Status

| Component | Status | Notes |
|-----------|--------|-------|
| Base Fork | ⏳ Planning | Brave browser fork |
| Skia Patch | ⏳ Pending | Performance optimization |
| Build System | ⏳ Pending | Android build configuration |
| Testing | ⏳ Pending | Device testing |
| Release | ⏳ Pending | Initial release |

**Legend:** ✅ Complete | ⏳ In Progress | ❌ Blocked | 📋 Planned

---

## 🤝 Contributing

### Areas for Contribution
1. Performance patches
2. Privacy enhancements
3. UI/UX improvements
4. Bug fixes
5. Documentation

### Patch Submission
1. Fork the repository
2. Create a feature branch
3. Implement changes
4. Test thoroughly
5. Submit pull request with patch file

---

## ⚠️ Important Notes

### Legal Considerations
- Respect Chromium and Brave licenses
- Maintain attribution
- Don't use official Brave branding without permission
- Comply with WebKit/Chromium trademark policies

### Build Requirements
- **Time:** Initial build takes 2-4 hours
- **Storage:** ~200GB for full checkout
- **RAM:** 16GB minimum, 64GB recommended
- **CPU:** Multi-core processor recommended

---

## 🔙 Navigation

- [← Back to Home](Home)
- [← ADB Commands](ADB-Commands)
- [Claude AI Instructions →](Claude-AI-Instructions)
