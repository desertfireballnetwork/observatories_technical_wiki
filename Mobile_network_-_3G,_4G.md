## Mobile network modem installation in DFNEXT camera system type

The DFNEXT / DFNSMALL observatories usually ship without modems as many
of them are deployed with WiFi or Ethernet connectivity and different
areas of the world require different modems.

If you wish to network your DFNEXT observatory via mobile broadband
instead of WiFi or Ethernet you need to source a modem and request a
modem installation care package from DFN Camera Help. You will also need
to organize a 2FF ("standard") sized SIM with an active data plan.

### Recommended modem module types

The recommended embedded modem modules are the 3G/4G capable [Sierra
Wireless AirPrime MC
Series](https://www.sierrawireless.com/-/media/iot/pdf/datasheets/sierrawireless_airprime_mc_series_datasheet.pdf)
**MC7455** (for the Americas, Europe, the Middle East and Africa) and
the **MC7430** (for Asia Pacific including Australia). In most cases, 3G
speeds are more than sufficient. A lot of countries use 800-900MHz 3G
for coverage of the country anyway because the lower frequency reaches
further, while the higher speeds are demanded and hence 4G/5G technology
deployed mostly in metropolitan areas to handle data traffic from all
the city dwellers. **MC8705** is a very good 3G modem module and
nowadays (2018+) very cheap and nearly global option. Good for Europe,
Asia, Australia, Africa, possibly South America, unfortunately not
really in North America.

### Mobile network modem module installation

To install modem module into the DFNEXT camera system you will need a
modem converter card - which provides mPCIe to USB conversion, including
USB and HF patch coaxial cable/pigtail. Some of the [DFNSMALL camera
systems](https://wiki.dfn.net.au/index.php/File:DFNSMALL_camera_inside_1000.jpg)
have [Commel
PC](https://wiki.dfn.net.au/index.php/File:DFNSMALL_Commell_PC_eth_ports_marked.jpg)
, which has a free mPCIe card slot and SIM card slot as well. In such a
case you will need only the modem module and HF patch coaxial cable.

[DFNEXT Modem Installation](DFNEXT_Modem_Installation.md "wikilink") - Blue
PCB modem converter card

[DFN Modem Installation - Green
card](DFN_Modem_Installation_-_Green_card.md "wikilink") - Green PCB modem
converter card

### Mobile network modem configuration

[Mobile network configuration](Mobile_network_configuration.md "wikilink")
in the observatory PC operating system

### Checking the quality of reception - signal strength

[Mobile signal strength check](Mobile_signal_strength_check.md "wikilink")
in the observatory PC operating system

## Alternative: Rugged mobile network router

The configuration of the modem card in the embedded linux system can
become complicated, especially on remote sites with marginal signal
coverage.

The other option for 3G/4G mobile network connection is to use an
external router with Ethernet port, ideally some rugged industrial type
(wide temperature range and designed for 24x7 operation). In that case,
no special configuration of the camera system is required, just connect
the router with the camera system using Ethernet cable. The router comes
at high initial cost than the modem module, but it is more
straightforward to set up as well as more robust for operation.

Easiest solution might be to use a router recommended and supported by
local operator for the local climatic conditions. Staying with the same
brand we use for modem modules, Sierra Airlink router (for example RC50X
or similar
<https://www.sierrawireless.com/products-and-solutions/routers-gateways/rv50/>
) is a good option.

Important is to make sure it is appropriate region version and supports
all the bands/frequencies of the local mobile operator. Since the
introduction of 4G/LTE, there is really high number of bands with
different frequencies and encodings, and very few truly global devices
exist that would support all of them. Typically there are 2-3 sibling
versions, for the 3 main regions: EMEA (Europe/Middle East, Africa),
Americas, and Asia/Pacific (including Australia). Regarding temperature
range, RC50X is rated up to 70C, that should be enough everywhere.