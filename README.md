
# Kali NetHunter on OnePlus 6T (fajita)

![alt text](https://gitlab.com/kalilinux/nethunter/build-scripts/kali-nethunter-project/-/raw/main/images/nethunter-git-logo.png)

This workspace contains ROM, recovery, and NetHunter assets drive link for OnePlus 6T.

## Guides

- [Install LineageOS 22.2 on OnePlus 6T (fajita)](docs/oneplus6t-fajita/INSTALL_LINEAGEOS_22.2_ON_ONEPLUS6T.md)
- [Install Kali NetHunter on LineageOS (OnePlus 6T / fajita)](docs/oneplus6t-fajita/INSTALL_NETHUNTER_ON_LINEAGEOS_22.2.md)

## Repo contents (high level)

Here is the Drive [Folder Link](https://drive.google.com/drive/folders/164K7PJe4b6fSHYCbrofZyhAs9-mMzQrK?usp=sharing
) to download all files, inside of that you will find the following folders:

- `Lineage_OS_22/` — LineageOS 22.2 (fajita) images + Magisk zip
- `Oneplus/TWRP/` — TWRP image + installer zip
- `Nethunter/` — NetHunter zips (generic + custom-kernel builds)
- `Oneplus/MSM_(Nuclear_Reset)/` — MSM/EDL recovery tooling (use with caution)

## Tips / Recovery

- If your recovery environment is glitchy or gets “stuck” (common after some display replacements, e.g. FocalTech panels), prefer **temporary booting** recovery via `fastboot boot ...` instead of permanently flashing recovery.
- If you hard-brick/soft-brick at any point, you can restore the phone using the **MSM tool** included under `Oneplus/MSM_(Nuclear_Reset)/` (EDL reset option).

## Video Tutorials

The steps shown in Video is similar to the written guides above, but may differ in at some points so please follow the written guides for best results.

- OnePlus 6T / fajita - Install LineageOS 22.2 + Kali NetHunter
    - Part 1: 
        https://youtu.be/IPw7lJyKFMo?si=6ziCwUdjkIAV-DB7
    - Part 2:
        https://youtu.be/IPw7lJyKFMo?si=RiukQ0j56B5rVlKS


## Special Thanks
- [Ethical Explorers YouTube Channel](https://www.youtube.com/@Ethicalexplorers18) 
for their video tutorials on installing LineageOS and Kali NetHunter on OnePlus 6T (fajita).

- [Siddhesh Koyande](https://sourceforge.net/u/siddhrsh/profile/) For providing the Oneplus 6/6T [TWRP and Fastboot Files](https://sourceforge.net/projects/oneplus-6-series/files/)
