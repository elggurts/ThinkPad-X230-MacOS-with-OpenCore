#具有OpenCore的ThinkPad X230 MacOS

在ThinkPad X230上运行的MacOS（当前为Catalina`10.15.6`）

**状态：进行中**

<img align="right" src="https://ftp.bmp.ovh/imgs/2020/10/afdccda005d2aed8.png" alt="ThinkPad X230 Catalina" width="300"/>

[![ThinkPad](https://img.shields.io/badge/ThinkPad-X230-blue.svg)](https://psref.lenovo.com/syspool/Sys/PDF/withdrawnbook/ThinkPad_X230.pdf) [![release](https://img.shields.io/badge/Download-latest-brightgreen.svg)](https://github.com/banhbaoxamlan/X230-Hackintosh/releases/latest) [![OpenCore](https://img.shields.io/badge/OpenCore-0.6.1-blue.svg)](https://github.com/acidanthera/OpenCorePkg/releases/latest) [![MacOS Catalina](https://img.shields.io/badge/macOS-10.15.6-brightgreen.svg)](https://www.apple.com/macos/catalina/)

**免责声明：**
开始之前请阅读整个自述文件。我对您可能造成的任何损失不承担任何责任。

## 介绍

<details>

<summary><strong>我的硬件</strong></summary>

| Specifications      | Detail                                      |
| :------------------ | :------------------------------------------ |
| Computer model      | Lenovo ThinkPad X230 (Type: 2325)           |
| Processor           | Intel Core i7-3520M (2C4T, 2.9/3.6Ghz, 4MB) |
| Memory              | Crucial 16GB DDR3L 1867MHz, dual-channel    |
| Hard Disk           | Crucial BX500 3D-NAND 240GB                 |
| Integrated Graphics | Intel HD Graphics 4000                      |
| Display             | 12.5" HD (1366x768) TN - B125XW01.V0        |
| Audio               | Realtek ALC3202 (Layout-id: `18`)           |
| Ethernet            | Intel 82579LM Gigabit Network Connection    |
| WIFI+BT             | AzureWave AW-CB160H (BCM94360HMB)           |
| Keyboard            | 6-row, multimedia Fn keys, LED backlight    |
| Dock                | ThinkPad UltraBase Series 3                 |

</details>

<details>

<summary><strong>硬件兼容性</strong></summary>

This EFI will suit any X230 regardless of CPU model, amount of RAM, display resolution, and internal storage.

  1. Optional custom CPU Power Management guide (see below post-install)
  1. Modified
      - 1440p display models should change `NVRAM>>Add>>7C436110-AB2A-4BBB-A880-FE41995C9F82>>UIScale`: 2
      - X220 7-row keyboard should use : `SSDT-X220-KBD.aml`

</details>

<details>

<summary><strong>主要软件</strong></summary>

| Component      | Version           |
| :------------- | :---------------- |
| MacOS Catalina | 10.15.6 (19G2021) |
| OpenCore       | 0.6.1             |

</details>

<details>

<summary><strong>内核扩展</strong></summary>

| Kext                | Version |
| :------------------ | :------ |
| AirportBrcmFixup    | 2.0.9   |
| AppleALC            | 1.5.2   |
| BrcmPatchRAM        | 2.5.4   |
| EFICheckDisabler    | 0.5.0   |
| IntelMausi          | 1.0.3   |
| Lilu                | 1.4.7   |
| USBPorts            |         |
| VirtualSMC          | 1.1.6   |
| VoodooPS2Controller | 2.1.6   |
| WhateverGreen       | 1.4.2   |

</details>

<details>

<summary><strong>UEFI驱动程序</strong></summary>

| Driver          | Version           |
| :-------------- | :---------------- |
| HfsPlus.efi     | OcBinaryData      |
| OpenRuntime.efi | OpenCorePkg 0.6.1 |

</details>


## 安装

<details>

<summary><strong>如何安装macOS</strong></summary>

要安装macOS，请遵循提供的指南 [Dortania](https://dortania.github.io/getting-started/)

有用的工具 [CorpNewt](https://github.com/corpnewt) 和 [headkaze](https://github.com/headkaze/Hackintool)

完整的EFI可在 [releases](https://github.com/banhbaoxamlan/X230-Hackintosh/releases/latest) 页

</details>

<details>

<summary><strong>BIOS设置 :100:</strong></summary>

提供了一种安装修改后的BIOS的简单方法 [here](https://github.com/n4ru/1vyrain/) (无需外部编程器).

| Main | Sub #1                                 | Sub #2 | Sub #3 | Setting |
| :------------ | :----------- | ------------- | ------------- | ------------- |
| Config | Network | Wake On Lan |  | Disabled |
|  | Serial ATA (SATA) | Mode |  | AHCI |
| Advanced | System Agent (SA) configuration | Graphics Configuration | DVMT Pre-Allocated | 128MB |
|  |  |  | DVMT Total Gfx Mem | MAX |
| Security | Security Chip |  |  | Disabled |
|  | Memory Protection | Execution Prevention |  | Enabled |
|  | Anti-Theft | Current Setting |  | Disabled |
|  |  | Computrace | Current Setting | Disabled |
|  | Secure Boot |  |  | Disabled |
| Startup | UEFI/Legacy Boot |  |  | UEFI Only |
|  |  | CSM Support |  | Disabled |

</details>

## Post-install

<details>

<summary><strong>Generate your own SMBIOS</strong></summary>

For setting up the SMBIOS info, use [GenSMBIOS](https://github.com/corpnewt/GenSMBIOS)

- Run GenSMBIOS, pick option 1 for downloading MacSerial and Option 3 for selecting out SMBIOS

  - MacBookPro10,2

- Open `Config.plist`, find PlatformInfo >> Generic

  - The `Serial` part gets copied to SystemSerialNumber.

  - The `Board Serial` part gets copied to MLB.

  - The `SmUUID` part gets copied to SystemUUID.

**Reminder that you want either an invalid serial or valid serial numbers but those not in use, you want to get a message back like: "Invalid Serial" or "Purchase Date not Validated"** [Apple Check Coverage](https://checkcoverage.apple.com/)

</details>

<details>

<summary><strong>CPU power management</strong></summary>

Recommended additional steps to improve battery life with optimized CPU power management:

- Open Terminal, copy and paste the following command:

  ```bash
  curl -o ~/ssdtPRGen.sh https://raw.githubusercontent.com/Piker-Alpha/ssdtPRGen.sh/master/ssdtPRGen.sh
  chmod +x ~/ssdtPRGen.sh
  ./ssdtPRGen.sh
  ```

- A customized `SSDT.aml` for your specific machine will now be in the directory **/Users/yourusername/Library/ssdtPRGen**

- Rename to `SSDT-PM.aml` , and copy to **EFI/OC/ACPI/**

- Open `Config.plist`, enable `ACPI>>Add>>SSDT-PM.aml`

- Reboot

</details>

<details>

<summary><strong>USB ports map</strong></summary>

If you are using different model and alternative kext from Other folder does not work for you. Try:

- [USBMap](https://github.com/corpnewt/USBMap)

- [Hackintool](https://github.com/headkaze/Hackintool)

</details>

<details>

<summary><strong>Fully functioning multimedia Fn keys</strong></summary>

- Download and install [ThinkpadAssistant](https://github.com/MSzturc/ThinkpadAssistant/releases)
- Open the app and check the `launch on login` option

</details>

<details>

<summary><strong>Use PrtSc key as Screenshot shortcut</strong></summary>

- Go under `SystemPreferences > Keyboard > Shortcuts > Screenshots`
- Click on `Screenshot and recording options` key map
- Press `PrtSc` on your keyboard (it should came out as `F13`)

</details>

<details>  
<summary><strong>Mac bootloader GUI</strong></summary>

- Download [Binary Resources](https://github.com/acidanthera/OcBinaryData) and [OpenCanopy.efi](https://github.com/acidanthera/OpenCorePkg/releases)
- Copy the [Resources folder](https://github.com/acidanthera/OcBinaryData) to `EFI/OC`
- Add OpenCanopy.efi to `EFI/OC/Drivers`
- Make these changes inside `config.plist`:
    - `Misc >> Boot >> PickerMode`: `External`
    - `Misc >> Boot >> PickerAttributes`:`1`
    - `UEFI >> Drivers` and add `OpenCanopy.efi`

</details>

## Status

<details>
<summary><strong>What's working :white_check_mark:</strong></summary>

- [x] Battery Percentage
- [x] Bluetooth
- [x] Brightness
- [x] Camera
- [x] CPU Power Management
- [x] Dock Support `ThinkPad UltraSeries 3`
- [x] GPU Intel HD 4000 Graphics QE/CI
- [x] Intel Ethernet
- [x] Keyboard `Volume and brightness hotkeys`
- [x] Sleep/Wake
- [x] Sound `Automatic headphone detection, mute, volume controls fully working`
- [x] Touchpad `1-4 fingers swipe works`
- [x] TrackPoint  `Works perfectly. Just like on Windows or Linux`
- [x] eGPU  (Thanks [lese9855](https://github.com/lese9855) have confirmed it [#11](https://github.com/banhbaoxamlan/X230-Hackintosh/issues/11))

</details>

<details>

<summary><strong>What's not working :warning:</strong></summary>

- [ ] Fingerprint Reader
- [ ] VGA
- [ ] SD Card Reader (Disable with `SSDT-SDC.aml`)

</details>

<details>

<summary><strong>Bug tracker :heavy_exclamation_mark:</strong></summary>

- [ ] Trackpoint not working after wake from sleep

</details>

## Credits

[Apple](https://www.apple.com) for macOS

[Acidanthera](https://github.com/acidanthera) for all the kexts/utilities that they made

[Rehabman](https://github.com/RehabMan) and [Daliansky](https://github.com/daliansky) for the patches and guides and kexts

[George Kushnir](https://github.com/n4ru) for modified BIOS

[Dortania](https://github.com/dortania) for for the OpenCore Install Guide

[MSzturc](https://github.com/MSzturc) for ThinkpadAssistant

[simprecicchiani](https://github.com/simprecicchiani) for inspirational ThinkPad configurations
