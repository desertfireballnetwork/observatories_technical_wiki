## DFNEXT

This section describes how to focus FUJINON video camera lens in DFNEXT
camera systems (Point grey / FLIR digital cameras) using freeture video
capture software.

### Stop the freeture SW running as service

`systemctl stop freeture.service`

### Run freeture in to display currently captured video live on screen

Login as user which is not root locally (keyboard + monitor + mouse
needed).

Start windows manager

` startx`

Start terminal. The following commands are to be executed in x-terminal.

Switch user to root may be needed

`su - root`

Run freeture, zoom in, set window with video full screen or enlarge it
to se the focus is sharp:

`freeture -m 2 --display -g GAIN -e EXPOSURE_TIME`

Use GAIN (0 .. 29.99) and EXPOSURE time (0-33000 in microseconds; max
exposure time is driven by frame rate 30Hz) appropriate for the current
lighting conditions.

If getting complains, try to disconnect/re-connect the video camera USB3
cable or explicitly use following config file with a commandline switch:

`freeture -m 2 --display -c /usr/local/etc/dfn/freeture.cfg`

In full screen/enlarged mode, the image update may be slow or freeze.
This is related to the CPU load and I/O over USB load - transferring
image from video camera over USB and scaling it to screen buffer.
Restart the above command if that happens and zoom in only partially or
zoom 1:1, but shrink the window.

A tiny hex/allen key (size 0.9 mm) is needed to loosen the DFNEXT's
Fujinon fish-eye video lens focus fixing bolts and to tighten them again
when focused. There are 3 of these tiny set screws - to access them, the
lens needs to be temporarily removed from the camera enclosure (undo the
flange bolts from top of the camera system box with M7 spanner). On a
new lens, it may be quite hard to get the hex key into the screw head as
they are fixed with thread locking glue and some of the glue might be in
the hex socket insert.

*Note: Set the F-stop (mechanical lever on the lens) to full open, check
if the focus is sharp as with f/1.8 or f/2.0. If yes, keep the wide open
setting. If not, stop down to f/1.8 or f/2.0.*

### Stop freeture

By pressing \[Esc\] in the terminal window where it was started - not in
the window with live video stream.

### Start the freeture SW running as service again

`systemctl start freeture.service`

## AMOS Spec (or any machine vision camera equipped with a rectilinear lens)

### Things required

- physical access to the camera
- a laptop (Windows has more software GUIs for this job)
- a USB3 Micro B cable (camera side) + whatever your computer will take
  on the other side (USB3 A or USB-C)
- GUI to drive the camera in live view (see below)

### Software needed

Camera manufacturers typically ship basic GUIs for interacting with
their cameras, should be sufficient in most cases:

- The Imaging Source:
  <https://www.theimagingsource.com/en-us/support/download/iccapture-2.5.1557.4007/>
- Allied Vision:
- Point Grey/FLIR:

Astronomy capture GUIs can sometimes offer more control:

- <https://www.sharpcap.co.uk/>
- <https://www.firecapture.de/>

### Procedure

Best results are obtained using stars.

Using a foreground object \>50m away (building, tree...) is an
alternative.

- wait for a clear night, point camera at the sky.
- connect camera to your laptop directly using USB cable.
- launch GUI, connect to the camera.
- whack the gain up.
- set exposure to something high enough that enough light is collected,
  but live view feedback is still quick (0.1 s).
- make sure aperture ring is set to fully open (typically f/1.0 f/1.4,
  the lowest number possible), even if the setting in operational mode
  is not fully open.
- stretch the histogram on the GUI to enhance the brightness of faint
  detail (the live view image should be visibly noisy). Not all GUIs
  have this option. See "Adjusting the Display Histogram Stretch" on
  <https://docs.sharpcap.co.uk/3.2/13_CameraControls.htm>
- if focusing for the first time, play with the focus ring until you see
  some stars.
- fine tuning: easiest way to know you have achieved optimal focus is to
  look at faint stars in the field, the faintest ones will only be
  visible for a very short range on the focus ring.
- lock the ring with the thumb screw, and then check that you didn't
  accidentally move it.
- finally, check the orientation of the diffraction grating (as it
  sometimes rotates when focusing), so that it provides a horizontal
  spectra from the source. You can check this by shining a light on the
  camera and looking at the output.
