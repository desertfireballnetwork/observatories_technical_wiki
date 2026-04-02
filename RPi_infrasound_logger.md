Also see instructions from Masa-yuki-san.

# Power

Powered using 12V 100Ah+ [temporary battery with
sunshield](temporary_battery_with_sunshield.html).

# Localisation

Location of the instrument is NOT logged by the instrument itself. Do at
least [handheld GPS unit
logging](handheld_GPS_position_logging.html). AND relative
position with respect to 3 nearby sites using tape measurer.

# Set up

- Remove transport padding from inside the box.
- Visually check cable connexions
- Feed 2.1 mm jack to inside the box.
- Place box under sunshield (South of the battery)

# Plug in / Turn on

Plug in instrument box via 2.1 mm 12V jack to cigarette lighter plug.

Inside the box, screen will display its status:

- "GPS OK"
- "starting logging"
- "logging"

When you leave the screen should still be on "logging".

# Turn off

Simply unplug the power as there is no power down button (there is a
small risk of SD card corruption).

# Data retrieval

## Option 1: ssh

- connect ethernet cable between RPi and laptop
- set laptop IP to some thing in the same range
- "mkdir -p ~/Hayabusa_collected_data/infrasound/instrument_ID"
- "rsync -a pi@192.168.1.56:~/data/
  ~/Hayabusa_collected_data/infrasound/instrument_ID"
- you can use the opportunity to be connected to safely turn it off (if
  packing up at the same time): "ssh pi@192.168.1.56 sudo reboot", wait
  a couple of seconds, and then unplug.

## Option 2: direct card read

- Take micro SD card out, plug it in a good brand card reader directly
  (avoid using micro SD -\> SD adapter)
- "lsblk"
- "mkdir -p ~/Hayabusa_collected_data/infrasound/instrument_ID"
- "sudo mount /dev/sdX1" ~/rpi_data_mount
- "rsync -a ~/rpi_data_mount/home/pi/data/
  ~/Hayabusa_collected_data/infrasound/instrument_ID"
- "umount ~/rpi_data_mount"
- Put micro SD card back into RPi (do not erase data)