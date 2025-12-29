# Install LineageOS 22.2 (Android 15) on OnePlus 6T (fajita)

This guide is written for the contents of this repo and targets:

- Device: **OnePlus 6T** (codename: `fajita`)
- ROM: **LineageOS 22.2** (Android 15)

> Warning
>
> - Unlocking the bootloader and installing a custom ROM will **wipe your phone**.
> - There is always a risk of a **soft-brick** if you flash the wrong images.
> - Read the whole guide once before starting.

---

## Table of Contents

- [Understanding the A/B Partition Scheme](#understanding-the-ab-partition-scheme)
- [0) Files you already have in this repo](#0-files-you-already-have-in-this-repo)
- [1) PC prerequisites (Windows)](#1-pc-prerequisites-windows)
- [2) Phone prerequisites](#2-phone-prerequisites)
- [3) Verify you’re really on OnePlus 6T (fajita)](#3-verify-youre-really-on-oneplus-6t-fajita)
- [4) Unlock the bootloader](#4-unlock-the-bootloader)
- [5) Boot TWRP (recovery)](#5-boot-twrp-recovery)
- [6) (Crucial) Install OxygenOS 11.2.2 to BOTH slots (A/B) via ADB sideload](#6-crucial-install-oxygenos-1122-to-both-slots-ab-via-adb-sideload)
- [7) (Optional) Make TWRP permanent](#7-optional-make-twrp-permanent)
- [8) Install LineageOS 22.2](#8-install-lineageos-222)
- [9) Install Magisk (Root)](#9-install-magisk-root)
- [10) Reboot and first boot](#10-reboot-and-first-boot)

---

## Understanding the A/B Partition Scheme

The OnePlus 6T uses an **A/B (seamless) partition scheme**, which differs from traditional A-only Android devices. Understanding this is important for successful flashing.

### What is A/B Partitioning?

A/B devices have two copies of critical partitions (called **Slot A** and **Slot B**). The system runs from one slot while the other remains inactive. This allows:

- **Seamless updates**: Updates install to the inactive slot in the background
- **Rollback protection**: If an update fails, the device boots back to the working slot
- **No dedicated recovery partition**: Recovery lives inside the `boot` partition

### OnePlus 6T Partition Layout

<table>
  <thead>
    <tr>
      <th>Partition</th>
      <th>A/B (Duplicated)</th>
      <th>Shared (Single)</th>
      <th>Purpose</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><code>boot</code></td>
      <td align="center">✅</td>
      <td align="center"></td>
      <td>Kernel + ramdisk + recovery</td>
    </tr>
    <tr>
      <td><code>system</code></td>
      <td align="center">✅</td>
      <td align="center"></td>
      <td>Android OS, system apps</td>
    </tr>
    <tr>
      <td><code>vendor</code></td>
      <td align="center">✅</td>
      <td align="center"></td>
      <td>Hardware-specific HALs/blobs</td>
    </tr>
    <tr>
      <td><code>dtbo</code></td>
      <td align="center">✅</td>
      <td align="center"></td>
      <td>Device tree blob overlays</td>
    </tr>
    <tr>
      <td><code>vbmeta</code></td>
      <td align="center">✅</td>
      <td align="center"></td>
      <td>Verified boot metadata</td>
    </tr>
    <tr>
      <td><code>modem</code></td>
      <td align="center">✅</td>
      <td align="center"></td>
      <td>Cellular radio firmware</td>
    </tr>
    <tr>
      <td><code>bluetooth</code></td>
      <td align="center">✅</td>
      <td align="center"></td>
      <td>Bluetooth firmware</td>
    </tr>
    <tr>
      <td><code>dsp</code></td>
      <td align="center">✅</td>
      <td align="center"></td>
      <td>Digital signal processor firmware</td>
    </tr>
    <tr>
      <td><code>xbl</code> / <code>abl</code></td>
      <td align="center">✅</td>
      <td align="center"></td>
      <td>Bootloader components</td>
    </tr>
    <tr>
      <td><code>tz</code> / <code>hyp</code></td>
      <td align="center">✅</td>
      <td align="center"></td>
      <td>TrustZone / Hypervisor</td>
    </tr>
    <tr>
      <td><code>userdata</code></td>
      <td align="center"></td>
      <td align="center">✅</td>
      <td>User data, apps, internal storage</td>
    </tr>
    <tr>
      <td><code>persist</code></td>
      <td align="center"></td>
      <td align="center">✅</td>
      <td>Device-specific calibration data</td>
    </tr>
    <tr>
      <td><code>misc</code></td>
      <td align="center"></td>
      <td align="center">✅</td>
      <td>Bootloader messaging</td>
    </tr>
    <tr>
      <td><code>op2</code></td>
      <td align="center"></td>
      <td align="center">✅</td>
      <td>OnePlus factory partition</td>
    </tr>
  </tbody>
</table>

### Key Points for Flashing

- **No separate recovery partition**: On A/B devices, recovery is embedded in `boot`. This is why we use `fastboot boot <recovery.img>` to temporarily boot TWRP.
- **Flashing affects one slot**: When you flash via fastboot, it goes to the current active slot unless you specify `_a` or `_b` suffix.
- **Both slots should match**: For stability, both slots should have consistent firmware. This guide covers flashing OxygenOS to both slots first.
- **Slot switching**: Use `fastboot set_active a` or `fastboot set_active b` to switch slots.
- **Check current slot**: Use `fastboot getvar current-slot` to see which slot is active.

---

## 0) Files you already have in this repo

### LineageOS 22.2 (fajita)
Located in `Lineage_OS_22/`:

- `lineage-22.2-20251223-nightly-fajita-signed.zip`
- `boot.img`
- `dtbo.img`
- `vbmeta.img`
- `Magisk.zip`

### Recovery (TWRP)
Located in `Oneplus/TWRP/`:

- `TWRP-3.6.2_11-OP6xT.img`
- `TWRP-3.6.2_11-OP6xT_INSTALLER.zip`

### OxygenOS 11 baseline (recommended)
Located in `Oneplus/ROM/`:

- `OnePlus6TOxygen_11.2.2_Oneplus6T_Fajita_A6010.zip`

### Emergency recovery tools (optional)
Located in `Oneplus/MSM_(Nuclear_Reset)/`:

- `OnePlus6T_MSM_OOS_10.zip`
- Qualcomm QDLoader driver installer

---

## 1) PC prerequisites (Windows)

1. Install **Android Platform Tools** (ADB/Fastboot).
2. (Optional) Install OnePlus/Qualcomm USB drivers (useful for EDL/MSM recovery):
   - `Oneplus/MSM_(Nuclear_Reset)/Drivers/…`
3. Use a known-good USB cable and a USB port directly on the PC (avoid hubs).

---

## 2) Phone prerequisites

On the phone (stock OxygenOS is fine for prep):

1. Backup everything you care about.
2. Enable Developer Options:
   - Settings → About phone → tap **Build number** 7 times
3. Enable:
   - **OEM unlocking**
   - **USB debugging**

---

## 3) Verify you’re really on OnePlus 6T (fajita)

With the phone booted into Android and plugged in:

```powershell
adb devices
adb shell getprop ro.product.device
adb shell getprop ro.build.product
```

You should see `fajita`.

---

## 4) Unlock the bootloader

> This will factory reset the phone.

1. Reboot to bootloader/fastboot:

```powershell
adb reboot bootloader
fastboot devices
```

2. Unlock (OnePlus devices commonly accept one of these):

```powershell
fastboot flashing unlock
```

If that fails:

```powershell
fastboot oem unlock
```

3. Confirm the unlock prompt on the phone.
4. Let the phone wipe and boot once.

---

## 5) Boot TWRP (recovery)

1. Reboot to bootloader again:

```powershell
adb reboot bootloader
fastboot devices
```

2. **Temporarily boot** the TWRP image:

```powershell
fastboot boot Oneplus\TWRP\TWRP-3.6.2_11-OP6xT.img
```

> Note
>
> If you have issues where recovery “sticks” (e.g., after hardware/display replacement), temporary booting recovery is often the safest route.

---

## 6) (Crucial) Install OxygenOS 11.2.2 to BOTH slots (A/B) via ADB sideload

This is an optional-but-recommended “baseline” step to make sure both slots are consistent before moving on to LineageOS.

> Important notes
>
> - Some OEM “full” OxygenOS packages can be **picky about recovery**.
> - If `adb sideload` fails in TWRP, the most reliable alternative is using the included MSM tool (`Oneplus/MSM_(Nuclear_Reset)/`) to return to stock.
> - This process may wipe data (or you may choose to wipe later). Back up first.

### 6.1 Sideload to slot A

1. In TWRP:
   - Advanced → **ADB Sideload** → Swipe to Start

2. On your PC:

```powershell
adb devices
adb sideload Oneplus\ROM\OnePlus6TOxygen_11.2.2_Oneplus6T_Fajita_A6010.zip
```

3. Reboot to bootloader and set slot A active:

```powershell
adb reboot bootloader
fastboot set_active a
fastboot getvar current-slot
```

### 6.2 Sideload to slot B

1. Switch to slot B and boot TWRP again:

```powershell
fastboot set_active b
fastboot getvar current-slot
fastboot boot Oneplus\TWRP\TWRP-3.6.2_11-OP6xT.img
```

2. In TWRP → Advanced → ADB Sideload → Swipe, then on PC:

```powershell
adb sideload Oneplus\ROM\OnePlus6TOxygen_11.2.2_Oneplus6T_Fajita_A6010.zip
```

3. Reboot back to bootloader:

```powershell
adb reboot bootloader
fastboot getvar current-slot
```

At this point both slots should be based on the same OxygenOS build, which reduces “slot mismatch” weirdness later.

---

## 7) (Optional) Make TWRP permanent

If you want a persistent recovery:

- In TWRP → Install → select `Oneplus/TWRP/TWRP-3.6.2_11-OP6xT_INSTALLER.zip` → Swipe

If you hit encryption/decryption issues in TWRP, you may need to **Format Data** (this wipes internal storage).

---

## 8) Install LineageOS 22.2

There are multiple valid install flows for A/B devices.

### 8.1 Wipe / format (recommended)

In recovery:

- Wipe → **Format Data** → type `yes`
- Wipe → Advanced Wipe → select **Dalvik/ART Cache**, **Cache**, **System** → Swipe

### 8.2 Flash boot / dtbo / vbmeta (fastboot method)

1. Reboot to bootloader:

```powershell
adb reboot bootloader
fastboot devices
```

2. Flash `vbmeta` with verity/verification disabled (commonly required for custom ROM installs):

```powershell
fastboot --disable-verity --disable-verification flash vbmeta Lineage_OS_22\vbmeta.img
```

> Note
>
> If you’ve previously had issues with the `vbmeta` step, proceed carefully and make sure the image matches your exact build.

3. Flash boot and dtbo (example shown for slot A):

```powershell
fastboot --set-active=a
fastboot flash boot Lineage_OS_22\boot.img
fastboot flash dtbo Lineage_OS_22\dtbo.img
```

4. Reboot to recovery:

```powershell
fastboot reboot recovery
```

### 8.3 Flash the LineageOS zip

You can install LineageOS via:

#### Option A: LineageOS Recovery sideload

- In LOS Recovery: Apply Update → **ADB Sideload**
- On PC:

```powershell
adb sideload Lineage_OS_22\lineage-22.2-20251223-nightly-fajita-signed.zip
```

If your display freezes in recovery, use the temporary-boot recovery method instead.

#### Option B: TWRP install

- In TWRP: Install → select `Lineage_OS_22/lineage-22.2-20251223-nightly-fajita-signed.zip` → Swipe

Then:

- Wipe cache/dalvik when prompted

---

## 9) Install Magisk (Root)

> Important
>
> Install Magisk **before** rebooting into the system for the first time. This ensures root is available immediately after LineageOS boots.

### Magisk zip

Located in `Lineage_OS_22/`:

- `Magisk.zip`

### 9.1 Flash Magisk via ADB sideload (LOS Recovery)

If you're in LineageOS Recovery:

1. Apply Update → **ADB Sideload**
2. On your PC:

```powershell
adb sideload Lineage_OS_22\Magisk.zip
```

3. If prompted about signature verification, choose to continue (the Magisk zip is not signed by LineageOS).

### 9.2 Flash Magisk via TWRP

If you're in TWRP:

1. Install → select `Lineage_OS_22/Magisk.zip` → Swipe to confirm
2. Wipe cache/dalvik when prompted

---

## 10) Reboot and first boot

- Reboot system

First boot can take 5–15 minutes.

After the first boot completes:

1. Open the **Magisk** app (it should be installed automatically).
2. If Magisk prompts you to complete setup or install the full app, follow the on-screen instructions.
3. Verify root by checking the Magisk home screen — it should show your Magisk version and "Installed" status.
