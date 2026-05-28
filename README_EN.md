<div align="center">

# YellowKey - BitLocker Bypass Vulnerability

[![English](https://img.shields.io/badge/English-README-blue)](README_EN.md)
[![中文](https://img.shields.io/badge/中文-README-green)](README.md)

> ⚠️ **Disclaimer:** This project is for security research and authorized penetration testing only. Users must comply with local laws and regulations. The author is not responsible for any misuse.

</div>

## Overview

**YellowKey** is a critical security vulnerability affecting Windows BitLocker encryption. Attackers can gain unrestricted access to BitLocker-protected volumes without requiring a password.

The vulnerability resides in a special component within the Windows Recovery Environment (WinRE). The original author discovered that its behavior is extremely suspicious, almost like a **backdoor**.

---

## Affected Systems

| System | Status |
|--------|--------|
| Windows 11 | ✅ Affected |
| Windows Server 2022 | ✅ Affected |
| Windows Server 2025 | ✅ Affected |
| Windows 10 | ❌ Not Affected |

---

## How to Reproduce

### Prerequisites

- A USB storage device (NTFS preferred, FAT32/exFAT also works)
- A Windows 11 / Server 2022/2025 device with BitLocker enabled
- The `FsTx` folder from this repository

### Steps

1. **Prepare USB:** Copy the `FsTx` folder to `YourUSBStick:\System Volume Information\FsTx`
2. **Insert USB:** Plug the USB into the target computer with BitLocker enabled
3. **Enter WinRE:** Hold `SHIFT` while clicking Restart
4. **Trigger:** After clicking Restart, release `SHIFT` and hold `CTRL` (do NOT release)
5. **Shell Access:** A shell will spawn with unrestricted access to the BitLocker-protected volume

![Shell Screenshot](https://github.com/user-attachments/assets/eda6c823-4a6b-4aec-bad2-b9afad640dd6)

---

## Why Backdoor?

- The component responsible for this bug exists **only** in WinRE images
- The same component with the same name exists in normal Windows installations, but without the bypass functionality
- Only Windows 11 and Server 2022/2025 are affected, Windows 10 is not

---

## Credits

Thanks to **MORSE**, **MSTIC**, and **Microsoft GHOST** for making this public disclosure possible.

---

## License

[MIT License](LICENSE)

---

## Legal Notice

This project is for:
- ✅ Security research and vulnerability analysis
- ✅ Authorized penetration testing
- ✅ Security training and education

**Strictly prohibited:**
- ❌ Unauthorized system access
- ❌ Any illegal intrusion
- ❌ Malicious data theft

Users are responsible for their own legal compliance.
