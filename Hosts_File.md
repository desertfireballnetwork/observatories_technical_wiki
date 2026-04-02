An [IP address](wikipedia:IP_address.md "wikilink") is the address of your
observatory on the DFN's network of computers and observatories. The
[VPN](Logging_in_remotely_over_the_network.md "wikilink") is used to
connect all these computers and observatories even when they are behind
routers or on connections that do not provide a public IP address (like
mobile broadband connections).

The IP address of your observatory depends on it's number and what model
of observatory it is (see [Observatory
Types](Observatory_Types.md "wikilink")).

DFNSMALL: `10.1.17.xx` (where xx is the observatory number)

DFNKIT: `10.1.21.xx` (where xx is the observatory number)

DFNEXT: `10.1.23.xxx` (where xxx is the observatory number)

FIREOPAL: `10.1.25.xxx` (where xxx is the observatory number)

It is important to note that these IP addresses will only work if you
are connected to the
[VPN](Logging_in_remotely_over_the_network.md "wikilink").

# Local IP Address

If you're connected to an observatory via it's WiFi network, the IP
address is `172.16.0.101`, while via ethernet cable the IP address is
`172.16.1.101`.

# Examples

For example, the VPN IP for DFNKIT18 would be `10.1.21.18`, DFNSMALL25
would be `10.1.17.25` and DFNEXT009 would be `10.1.23.9`.

# Hosts File

To avoid having to remember the IP system, you can gives aliases to the
IPs of your cameras by modifying the [hosts
file](wikipedia:Hosts_(file) "wikilink") on **your computer**.

[How to edit your hosts file on Linux - Mac -
Windows](https://support.rackspace.com/how-to/modify-your-hosts-file/)

Note that you need administrator privileges to modify the hosts file on
your computer.

You can append the entire set of custom aliases below, a subset of them,
or make your own (eg. if you have the DFNEXT009 camera set up in New
York City, you can add a line like `10.1.23.9 DFNEXT009 NYC`, and you
will be able to connect to it through the VPN just by typing
`ssh root@NYC`). You will also be able to use these aliases in the
address bar of your web browser.

Note: **Do not modify the hosts file on the observatory itself!** You
want to modify the hosts file on your local computer.

    # DFNEXT cameras
    10.1.23.5   DFNEXT005
    10.1.23.6   DFNEXT006
    10.1.23.7   DFNEXT007
    10.1.23.8   DFNEXT008
    10.1.23.9   DFNEXT009
    10.1.23.10  DFNEXT010
    10.1.23.11  DFNEXT011
    10.1.23.12  DFNEXT012
    10.1.23.13  DFNEXT013
    10.1.23.14  DFNEXT014
    10.1.23.15  DFNEXT015
    10.1.23.16  DFNEXT016
    10.1.23.17  DFNEXT017
    10.1.23.18  DFNEXT018
    10.1.23.19  DFNEXT019
    10.1.23.20  DFNEXT020
    10.1.23.21  DFNEXT021
    10.1.23.22  DFNEXT022
    10.1.23.23  DFNEXT023
    10.1.23.24  DFNEXT024
    10.1.23.25  DFNEXT025
    10.1.23.26  DFNEXT026
    10.1.23.27  DFNEXT027
    10.1.23.28  DFNEXT028
    10.1.23.29  DFNEXT029
    10.1.23.30  DFNEXT030

    # DFNKIT cameras
    10.1.21.0     DFNKIT00
    10.1.21.1     DFNKIT01 DFNKIT1
    10.1.21.2     DFNKIT02 DFNKIT2
    10.1.21.3     DFNKIT03 DFNKIT3
    10.1.21.4     DFNKIT04 DFNKIT4
    10.1.21.5     DFNKIT05 DFNKIT5
    10.1.21.6     DFNKIT06 DFNKIT6
    10.1.21.7     DFNKIT07 DFNKIT7
    10.1.21.8     DFNKIT08 DFNKIT8
    10.1.21.9     DFNKIT09 DFNKIT9
    10.1.21.10    DFNKIT10
    10.1.21.11    DFNKIT11
    10.1.21.12    DFNKIT12
    10.1.21.13    DFNKIT13
    10.1.21.14    DFNKIT14
    10.1.21.15    DFNKIT15
    10.1.21.16    DFNKIT16
    10.1.21.17    DFNKIT17
    10.1.21.18    DFNKIT18
    10.1.21.19    DFNKIT19
    10.1.21.20    DFNKIT20
    10.1.21.21    DFNKIT21
    10.1.21.22    DFNKIT22
    10.1.21.23    DFNKIT23
    10.1.21.24    DFNKIT24
    10.1.21.25    DFNKIT25
    10.1.21.26    DFNKIT26
    10.1.21.27    DFNKIT27
    10.1.21.28    DFNKIT28
    10.1.21.29    DFNKIT29
    10.1.21.30    DFNKIT30

    # DFNSMALL cameras
    10.1.17.0     DFNSMALL00
    10.1.17.1     DFNSMALL01
    10.1.17.2     DFNSMALL02
    10.1.17.3     DFNSMALL03
    10.1.17.4     DFNSMALL04
    10.1.17.5     DFNSMALL05
    10.1.17.6     DFNSMALL06
    10.1.17.7     DFNSMALL07
    10.1.17.8     DFNSMALL08
    10.1.17.9     DFNSMALL09
    10.1.17.10    DFNSMALL10
    10.1.17.11    DFNSMALL11
    10.1.17.12    DFNSMALL12
    10.1.17.13    DFNSMALL13
    10.1.17.14    DFNSMALL14
    10.1.17.15    DFNSMALL15
    10.1.17.16    DFNSMALL16
    10.1.17.17    DFNSMALL17
    10.1.17.18    DFNSMALL18
    10.1.17.19    DFNSMALL19
    10.1.17.20    DFNSMALL20
    10.1.17.21    DFNSMALL21
    10.1.17.22    DFNSMALL22
    10.1.17.23    DFNSMALL23
    10.1.17.24    DFNSMALL24
    10.1.17.25    DFNSMALL25
    10.1.17.26    DFNSMALL26
    10.1.17.27    DFNSMALL27
    10.1.17.28    DFNSMALL28
    10.1.17.29    DFNSMALL29
    10.1.17.30    DFNSMALL30
    10.1.17.31    DFNSMALL31
    10.1.17.32    DFNSMALL32
    10.1.17.33    DFNSMALL33
    10.1.17.34    DFNSMALL34
    10.1.17.35    DFNSMALL35
    10.1.17.36    DFNSMALL36
    10.1.17.37    DFNSMALL37
    10.1.17.38    DFNSMALL38
    10.1.17.39    DFNSMALL39
    10.1.17.40    DFNSMALL40
    10.1.17.41    DFNSMALL41
    10.1.17.42    DFNSMALL42
    10.1.17.43    DFNSMALL43
    10.1.17.44    DFNSMALL44
    10.1.17.45    DFNSMALL45
    10.1.17.46    DFNSMALL46
    10.1.17.47    DFNSMALL47
    10.1.17.48    DFNSMALL48
    10.1.17.49    DFNSMALL49
    10.1.17.50    DFNSMALL50
    10.1.17.51    DFNSMALL51
    10.1.17.52    DFNSMALL52
    10.1.17.53    DFNSMALL53
    10.1.17.54    DFNSMALL54
    10.1.17.55    DFNSMALL55
    10.1.17.56    DFNSMALL56
    10.1.17.57    DFNSMALL57
    10.1.17.58    DFNSMALL58
    10.1.17.59    DFNSMALL59
    10.1.17.60    DFNSMALL60
    10.1.17.61    DFNSMALL61
    10.1.17.62    DFNSMALL62
    10.1.17.63    DFNSMALL63
    10.1.17.64    DFNSMALL64
    10.1.17.65    DFNSMALL65