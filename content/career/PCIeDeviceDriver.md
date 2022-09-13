---
title: "PCIe Device Driver"
subtitle: "PCIe AHCI device driver development"
excerpt: "I was responsible for developing the PCIe AHCI Device Driver in the Hybrid HDD team."
date: 2022-08-30
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


### Participated in the SSHDD development.

A small-capacity SSD inside the HDD is a drive connected by a PCIe bus. So, frequently read data is stored on the SSD, so it is a drive with improved speed compared to the existing HDD. My part is PCIe AHCI drive driver development, because the SSD was used AHCI connected through PCIe bus.

### Steps for implment the PCIe AHCI device driver
Assume the PCIe End point(EP) address is hardcorded to 0x10000.
![screenshot](/img/PCIeOverall.png)

#### Register PCIe AHCI driver

1. Read the PCI configuration ( such as VendorID, DeviceID and so on.)
2. Write the configuration (such as bus master(DMA) enable and so on)
3. Map the base address Registers to the system address.

![screenshot](/img/BarMap.png)

#### Now we can config a SATA port at the AHCI.

4. Port register initialization
5. Setup command list, FIS base address
6. Setup Interrupt (I didn't use MSI interrupt due to OS did not support it, so I used the regacy interrupt)
7. Confirm there is a SATA device is connected through the Port's Task File and ATA Status register.

### Packet Flow
In order to understand the connection and data flow between SSD and HDD, it was analyzed with a bus analyzer.

1. Link Trainning and Initialization(LTSSM) using Ordered-Set
 
![screenshot](/img/LTSSM.png)
```
  - Detection : Detect when an end point is present. 
  - Polling : Port transmits training ordered sets and responds to the received
  - Configuration : Both the transmitter and receiver are sending and receving data at the negotiated data rate.
  - LO : Normal data transfer state, once it reacehed the L0, which means Root complextor and the endpoint can communicate successuflly.
  - Recovery : This state is used for a variety of reasons, such as changing back from a low-power Link state, like L1 or L2, or changing the link Bandwidth. 
```


2. Packet Transfer using TLP and DLLP

When transmitting data, TLP and DLLP packets are used. When the two devices are connected through Ordered Set, they exchange necessary data through TCL and DLLP.

![screenshot](/img/PCIePacketTrans.png)

