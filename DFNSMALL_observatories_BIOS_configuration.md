## Commell LE-37D single board industrial PC

This type of SBC is used in later DFNSMALL and all DFNKIT observatories.
DFN observatories specific configuration is described below.

For more details regarding the Commell LE-37D single board industrial
computer please refer to user manual on the manufacturer's web site
[1](http://www.commell.com.tw/Support/Product%20Technical%20Support/LE-37D.htm).

### Standard HW jumpers setup - LE-37D, DFNKIT cameras

In case of mSATA system SSD disk (mini card) used
- HW jumper JMSATA - must be in position 1-2 (default is 2-3)

### BIOS - LE-37D, both DFNSMALL and DFNKIT cameras

1\. PC behaviour after power resume

`BIOS -> Others-> Super IO Configuration -> PowerLoss -> Power control [Always ON]`

2\. SATA

`BIOS -> Advanced -> South cluster configuration -> SATA Drives - select AHCI mode (This is default setting)`

3\. USB3
There is one USB3 port and 5 USB2 ports, by default USB3 port is
extended power only, but USB2 protocol/link speed. To enable USB3:

`BIOS -> Advanced -> South cluster configuration -> USB - disable EHCI nmode, enable xHCI mode`

4\. SIM card slot - only for use with mPCIe mobile data 3G/4G modem,
otherwise can be left on defaults

`BIOS -> Advanced -> South cluster configuration -> Miscelaneous    - WLAN card presence - UHPAM card inserted -> ENABLE`

5\. RTC alarm every day wake up (effective only in case of accidentally
suspended/hibernated, not when powered down or failed to boot)

`BIOS -> Others -> APM Configuration ... Enabled, Everyday, 08:00 (HW clock is in UTC)`

6\. Watchdog timer

`BIOS -> Others-> Super IO Configuration -> Watch dog timer -> Watch dog timer select -> disable (This is default setting)`

*Note: disable Watchdog in BIOS temporarily when cloning system*

7\. Save and exit

`Exit -> Exit Saving changes`