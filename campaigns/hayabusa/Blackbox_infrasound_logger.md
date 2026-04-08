---
title: "Blackbox Infrasound Logger"
parent: "Hayabusa"
grand_parent: "Special Campaigns"
---
These are designed to go where there is internet access. They have to be
co-located with a DFNEXT observatory, as the DFNEXT can easily provide
Wi-Fi with internet patch-through.

# Turn on for config

Things required:

- USB hub
- keyboard
- (mouse)
- HDMI cable
- portable screen

Sequence:

- Do not plug in the USB hub at boot time.
- plug in HDMI cable, system goes straight into X mode.
- plug in USB-C power cable
- no password required
- once booted: plug in USB hub with keyboard, mouse.

# Turn on for operation

Plug in:

- GPS antenna
- WiFi antenna
- sensor (via RS232)
- last: power via USB-C cable

If configured properly, the system will auto connect to the designated
WiFi, and it can be accessed remotely.

![infra_blackbox_connections.jpg]({{ site.baseurl }}/images/Infra_blackbox_connections.jpg)

# Turn off

- Simply unplug power (a safe shutdown is performed automatically by the
  box when it looses power)

# WiFi confiduration

to configure Wi-Fi once the RPi is booted:

- Open a terminal
- "sudo raspi-config"
- select "Network Options"
- select WiFi
- SSID: "DFNEXTXXX" XXX= DFNEXT system number (written on the box)
- password: not provided here.
- save
- "ping www.google.com" to confirm