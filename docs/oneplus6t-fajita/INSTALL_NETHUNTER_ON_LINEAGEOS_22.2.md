# Install Kali NetHunter on LineageOS (OnePlus 6T / fajita)

This guide assumes you already have **LineageOS 22.2 (Android 15)** running on your OnePlus 6T.

If you still need to install LineageOS first, follow:

- [Install LineageOS 22.2 on OnePlus 6T](INSTALL_LINEAGEOS_22.2_ON_ONEPLUS6T.md)

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

## 3) Option B — Rooted NetHunter (Lite-style)

> You will need root (e.g., Magisk) for the NetHunter app features that require root.

### 3.1 Root with Magisk (high level)

Because Magisk changes frequently, this stays high-level:

1. Download the latest Magisk APK from the official GitHub releases.
2. Patch your ROM `boot.img` in Magisk.
3. Flash the patched boot image in fastboot.

Example flow:

```powershell
adb pull /sdcard/Download/magisk_patched-*.img .
adb reboot bootloader
fastboot flash boot magisk_patched-*.img
fastboot reboot
```

### 3.2 (Custom Kernel) Flash a NetHunter zip

If you are flashing a NetHunter zip via recovery, ensure it is compatible with your Android/ROM.

This repo includes:

- `Nethunter/Custom_Kernel_A15/nethunter-20251229_135544-oneplus6-los-fifteen.zip`
- `Nethunter/Generic_ARM64/kali-nethunter-2025.4-generic-arm64-full.zip`

Typical flow (recovery sideload):

1. Boot to recovery (LineageOS Recovery or TWRP).
2. Start **ADB sideload** in recovery.
3. On your PC:

```powershell
adb sideload Nethunter/Custom_Kernel_A15/nethunter-20251229_135544-oneplus6-los-fifteen.zip
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
