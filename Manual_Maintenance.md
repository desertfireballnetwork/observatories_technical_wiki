## Connecting to your camera system

If you are ever required to perform any manual maintenance, first
[connect to your
camera](Logging_in_locally_via_WiFi_or_Ethernet.html).

If you are unable to connect to the camera system [locally via the
network (ethernet or
WiFi)](Logging_in_locally_via_WiFi_or_Ethernet.html), or [remotely
over the VPN](Logging_in_remotely_over_the_network.html), you can
log in locally using a HDMI or VGA monitor and USB keyboard.

### Direct Connection

1.  Before turning on system, connect a keyboard using USB and a monitor
    using either HDMI or VGA \*insert figure of where to connect\*
2.  log in as `root`

## Checking removable hard drives

General notes:

- Before deploying cameras the drives should be empty - delete the data
  recorded eg during testing in the lab, from all drives including
  /data0
- It is a good practice to check the how the drives are full before
  replacing them when servicing the observatory. If it was running for
  several months, at least /data1 should be full. If that is not true,
  the observatory was not working and needs to be serviced. Also you can
  decide to replace only the drives that actually have some data on them
  and leave the empty ones in the box.

**Now let's start - first switch the drives on and mount them:**

`$ python /opt/dfn-software/enable_ext-hd.py`

In case of **DFNEXT** observatories, wait at least 20 seconds fro the
drives to spin up. then mount the drives:

`$ mount /data1`
`$ mount /data2`
`$ mount /data3`

In case of **DFNSMALL** observatories, wait at least 40 seconds fro the
system to recognize the USB enclosure and spin up the drives and then
mount them:

`$ mount -a`

The next step is to list the drives - we are interested only in the
**data** partitions:

`$ df -h `

In case of **DFNEXT** observatory with three 6TB removable drives
installed and running for several weeks, you will get listing like this:

`Filesystem      Size  Used Avail Use% Mounted on`
`...`
`/dev/sda3       390G   55G  316G  15% /data0`
`/dev/sdb1       5.5T  1.1T  4.2T  21% /data1    `*`..... This drive is 21% full`*
`/dev/sdd1       5.5T   58M  5.2T   1% /data2    `*`..... This drive is empty`*
`/dev/sdc1       5.5T   89M  5.2T   1% /data3    `*`..... This drive is empty`*

In case of **DFSMALL** observatory with two 8TB removable drives
installed and runnin \$ cd /data0/DFNXXXNN/YYYY/MM/g for more than 1/2
year, now pretty much full of data, you will see:

`Filesystem      Size  Used Avail Use% Mounted on`
`...`
`/dev/sda5       406G   59G  327G  16% /data0`
`/dev/sdc1       7.3T  6.8T   90G  99% /data1    `*`..... This drive is full`*
`/dev/sdb1       7.3T  6.5T  367G  95% /data2    `*`..... This drive is nearly full`*

*Note: the /data0 partition is on the system SSD drive, this one is
available all the time and contains the recent data (last 1-2 nights)
and logs.*

When done with the drives check, unmount them, tell the OS to forget
about the SATA devices (it's SATA hot-swap) and finally switch them off:

In case of **DFNEXT** observatory:

`$ python /opt/dfn-software/disable_ext-hd.py`

*Note: this command actually internally calls also these commands*

`$ umount /data1 /data2 /data3`
`$ echo 1 > /sys/block/sdb/device/delete`
`$ echo 1 > /sys/block/sdc/device/delete`
`$ echo 1 > /sys/block/sdd/device/delete`

*so there is no need to run these individually in case of nominal
conditions.*

In case of **DFSMALL** observatory:

`$ umount /data1 /data2`
`$ python /opt/dfn-software/disable_ext-hd.py`

### SMART disk diagnostics

This is applicable for DFNEXT, manually checking/testing magnetic drives
using smartmontools.

To figure out what drives there are, turn them on and mount first, then
use lsblk:

`root@DFNEXT018:~# lsblk`
`NAME   MAJ:MIN RM   SIZE RO TYPE MOUNTPOINT`
`sda      8:0    0   477G  0 disk `
`├─sda1   8:1    0  31.3G  0 part /`
`├─sda2   8:2    0     4G  0 part [SWAP]`
`└─sda3   8:3    0 396.3G  0 part /data0`
`sdb      8:16   0   5.5T  0 disk `
`└─sdb1   8:17   0   5.5T  0 part /data3`
`sdc      8:32   0   9.1T  0 disk `
`└─sdc1   8:33   0   9.1T  0 part /data2`
`sdd      8:48   0   9.1T  0 disk `
`└─sdd1   8:49   0   9.1T  0 part /data1`

To check drives SMART status, watch for the highlighted parameters:

`root@DFNEXT018:~# smartctl -a /dev/sdb`
`smartctl 6.6 2016-05-31 r4324 [x86_64-linux-4.9.0-19-amd64] (local build)`
`Copyright (C) 2002-16, Bruce Allen, Christian Franke, www.smartmontools.org`

`=== START OF INFORMATION SECTION ===`
`Model Family:     Western Digital Red`
`Device Model:     WDC WD60EFRX-68MYMN1`
`Serial Number:    WD-WX31D55DFVC4`
`LU WWN Device Id: 5 0014ee 2b6ef65ab`
`Firmware Version: 82.00A82`
`User Capacity:    6,001,175,126,016 bytes [6.00 TB]`
`Sector Sizes:     512 bytes logical, 4096 bytes physical`
`Rotation Rate:    5700 rpm`
`Device is:        In smartctl database [for details use: -P show]`
`ATA Version is:   ACS-2, ACS-3 T13/2161-D revision 3b`
`SATA Version is:  SATA 3.1, 6.0 Gb/s (current: 6.0 Gb/s)`
`Local Time is:    Fri Sep 12 11:00:46 2025 AEST`
`SMART support is: Available - device has SMART capability.`
`SMART support is: Enabled`
` `
`... `

`'''196 Reallocated_Event_Count 0x0032   200   200   000    Old_age   Always       -       0`
`'''197 Current_Pending_Sector  0x0032   200   200   000    Old_age   Always       -       0`
`'''198 Offline_Uncorrectable   0x0030   100   253   000    Old_age   Offline      -       0`
`199 UDMA_CRC_Error_Count    0x0032   200   200   000    Old_age   Always       -       0`
`200 Multi_Zone_Error_Rate   0x0008   100   253   000    Old_age   Offline      -       0`

`SMART Error Log Version: 1`
`No Errors Logged`

`SMART Self-test log structure revision number 1`
`Num  Test_Description    Status                  Remaining  LifeTime(hours)  LBA_of_first_error`
**`# 1 Short offline Completed without error 00% 83 -`**
`# 2  Short offline       Completed without error       00%        71         -`
`# 3  Short offline       Completed without error       00%         0         -`
`...`

To start short test:

`smartctl -t short  /dev/sdb`

Then check the status again. Any of the 'bad sector' parameters 196,
197, 198 is non-zero, the drive has problem with the magnetic platters
surface and may die any time. For helium filled models, watch for
"Helium leve" parameter too, must be 100%. This parameter number can be
different for individual disk models.

`22 Helium_Level            0x0023   100   100   025    Pre-fail  Always       -       100`

There is also a long test that reads all the capacity of drive, but that
typically runs for many hours. Recommended to perform only if unsure
with a specific drive.

## Installing new HDDs

#### 1. Make sure the hard drives are powered off

Also consider what time it ts - there is daily task to move data from
/data0 partition to the removable drives sheduled using crontab for
10:55 local time.

#### 2. Physically replace the drives

Remember labeling the drives taken out of the observatory - put a
sticker on the drive, note observatory type, number, site name and
replacement date.

#### 3. Format the new drives after replacing

##### DFNEXT observatory

Power on the enclosure with hard drives and start the formatting script:

`$ python /opt/dfn-software/enable_ext-hd.py`

wait 20 seconds, then probe the observatory type and HDDs connection
type

`$ cd /root/bin`
`$ ./dfn_setup_data_hdds.sh -p`

prints

`Probe result: DFNEXT SATA /dev/sdb data2 /dev/sdc data3 /dev/sdd data1`
`Suggested command to format all drives: /root/bin/dfn_setup_data_hdds.sh /dev/sdb data1 /dev/sdc data2 /dev/sdd data3`

To format all three drives, just execute the suggested command (the
dataX labelling will be re-created)

`$ ./dfn_setup_data_hdds.sh /dev/sdb data1 /dev/sdc data2 /dev/sdd data3`

To format only drive data1 (eg in case the others are not replaced or
are not to be wiped), execute the suggested command

`$ ./dfn_setup_data_hdds.sh /dev/sdd data1`

Please note the sdd being originally /data1. It is also possible to
check which HDD is which in the data move log, eg:

`$ less /data0/log/move_data/move_data_2025-04-02*log`
`...`
`Camera type / HW probe result: DFNEXT, removable drive(s) type:  SATA`
`/data1 /dev/sdd1 Device Model: WDC WD100EFAX-68LHPN0 Serial Number: 2YJP7J4D`
`/data2 /dev/sdb1 Device Model: WDC WD100EFAX-68LHPN0 Serial Number: JEHM02DN`
`/data3 /dev/sdc1 Device Model: WDC WD100EFAX-68LHPN0 Serial Number: JEHMY40N`
`Filesystem      Size  Used Avail Use% Mounted on`
`/dev/sda3       390G  1.3G  369G   1% /data0`
`/dev/sdd1       9.1T  3.8T  4.8T  45% /data1`
`/dev/sdb1       9.1T   32K  8.6T   1% /data2`
`/dev/sdc1       9.1T   32K  8.6T   1% /data3`
`...`

*Note: The formatting procedure includes SMART selftest of all the
drives.*

This test can be run also manually, individually on each drive, see
[below](#SMART_disk_diagnostics.html).

Tell the OS to forget about the SATA devices (it's SATA hot-swap) and
power them off.

`$ python /opt/dfn-software/disable_ext-hd.py`

*Note: this command actually internally calls also these commands*

`$ umount /data1 /data2 /data3`
`$ echo 1 > /sys/block/sdb/device/delete`
`$ echo 1 > /sys/block/sdc/device/delete`
`$ echo 1 > /sys/block/sdd/device/delete`

*so there is no need to run these individually in case of nominal
conditions.*

##### DFNSMALL observatory

Power on the enclosure with hard drives and start the formatting script:

`$ python /opt/dfn-software/enable_ext-hd.py `

wait 40 seconds

`$ cd /root/bin`
`$ ./setup_usb_hdds_jmicron.sh`

When prompted, for various settings:

\- prompt gparted --\> N

\- prompt "Create partition /dev/sdb1, Format /dev/sdb1 as ext4" --\> Y

\- prompt "Create partition /dev/sdc1, Format /dev/sdc1 as ext4" --\> Y

\- wait for quick smart self test to finish, check result for the 1st
drive, particularly the following lines:

`...`
`196 Reallocated_Event_Count 0x0032   200   200   000    Old_age   Always       -       0`
`197 Current_Pending_Sector  0x0032   200   200   000    Old_age   Always       -       0`
`198 Offline_Uncorrectable   0x0030   100   253   000    Old_age   Offline      -       0`
`...`
`# 1  Short offline       Completed without error       00%       586         -`
`... `

\- press enter, check result for the 2nd drive the same way

\- At the end, check that the freshly formated drives mounted and the
expected capacities are listed

`Filesystem      Size  Used Avail Use% Mounted on`
`/dev/sdb1       5.5T   58M  5.2T   1% /data2    ..... this is 6TB drive`
`/dev/sdc1       3.6T   68M  3.4T   1% /data1    ..... this is 4TB drive`

And finally power off the

`$ python /opt/dfn-software/disable_ext-hd.py `

## Transfer data from data0 to removable HDs manually

`$ nohup /usr/local/bin/move_data_files.sh &`

This will store info on move in nohup.out file in the present working
directory. The data moving task will continue even if you disconnect
from the camera.

## Check CF card

To check if there are images on the camera CF card:

`$ python /opt/dfn-software/enable_camera.py`
`$ gphoto2 -L -R    # this will list all files on camera`

To format the CF card, see the dedicated instruction [page
here](Formatting_the_CF_Card.html).

## Capture control test

#### 1. Run the test

`$ /opt/dfn-software/int_control_test.sh`

This will take ~10 mins. You should start to hear shutter clicks as test
photos are taken.

To monitor interval test as it goes (in other terminal):

`$ tail -f /data0/latest/*interval.txt`

#### 2. Check interval control test successfully took pictures

Check there are ~10 pictures taken at the time test was run in previous
images

`$ cd /data0/latest_prev`
`$ ls`

## Configure timeozne

This command starts text GUI app that allows to set the timesone.

`$ dpkg-reconfigure tzdata`

*Note: it is not recommended to change the timezone during the night
time when the exposure capture control SW is running. I is recommended
to restart the OS of the camara system embedded PC after changing the
timezone. Warm reboot is OK.*

## Reset/Reboot the camera system

This command initiates warm reset:

`$ reboot`

### DFNKIT camera system reset

In case of DFNKIT camera systems, the 'reboot' command warm reset is the
only available option for reboot. Full HW (cold) reset including the
microcontroller and DSLR needs power cycling, ideally power off at the
time when the system is shut down or in reboot - when a beep sounds.

### DFNSMALL camera system reset

This command initiates cold reset in case of DFNSMALL camera systems
(including the microcontroller, DSLR and video camera):

`$ hard_reset.sh`

### DFNEXT camera system reset

This command initiates cold reset in case of DFNEXT camera systems
(including the microcontroller, DSLR and video camera):

`$ poweroff`

*Note: there is an electronic circuit which detects that the embedded PC
is shut down, cuts the power for all sub-systems and waits a few seconds
before turning the power on again.*

## Check Shutter Count

`$ cd /data0/latest_prev    `
`or folder with the format:`
`$ cd /data0/DFNXXXNN/YYYY/MM/YYYY-MM-DD_DFNXXXNN_1XXXXXXXXX`

`$ exiv2 -p a **image**.NEF | grep hutter`
`or`
`$ grep hutter *interval.txt`

## Checking GPS

First check GPS lock and position/time information acquired by the GPS
and reported to the PC - run command:

`$ cgps`

See the upper part of screen for position and time. In the bottom part
of the screen, the NMEA messages from GPS should be scrolling.

Press \[Q\] to exit cgps.

*Note: No lock means no reception, as long as there is text scrolling in
the bottom of the page, the GPS communicates with the observatory PC.*

Second thing to check is that the capture control SW hets the leocation
and time when it runs with GPS antenna connected and with good signal
reception. Inspect the \*interval.txt logs, either produced by [capture
control test](Configuring_the_Software#Capture_control_test.html)
or by regular overnight operation. In case of nominal GPS functionality,
the ntp NMEA/PPS time correction should be active:

`INFO, interval_control_lin, ntp, +SHM(0),.NMEA.,0,l,9,16,377,0.000,-11.851,3.472`
`INFO, interval_control_lin, ntp, *SHM(1),.PPS.,0,l,9,16,377,0.000,0.022,0.008`

and coordinates should be passed from the GPS receiver (the last number
'1' means GPS has lock:

`INFO, interval_control_lin, GPS_lonlat, 135.274305, -30.857625, 156.26, 1`

There is also a python script to query lhe leostick/arduino
microcontroller for the GPS status

`python /opt/dfn-software/leostick_get_status.py -g`
`GPGGA,081358.000,3140.0427,S,11639.9456,E,`**`1`**`,16,0.6,195.02,M,-23.9,M,,`

The above GPS sentence is example of receiver having lock (bold "1"),
coordinates 31 deg 40.0427 min S, 116 deg 39.9456 min E, elevation
195.02 m, while the sentence below shows situation without lock (no
reception).

`python /opt/dfn-software/leostick_get_status.py -g`
`GPGGA,045843.000,3102.8703,S,11550.3033,E,`**`0`**`,00,99.0,202.58,M,-27.9,M,,INFO, interval_control_lin`

## Software Updates

### Automated software updates

As long as the observatory is connected to the Internet, the software
that controls observatory auto updates daily from a dedicated DFN
server. There are two attempts to do so in the afternoon local time, ~
40 minutes before the daily reboot and ~ 20 minutes after; the default
times are 3:30 PM and 4:30 PM for the SW update and 4:10 PM for reboot.

### Manual software update over Internet

It might be handy to execute the network SW update manually, for example
in case of testing or deploying new observatory that was off-line for
some time or in transport or stored as a spare. In this case log in to
the observatory and execute command

`$ dfn_down_install_sw_from_server.sh `

### Manual software update using local copy

In case of remote site without Internet connection the only way to
update the observatory software is to bring a copy of the software eg on
laptop do the update locally.

The Australian DFN team members can find the latest stable software in
the internal DFN repo in operation/SW/dfnsmall/stable (DFNSAMLL type of
observatory) or operation/SW/dfnext/stable (DFNEXT type). External
collaborators will be provided with a copy of the software on request.

Assuming the servicing person has Linux environment on her/his laptop,
first step is to do a dry-run to check there are no command typo errors
by running:

`$ rsync -nrv opt usr root@172.16.1.101:/`

If there are no errors or anomalies, just list of files that would copy,
you can run the real update:

`$ rsync -rv opt usr root@172.16.1.101:/`

*Note: The above IP address is for local wired connection (over
etherenet cable), for WiFi connection use IP `172.16.0.101`*

## Re-flash microcontroller firmware

Re-writing the firmware in arduino microcontroller can be done in the
camera system, from the embedded PC OS commandline, even remotely. We
recommend to do this at day time, not during data capture at night -
please consider that the timezone at the camera location can be
different from your local time!

The command is using the recommended stable firmware binary image as
parameter:

`leo_stick_program.sh /opt/dfn-software/micro_firmware_30_may_2017_27s.hex`

The whole process takes several minutes. Make sure the power is not cut
during the flashing, otherwise it may end up in failure that will need
physical attention on the camera site.

## Replacing embedded PC board

### Commell LE-37D (DFNSMALL, DFNKIT)

When replacing the PC board, the system drive (DFNKIT: mSATA SSD card;
DFNSMALL: 2.5" SATA SSD underneath the custom design PCB) and optionally
mobile network 3G/4G modem card are moved from the old PC to the new
one.

#### Ethernet ports

The Ethernet networking will not work out of the box, because the udev
subsystem automatically pairs the network interfaces (eth0, eth1) with
unique MAC addresses for each board and remembers that setting. With the
system drive in new PC board, udev recognizes the new network
interfaces, but names them eth2 and eth3, which does not match with the
network configuration.

To fix that, one needs to clear that configuration: PC needs to be
booted with screen (we use cheap upcycled VGA panels in our lab most of
the time but HDMI should work as well) and keyboard connected to be able
to run script

` /root/bin/rm_udev.sh `

and then reboot.

*Note: when the system is first booted and later a monitor connected, it
most likely will not work. Keyboard should be no problem including
connecting it later for a blind CTRL+ALT+DEL reboot.*

*Note: the udev network interface persistent config does not affect the
mobile network 3G/4G modem card interface, however, the user needs to
make sure the operator and modem type setting is correct for the current
location.*

#### BIOS configuration

If using brand new unconfigured PC board or if the CMOS battery on the
new PC board is replaced or if in case of unexpected camera system
behaviour, make sure the [BIOS and HW jumpers
configuration](DFNSMALL_observatories_BIOS_configuration.html) is
correct.

### Commell LE-37G (DFNEXT)

#### Physical PC board swap

<figure>
<img src="DFNEXT_PCboard_above_custom_PCB.jpg"
title="DFNEXT_PCboard_above_custom_PCB.jpg" />
<figcaption>DFNEXT_PCboard_above_custom_PCB.jpg</figcaption>
</figure>

1.  Get tools ready: a M7 nut and a ratchet for the lens flange bolts; a
    short Phillips head screwdriver \#PH1; a larger flat head
    screwdriver. Tweezers or long nose pliers might help with the
    connectors unplugging/plugging.
2.  If the DSLR needs replacing as well, remove the outer blower ducting
    (use snal off knife to cu the silicone glue, then clean out the
    residual silicone); remove the DSLR and lens from the box ([reverse
    order of these
    steps](How_to_install_flanged_Samyang_lens_and_DSLR_to_the_DFNEXT_enclosure.html)).
3.  Optional, but recommended: Take a few photos of what connectors are
    plugged where.
4.  Remove the connectors from the DFNEXT camera custom electronics PCB.
    Be careful and gentle particularly with the tiny GPS RF connector
    and the LC shutter connector.
5.  Remove the DFNEXT camera custom electronics PCB - it is not bolted
    down, just inserted on the corners
    ![](DFNEXT_custom_PCB_removed.jpg "DFNEXT_custom_PCB_removed.jpg")
6.  Unscrew the 4 small Phillips head bolts in the corners of the
    Commell PC board.
    ![](DFNEXT_unscrew_PC_board.jpg "DFNEXT_unscrew_PC_board.jpg")
7.  Remove wires and connectors from the original PC board. Mind the
    polarity of power terminal.
    ![](DFNEXT_where_to_insert_flat_screwdriver.jpg "DFNEXT_where_to_insert_flat_screwdriver.jpg")
8.  Pry off the PC board with flat screwdriver inserted between the
    aluminium heat spreader plate and inner box chassis.
9.  There is a white thermal transfer adhesive (a kind of soft rubber
    layer \<1mm thick). If that one stays in good shape, it can be
    re-used, leave it on the heat spreader of the original board. Cover
    it by plastic bag or other foil, cut the extra.
10. If the replacement PC does not have this adhesive on the heat
    spreader plate, use new one. Remove the transparent transport foil
    and install the adhesive on the heat spreader plate. Then remove the
    other transport foil (the one with letters) and carefully install
    the PC board into the camera box. Mind to match the holes for the
    bolts.
    ![](DFNEXT_replacement_PC_with_bare_heatspreader_and_thermal_adhesive.jpg "DFNEXT_replacement_PC_with_bare_heatspreader_and_thermal_adhesive.jpg")
    ![](DFNEXT_stick_the_thermal_adhesive_writing_up.jpg "DFNEXT_stick_the_thermal_adhesive_writing_up.jpg")
11. Screw in the 4 bolts holding the PC. Do not tighten too much, that
    would bend the plate and reduce thermal contact.
12. Re-attach wires and connectors to the new PC board. Mind the
    polarity of the power terminal - the yellow wire (+12V) is deeper in
    the box, the black wire (GND) is on top.
13. Re-install the DFNEXT custom PCB
14. Plug in all the connectors (not the photo might come handy...).
    Again, carefully with the GPS antenna pigtail cable and the tiny LC
    shutter connector.
15. Configure the Ethernet ports and BIOS - see below
16. Run interval control test in the lab (with DSLR+lens attached, does
    not have to be bolted to the box for this test).
17. It is recommended to test the box as much as possible. Please refer
    to
    [checklists](Camera_Maintenance#Servicing_and_deployment_checklists.html),
    particularly the one for the lab. There are some useful details in
    the others too.
18. Install the DSLR and lens ([follow these
    steps](How_to_install_flanged_Samyang_lens_and_DSLR_to_the_DFNEXT_enclosure.html)).
    Glue in the outer blower ducting using UV resistant silicone
    adhesive ("roof and gutters" grade).

*Note: If shipping the camera box or if transporting it to the
deployment site on rough roads, just bolt the lens to the box and
install the DSLR on the remote site. Otherwise there is a risk of
damaging the Samyang lens internals, as the DSLR 'hangs' on the lens
bayonet mount.*

*Note: When replacing the PC board, the system drive (mSATA SSD card
plugged into the PC board) and mPCIe SATA controller card are moved from
the old PC to the new one.*

*Note: It is sensible to replace the CMOS battery (commonly available
CR2035) after ~5 years or if not sure how old it is.*

<figure>
<img src="DFNEXT_replace_the_CMOS_batt_CR2035.jpg"
title="DFNEXT_replace_the_CMOS_batt_CR2035.jpg" />
<figcaption>DFNEXT_replace_the_CMOS_batt_CR2035.jpg</figcaption>
</figure>

#### Ethernet ports

The Ethernet networking ports will not be assigned correctly before
replacing the PC (after moving the system drive - mSATA SSD card - from
the old to the new PC. They may be swapped (eth0 vs eth1). When first
time booting the PC, have both ethernet ports unplugged and monitor +
keyboard connected. After system boots, it should ato login. If not, use
the generated password string corresponding to the system drive camera
system hostname/number. Then run following command to configure the
ethernet ports. It is interactive - asks for user input 3 times, see the
bold lines below - and it finishes with system reboot.

` root@DFNEXT036:~# `**`/root/bin/post_clone_config_gen3.sh`**
` eth0/1 ports configurations based on MAC addresses`
` Configure which ethernet port is eth0 and eth1 [Y|n]: `**`Y`**
` there are two Ethernet ports with MAC addresses:`
` Possible configuration options:`
` [a]   eth0 laptop local connection: 00:03:1d:12:3f:d8   eth1 Internet connection: 00:03:1d:12:3f:d7`
` [b]   eth0 laptop local connection: 00:03:1d:12:3f:d7   eth1 Internet connection: 00:03:1d:12:3f:d8`
` Select a or b (a is default): `**`a`**
` Selection made:  eth0 00:03:1d:12:3f:d8  eth1 00:03:1d:12:3f:d7`
` This change will become effective after system re-boot or possibly by re-starting the interfaces (ifdown/ifup eth0/eth1).`
` Note: if you chose re-boot, you can skip the eth0/1 configuration running ./post_clone_config_gen3.sh after re-boot to finish off the configuration`
` Reboot the OS now [Y|n]: `**`Y`**

Make sure the MAC addresses (eg 00:03:1d:12:3f:d8) are correct - there
is MAC address sticker on each eth connector and "eth0" / "eth1"
stickers on the inside of the camera system box door. After this is
done, the eth0/eth1 ports are configured correctly and the change will
stay persistent as long as the PC is not changed again.

#### BIOS configuration

If using brand new unconfigured PC board or if the CMOS battery on the
new PC board is replaced or if in case of unexpected camera system
behaviour, make sure the [BIOS and HW jumpers
configuration](DFNEXT_onservatories_BIOS_configuration.html) is
correct.

## Useful Commands

`$ df -h                 # checks if hard drives are mounted - list of mounted disk devices with disk usage/free information`
`$ lsblk                 # this command lists all hard disk devices in the system and where (if) they are mounted`
`$ cgps                  # gives gps coordinates in a table if sat lock and monitors communication GPS->PC. Press [Q] to exit.`
`$ ntpq -p               # check NTP time correction status`
`$ watch df              # monitors df changes, good for checking data transfers`
`$ du -hs * | grep G     # will show folders with folders >GB`
`$ crontab -l            # shows the scheduled tasks.... good for finding commands you want to manually run now`