# Flashing Guide — OnePlus SM8150 Kernel with KernelSU-Next

This guide explains how to flash the custom kernel on your OnePlus SM8150 device and set up KernelSU-Next for root access.

---

## Supported devices

| Device | Codename |
|--------|----------|
| OnePlus 7 | guacamoleb |
| OnePlus 7 Pro | guacamole |
| OnePlus 7T | hotdogb |
| OnePlus 7T Pro | hotdog |

---

## Prerequisites

- A custom recovery installed (e.g. [TWRP](https://twrp.me/))
- USB debugging enabled (Settings → Developer options → USB debugging)
- An unlocked bootloader
- At least 50% battery
- A backup of your important data

---

## Step 1 — Download the flashable zip

1. Go to the [Releases page](../../releases) of this repository.
2. Download the latest `AnyKernel3-sm8150-kernelsu-next-*.zip` file.

> Do **not** download `Image`, `Image.gz`, or `Image.gz-dtb` directly — these are raw kernel images and cannot be flashed on their own without AnyKernel3.

---

## Step 2 — Boot into TWRP (custom recovery)

**Option A — from a powered-off phone:**
1. Hold **Volume Down + Power** until the OnePlus logo appears, then release both buttons immediately.
2. The phone will boot into TWRP.

**Option B — using ADB:**
```bash
adb reboot recovery
```

---

## Step 3 — Flash the kernel zip

1. Transfer the downloaded `.zip` to your phone (via MTP or `adb push`):
   ```bash
   adb push AnyKernel3-sm8150-kernelsu-next-*.zip /sdcard/
   ```
2. In TWRP, tap **Install**.
3. Navigate to and select the `.zip` file.
4. Swipe to confirm the flash.
5. Once done, tap **Reboot System**.

---

## Step 4 — Install the KernelSU-Next manager app

1. Download the latest KernelSU-Next manager APK from:
   **https://github.com/rifsxd/KernelSU-Next/releases**
2. Install the APK on your device.
3. Open the app — it should display **"Working"** and show the kernel version.

> If the app shows **"Not Installed"**, the kernel was not flashed correctly. Repeat from Step 2.

---

## Step 5 — Grant root access to apps

1. Open the **KernelSU-Next** manager app.
2. Tap **SuperUser**.
3. When an app requests root access, approve it here.

---

## Notes

- **Magisk users:** KernelSU-Next and Magisk cannot be used simultaneously. Uninstall Magisk (or restore the stock boot image) before flashing this kernel. You can use Magisk's built-in "Restore Stock Boot Image" option if available.
- **OTA updates:** Flashing a system OTA update will overwrite this kernel. Re-flash after any OTA update.
- **Modules:** KernelSU-Next supports kernel modules similar to Magisk modules. Install them through the KernelSU-Next manager app.
- **Reverting:** To go back to the stock kernel, flash your device's stock firmware via [OnePlus's official tool](https://www.oneplus.com/support/softwareupgrade) or a stock boot image via fastboot:
  ```bash
  fastboot flash boot stock_boot.img
  ```

---

## Troubleshooting

| Symptom | Fix |
|---------|-----|
| Bootloop after flashing | Boot to TWRP, flash stock boot image |
| KernelSU-Next app says "Not Installed" | Re-flash the AnyKernel3 zip |
| KernelSU-Next app says "Incompatible" | Your device or ROM may not be supported |
| Device not detected in TWRP | Re-seat USB cable; try a different port/cable |

---

## Build information

This kernel is built automatically via GitHub Actions using:
- **Compiler:** Clang (LLVM) with LLD linker
- **Defconfig:** `vendor/sm8150_defconfig` + `vendor/oplus.config`
- **KernelSU-Next:** [rifsxd/KernelSU-Next](https://github.com/rifsxd/KernelSU-Next) (enabled via `CONFIG_KSU=y`)

Source code and full build logs are available in this repository.
