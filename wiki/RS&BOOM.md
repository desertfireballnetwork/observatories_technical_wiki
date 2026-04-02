# Raspberry Shake

## Setting up

Plug in usb connector (gives rfemale USB port). Plug in GPS unit

Plug Ethernet cable in and connect other end with Ethernet jack in wall
or Wifi router

Plug USB power cable in (be gentle with the connector) and plug into 12V
splitter USB port.

Blue LED should light up when powered on.

Unit needs to be well coupled to the ground. Either place on a solid
rock surface, or scrape off any loose material to place on the firmest
ground possible.

3D units must be aligned to North.

Use screw legs and bubble level to ensure unit is flat.

## Connecting

Connect to the pi in order to check date and that data are being
collected. Plug ethernet cable into Pi and other end into wifi
router/hub

connect to that wifi via laptop

Open browser on desktop computer (or tablet, phone) and navigate to
<http://rs.local> (if this does not work, find IP address of raspberry
shake (for Windows 10 cmd: arp –a) and type in your browser
<http://>???.?.??.???) . Raspberry Shake can be configured by choosing
Settings icon

In a terminal, ssh to myshake@rs.local

Default username and password: username: myshake, password: shakeme

### checking the time

In terminal \$date

In browser, should be on home page as "System Time"

Note time should be in UTC

If needed, setting time to laptop time \$ ssh myshake@rs.local "sudo
date --set \\\$(date)\\"

check connection of GPS: \$ ntpq -p

### Checking status

System Status should be RUNNING. May take a few moments in BOOTING.

Ensure that the data-producer is ON

### checking out the data

Helicorder plot (last 12h of data, updates every 2 min) can be seen on
<http://rs.local/heli>

Program ‘swarm’ (available on Linux, MAC, Windows) can be downloaded on
webpage to visualize seismic data from Raspberry Shake (http://rs.local
\>\> Actions icon (green icon on bottom) \>\> Downloads \>\> Swarm), for
Windows: open swarm_console.bat, Linux+MAC: sh swarm.sh (note, you will
need to have openjdk-8-jre installed).

Data from Raspberry shake is in folder myShake – Networks – AM, RS
Community folder shows data from raspberry shake community

Click into helicorder plot (shows last 24h of data in 30 min segments)
to see detailed waveform data, first right click: power spectra, second
right click: spectrogram

Shut down from web front-end, never just pull plug (system \>\>
shutdown)

Note: If there are more than one Raspberry Shake on same network, they
will appear as rs.local, rs-2.local, …, rs-n.local.

### Working Raspberry Shake lights ensemble

Pi board (red (solid), green (flashing every 2-5 s)

Shake board (blue (solid))

Ethernet port (green (flashing repeatedly), orange (solid))

More detailed information:

<https://manual.raspberryshake.org/quickstart.html>

<https://manual.raspberryshake.org/traces.html>

for offline application:
<https://manual.raspberryshake.org/no-network.html>

for GPS commands: <https://manual.raspberryshake.org/ntp.html#ntptiming>

## Data download

open rs.local \>\> Actions icon \>\> DOWNLOADS \>\> DATA, opens FTP
connection for individual file download

/opt/data/archive/2020/AM/R640E/EHZ.D

Labels

AM.R640E.00 – Shake 1D + Boom

AM.RABE8.00 – Shake 1D + Boom

AM.RC24D.00 – Shake 1D + Boom

AM.RBB35.00 – 3D Shake

### Notes

Note: 3Ds will need aligning to true North

Useful terminal commands

ls –lh: check if it records

date: to check UTC date

rsh: shut down

ip n: shows ip addresses

ssh myshake@172.16.1.136 (if this does not work try ssh
myshake@rs.local)

#### GPS software modifications

change /etc/default/gpsd

to look like this:

`  START_DAEMON="true"               `
`  GPSD_OPTIONS="-b -n -G"              `
`  DEVICES="/dev/ttyUSB0"`
`  USBAUTO="true"                    `
`  GPSD_SOCKET="/var/run/gpsd.sock"`

change /etc/ntp.conf

only this section:

`  # GPS Serial data reference                           `
`  server 127.127.28.0 iburst minpoll 4 maxpoll 4      `
`  fudge 127.127.28.0 time1 0.500 refid GPS            `

check GPS reception: cgps

check ntp time sync status: ntpq -p