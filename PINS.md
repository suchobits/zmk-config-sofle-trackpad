# Pin Configuration

## RGB LEDs (Both Sides)

```
Pin: P0.08 (GPIO 8)
Location: boards/nice_nano_v2.overlay
Count: 29 per side (58 total)
```

**Configuration:**
```c
psels = <NRF_PSEL(SPIM_MOSI, 0, 8)>;  // P0.08
chain-length = <29>;                   // 29 LEDs per side
```

---

## HolyKeebs Trackpad (Right Side Only)

```
SDA:  P0.17 (I2C Data)
SCL:  P0.20 (I2C Clock)
VCC:  3.3V power
GND:  Ground
```

**Why only 2 data pins?**
The HolyKeebs adapter module handles RESET and RDY signals internally. You only need to connect:
- SDA (data)
- SCL (clock)  
- VCC (power)
- GND (ground)

**Configuration:**
```c
// In sofle_custom_right.overlay
&pro_micro_i2c {
    status = "okay";
    
    tps43: iqs5xx@74 {
        compatible = "azoteq,iqs5xx";
        reg = <0x74>;  // I2C address
        // SDA and SCL are auto-configured
    };
};
```

---

## Complete Pin Assignment

### Right Side (Central - has trackpad)

| Component | Pin | Purpose |
|-----------|-----|---------|
| LED Data | P0.08 | WS2812 RGB control |
| Trackpad SDA | P0.17 | I2C data |
| Trackpad SCL | P0.20 | I2C clock |
| Matrix rows | P0.05, P0.06, P0.07, P0.08 | Keyboard matrix |
| Matrix cols | Various | Keyboard matrix |

### Left Side (Peripheral)

| Component | Pin | Purpose |
|-----------|-----|---------|
| LED Data | P0.08 | WS2812 RGB control |
| Matrix rows | P0.05, P0.06, P0.07, P0.08 | Keyboard matrix |
| Matrix cols | Various | Keyboard matrix |

---

## Verification Checklist

Before flashing, verify your physical connections:

### RGB LEDs
- [ ] LED strip data wire connected to P0.08 (both sides)
- [ ] LED strip power connected to VCC
- [ ] LED strip ground connected to GND
- [ ] Counted LEDs: 29 per side?

### Trackpad (Right Side Only)
- [ ] Trackpad SDA connected to P0.17
- [ ] Trackpad SCL connected to P0.20
- [ ] Trackpad VCC connected to 3.3V
- [ ] Trackpad GND connected to ground
- [ ] Module is HolyKeebs adapter (not bare TPS43)

### General
- [ ] Both controllers are nice!nano v2 or compatible
- [ ] Power switches work (if present)
- [ ] Reset buttons accessible

---

## If You Need to Change LED Count

**If you have a different number of LEDs per side:**

1. Edit: `config/boards/shields/sofle_custom/boards/nice_nano_v2.overlay`
2. Find: `chain-length = <29>;`
3. Change to your actual count (per side, not total!)
4. Example: 27 LEDs per side = `chain-length = <27>;`

---

## Common LED Counts

| Total LEDs | Per Side | chain-length value |
|------------|----------|-------------------|
| 20 | 10 | `<10>` |
| 54 | 27 | `<27>` |
| 58 | 29 | `<29>` ← Your config |
| 60 | 30 | `<30>` |

---

## If Trackpad Doesn't Respond

**Check I2C address:**

The TPS43 uses address `0x74` by default. If your module is different:

1. Edit: `sofle_custom_right.overlay`
2. Find: `reg = <0x74>;`
3. Change to your address (e.g., `<0x75>`)

**Common I2C addresses for Azoteq:**
- TPS43: `0x74`
- TPS65: `0x74`
- Some variants: `0x75`

---

## Pin Number Reference

**nice!nano v2 pin mapping:**

```
Pro Micro Pin → nRF52840 GPIO

D0  = P0.08  ← Your LED pin!
D1  = P0.06
D2  = P0.17  ← Trackpad SDA
D3  = P0.20  ← Trackpad SCL
D4  = P0.22
D5  = P0.24
D6  = P0.09
D7  = P1.00
D8  = P0.11
D9  = P0.13
```

---

## No Changes Needed!

**This configuration is ready to flash as-is.**

Your hardware matches the pre-configured pins:
✅ P0.08 for LEDs  
✅ P0.17/P0.20 for trackpad  
✅ 29 LEDs per side  

**Just flash and test!**
