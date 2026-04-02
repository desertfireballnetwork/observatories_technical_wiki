# NOTE: ALL THE CONTENT BELOW HAS BEEN PERMAMENTLY MOVED TO

=<https://desertfireballnetwork.github.io/drone_meteorite_doc/=>

# GO FIND UP-TO-DATE INFORMATION THERE

# DO NOT KEEP UPDATING THIS PAGE

## Introduction

<figure>
<img src="DFN_drone_Meteorite_detection_xsmall.png"
title="DFN_drone_Meteorite_detection_xsmall.png" width="600" />
<figcaption>DFN_drone_Meteorite_detection_xsmall.png</figcaption>
</figure>

## Main Survey Data Collection

### Main Survey requirements - in case using other drone than M300 + Zenmuse P1 & 50mm lens

#### Pictures quality

- Final **ground resolution** should be between 1.8 mm/pix and 2.2 mm/
  pix resolution, know your drone, know your resolution. This may be
  relaxed if looking for larger meteorites (\>300g).
- **Images should not be blurry**. You should calculate what the motion
  blur is based on your ground sampling distance, flight speed, and
  shutter speed, so it does not exceed 1 pixel during one exposure.
  Note: motion blur may not be visible in the images, so this really
  needs to be calculated.
- Flight speed calculation: at shutter speed 1/2000, 1 pixel travelled
  per shot, for example 2mm/pixel resolution required -\> v = 2mm /
  (1/2000)s = 4m/s.
- There technically doesn't need to be any overlap between the images.
  However, you need to figure out a safe overlap to make sure there are
  no gaps in the survey, considering no drone will fly a perfect survey.
- **Minimum sun elevation: TBD X degrees** (to avoid long shadows
  creating too many false positives.). Use
  <https://www.sunearthtools.com/dp/tools/pos_sun.php> to calculate
  survey start/end times for your location. **Ignore if overcast**.

#### Metadata

Special attention should be given to make sure appropriate **metadata is
tagged in the images**, otherwise it will be a mess and the survey may
have to be redone from scratch.

- Drone GPS, Drone speed, Drone and gimbal yaw/pitch/roll. These should
  all be present in the Exif of the survey images.
- Flying the survey drone with **Real-Time Kinematics** (either using
  your own RTK base station, or connecting to a nearby one online) is
  very desirable. It will record better metadata for georeferencing
  (altitude notably). And, depending on the configuration of your drone,
  it will help it fly at a constant altitude.
- Timezone set to UTC

### Survey Preparation Prior to Fieldtrip

When the data is processed, the oldest images are somewhat prioritised.
Hence for the higher priority/probability areas to be processed and seen
first, they should be surveyed first.

The simplest way to achieve this is to use Google Earth to cut the
search area polygon into 5-20 smaller polygons, depending on the size of
the survey:

- Import the kmz/kml fall line bounds that are output by the darkflight
  calculations
- Adjust the transparency to ~30-40 percent and change the colour (pick
  your favourite), this will allow you to keep the background visible so
  you know if you planned flights cover the entire fall zone
- Move the cursor back and forth across the fall zone, while monitoring
  the ground elevation in the bottom corner of the screen, make a mental
  note of any major elevation shifts or gradual inclines across the fall
  zone
- Using the polygon tool, draw a ~1 km x 1 km box at one end of the fall
  zone
- If there is an elevation shift or difference of more than 5 m, tailor
  the shape such that the elevation does not change by more than 5 m
  within the polygon
- Repeat drawing polygons until the entire fall zone is covered, make
  sure to overlap the polygons by 30 m. Try not to make the polygons too
  elongated (no more than a 10:1 ratio for longest:shortest axes).
- To save each polygon to a kml, right click on its entry under ‘Places’
  (a tab menu on the left side of Google Earth), and Rename it. Try to
  use a naming convention that makes sense, such as the name of the fall
  site, and general relative direction, and a sequence number that will
  sort them by survey priority.
  - BoreZ_03_FarEast
  - BoreZ_01_East
  - BoreZ_03_MiddleNorth ... etc
- Once renamed, right click again and select ‘Save Place As…’, which
  should show you a filesave dialog window. Make sure to save as a .kml

### Upload to the drone flight controller (works on M300 and Mavic 3 Pro)

- Once all are saved, copy the kmls to the micro-SD card that will slot
  into the M300 smart controller, make sure to save another set of
  copies in the trip log directory.
- convert the .kml files into flightplans on the M300 smart controller
  - TODO This section needs to be redone by Seamus once I have the
    flight controller in hand, I want to be more specific about the sub
    menu details and names
  - With the .kml-loaded micro-SD card inserted into the smart
    controller, select 'Flight Route', 'Import Route(KMZ/KML)', and
    select your .kml, then select 'mapping' as the flight type, the
    flight mission should appear in the list of missions
  - Select your new mission, near the top left you should see your
    mission name, and to the left of the name a blue circle with a white
    triangle, this will begin the mission when you are ready
  - To the right of the survey name should be be a drop down arrow,
    select that and a menu should appear with some flight info, elect
    the pen and line icon to change the parameters. See parameters in
    the next sections.

### Flight parameters (works on M300)

- - First, scroll to the bottom and select 'Advanced Settings'
  - change Side and Frontal Overlap to 10% each, Margin should be 0.
  - Distance Interval Shot.
  - Shutter priority.
  - Shutter speed: 1/2000 s.
  - Go back to parent menu, and start from the top.
  - Select your camera 'Zenmuse P1' and your camera lens '50 mm'
  - Smart Oblique 'OFF'
  - GSD should be ~0.2 cm/pix when you are finished, but it should not
    be correct when it first appears
  - Safe takeoff altituted should be 20 m, adjust if local terrain
    requires
  - Terrain Follow 'OFF'
  - Altitude Mode 'Relative to Takeoff Point'
  - Flight Route Altitude should be between 25-30 m
  - Target Surface to Takeoff point should be 0 unless there are major
    elevation differences between your takeoff point and the survey area
  - Takeoff Speed '10 m/s'
  - Speed '4 m/s'
  - Course Angle: adjust to whatever makes sense for your polygon,
    typically cross the fall line.
  - Elevation optimization 'OFF'
  - Upon Completion 'Return to Home'
  - Once Complete, double check that the parameters are correct, and
    press the back arrow at the top left of the screen, make sure to
    save your changes
  - P1 camera settings - in camera view
    - Shutter priority, 1/2000
    - ISO auto
    - First waypoint AF
    - Check focus, mind trees - we need to focus on ground not top of
      trees!
    - The drone does focus on flight path resume (after eg swapping
      batts/mem card), so focus needs to be checked each time.

### Conducting the survey

**One person should always be monitoring the drone during flight, this
is their only job, this is a critical point of failure even though it is
boring. This is also the only part of the process that could be
dangerous as the drone could collide with crewed aircraft, people on the
ground, or power lines (fire hazard)**

- Drone IMU/Compass calibration - once on a new site, better every day
  before flying
  - Camera view, Flight controller settings, Sensor status -
    IMU/Compass - calibrate both as per instructions
  - Camera gimball calibration - Camera, Gimball settings - calibrate
- Every pass across the current flight zone you should look up to
  visually check the altitude and clearance above any obstacles
  - A previous error was encountered when the survey height was less
    than 30 m, the drone’s height would begin to drop over the course of
    the flight and the barometer sensor would not register the change,
    only the ventral ultrasonic sensors. TODO: to check, but hopefully
    RTK surveying will help maintain proper altitude.
- The smart controller volume should be loud enough to just make out the
  image capture sound effect every ~1 to 2 seconds, this will help
  inform you if the drone is behaving properly
- The drone pilot should rotate out with another pilot after 2 flights
  to keep them alert, rotating out between each flight is acceptable
- When the drone batteries are below 20% you should wait until it has
  completed a pass across the fall zone and is on the side closer to you
  to recall the drone
- recall the drone by pressing and holding the ‘home’ button on the
  controller (the controller should start beeping)
- monitoring its return
- periodically check the images that come back using 100% zoom level on
  a computer, to check the focus.
- every time moving RTK basestation: Reset position in the basestation
  web interface

*Note:*

## Data upload

Instructions for data upload are on Github:
<https://github.com/desertfireballnetwork/dfn-meteorite-drone-webapp/blob/main/cli/README.md>

## Training Data Collection

Capturing quality training data is key to the whole process. Remember:
Garbage in, garbage out. Hence take extra care in doing this step well.

### True images (meteorites)

#### Pack list

- Meteorites OR meteoritic shaped rocks (meaning a rock between 2 and 10
  cm diameter, with no elongated axis, or sharp edges)
- 2 people: one drone pilot, one pointer.
- black spray paint (matte or shiny).
- a drone
- a cardboard box and some gloves can be useful to avoid making a mess
  with the painted rocks

#### Capturing true training data

- Final resolution should be between 1.8 mm/pix and 2.2 mm/ pix
  resolution, know your drone, know your resolution. If using the M300
  with the Zenmuse P1 (48 MP) camera and 50 mm lens, your altitude
  should be between 18 and 30 m.

If you have real meteorites with fresh fusion crusts,

- place a meteorite and point to it at least 3 m away, then pick up the
  meteorite to re-place it, to then take the next picture (only repeat
  up to 5 times, rotating and changing orientation each time).
- If possible place a small piece of Aluminum Foil that is smaller than
  the meteorite between the rock and the ground, your curator will thank
  you
- DO NOT LEAVE REAL METEORITES IN A LINE LIKE FAKE ONES. WE ARE HERE TO
  ADD TO THE METEORITE COLLECTION NOT SUBTRACT FROM IT

If you have fake meteorites (i.e. spray-painted rocks)

- place them in a line near different background objects (limestone
  rocks, saltbush, grass tuft, hole, lichen colony, etc) at least 8 m
  apart from the others, then walk next to the line and point to each
  rock
- The pointer should walk slowly, pointing to the rock nearest to them
  (standing at least 3m from the rock), the drone flyer should take only
  one image per rock, and should call out to the pointer if the pointer
  moved too quickly and the flyer was unable to take an image, otherwise
  the flyer should confirm ‘good’ once they have taken an image of the
  current rock in question

For Both Real and Fake Meteorites

- Between images, rotate the drone by up to 90 degrees to change the
  angle of shadows in the image.
- Before uploading the data, remove non-useful images (duplicates of the
  same pointed-at rock, unrelated random pictures). This clean up step
  will help limiting the amount of work downstream to label the data,
  and ultimately keep the training dataset clean.
- When labelling (drawing a box around the rocks) make sure to include
  the shadow the rock makes, that will help distinguish the rock from
  holes in the ground or other features that have no shadow. -\> TODO
  copy that bit to the webapp documentation.

*Do we need to capture true training data at every site?* We have built
up a large dataset of meteorite and meteorite-looking rocks for
training, hence the return on adding more training data tends to
diminish. However, this step is a relatively low effort thing to do at
each new site visited (assuming you are just doing 10-20 meteorites).
Over time it does truly really help increase the variance in the
training set, ultimately making model predictions more accurate. So
avoid skipping it.

### False Images (background)

- Fly in a perimeter around the fall zone and take images every 10-30
  seconds.
- During the perimeter flight, slowly adjust the height +- 5 m around
  the height you intend to survey at
- If the search area displays significant variations in ground features
  (rock type, vegetation), try to capture as much of these different
  backgrounds as possible.

*Can I skip false training data collection at one site?* No, the
prediction model will perform better if trained on the local background
(vegetation, rock types, animals...). So far we have not seen two fall
sites that are similar enough to a point where that step could be
skipped.

## Low-resolution orthomosaic

(optional, but this is a relatively cheap thing to do)

Satellite imagery available online is usually not high-enough resolution
or up-to-date to be an effective background map.

One solution is to create a low-resolution orthomosaic using a
low-resolution high-overlap drone flight. 10cm/pixel is a good target
ground sampling distance. The issue is that most drones actually give a
too-high ground sampling resolution, even when flying at maximum height
(120 m in Australia).

Why not just use the main (high-res) survey image to do this, instead of
having to capture a dedicated flight? Because stitching lots of images
is very computationally costly, and the overlap in the main survey is
not large enough. This mini survey will be a tiny fraction of time and
bandwidth compared to the main survey, it is worth it!

### Data Collection

- survey area: define a polygon that extends the main survey polygon by
  at least 100m in all directions.
- Ground resolution: 5 cm /pixel
- Overlap: 70%
- If available, definitely do RTK surveying. RTK + stitching adjustments
  will make this background map very accurate.
- (optional), it is possible to use Ground Control Points
  (https://docs.webodm.net/how-to/ground-control-points), to get even
  more accuracy (or if RTK is not possible with your setup).

### Data Processing

- Upload and process the data on WebODM Lighning (cloud version)
  [1](https://webodm.net/dashboard).
- Download the Orthophoto data product (large GeoTiff file).
- Degrade the GeoTiff resolution to 0.1 m/pixel using gdal: gdalwarp -tr
  0.1 -0.1 source.tiff destination.tiff
- Upload and process the tileset on Mapbox: follow instructions here:
  \[Mapbox tileset Command Line Interface\]
- set the background tileset ID for the survey in the Webapp admin
  console.

### WebODM setup

- feature-type: orb
- use-fixed-camera-params: enable
- sfm-no-partial: disable
- skip-3dmodel: enable
- skip-report: enable
- fast-orthophoto: enable
- dsm: disable

### Mapbox tileset Command Line Interface

- install the Mapbox Tilesets CLI
  [2](https://github.com/mapbox/tilesets-cli)
- create a Mapbox token
  [3](https://console.mapbox.com/account/access-tokens), that has all
  the "Upload" and "Tilesets" scopes checked.
- set the token as environment variables: export MAPBOX_ACCESS_TOKEN=
- upload the GeoTiff as data source: tilesets upload-raster-source
  <username> <source_id> ./file.tif
- create a recipe json file for Mapbox Tiling Service raster processing:
  use [this as
  template](https://gist.github.com/hdevillepoix/dec56b89989dcd4cede9fb12b79197e1)
- create the tileset: tilesets create hadry.dn230523 -r recipe.json -n
  dn230523-05-low
- publish the tileset (this triggers the processing job): tilesets
  publish hadry.dn230523
- if all went well there should be a mapbox tileset available with ID
  "hadry.dn230523"

## Starlink and WiFi extender set-up

- The Startlink and TP-link routers shall be pre-configured as on
  following diagram.
- The TP-link router shares the IP range with the Starlink (in some sort
  of bridge mode).
- The use of battery powered DFN_repeater is optional. Handheld devices
  can connect directly either to the Starlink or to the TP-link router.

<figure>
<img src="Starlink_wifi_extender.jpg"
title="File:Starlink_wifi_extender.jpg" />
<figcaption><a
href="File:Starlink_wifi_extender.jpg">File:Starlink_wifi_extender.jpg</a></figcaption>
</figure>

## RTK base station setup

Receiver borrowed from Geospatial Sciences: Trimble R10 [(help
portal)](https://help.fieldsystems.trimble.com/r10/home.htm)

Factory reset if needed - clears all WiFi client settings, that can be
whatever from previous user. It starts in WiFi AP mode then.

Default login: admin/password

Default IP address: is on a sticker on the receiver.

- I/O config – Port Configuration:
  - NTRIP caster 1
  - Mount point: RTCM32MP
  - RTCM Enable, 3.x (turns green)
  - If confirm/save on the bottom of the page complains about reference
    station too far away:
- Receiver Configuration – Reference station:
  - Click on \[Here\] to load the current position of the receiver as
    the reference station position.
- WiFi:
  - Client configuration: enter SSID and password for Starlink router
    (or other/alternative WiFi router); Static IP seems to work better
    than dynamic, assigned by the router - even if the router was
    configured to assign a fixed IP.
  - Status: select client mode

*Note: the Rhonda US-riot router had difficulties to work with Starlink,
connected to it's Ethernet port as LAN router.*

## Meteorite Candidates Follow-up

### Phone/Tablet RTK setup

- Install GNSS Master app (Android only, N/A for Apple devices)
  - NTRIP Client V2
  - NTRIP port 2101
  - IP address as selected in Trimble R10 base station setup
  - Credentials: admin/password
  - Status -\> Mock location

<!-- -->

- GNSS -\> Bluetooth
  - Pairing PIN: 1234
  - Bluetooth device name: RTK_GNSS_long-number

*(Note: put a sticker on the GNSS receivers so the will not mix up,
people easily select the one that they already had paired.)*

- Android settings
  - Enable developer mode (Settings -\> About phone -\> Software
    information -\> tap 7x on Build number)
  - In Developer options -\> Select mock location app -\> GNSS Master

*(Note: this cuts off the in-device GPS, so when GNSS RTK receiver is
not blootooth tethered, there is no location update for the Andoid
device, silently stuck at the last position.)*

## Webapp Deployment

See deployment notes:
<https://github.com/desertfireballnetwork/dfn-meteorite-drone-webapp/tree/main/webapp>

## GPU machine Deployment

Heavy lifting jobs (Machine Learning training and inference) need a GPU
desktop or beefy VM. This machine just needs to be connected to the
internet, and will work as a slave wrt the webapp on the VM.

See install notes:
<https://github.com/desertfireballnetwork/dfn-meteorite-drone-webapp/tree/main/mldaemon>

## Checklists

### Misc

### Computer things

- Laptop + backup laptop. Linux distro, ~1TB free space, SD card reader.
  On DFN VPN, with relevant people's public keys authorized so remote
  debugging can happen.
- Spin-up Webapp VM.
- Start high-performance GPU computer (lab), and/or reserve a g2.xlarge
  GPU VM on Nectar
  (https://dashboard.rc.nectar.org.au/project/reservations/), ideally on
  QLD or Swinburne (easier because they have ephemeral disk). Start the
  client (see
  [\#GPU_machine_Deployment](#GPU_machine_Deployment "wikilink")).

### Comms

- Starlink unit.
- Starlink ethernet adapter.
- Re-activate Starlink subscription that should be on pause (ask
  Hadrien).
- WiFi router with WAN port and swappable antenna (SMA).
  - TP-link router as wifi extender with a mast to go on the ute tray
  - 5+ m ethernet cable for the router
- high-gain 2.4GHz WiFi antenna.
- hand-held UHF radios (1 pp) + charger.

### Power

- Generator.
- Petrol and/or diesel. Usage: ~15 litres / 12h
- Battery station + cables (notably XT90 to Anderson plug connector).
- Rhonda roof-rack solar system: ~300W panel + MC4 fuse + MC4 extension
  leads + MC4 to Anderson adapter (all of this should be mounted
  permanently on Rhonda and not removed).

### Drone

- Drone.
- Drone camera + lenses.
- Drone controller.
- Batteries (charge them just before the trip).
- Battery charger.
- SD cards.
- Something to make a landing pad (e.g. tarp).
- stuff for making ground control points (optional, for the low-res
  survey)

### Extra surveying equipment

- RTK base station + batteries (typically Trimble R10 or R12)
- Android mobile devices (1 pp who does stage 4). These need good gyro
  and compass. Cheap devices (\<\$500) are usually not suitable for this
  task (Samsung: S series devices are okay, but not the A series). Most
  people prefer a tablet over a phone for this task.
- optional, for doing stage 4 RTK: ardusimple units (1 per stage 4
  device)