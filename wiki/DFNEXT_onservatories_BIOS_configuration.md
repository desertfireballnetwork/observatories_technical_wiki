## Commell LE-37G single board industrial PC

This type of SBC is used in DFNEXT observatories. DFN observatories
specific configuration is described below.

For more details regarding the Commell LE-37G single board industrial
computer please refer to user manual on the manufacturer's web site
[1](http://www.commell.com.tw/Support/Product%20Technical%20Support/LE-37G.htm).

### Standard HW jumpers setup - LE-37G7

`JAT: AT/ATX mode select jumper: switch to AT instead of ATX mode`
**`1-2 AT mode`**
`2-3  ATX mode (Default)`

`JMSATA: Setting MINI_CARD2 to support PCIe/mSATA: select Support mSATA instead of default`
**`1-2 Support mSATA`**
`2-3  Normal operation (Default)`

### BIOS - LE-37G7

1\. Enable Diagnostic Splash Screen

*Note: prints eg "Hit Del to enter BIOS setup"*

` Boot Features -> Diagnostic Splash Screen  [Enabled]`

2\. SATA configuretion - enable hot swap

*Note: default is Disabled (symptoms if not set - only 2 removable
drives recognized - those attached to the add-on mini PCIe SATA card)*

`Advanced -> Intel Advanced menu -> PCH-IO Configuration -> SATA Configuration`
`Serial ATA port 1 - Hot plug - set to  [Enabled]`
`Serial ATA port 2 - Hot plug - set to  [Enabled]`
`Serial ATA port 3 - Hot plug - set to  [Enabled]`
`Serial ATA port 4 - Hot plug - set to  [Enabled]`

3\. Set auto wake up from S5 power down (not critical)

*Note: S5 is power down. This will save the day if the JAT jumper had
fallen off - otherwise not effective*

`Advanced -> Intel Advanced menu -> ACPI settings`
`Wake system from S5 [Enabled]`
`Wake Up Hour  [8]`

4\. Disable unused serial ports to suppress error message during boot
(not critical)

*Note: just to suppress an error print in POST boot message)*

` Advanced -> Intel Advanced menu -> Super IO Chip`
` Serial Port 3 [Disable]`
` Serial Port 4 [Disable]`
` Serial Port 5 [Disable]`
` Serial Port 6 [Disable]`

5\. Save and exit

`Exit -> Exit Saving changes`