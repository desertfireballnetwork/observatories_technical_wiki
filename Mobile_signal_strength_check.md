This page provides detailed information how to check mobile signal
reception quality from command line. For general information on mobile
networks and modems, please go to [Mobile network - 3G/4G modem
installation and configuration](Mobile_network_-_3G,_4G.md "wikilink").

*Note: This link shows a good description of what signal quality is
needed:*
[1](https://wiki.teltonika-networks.com/view/Mobile_Signal_Strength_Recommendations)

# Standard method

In 2021 the software in most of the camera systems was upgraded to
provide relatively user friendly mobile signal check functions.

## Web GUI

click on tab "Network" then "Check modem signal"

## Commandline

` dfn_print_mobile_signal_quality.sh`

## Critical details of the printed info explained

Both these print the same information - the Web GUI menu command just
calls the script. The printed information depends on the type of modem
installed in the camera system. The key things are:

There actually is a modem module, it is powered, and the host operating
system can detect it on the USB bus: This is the 3G modem module:

` Detecting modem type...`
` Bus 001 Device 042: ID 1199:68a3 Sierra Wireless, Inc. MC8700 Modem`

This is the 4G modem module (Americas/EU version):

` Detecting modem type...`
` Bus 001 Device 012: ID 1199:9071 Sierra Wireless, Inc. `

Modem missing or defunct:

` Detecting modem type...`
` Error: no supported Sierra Wireless modem found.`
` Modem missing? Or broken? Wiring problem? Here is a list of devices on USB:`

It is possible to send AT commands to configure/query the modem, and it
responds (3G+ modem module):

` Manufacturer: Sierra Wireless, Incorporated`
` Model: MC8705`

This is the 4G modem module (Americas/EMEA version):

` Manufacturer: Sierra Wireless, Incorporated`
` Model: MC7455`

Modem hardware and firmware version

`  Revision: XXXXXXXXX YYYY XXXXXXXXX`

Temperature reading! (In Celsius degrees, sensor in the modem module)

` Temperature: 40`

Connection and registration state - this means whether the modem is
attached/registered to the operator's network

All good:

` PS state:    Attached`
` GMM (PS) state:REGISTERED       NORMAL SERVICE `
` MM (CS) state: IDLE             NORMAL SERVICE `

Not good - in this case, missing SIM card:

` PS state:    Not attached`
` GMM (PS) state:DEREGISTERED     PLMN SEARCH    `
` MM (CS) state: IDLE             NO IMSI        `

Mobile technology, band and channel used to connect with the tower

3G/WCDMA - example:

` System mode:   WCDMA`
` WCDMA band:    WCDMA800`
` WCDMA channel: XXXX`

4G/LTE - example:

` LTE band:      B2               LTE bw:      20 MHz  `
` LTE Rx chan:   XXX              LTE Tx chan: XXXXX`

Signal reception quality:

*Note: This link shows a good description of what signal quality is
needed:*
[2](https://wiki.teltonika-networks.com/view/Mobile_Signal_Strength_Recommendations)

3G/WCDMA:

` RX level (dBm):-85`

4G/LTS:

` PCC RxM RSSI:  -75              RSRP (dBm):  -112`
` RSRQ (dB):     -19.0 `
` ...`
` LTE:`
`       RSSI: '-73 dBm'`
`       RSRQ: '-16 dB'`
`       RSRP: '-113 dBm'`
`       SNR: '0.0 dB'`

Simple signal quality print - should be at least 5-6 in remote areas, \>
~12-15 with busy tower:

` AT+CSQ`
` +CSQ: 12,99`

Lowest possible signal

` AT+CSQ`
` +CSQ: 0,99`

No signal

` AT+CSQ`
` +CSQ: 99,99`

Missing SIM card

` AT+CSQ`
` ERROR`

Mobile network operator - examples:

` +COPS: 0,0,"Telstra Mobile",2`

or

` +COPS: 0,0,"OMANTEL",2`

or

` +COPS: 0,0,"TELUS",7`

missing SIM card or missing antenna or no reception

` +COPS: 0,0,"Limited Service",2`

# Low level commands for advanced users

## 4G (MC74xx) modem modules

Login to the camera system locally or remotely, in text console run the
following commands.

Verbose signal reception print - 4G/LTE connection, good signal quality:

` root@XXXXXX:~# `**`qmicli -d /dev/cdc-wdm1 --nas-get-signal-strength`**
` [/dev/cdc-wdm1] Successfully got signal strength`
` Current:`
`       Network 'lte': '-68 dBm'`
` RSSI:`
`       Network 'lte': '-68 dBm'`
` ECIO:`
`       Network 'lte': '-2.5 dBm'`
` IO: '-106 dBm'`
` SINR: (8) '9.0 dB'`
` RSRQ:`
`       Network 'lte': '-12 dB'`
` SNR:`
`       Network 'lte': '1.0 dB'`
` RSRP:`
`       Network 'lte': '-104 dBm'`

Brief signal reception print - 4G/LTE connection, good signal quality:

` root@XXXXXX:~# `**`qmicli -d /dev/cdc-wdm1 --nas-get-signal-info`**
` [/dev/cdc-wdm1] Successfully got signal info`
` LTE:`
`       RSSI: '-68 dBm'`
`       RSRQ: '-12 dB'`
`       RSRP: '-104 dBm'`
`       SNR: '-4.4 dB'`

Brief signal reception print - 4G/LTE connection, excellent signal
quality:

` root@XXXXXX:~# `**`qmicli -d /dev/cdc-wdm1 --nas-get-signal-info`**
` [/dev/cdc-wdm1] Successfully got signal info`
` LTE:`
`       RSSI: '-62 dBm'`
`       RSRQ: '-7 dB'`
`       RSRP: '-90 dBm'`
`       SNR: '23.8 dB'`

Brief signal reception print - 3G/WCDMA connection, marginal signal
strength, but still good signal quality thanks to low
noise/interference; typical for remote areas with low population:

` root@DFNEXT043:~# `**`qmicli -d /dev/cdc-wdm1 --nas-get-signal-info`**
` [/dev/cdc-wdm1] Successfully got signal info`
` WCDMA:`
`       RSSI: '-103 dBm'`
`       `**`ECIO: '-6.5 dBm`**`'`

Verbose signal reception print - 3G/WCDMA connection, marginal signal
strength, but still good signal quality thanks to low
noise/interference; typical for remote areas with low population:

` root@DFNEXT043:~# `**`qmicli -d /dev/cdc-wdm1 --nas-get-signal-strength`**
` [/dev/cdc-wdm1] Successfully got signal strength`
` Current:`
`       Network 'umts': '-103 dBm'`
` RSSI:`
`       Network 'umts': '-103 dBm'`
` `**`ECIO:`**
`       Network 'umts': `**`'-7.5 dBm`**`'`
` IO: '-106 dBm'`
` SINR: (8) '9.0 dB'`

*Note: if device /dev/cdc-wdm1 was not working, try substitute 1 with 0
or 2. List devices available in your system with 'ls -l
/dev/cdc-wdm\*'.*

` ls -l /dev/cdc-wdm*`
` crw------- 1 root root 180, 0 Nov 26 11:53 /dev/cdc-wdm0`
` crw------- 1 root root 180, 1 Nov 26 11:53 /dev/cdc-wdm1`

## 3G modem modules (MC8705)

Login to the camera system, in text console run command 'minicom'. It
should be preset to use the correct virtual serial port /dev/ttyS3. If
not, run 'minicom -s' and select this port.

(To exit minicom, press 'Ctrl+a q'.)

Type following AT modem commands:

` `**`at+csq`**
` +CSQ: 14,99`
` `
` OK`
` `**`at!gstatus?`**
` !GSTATUS: `
` Current Time:  7833862          Temperature: 61`
` Bootup Time:   6442483          Mode:        ONLINE         `
` System mode:   WCDMA            PS state:    Attached     `
` WCDMA band:    WCDMA800         GSM band:    Unknown    `
` WCDMA channel: 4436             GSM channel: 65535`
` GMM (PS) state:REGISTERED       NORMAL SERVICE `
` MM (CS) state: IDLE             NORMAL SERVICE `
` `
` WCDMA L1 State:L1M_DCH          RRC State:   CELL_DCH       `
` RX level (dBm):-85`
`   `
` OK`

Signal strength (CSQ) \>=5 is acceptable, although in country areas the
internet may work even with lower signal quality as the masts are not
too busy. In any case, the more the better. More than ~ 10 is usually
very reliable connection in the remote areas. In urban areas with busy
mobile network, better signal quality is required for reliable
connection and data transfers - other devices connected to the same
telco tower might "shout down" the communication of the camera system.