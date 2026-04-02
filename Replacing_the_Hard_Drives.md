## Physical drive swap

Firstly, for all cameras, ensure hard drives are not mounted (i.e.
"data1"). In a terminal, check:

`$ df -h `

If they appear, they will need to be unmounted and any enclosures
disabled:

`$ umount /data1`
`$ /opt/dfn-software/disable_ext-hd.py  # this is for DFN-EXT and DFNSMALL only, not necessary for kits.`

Depending on your camera model, the drives can be swapped as follows:

### DFN EXT

### Kit Cameras

Your camera model has one hard drive that hangs from a plastic mount.

1.  Open lid using a large flat-head screwdriver.
2.  Remove short cables from camera (most likely just the USB connector,
    possibly also the trigger cable and power.
3.  Unscrew 4 small philips-head screws that hold the HDD.
4.  Remove SATA cable from HDD.
5.  Remove the 4 extension pins from the old drive and screw them into
    the new HDD.
6.  Reconnect SATA cable to new HDD.
7.  Reattach to plastic mount using the 4 philips-head screws.
8.  Reconnect cables removed from camera.
9.  Before closing lid, check that the HDD SATA cable is properly
    attached; check camera trigger cable, power cable, LC shutter cables
    and USB cable are all properly connected.
10. Close lid. (suggested to run test procedures before fully tightening
    lid)

### DFN Small 

Your camera model should have the hard drive enclosure mounted on the
door.

\- Data1 can be removed by simply opening the outer enclosure door.

\- Data2 requires the enclosure to be removed to access.

1.  remove screws at the base of the hard drive enclosure. (Keep them
    safe, they are easy to get lost!)
2.  swap the drive
3.  replace the enclosure ensuring all screws are tight. Wobbling when
    in use could affect images.

## Subsequent procedures

### GUI

- insert how to do format HDDs and interval control via GUI

### Manually installing new HDDs

If there are any issues with the GUI while attempting to format HDDs or
run the interval control test, see the [Manual
Maintenance](Manual_Maintenance.md "wikilink") page.