---
title: GeekRF One Quick Start Guide
product: GeekRF One
doc_id: OSSDRLAB-GeekRF-One-QSG
version: 1.0
date: 2026-11-17
repo: https://github.com/OpenSourceSDRLab/GeekRF-One
---

<!-- ossl:header -->
<p><a href="https://www.opensourcesdrlab.com"><img src="../images/OpenSourceSDRLab-banner.jpg" alt="OpenSourceSDRLab" width="100%"></a></p>

# GeekRF One Quick Start Guide

| Product | Document | Version | Date |
|---|---|---|---|
| GeekRF One | OSSDRLAB-GeekRF-One-QSG | 1.0 | 2026-11-17 |
<!-- /ossl:header -->

## 1. Overview

The GeekRF One is a software-defined radio of the OpenSourceSDRLab PlutoSky series, derived from the Analog Devices ADALM-PLUTO. It has a Zynq XC7Z020, 512 MB DDR3, 32 MB QSPI flash, an AD9363 transceiver used over the AD9361 tuning range of 70 MHz to 6 GHz with 2 transmit and 2 receive channels, Gigabit Ethernet, a TF card slot and a 0.5 ppm TCXO.

The device can start in two ways:

- **From the TF card** (Analog Devices Kuiper Linux with the GeekRF One boot files). The card is prepared and tested before shipping.
- **From the on-board flash** (PlutoSDR firmware v0.37), without a TF card. SDR# and SDRangel use this mode.

This guide covers the connection methods, the TF card and re-installing the on-board flash firmware. The software tutorials for Windows and Ubuntu (IIO Oscilloscope, GNU Radio, SDR#, SDRangel, libiio, Ethernet set-up) are in the user manual: <https://blog.opensourcesdrlab.com/archives/geekrf-one-user-manual>.

## 2. What you need

| Item | Notes |
|---|---|
| GeekRF One | with the TF card shipped with it |
| USB Type-C cable | for the **OTG** port or the **Serial** port |
| Gigabit Ethernet cable | only for the Ethernet connection |
| PC | Windows 10/11 64-bit or Ubuntu 22.04 LTS |
| PlutoSDR USB drivers (Windows), libiio, IIO Oscilloscope | from Analog Devices: <https://wiki.analog.com/university/tools/pluto/users> |
| Serial terminal | for example PuTTY, 115200 baud |
| TF card reader and a disk-image writer (for example Win32 Disk Imager) | only to make a new TF card or re-install the flash firmware |
| Kuiper Linux image | only to make a new TF card; from Analog Devices: <https://wiki.analog.com/resources/tools-software/linux-software/adi-kuiper_images> |
| Firmware files | from the [Releases](../../../releases) page |

## 3. Connectors

| Connector | Function |
|---|---|
| TX1, RX1, TX2, RX2 | SMA RF ports |
| OTG (USB Type-C) | USB connection to the PC: network adapter, serial console and DFU firmware update of the PlutoSDR firmware |
| Serial (USB Type-C) | serial console (two COM ports appear on the PC) and USB-JTAG |
| ETH | Gigabit Ethernet |
| TF | TF (microSD) card slot |

## 4. First connection

**Method 1: Ethernet and the Serial port.** Connect the GeekRF One to the PC with an Ethernet cable and connect a USB cable to the **Serial** port. Two **USB Serial Port** COM ports appear in Device Manager; the console is on the second one. IIO Oscilloscope finds the device on the network; in GNU Radio the IIO context URI is `ip:analog.local` by default. To use a static address, see the user manual (section *Ethernet Connection Test*).

**Method 2: the OTG port only.** Connect one USB cable to the **OTG** port. After 10–20 seconds a **Generic Serial Console** COM port appears. IIO Oscilloscope finds the device over USB; in GNU Radio the IIO context URI is `usb:x.x.x` (find the address with `iio_info -s`, with `sudo` on Linux).

Check that the connection works with IIO Oscilloscope: connect, enable all channels and start the capture; the time-domain waveform is displayed.

> **Tip:** the sample rate must be between 520 833 and 61 440 000 samples per second; other values cause an error.

## 5. Make a new TF card

The TF card shipped with the device is ready to use. To make a new one:

1. Write the latest Analog Devices Kuiper Linux image to a TF card (for example with Win32 Disk Imager; it takes 10 to 15 minutes).
2. Unzip `GeekRF-One_TF-card-boot-files.zip` from the [Releases](../../../releases) page.
3. Copy `BOOT.BIN` and `image.ub` to the **BOOT** partition of the card, replacing the files there. (`uImage` is not needed.)
4. Insert the card into the TF slot of the GeekRF One.

## 6. Re-install the on-board flash firmware

> **Caution:** step 4 erases the whole on-board flash. Complete all steps; until the last step is done, the device cannot start from the flash.

You need a working TF card (section 5) and `GeekRF-One_flash-firmware_v0.37.zip` from the [Releases](../../../releases) page.

**Step 1.** Unzip `GeekRF-One_flash-firmware_v0.37.zip` and copy the `burn_flash` folder to the **BOOT** partition of the TF card.

**Step 2.** On Windows, install the PlutoSDR USB drivers from Analog Devices. If the USB Serial Port entries do not appear in Device Manager, also install the Digilent USB driver (`install_digilent.exe`, included in AMD Vivado and Vivado Lab Edition).

**Step 3.** Insert the TF card. Connect Ethernet and a USB cable to the **Serial** port, and open the console COM port in PuTTY (115200 baud).

**Step 4.** Power on the device and press any key before the U-Boot countdown ends. At the U-Boot prompt enter:

```
sf probe
sf erase 0x0 0x2000000
```

**Step 5.** Power the device off and on again. When the Linux prompt `root@analog:~#` appears, enter:

```
mount /dev/mmcblk0p1 /mnt
cd /mnt/burn_flash
chmod +x autoconfig.sh
./autoconfig.sh
sudo shutdown -h now
```

**Step 6.** Disconnect the Serial cable and remove the TF card. Connect a USB cable to the **OTG** port.

**Step 7.** When **USB download gadget** appears in Device Manager, open a terminal in the unzipped firmware folder and enter:

```
.\UPDATE.BAT .\plutosdr-fw-v0.37\pluto.dfu
```

**Step 8.** Wait until the firmware has been written, then power the device off and on again. The LED flashes and the GeekRF One starts from the on-board flash without a TF card.

The screenshots of every step are in the user manual, section *Flash the On-board Firmware*.

## 7. Firmware files

| File (in Releases) | Contents |
|---|---|
| `GeekRF-One_TF-card-boot-files.zip` | `BOOT.BIN` (boot loader, FPGA bitstream, U-Boot) and `image.ub` (Linux kernel and device tree) for the TF card; `uImage` (Linux kernel only) |
| `GeekRF-One_flash-firmware_v0.37.zip` | `burn_flash/` (`autoconfig.sh`, `mtd0`, `mtd1`, `mtd2`), `plutosdr-fw-v0.37/` (`pluto.dfu`, `boot.dfu`, `uboot-env.dfu`, `pluto.frm`, `boot.frm`) and `UPDATE.BAT` |
| `SHA256SUMS.txt` | SHA-256 checksums |

Licences of the firmware components: [NOTICE.md](../NOTICE.md).

## 8. Support

- Email: support@opensourcesdrlab.com
- Forum: <https://forum.opensourcesdrlab.com>
- Discord: <https://discord.gg/nnxY6DkmvT>
- Blog and user manuals: <https://blog.opensourcesdrlab.com>
- Product page: <https://opensourcesdrlab.com/products/geekrf-one-ad9363-zynq7020-plutosdr>

<!-- ossl:footer -->
---
<sub>© 2026 OpenSourceSDRLab. All rights reserved. · <a href="https://www.opensourcesdrlab.com">www.opensourcesdrlab.com</a></sub>
<!-- /ossl:footer -->
