# Quick Start - 5 Minutes to Flashing

## 1. Upload to GitHub (2 minutes)

1. Go to: https://github.com/zmkfirmware/unified-zmk-config-template
2. Click **"Use this template"** → **"Create repository"**
3. Name it anything, make it **Public**
4. Click **"Add file"** → **"Upload files"**
5. Drag ALL extracted files (keep folder structure!)
6. Click **"Commit changes"**

## 2. Wait for Build (5-10 minutes)

1. Click **"Actions"** tab
2. Wait for green checkmark ✅
3. Click on the workflow
4. Download **"firmware.zip"** from Artifacts
5. Extract the zip

## 3. Flash Right Side First! (1 minute)

1. Plug RIGHT half into USB
2. **Double-tap** reset button (quick!)
3. USB drive appears
4. Drag `sofle_custom_right-nice_nano_v2-zmk.uf2` to drive
5. Wait for drive to disappear

## 4. Flash Left Side (1 minute)

1. Plug LEFT half into USB
2. **Double-tap** reset button
3. USB drive appears
4. Drag `sofle_custom_left-nice_nano_v2-zmk.uf2` to drive
5. Wait for drive to disappear

## 5. Test! (1 minute)

1. Power on both halves
2. Plug right side into computer (or pair Bluetooth)
3. Type on both sides → Should work!
4. Move finger on trackpad → Cursor moves!
5. Hold right side bottom row (2nd from right) + top left key → All 58 LEDs turn white!

---

## ⚠️ Important Notes

- **Flash RIGHT first** (it's the central controller)
- **Left auto-connects** to right wirelessly
- **All 58 LEDs start white** at 50% brightness
- **Trackpad is on right side only**

---

## 🎨 Control RGB LEDs

Hold **RAISE** (right side, bottom row, 2nd from right) and press:

- **Top left key**: Toggle LEDs on/off
- **Top row key 2**: Brightness up
- **Top row key 3**: Brightness down
- **Top row key 4**: Next color effect
- **Top row key 5**: Previous color effect

---

## 🔧 If Something Doesn't Work

### LEDs don't light up:
- Check physical connection to P0.08
- Try: Hold RAISE + top left key to toggle

### Trackpad doesn't work:
- Check SDA (P0.17) and SCL (P0.20) connections
- Module connected to RIGHT side only?
- Power (VCC/GND) connected?

### Halves won't pair:
1. Flash `settings_reset-*.uf2` to BOTH sides
2. Power cycle both
3. Reflash regular firmware

### Computer doesn't see keyboard:
- Only RIGHT side connects to computer
- Try USB cable first
- For Bluetooth: RAISE + BT1 (top row 2nd key)

---

## 📖 Want More Details?

See **README.md** for:
- Full customization guide
- Keymap editing
- RGB color customization
- Detailed troubleshooting

---

## ✅ Your Configuration

```
LED Pin:       P0.08 (both sides)
LED Count:     29 per side (58 total)
Trackpad SDA:  P0.17 (right only)
Trackpad SCL:  P0.20 (right only)
Start Color:   White at 50%
```

**Everything is pre-configured - just flash and use!**
