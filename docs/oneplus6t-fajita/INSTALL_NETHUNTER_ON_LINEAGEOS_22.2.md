# Install Kali NetHunter on LineageOS (OnePlus 6T / fajita)

This guide assumes you already have **LineageOS 22.2 (Android 15)** running on your OnePlus 6T.

If you still need to install LineageOS first, follow:

- [Install LineageOS 22.2 on OnePlus 6T](INSTALL_LINEAGEOS_22.2_ON_ONEPLUS6T.md)

## Table of Contents

- [Prerequisites](#prerequisites)
- [0) What you already have in this repo](#0-what-you-already-have-in-this-repo)
- [1) Pick your NetHunter edition](#1-pick-your-nethunter-edition)
- [2) Option A — NetHunter Rootless (recommended)](#2-option-a--nethunter-rootless-recommended)
- [3) Option B — Rooted NetHunter (Lite & Full)](#3-option-b--rooted-nethunter-lite--full)
- [4) Post-install checklist](#4-post-install-checklist)
- [5) Official reference docs](#5-official-reference-docs)

## Prerequisites

- **LineageOS 22.2** installed and booting successfully
- **Magisk (root)** installed — this is done during the LineageOS installation process (see Section 9 of the LineageOS guide)
- Verify root is working by opening the Magisk app and confirming "Installed" status

> Note
>
> Root access is **required** for both **NetHunter Lite** and **NetHunter Full (Custom Kernel)** options. Only the **Rootless** option works without root, but it has limited features.

---

## 0) What you already have in this repo

### NetHunter packages

Located in `Nethunter/`:

- `Nethunter/Generic_ARM64/kali-nethunter-2025.4-generic-arm64-full.zip`
- `Nethunter/Custom_Kernel_A15/nethunter-20251229_135544-oneplus6-los-fifteen.zip`

> Note about the “oneplus6” kernel zip
>
> That filename says `oneplus6` (not `oneplus6t`). Do **not** flash device-specific kernel zips unless you are certain they are compatible with **fajita**.

---

## 1) Pick your NetHunter edition

NetHunter has three “levels”:

- **Rootless**: easiest, no root required (recommended for most users)
- **Lite**: rooted + custom recovery (NetHunter app features, but no NetHunter kernel)
- **Full**: rooted + NetHunter kernel (adds kernel features; Wi‑Fi injection/HID depend on kernel/hardware)

If your goal is “Kali tools + KeX desktop”, **Rootless** is usually enough.

---

## 2) Option A — NetHunter Rootless (recommended)

This follows the official Kali docs flow (Termux + proot container).

### 2.1 Install apps

1. Install the NetHunter Store app:
   - https://store.nethunter.com/

2. From NetHunter Store, install:
   - Termux
   - NetHunter KeX
   - Hacker’s Keyboard

### 2.2 Install the NetHunter rootless environment

Open Termux and run:

```sh
termux-setup-storage
pkg update -y
pkg install wget -y
wget -O install-nethunter-termux https://offs.ec/2MceZWr
chmod +x install-nethunter-termux
./install-nethunter-termux
```

### 2.3 Usage

```sh
nethunter
nethunter kex passwd
nethunter kex &
```

---

## 3) Option B — Rooted NetHunter (Lite & Full)

> Your device should already be rooted with Magisk from the LineageOS installation. If not, refer to Section 9 of the [LineageOS installation guide](INSTALL_LINEAGEOS_22.2_ON_ONEPLUS6T.md).

### 3.1 Verify root access

Before proceeding, confirm Magisk is working:

1. Open the **Magisk** app.
2. Check that it shows "Installed" with a version number.
3. (Optional) Use a root checker app to verify root access.

### 3.2 Flash NetHunter Zip (Lite or Full)

If you are flashing a NetHunter zip via recovery, ensure it is compatible with your Android/ROM.

#### For NetHunter Lite (Generic):
- Use `Nethunter/Generic_ARM64/kali-nethunter-2025.4-generic-arm64-full.zip`
- This provides the NetHunter apps and overlay but **keeps the LineageOS kernel**.

#### For NetHunter Full (Custom Kernel):
- Use `Nethunter/Custom_Kernel_A15/nethunter-20251229_135544-oneplus6-los-fifteen.zip`
- This provides apps, overlay, **AND** the custom kernel for Wi-Fi injection/HID support.

> **Warning**: Only flash the custom kernel if you are sure it matches your ROM version (LineageOS 22.2 / Android 15).

### 3.3 Installation Steps (Recovery Sideload)

1. Boot to recovery (LineageOS Recovery or TWRP).
2. Start **ADB sideload** in recovery.
3. On your PC, sideload the zip you chose above:

```powershell
# Example for Custom Kernel (Full)
adb sideload Nethunter/Custom_Kernel_A15/nethunter-20251229_135544-oneplus6-los-fifteen.zip

# OR Example for Lite
adb sideload Nethunter/Generic_ARM64/kali-nethunter-2025.4-generic-arm64-full.zip
```

4. If the recovery warns about signature verification, choose the option to continue only if you trust the zip you are flashing.
5. Reboot after installation completes.

If the zip fails to flash or the device becomes unstable, prefer **Rootless** on modern Android, or verify you have a device/ROM-matched NetHunter build.

Proceed to the post-install steps below.

---

## 4) Post-install checklist

If you are using Magisk modules:

1. Copy `Nethunter/Generic_ARM64/kali-nethunter-2025.4-generic-arm64-full.zip` to your phone.
2. Open Magisk → **Modules** → **Install from storage**.
3. Select `kali-nethunter-2025.4-generic-arm64-full.zip`.
4. Wait for the install to finish (it may take a while). Do not interrupt.
5. Reboot.


---

## 5) Official reference docs

- NetHunter overview and install: https://www.kali.org/docs/nethunter/
- Rootless install steps: https://www.kali.org/docs/nethunter/nethunter-rootless/
