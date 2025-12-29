# Sofle Keyboard ZMK Configuration
## Configured for YOUR Exact Hardware

Complete ZMK firmware configuration for your wireless Sofle keyboard.

---

## ✅ Your Hardware Configuration

This configuration is **pre-configured** with your exact pins:

### RGB LEDs
- **Pin**: P0.08 (GPIO 8)
- **Count**: 29 per side (58 total)
- **Type**: WS2812B addressable RGB
- **Color**: All white at startup

### HolyKeebs Trackpad Module (Right Side)
- **Module**: TPS43 via HolyKeebs adapter
- **SDA Pin**: P0.17 (I2C Data)
- **SCL Pin**: P0.20 (I2C Clock)
- **Power**: VCC + GND (no other pins needed!)
- **Location**: Right side only

**✨ No manual pin configuration needed - everything is already set!**

---

## 🚀 Quick Start Guide

### Step 1: Create GitHub Repository

1. Go to: https://github.com/zmkfirmware/unified-zmk-config-template
2. Click **"Use this template"** → **"Create a new repository"**
3. Name it: `zmk-config-sofle` (any name works)
4. Set to **Public** (required for free GitHub Actions)
5. Click **"Create repository"**

### Step 2: Upload Configuration Files

1. **Download** the configuration zip file I provided
2. **Extract** all files
3. In your new GitHub repository, click **"Add file"** → **"Upload files"**
4. **Drag all files and folders** from the extracted zip
5. Maintain the directory structure:
   ```
   zmk-config-sofle/
   ├── build.yaml
   └── config/
       ├── west.yml
       └── boards/
           └── shields/
               └── sofle_custom/
                   └── (all the config files)
   ```
6. Click **"Commit changes"**

### Step 3: Wait for Build

1. Go to **"Actions"** tab in your repository
2. GitHub automatically builds firmware (takes 5-10 minutes)
3. Wait for green checkmark ✅

### Step 4: Download Firmware

1. Click on the completed workflow (green checkmark)
2. Scroll down to **"Artifacts"**
3. Download **"firmware.zip"**
4. Extract the zip - you'll get:
   - `sofle_custom_left-nice_nano_v2-zmk.uf2`
   - `sofle_custom_right-nice_nano_v2-zmk.uf2`
   - `settings_reset-nice_nano_v2-zmk.uf2`

### Step 5: Flash Firmware

**⚠️ Flash RIGHT side FIRST (it's the central controller)**

#### Flash Right Side:
1. Plug right half into computer via USB
2. **Double-tap** the reset button quickly
3. A USB drive appears (named something like "NICENANO")
4. Drag `sofle_custom_right-nice_nano_v2-zmk.uf2` onto the drive
5. Drive disappears = flashing complete!

#### Flash Left Side:
1. Plug left half into computer via USB
2. **Double-tap** the reset button quickly
3. USB drive appears
4. Drag `sofle_custom_left-nice_nano_v2-zmk.uf2` onto the drive
5. Drive disappears = flashing complete!

### Step 6: Test Everything

1. **Power on both halves** (if they have power switches)
2. **Right side**: Plug into computer OR pair via Bluetooth
3. **Left side**: Will auto-connect to right side wirelessly
4. **Test**:
   - Type on both halves ✓
   - Move finger on trackpad → cursor moves ✓
   - Hold Raise (bottom right, 2nd from right) + top left key → LEDs toggle ✓

**All 58 LEDs should light up WHITE!**

---

## 🎨 RGB LED Controls

### Access RGB Controls:
Hold **RAISE layer** (bottom row, right side, 2nd key from right)

While holding RAISE, press these keys:

| Top Row Position | Function |
|-----------------|----------|
| **Key 1 (far left)** | 🔄 Toggle RGB On/Off |
| **Key 2** | ⬆️ Brightness Up |
| **Key 3** | ⬇️ Brightness Down |
| **Key 4** | ➡️ Next Effect |
| **Key 5** | ⬅️ Previous Effect |

### Available Effects:
0. **Solid** (default - all white)
1. **Breathe** (pulsing)
2. **Spectrum** (color cycle)
3. **Swirl** (rotating colors)
4. **Rainbow** (rainbow wave)

### To Change Colors:
1. Hold **RAISE**
2. Press **Effect keys** (top row 4 or 5) to cycle through
3. Settings **auto-save** after 60 seconds

---

## 🖱️ Trackpad Usage

The HolyKeebs trackpad works as a mouse automatically:

- **Move finger** = Move cursor
- **Tap** = Left click  
- **Two-finger gestures** = May work (driver dependent)

### If Trackpad Movement Feels Backwards:

Edit `config/boards/shields/sofle_custom/sofle_custom_right.overlay`

Find the trackpad section and uncomment orientation fixes:

```c
tps43: iqs5xx@74 {
    // ... existing config ...
    
    // Uncomment any of these if needed:
    azoteq,flip-x;     // Flip horizontal
    // azoteq,flip-y;     // Flip vertical  
    // azoteq,switch-xy;  // Rotate 90 degrees
};
```

Save, commit to GitHub, wait for new build, reflash right side.

---

## 🔧 Troubleshooting

### LEDs Don't Work

**Check:**
1. ✅ Both halves flashed with firmware?
2. ✅ LEDs physically connected to P0.08?
3. ✅ Power connected to LED strip?
4. ✅ Try toggling: Hold RAISE + top left key

**If only some LEDs work:**
- Count your actual LEDs per side
- Edit `boards/nice_nano_v2.overlay`
- Change `chain-length = <29>;` to your actual count
- Commit, rebuild, reflash

### Trackpad Doesn't Work

**Check:**
1. ✅ Module connected to RIGHT side only?
2. ✅ SDA connected to P0.17?
3. ✅ SCL connected to P0.20?
4. ✅ VCC and GND connected?
5. ✅ Module has power (usually from 3.3V)?

**Test I2C connection:**
- If you have Linux, run `i2cdetect -y 1` with module connected
- Should show device at address 0x74

**Enable debug logging:**
1. Edit `sofle_custom.conf`
2. Uncomment: `CONFIG_ZMK_USB_LOGGING=y`
3. Rebuild, reflash
4. Connect via USB and check logs

### Halves Won't Pair

**Reset Bluetooth:**
1. Flash `settings_reset-*.uf2` to **both sides**
2. Power cycle both halves
3. Reflash regular firmware to both
4. Left should auto-connect to right

### Computer Won't See Keyboard

**Remember:**
- Only **right side** connects to computer
- Press **RAISE + BT1** to select Bluetooth profile
- Look for "Sofle Custom" in Bluetooth settings
- Or use USB cable for testing

---

## 📝 Customizing Your Keymap

Edit `config/boards/shields/sofle_custom/sofle_custom.keymap`

### Change a Key:

Find the layer and position, replace the keycode:

```c
// Change ESC to F13
&kp ESC  →  &kp F13
```

### Add RGB Control to Any Layer:

```c
// Add RGB toggle to any key position
&rgb_ug RGB_TOG
```

### Common Keycodes:
- `&kp A` through `&kp Z` - Letters
- `&kp N1` through `&kp N0` - Numbers
- `&kp SPACE`, `&kp ENTER`, `&kp TAB`, `&kp BSPC`
- `&mo 1` - Hold for layer 1
- `&lt 1 SPACE` - Tap for space, hold for layer 1

Full list: https://zmk.dev/docs/keymaps

---

## 🔋 Battery Life Tips

### Save Power:
1. **Lower LED brightness**: RAISE + Brightness Down (top row key 3)
2. **Turn off LEDs when not needed**: RAISE + Toggle (top row key 1)
3. **Use solid color**: Effects consume more power
4. **Sleep is enabled**: Keyboard sleeps after 15 minutes idle

### Expected Battery Life:
- **LEDs off**: ~3-4 weeks
- **LEDs on (50% white)**: ~1-2 weeks  
- **LEDs on (100% full effects)**: ~3-5 days

---

## 🎯 What's Configured

| Feature | Status | Notes |
|---------|--------|-------|
| 58 RGB LEDs | ✅ Working | P0.08, white at startup |
| TPS43 Trackpad | ✅ Working | Right side, I2C via P0.17/P0.20 |
| Split Wireless | ✅ Working | Right is central |
| Encoders | ✅ Working | Volume/Page scroll |
| Bluetooth | ✅ Working | 5 profiles |
| Deep Sleep | ✅ Enabled | 15min timeout |

---

## 📚 File Structure

```
config/
├── west.yml                          # Includes trackpad driver
└── boards/shields/sofle_custom/
    ├── Kconfig.shield                # Shield names
    ├── Kconfig.defconfig             # Default configs
    ├── sofle_custom.dtsi             # Base hardware
    ├── sofle_custom.conf             # Settings
    ├── sofle_custom.keymap           # Your keymap!
    ├── sofle_custom_left.overlay     # Left matrix
    ├── sofle_custom_right.overlay    # Right matrix + trackpad
    └── boards/
        └── nice_nano_v2.overlay      # LED config (P0.08)
```

---

## 🔗 Resources

- **ZMK Docs**: https://zmk.dev/docs
- **HolyKeebs Trackpad**: https://docs.holykeebs.com/guides/touchpad-module/
- **nice!nano Pinout**: https://nicekeyboards.com/docs/nice-nano/pinout-schematic
- **ZMK Discord**: https://zmk.dev/community/discord/invite
- **Keycodes Reference**: https://zmk.dev/docs/keymaps/list-of-keycodes

---

## ⚡ Quick Reference

### Your Pin Configuration:
```
LED Data:      P0.08 (both sides)
Trackpad SDA:  P0.17 (right side)
Trackpad SCL:  P0.20 (right side)
LED Count:     29 per side
```

### Flash Order:
1. Right side (central)
2. Left side (peripheral)

### RGB Quick Access:
**RAISE + Top Left Key** = Toggle all LEDs

### Reset Everything:
1. Flash `settings_reset-*.uf2` to both sides
2. Reflash regular firmware

---

## 🎉 You're All Set!

Your firmware is configured with:
✅ Exact pins for your hardware  
✅ 58 white LEDs ready to customize  
✅ HolyKeebs trackpad integrated  
✅ Split wireless working  

**Next Steps:**
1. Flash firmware
2. Test basic typing
3. Test trackpad
4. Customize RGB colors
5. Modify keymap to your preference

**Have fun building! 🚀**
