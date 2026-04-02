This page will contain instructions for

- recognising fireballs from satellites and other night-time phenomena
- downloading and viewing images from DFN observatories
- processing event detection emails
- initiating the data processing pipeline for triangulation and orbits

### Automated software for detecting fireballs

The images are automatically processed on the cameras.

The software basically works be differencing consecutive images, and
tries to identify lines in the pictures.

That means that it is unable to tell apart fireballs from satellites or
planes.

The software logs in a file called
YYYY-MM-DD_DFNXXXNNN__log_processing.txt.

In addition, each detection generates several files:

- .tile.jpg : light weight compressed JPG cut-out of the detection
- .pixels.txt : rough pixel coordinates of the event
- .raw_pixels.txt
- .radec.txt : rough astrometric coordinates of the event

### Central server vetting

As mentioned above, the results of the fireball detection software
contain numerous false alerts (satellites, planes...), therefore the
candidate events are vetted by a rough triangulation with other cameras.

### Email alerts for human review

Emails are sent only when a triangulation is validated by the central
server.

#### Valid detections

Each email is the result of a valid triangulation between 2 cameras
detected by the server. This means that if more than 2 cameras detected
the event, there are going to be one email for each pair of cameras.
![](005_2017-07-31_115327_E_DSC_0401.thumb.5200.3600.tile.jpg "005_2017-07-31_115327_E_DSC_0401.thumb.5200.3600.tile.jpg")
![](12_2017-06-30_142632_S_DSC_3018.thumb.masked.overkill.1200.400.tile.jpg "12_2017-06-30_142632_S_DSC_3018.thumb.masked.overkill.1200.400.tile.jpg")




















#### False positives

![](Satellite_detected_by_a_DFN_camera.jpg "Satellite_detected_by_a_DFN_camera.jpg")
![](17_2017-08-10_105313_S_DSC_0174.thumb.masked.6000.3200.tile.jpg "17_2017-08-10_105313_S_DSC_0174.thumb.masked.6000.3200.tile.jpg")