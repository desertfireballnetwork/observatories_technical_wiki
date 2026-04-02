the file on the camera

/opt/dfn-software/dfnstation.cfg

is the main configuration file for the camera operations. You **must**
set up some basic values before using the cameras.

You can edit it via the [GUI](Camera_GUI.md "wikilink"), or using the
command line on the camera (e.g, login as root, and run
`nano /opt/dfn-software/dfnstation.cfg`).

## Initial configurations needed

Use a handheld GPS unit, and get the local GPS coordinates for latitude,
longitude, and elevation. This should be in **decimal degrees**, with a
datum of **WGS84**, elevation is in **metres**. Use negative latitude
numbers for southern hemisphere. Don't forget the camera might be some
metres above the ground where you are holding the GPS unit.

The camera also needs a location name, which is a descriptive location.
**Do not have the string "test"** in the name, or the central server
will ignore data from this camera! (Unless you want the server to ignore
the camera, for example if you are repairing it and it is indoors)

Also, don't use spaces in the location; so for example, "New_york" is
OK. Capitals or lowercase doesn't matter.

You can then set the following fields - we've used example values below

`[station]`

`lat = 48.8580`

`lon = 2.2938`

`altitude = 354.0`

`location = Eiffel_Tower`

Email settings are used to tell the server about the camera. The
`owner_emails` field should be 0 or 1. if set to 1, then when the
triangulation server detects events involving this camera, it will email
the results to the `local_contact_email`. If you don't want to receive
emails from this camera, set to 0. `local_contact_email` can be a list
of email addesses, comma separated, no spaces.

`[link]`

`local_contact_email = guest@nospam.com`

`local_contact_name = John_Doe`

`owner_emails = 0`

The other vital step is to set up the [camera calibration
orientation](Calibrating_the_camera_orientation.md "wikilink").

## Other optional settings

Other settings that might be of interest, but dont need to be changed
normally - in the camera section:

camera_fstop is an index, which depends on the lens and camera chosen.
\#D800: 3=f3?,7=f6.3,9=f8?,11=f11?

camera_exposure_time is the length of time the shutter is open for each
exposure during the night operations. it is dependent on the DSLR used.
For D810, index 48 means 13sec, 49=15sec, 50=20sec ,51=25sec ,53=bulb
mode. it can be index number 51, or time 25s for normal.

camera_iso is the iso setting for the camera. for D810, setting 23 is
ISO 3200, 21 is ISO 6400.

enable_video is 0 or 1, to disable the video subsystem and only record
still images. Useful if power is limited, or disk space is low.

`[camera]`

`#camera hardware setup`

`still_lens = Samyang_8mm`

`still_camera = D810`

`vid_camera = Watec_902H2_Ultimate`

`vid_format = PAL`

`#PAL or NTSC`

`vid_lens = Fujinon046`

`camera_ser_no = 12345`

`vid_ser_no = 12345`

`camera_fstop = 3`

`camera_exposuretime = 53`

`clearing_exposuretime = 51`

`cloudy_exposuretime = 51`

`camera_iso = 23`

`#iso value used during twilight`

`twilight_iso = 5`

`#5 = iso100 on D810`

`enable_video = 1`

Internal settings control the operations during the night:

`[internal]`

`#time after sunset before start/finish in minutes, integer`

`sun_leeway = 0`

`#angle after sunset before start/finish in degrees`

`#we use -6=civil_twilight`

`twilight_horizon = -6`

`#number of minutes of 'twilight' mode after sunset before night operations`

`twilight_leeway = 10`