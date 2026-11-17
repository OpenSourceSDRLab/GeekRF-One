# Third-party notices

This repository and its release files contain work by OpenSourceSDRLab together with third-party software. The Apache License 2.0 in [LICENSE](LICENSE) applies **only** to the files created by OpenSourceSDRLab:

- `docs/`, `images/`, `README.md`
- the `README.txt` files inside the two zip files in Releases

All other material keeps its original copyright notices and licences:

| Component | Where | Owner | Licence |
|---|---|---|---|
| PlutoSDR firmware v0.37 build system | Releases (`plutosdr-fw-v0.37/`, `burn_flash/`) | Analog Devices, Inc. | see <https://github.com/analogdevicesinc/plutosdr-fw> |
| Linux kernel (`uImage`, `image.ub`, kernel in `pluto.dfu` and `mtd2`) | Releases | Linux kernel contributors, Analog Devices, Inc. | GPL-2.0 |
| U-Boot (`BOOT.BIN`, `boot.dfu`, `boot.frm`, `uboot-env.dfu`, `mtd0`, `mtd1`) | Releases | U-Boot contributors, Xilinx, Analog Devices | GPL-2.0 |
| Root file system in `pluto.dfu` / `mtd2`, built with buildroot | Releases | buildroot and the authors of each package (libiio, BusyBox, glibc ...) | GPL-2.0, LGPL and other open-source licences of each package |
| FPGA designs (bitstreams in `BOOT.BIN`, `pluto.dfu` and `mtd2`) | Releases | Analog Devices, Inc. (HDL), Xilinx/AMD (IP cores) | Analog Devices HDL licence; Xilinx/AMD licence terms |
| First-stage boot loader in `BOOT.BIN` | Releases | Xilinx/AMD | Xilinx/AMD licence terms |
| DFU update script `UPDATE.BAT` | Releases (`GeekRF-One_flash-firmware_v0.37.zip`) | Analog Devices, Inc. (PlutoSDR firmware project) | see <https://github.com/analogdevicesinc/plutosdr-fw> |

**Source code of the GPL components.** The firmware files were built from the Analog Devices PlutoSDR firmware v0.37, Linux, U-Boot and buildroot with changes for the GeekRF One hardware. The corresponding source code is not yet published in this repository. To receive it, write to support@opensourcesdrlab.com.

Software that is **not** included and is available from its owners:

- PlutoSDR USB drivers for Windows, libiio and IIO Oscilloscope: Analog Devices, <https://wiki.analog.com/university/tools/pluto/users>
- Kuiper Linux image for the TF card: Analog Devices, <https://wiki.analog.com/resources/tools-software/linux-software/adi-kuiper_images>
- Digilent USB driver (`install_digilent.exe`): Digilent Inc., included in AMD Vivado and Vivado Lab Edition
- Win32 Disk Imager, PuTTY, GNU Radio (radioconda), SDR# and its PlutoSDR plugin, SDRangel: from their own projects (links in the user manual)

"Analog Devices", "PlutoSDR", "ADALM-PLUTO", "AD9361", "AD9363", "FMCOMMS3", "Kuiper", "Xilinx", "AMD", "Zynq", "Vivado", "Digilent" and "MATLAB" are trademarks of their respective owners. They are used here only to describe compatibility and do not imply endorsement.
