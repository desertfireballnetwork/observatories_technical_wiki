## Frequently asked quastions

**Q: Is there a way to read out the voltage of the power supply (of the
battery) remotely?**

A: Yes, on some camera systems. All DFNEXT systems have regular
technological auxiliary data logs in /data0/log/hardware_log/. That
includes mostly temperatures, but also the DC power voltage - which is
battery voltage in case of solar-powered sites. Some of them, however,
have a hardware bug on the PCB from the manufacturer, where it reads
0.0V rather than the correct DC power voltage.

**Q: How can we detect weak 2G/3G signal strength and low band with?
(PING time?)** Web GUI

A: That is possible from both Command-line and Web GUI - Please see the
updated wiki page
<https://wiki.dfn.net.au/index.php/Mobile_signal_strength_check>

**Q: Should we apply for prioritising the specific GSM numbers in
communication with the tower? (Some operators say it is possible – but
not for free...) – has somebody experience?**

A: Not sure. The other thing that could help - in case of a camera site
far away from the town, where is the telco tower - would be to use a
higher gain antenna on the camera end. That would make the tower to
"hear the camera system louder".

**Q: What is the typical amount of data transfer per month?**

A: approximately 1 GB or less, providing

a\) people do not download a lot of files (one raw image is ~ 50 MB)

b\) there are not too many false positives caused for example by light
pollution or a nearby airport

A post-paid plan is big help to optimise the operational efforts, a lot
more efficient than pre-paid. DFN use M2M group plan intended for
machine-to-machine communication / Internet of things. Data bandwidth is
shared by a number of SIMs in the pool. We can even check the connection
status via a special web app provided by the operator, using the SIM
IMEI (serial number).

**Q: For which cameras would a «mask» help?**

A: Those with obstacles in the field of view