## Points of failure

In order for proper operation of the DFN observatory a few things need
to happen:

- The observatory box must be powered - LED indicator are lit (at least
  on the micro controller PCB)
- The embedded PC must boot successfully - [how to
  troubleshoot](#How_to_troubleshoot_PC_booting.md "wikilink")
- PC has to tell micro controller to power on
- DSLR has to power on
- PC has to tell microcontroller to start taking images
- Microcontroller has to trigger camera through optocoupler and shutter
  cable
- DSLR has to be configured properly to take photos via gPhoto2 over USB
- DSLR has to take photos - [how to
  fix](#Possible_cause_.26_fix_when_DSLR_is_not_taking_photos.md "wikilink")
- LC shutter has to flicker - [investigate LC shutter
  malfunction](investigate_LC_shutter_malfunction.md "wikilink")

## Initial Steps

The first task is to figure out what's not working, starting with the
most likely and easiest to check steps. If that doesn't work then we can
move on to the trickier steps later.

**Are the drives full?** Try `df -h` to see how full they are. This will
list a percentage full for all partitions (but not the external drives
if they aren't powered on and mounted).

## Simple check the camera system on remote site, reboot

### DFNSMALL system

#### Check the camera system status

Open the enclosure door - there is often a key hanging somewhere on the
camera stand. Or use long nose pliers. Inspect LED lights - Are any of
them lit?

If not, the problem might be in power. For mains powered sites, check
power cable, circuit breaker etc. If you have a voltmeter/multimeter,
disconnect the screw-in DC power cable from the bottom of the camera
system enclosure and check voltage - it should read ~ 12V.

If some indicators are lit, take a photo and e-mail it to us if unsure
what to expect.

*TODO: add photo of DFNSMALL camera system inside*

#### Reset the camera

Cycle the power of whole camera system.

There is a DC power cable connected to the bottom of the camera box - as
shown on the
photo![](DFN_camera_power_cable.jpg "DFN_camera_power_cable.jpg"). It is
the biggest connector, and it needs to be unscrewed before unplugging.
Once the power is disconnected, please wait for about 1 minute and then
connect it back, tightening it again. Shortly after re-connecting the
power, there should be a sound of a fan inside the box and a little
later a short beep sound like when PC is booting, and the fan should go
silent.

### DFNEXT system

#### Check the camera system status

The DFNEXT type of camera system has built-in WiFi, which is in most
cases configured to serve as an access point. That means, providing the
system is running, there is network DFNEXT0XX (XX depending on the unit
serial number). One can see this network with a phone, tablet or laptop.

If the WiFi network is detected, but the camera system is not
communicating remotely over the Internet, the best thing is to initiate
a reset in a web interface over WiFi.

#### Software reboot

Connect to the WiFi network and then open the [Web GUI
interface](Web_Interface.md "wikilink") and [reboot the
system](Using_the_GUI_for_Regular_Maintenance#Rebooting_the_system.md "wikilink").

#### Hard reset - cycling the power

If the WiFi network was not visible or the web interface reset was not
working, the other option is to power cycle the whole camera system.

There is a DC power cable connected to the bottom of the camera box - as
shown on the [photo](Media:DFN_camera_power_cable.jpg.md "wikilink"). It is
the biggest connector, and it needs to be unscrewed before unplugging.
Once the power is disconnected, please wait for about 1 minute and then
connect it back, tightening it again. Shortly after re-connecting the
power, there should be a sound of a fan inside the box and a little
later a short beep sound like when PC is booting, and the fan should go
silent.

## How to troubleshoot PC booting

1.  Power on the observatory box and try to locally connect with
    [ethernet
    cable](Logging_in_locally_via_WiFi_or_Ethernet#Ethernet.md "wikilink")
    or [WiFi](Logging_in_locally_via_WiFi_or_Ethernet#WiFi.md "wikilink")
    (if the box is is equipped with it, eg DFNEXT type)
    - for wired connection, make sure you using [the correct ethernet
      port](Logging_in_locally_via_WiFi_or_Ethernet#Ethernet.md "wikilink")
      for laptop connection. (Better try both if unsure.)
    - make sure the WiFi antenna is attached if you want to use WiFi -
      the white little stick on the bottom of the DFNEXT box.
    - remember it may take a few minutes to boot the camera OS since
      powering it on
    - for WiFi, the observatory is by default configured as an access
      point (2.4GHz 802.11g), with SSID same as the observatory host
      name, eg DFNEXT014 - try scanning for this SSID.
    - you can try to ping the corresponding IP address first before ssh
      connection - for wired


    `ping 172.16.1.101`

    or for WiFi

    `ping 172.16.0.101`
2.  If the previous step fails:

    It is possible that the filesystem got corrupted and the booting
    process is stuck at filesystem check & repair. That can happen if
    the observatory system was earlier powered off (unplugged) without
    shutting down the OS properly. In that case, user keyboard
    interaction is required, so you will need to:

    - connect USB keyboard
    - connect monitor (HDMI works with all cameras, some DFNSMALLs
      support also VGA)
      *please note that the monitor in most cases must be plugged in
      before powering on the camera. If the camera is already powered,
      connect the keyboard and monitor, hit Ctrl+Alt+Delete at least
      seven times and wait for the box to reboot. Only if that is not
      working, power off and power on the box to enforce reboot*
    - follow the instructions you will see on the screen
      root password will be required, then it is mostly hitting 'y' key
3.  If the rest of the system in the box seems to be working/powered,
    just the PC has no signs of life, check it is powered:
    - The power connector on the PC board may have bad connection.
    - In case of DFNSMALLs, there is either Advantech PC (black
      heatsink) or Commell LE-37D PC (small silver heatsink). The
      Advantech PC has a round DC power connector, which is prone to bad
      contact. Try to wiggle with is a bit, push it deeper. That often
      helps. On the other hand, the white plastic Molex connector of
      Commell LE-37D PC is very reliable.
    - In case of DFNEXTs, there is a screw terminal, where the screws
      can loosen while working on something else inside the camera
      enclosure or during transport. This is easy to fix by tightening
      the screws, just make sure the wires are well pushed into the
      terminal. It is also very easy to check the voltage on the
      terminal with multimeter - it should read approx. 12V DC.

## DSLR is not taking photos

power on the DSLR (or stop interval control service if running)

run command "lsusb", should list device like

`Bus 002 Device 004: ID 04b0:0436 `**`Nikon Corp.`**` `

if not, visit te site and check the wiring - USB data cable and power
cable to the DSLR

### Test if DSLR can capture images

if listed, run command "gphoto2 --capture-image-and-download"

### Samyang lens apperture ring not set correctly

if you get response

`root@DFNEXT077:/data0/latest# gphoto2 --get-config /main/capturesettings/f-number`
`Label: F-Number                                                                `
`Type: RADIO`
`Current: f/655.35`
`Choice: 0 f/655.35`

`root@DFNEXT077:/data0/latest# gphoto2 --capture-image`
`                                                                              `
`*** Error ***              `

`*** Error ***              `
`Camera Mode Not Adjust FNumber`
`ERROR: Could not capture image.`
`ERROR: Could not capture.`

that means the Aperture ring of the Samyang lens is in wrong position.

Set it to F/22 (which printed in red)

<figure>
<img src="Samyang_lens_800.jpg" title="File:Samyang lens 800.jpg" />
<figcaption><a href="File:Samyang">File:Samyang</a> lens
800.jpg</figcaption>
</figure>

- Photos have to be downloaded to the PC via gPhoto2 over USB
- Photos have to be stored in /data0 and renamed properly by the DFN's
  capture control software
- Photos have to be copied over to /data1, /data2 or /data3 by the data
  move script

### Images are dark

The images look consistently darker than they should (barely any star
visible).

One key indicator is when some bright light is in the field of view (the
Moon for example), and diffraction spikes appear (6 of them as the iris
has 6 blades), due to the tiny aperture:

<figure>
<img src="Moon_diffraction_spikes.jpg"
title="File:Moon_diffraction_spikes.jpg" />
<figcaption><a
href="File:Moon_diffraction_spikes.jpg">File:Moon_diffraction_spikes.jpg</a></figcaption>
</figure>

With the aperture set to f/22, the camera should be in control of the
lens aperture, and should open the iris to a wider aperture during
operations (f/4 normally). In some cases (could be a mechanical failure
of one of the parts in the aperture control), pictures are at f/22 (even
if the exif data in the pictures show f/4).

One solution is to replace the lens.

Another solution is to turn the lens into a dumb lens, telling the
camera that it cannot control the aperture, and manually setting the
lens aperture to f/4 or f/5.6 using the ring. The problem is it is not
possible to just tell the camera to ignore a CPU lens, so the electronic
connection between the camera and lens must be severed. An easy (and
reversible) way to do this is to tape the connectors on the lens side:

Cut a thin strip of tape (use nice tape that doesn't leave glue on the
surface):

<figure>
<img src="Lens_connectors_tape.jpg" title="Lens_connectors_tape.jpg"
width="300" />
<figcaption>Lens_connectors_tape.jpg</figcaption>
</figure>

And apply it to the connectors:

<figure>
<img src="Lens_connectors_taped.jpg" title="Lens_connectors_taped.jpg"
width="300" />
<figcaption>Lens_connectors_taped.jpg</figcaption>
</figure>

Then there are extra DSLR settings that must be set in order for the
correct metadata to be reported in the images:

<figure>
<img src="Non_CPU_lens_menu.jpg" title="Non_CPU_lens_menu.jpg"
width="200" />
<figcaption>Non_CPU_lens_menu.jpg</figcaption>
</figure>

<figure>
<img src="Non_CPU_lens_settings.jpg" title="Non_CPU_lens_settings.jpg"
width="200" />
<figcaption>Non_CPU_lens_settings.jpg</figcaption>
</figure>

Finally, manually set the aperture to f/4 (somewhere in between f/3.5
and f/5.6):

<figure>
<img src="Manual_aperture_set_f_4.jpg"
title="Manual_aperture_set_f_4.jpg" width="200" />
<figcaption>Manual_aperture_set_f_4.jpg</figcaption>
</figure>

Note that f/4 does not correspond to a click in the aperture, hence tape
the aperture ring in place once done, other it will wiggle itself to
either f/3.5 or f/5.6.

## DSLR memory card is full, download the images manually

This situation may occur for example when the regular interval capture
during night is interrupted (power loss, operator SW stop) or when all
the disks are full and no room is left to download images from the DSLR
internal storage.

The software will eventually cope with that (whatever images found on
the memory card DSLR are automatically downloaded at the beginning of
regular run in the evening. However, there may be situations when one
wants to put the files into specific directory - this section describes
how to do it manually or semi manually.

Check if DSLR memory card has any data on it by

` gphoto2 -L -R`

Long list of files means it may be full.

A very useful diagnostic command is

` gphoto2 --summary`

which (togethher with heaps of other info) prints the usage and free
space on the memory card:

` Storage Devices Summary:`
` store_00020001:`
`       StorageDescription: None`
`       VolumeLabel:  [Slot 2]`
`       Storage Type: Removable RAM (memory card)`
`       Filesystemtype: Digital Camera Layout (DCIM)`
`       Access Capability: Read Only with Object deletion`
`       Maximum Capability: 64014417920 (61048 MB)`
`       `**`Free Space (Bytes): 60096512 (57 MB)`**
`       `**`Free Space (Images): 0`**

This makes it clear the card is full.

To manually download the images and free up the card, there are two
options:

1\) Direct gphoto2 commands: 'gphoto2 -P -R' to download, followed by
delete 'ghoto2 -D -R' - then you can drop the images to the
corresponding (yesterday's or whatever) directory where they belong.

2\) May be better and easier is to run interval control test
/opt/dfn-software/interval_control_test.sh, that does everything like
regular interval control run at night, but it forces ~ 5 minutes test
run regardless the time of the day. Interval control test creates a new
directory wgere it dumps all the images on card in the beginning plus
the few taken during the short run.

## DSLR memory card file sytem is corrupted or card damaged

### Cannot delete images from card

The memory card problem can demonstrate by failing delete of all images
from the card after download. The Interval capture control SW uses the
following gphoto2 command internally and it is possible to run it
manually to troubleshoot the problem - which otherwise may demonstrate
as missing images from later part of the night.

`more than to directories with images listed`

`gphoto2 --delete-all-files -R`

or equivalent shorted

`gphoto2 -D -R`

in case of memory card FS problem we get

`*** Error ***              `
`PTP Device Busy`
`*** Error (-110: 'I/O in progress') ***       `

and the deleting fails.

Also two or more directories with images listed by

`gphoto2 -L -R`

`There is no file in folder '/'.                                                `
`There is no file in folder '/store_00020001'.`
`There is no file in folder '/store_00020001/DCIM'.`
`There are 999 files in folder '/store_00020001/DCIM/139ND810'.`
`#1     DSC_0042.NEF               rd 45431 KB application/x-unknown`
`#2     DSC_0043.NEF               rd 45458 KB application/x-unknown`
`#3     DSC_0044.NEF               rd 44759 KB application/x-unknown`
`...`
`#997   DSC_1038.NEF               rd 39808 KB application/x-unknown`
`#998   DSC_1039.NEF               rd 39851 KB application/x-unknown`
`#999   DSC_1040.NEF               rd 39710 KB application/x-unknown`
`There is no file in folder '/store_00020001/DCIM/140ND810'.`
`There is no file in folder '/store_00020001/DCIM/141ND810'.`
`There are 519 files in folder '/store_00020001/DCIM/142ND810'.`
`#1000  DSC_1041.NEF`
`#1001  DSC_1042.NEF               rd 39875 KB application/x-unknown`
`#1002  DSC_1043.NEF               rd 39764 KB application/x-unknown`
`...`

To fix this, [format the memory
card](Formatting_the_CF_Card.md "wikilink"). If the problem repeats,
replace the memory card and/or make sure the USB3 cable between the DSLR
and PC is in a good shape and not causing errors (syslog inspection
should reveal that if physical cable does not look damaged or sharp
bent).

## Solar Power

### Morning Star Sun Saver charging controller

[User manual - at manufacturer's
web](https://www.morningstarcorp.com/wp-content/uploads/2014/02/SS3.IOM_.Operators_Manual.01.EN_.pdf)

[User manual - at Battery Stuff
web](https://www.batterystuff.com/files/ss3-iom_operators_manual-01-en_.pdf)

**Status LED Error Indications**

|                               |                 |
|-------------------------------|-----------------|
| Solar overload                | Flashing Red    |
| High Voltage Disconnect       | Flashing Red    |
| High Temperature Disconnect   | Flashing Red    |
| Damaged local temp. sensor    | Solid Red *(1)* |
| Damaged heatsink temp. sensor | Solid Red *(1)* |
| Damaged input MOSFETs         | Solid Red *(1)* |
| Firmware Error                | Solid Red *(1)* |

*(1) - A heartbeat indication flickers the Status LED off briefly every
5 seconds. A solid red Status LED indicates that a critical fault has
been detected. Critical faults typically indicate that the controller is
damaged and requires service.*

**Battery Status LED Error Indications**

|                             |                      |
|-----------------------------|----------------------|
| High Voltage Disconnect     | R - G Sequencing     |
| High Temperature Disconnect | R - Y Sequencing     |
| External Wiring Error       | R&G - Y Sequencing   |
| Load Overcurrent            | R&G - Y Sequencing   |
| Load Short Circuit          | R&G - Y Sequencing   |
| Self-test Error             | R - Y - G Sequencing |

*Note:*

*LED error indications can be interpreted as follows:* *“R - G
sequencing” means that the Red LED is on, then the Green LED is on, then
Red LED is on....*

*“R&G - Y sequencing” means that both the Red LED and Green LED are on,
then just the Yellow LED is on, then Red and Green LED are on....*

**Common Problems**

**Problem:** No LED indications

**Solution:** With a multi-meter, check the voltage at the Battery
terminals on the SunSaver and the Solar terminals on the SunSaver. The
solar module must be in good sun and battery voltage must be at least 1
V to power the SunSaver and activate the dead battery charging function.
Problem: The SunSaver is not charging the battery. Solution: If the
Status LED is solid or flashing red, see Error Indications. If the
Status LED is off, measure the voltage across the Solar input terminals
of the SunSaver. Input voltage must be greater than battery voltage.
Check fuses and solar wiring connections. The solar module must in full
natural sunlight.

**Problem:** No load output.

**Solution:** If the battery status indication is Solid Red, the
SunSaver is in the Low Voltage Disconnect (LVD) condition. The load will
automatically switch on when the battery recharges to the Low Voltage
Reconnect (LVR) threshold voltage. See the specifications in section 7.0
for LVD & LVR settings.

*NOTE: If the SunSaver model is SS-6-12V or SS-10-12V (no load control
feature), the controller may be damaged.*