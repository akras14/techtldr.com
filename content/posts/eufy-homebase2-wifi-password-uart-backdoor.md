---
title: "Eufy HomeBase 2 management Wi-Fi password derived from an I2C chip serial and HMAC-SHA256"
slug: "eufy-homebase2-wifi-password-uart-backdoor"
date: 2026-10-16T22:02:43+0000
summary: "Researchers desoldered the flash chip from a Eufy HomeBase 2 (firmware 3.4.2.2h), dumped it, and reverse engineered how the OCEAN_XXXXXX management Wi-Fi password is made. It is no longer the weak scheme from a 2024 paper: the password is an HMAC-SHA256 of a value read from a secure chip over I2C, keyed with the hub's serial number, then URL-safe base64 encoded. The serial follows a known template, so the unknown input is the I2C value. They also bypassed the UART lockdown by patching the intact recovery image to drop to a root shell."
source: "https://adepts.of0x.cc/eufy-homestation2-firmware/"
source_title: "A story of WiFi secrets, UART lockdowns and a damaged flash memory chip"
source_author: "Adepts of 0xCC"
source_site: "Adepts of 0xCC"
source_date: "2026-09-28"
hn_url: "https://news.ycombinator.com/item?id=50119751"
---

The authors dumped the firmware of a Eufy HomeBase 2 and found that its management Wi-Fi credentials are no longer trivially guessable, then defeated the device's UART lockdown to get a root shell for further vulnerability hunting.

**Extraction.** In-circuit reads of the Winbond 25Q256 flash failed, because powering the chip also woke other components on the board. Three reads gave three different checksums. They desoldered the chip and read it with a Raspberry Pi over SPI. One 64 KB block, at the normal image's load address, was damaged in the process. Binwalk and 7z recovered most of the filesystem.

**Wi-Fi password.** The SSID is `OCEAN_` plus part of the MAC address, and the password comes from an NVRAM variable. In the binary, `set_wifi_pwd` takes one of two paths depending on device model. On the path they analyzed, it reads 0x11 bytes from a chip over I2C (command 0xC1), computes HMAC-SHA256 with that value as the message and the hub serial number as the key, and URL-safe base64 encodes the result. The serial number follows a known template per device type, so the only unknown input is the I2C value. This differs from the weaker algorithm described in a 2024 paper.

**UART lockdown.** The console prompts "press Enter", then runs `/sbin/login.sh`, which only gives a login if the NVRAM `debug_log` is 1 or `/mnt/enable_console` exists. The other debug route is a custom signed protocol over ARP whose private key isn't in the firmware. Since the main image was damaged anyway, they patched the intact recovery image at 0xe70000. They changed the condition to always true and replaced `/bin/login` with `/bin/sh`, then fixed the U-Boot header CRCs. They reflashed the image with write protection disabled, resoldered the chip, and got a shell over USB-TTL. Nothing in the firmware was encrypted or signed to stop this.

The post goes on to cover the filesystem fixes needed to run the main binary on the recovery image. It builds on the authors' earlier doorbell article, "From your doorbell to your home network".
