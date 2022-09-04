---
title: "Web RTC"
subtitle: "Real-time video communication in heterogeneous periods via p2p."
excerpt: " Now, it is made to develop easily p2p video communication in browser through java script api. What I did as a personal project was to develop an android application by changing the web rtc open source. And communication is implemented between the android native application and the web application developed using the web rtc javascript api."
date: 2022-08-30
author: "Myung Guk Lee"
draft: false
images:
  - /portfolio/assets/tachyons-thumbnail.png
  - /portfolio/assets/tachyons-logo-script-feature.png
series:
  - Getting Started
tags:
  - hugo-site
categories:
  - Theme Features
# layout options: single or single-sidebar
layout: single
---

### [Web RTC](https://github.com/truelinker/webrtc_p2pvideo)
Using the WebRTC open source, I'd modified to implment my own p2p video chat application between the android native app and chrom web app.
> Github : https://github.com/truelinker/webrtc_p2pvideo

## How it works
1. They connect to a room created by a server such as node.js and exchange information such as each other's network information (ip, port).
2.  When starting a video call, the call starts by exchanging each other's media information based on the given network information.
![How it works](/portfolio/assets/howWebRTCworks.jpg)
---
The following video shows the implementation.
[![Demo video](/portfolio/assets/webrtc_demo.jpg)](https://www.youtube.com/watch?v=PuphKiK7xmE "Demo Video")

