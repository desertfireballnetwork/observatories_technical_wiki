There are some settings you must configure before using your camera:

- its location, name, contact email, couple of preferences about emails
  you wish to receive.
- camera time zone.

You can do this either using the [Camera
GUI](Using_the_GUI_for_Regular_Maintenance "wikilink"), or using the
Command line as described below

Then you can runs some basic initial tests, as described below or in the
GUI page

## Using the GUI with a local connection

Connect to the camera locally, as described
[here](Logging_in_locally_via_WiFi_or_Ethernet "wikilink"). This could
be a laptop/cable, or via wifi, using a tablet/phone. Start your
browser, and follow the instructions in the [Camera
GUI](Using_the_GUI_for_Regular_Maintenance "wikilink") page.

## Command-Line steps.

This can be done remotely, or locally

#### Setting up the passwords

Instead of using passwords for each machine we normally log in with
keys, as it's easier (because you don't have to remember a different
password for each camera), and more secure in some ways. You can
generate these in the linux or macOS terminal using ssh-keygen. These
are some good instructions here:
<https://help.github.com/articles/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent/>

Make sure you use a password when you're creating the keypair. Then,
email dfn.camera.help@gmail.com the public key (it ends in .pub and is
ok to share around) which we will install in the camera once you plug it
in, and then you'll be able to log in using your private key (which you
should keep secure). The camera uses the public key to check that the
person logging in has your private key instead of the camera prompting
you to enter a password (although you will still enter a password on
your machine when you unlock your private key).

Once the key is installed it also works for sftp to browse through your
images if you want.

So just to summarise:

- generate a password protected keypair and send me the public key.
- plug the camera into power and Ethernet (it doesn't need to be
  outside—we test them on the ground first, but it will (should) start
  taking pictures at night).
- we'll email you back with the IP address once we've installed your
  key.

#### Configuring the camera time zone

As superuser, set the timezone using `dpkg-reconfigure tzdata`.

#### Configuring the dfnstation.cfg file

The file /opt/dfn-software/dfnstation.cfg is the main configuration file
for the camera operations and software. You need to configure some
settings in this, to set the camera location and name, so that the
camera still has sensible values in case the GPS systems has a problem.

Also, you need to change some preferences if you want to receive emails
of events from the camera.

See [detailed description](Dfnstation.cfg "wikilink"), use a command
like

`nano /opt/dfn-software/dfnstation.cfg` to edit the file

#### Capture control test

As superuser, run an capture control test
using `/opt/dfn-software/interval_control_test.sh`

This will take about 10 minutes, and simulate a nights worth of
observations in a short period of time. it will wait for 2 minutes until
"sunset", take photos for about 5 minutes, until "sunrise", and then
stop, and download the images.

**Note:** if the DSLR memory card is full or has a lot of images, this
test will take a long time, as it will first download all the existing
data from the DSLR.

After the test is finished, check images
using `ls /data0/latest_prev` (ensure there are a few .NEF image files).
We also recommend to check interval log (eg
2018-01-17_DFNEXT013_log_interval.txt) for errord and warnings.

**Note:** /data0/latest_prev is a symbolic link to a specific
(previsous) capture session directory, eg
/data0/DFNEXT013/2018/01/2018-01-15_DFNEXT013_1516021913. During the
capture control test run (or regular capture program run during the
night), it's /data0/latest that links to the current directory.

#### Removable hard drives check

This steps check that the removable HDDs are ready - installed, working,
and empty. Please follow the instructions on the [manual maintenance
page](Manual_Maintenance#Checking_removable_hard_drives "wikilink").

#### Overnight test

Let the observatory running in the lab for a few nights. After a night's
observations, check images the next morning using `ls /data0/latest`.
You can count the raw image files
using `ls -1 /data0/latest/*.NEF | wc -l`.

#### GPS test

If you can put the observatory outside or stick GPS antenna out of the
window to get GPS reception, you can also test GPS functionality -
detailed instructions are on the [manual maintenance
page](Manual_Maintenance#Checking_GPS "wikilink").