---
title: "Flash Writer"
subtitle: "Implemented a Flash writer program"
excerpt: "Implemented a Flash writer program"
date: 2024-04-29
author: "Myung Guk Lee"
draft: false
images:
  - /career/assets/tachyons-thumbnail.png
  - /career/assets/tachyons-logo-script-feature.png
series:
  - Getting Started
tags:
  - hugo-site
categories:
  - Theme Features
# layout options: single or single-sidebar
layout: single
---


## Implmented a flash writer.

When we first bring up the board and nothing has been written to the flash memory, we need an application to write to the flash memory that will use the secondary boot loader to the flash memory. The Flash writer I developed does this. 

### Boot Sequence
The bootloader in ROM has the simplest structure possible due to the ROM read-only nature and small size. The main purpose of this ROM bootloader is to load a secondary bootloader, RTOS, or a bare-matal program. In multi-core platforms, a secondary bootloader is required after the boot ROM bootloader. The reason is that the boot ROM's bootloader does not recognize any core other than the main core. So, the second bootloader works on booting other cores. The problem is that the secondary bootloader must be written in a flash memory in advance so that the first bootloader can load the secondary bootloader from flash memory into memory. The flash writer is responsible for writing a secondary bootloader to the flash memory.
![screenshot](/img/boot-sequence.png)
If there is nothing in the flash memory to load, then ROM Bootloader open a serial(UART) port and download a flash writer binary / load it into RAM. Once the download completed, ROM Bootloader kick in the flash writer.
![screenshot](/img/download-flashwriter.png)
The Flash writer also open the serial(UART) port to download SBL from the host PC. Once it connected with the host, download SBL and write it into the flash memory. Once the download completed, reset the platform to start from ROM bootloader.
![screenshot](/img/download-sbl.png)
The SBL downloads RTOS or Bare Metals if there is nothing in the flash memory.
![screenshot](/img/download-rtos.png)


### How to implment the flash writer
- Step 1: Active SPI HW block and Read SFDP(Serial Flash Discoverable - Parameter) on the flash memory
- Step 2: Allocate Input/Output Queue memory for Serial IO communication, and init UART port.
- Step 3: Init the UART port such as baud rate, pins enable and register the Input/Output queue with TX/RX DMA channels.
- Step 4: Open the UART port and waiting a message from an host PC. 
- Step 5: When a message is received, it decodes the command ID and processes it according to the command ID.
- Step 6: All messages are transfered from the PC successfully, then try to load the SBL written on the flash memory into a RAM memory.
- Step 7: Start the SBL by call from the entry point of the SBL loaded into the RAM memory.