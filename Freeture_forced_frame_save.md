The continuous capture mode was in the freeture already, but it did not
save anything to file; only to memory or screen (X required for screen;
useful eg for lens focussing). I have added an option to save the video
frames.

There is a script that stops the freeture service running in fireball
detection mode, runs this continuous capture mode for a given time
(seconds) and then re-starts freeture service again.

This script (`/usr/local/bin/freeture_continuous_video.sh`) and the
relevant freeture version is part of the EXT auto install process. Will
install automatically of on demand (with
dfn_down_install_sw_from_server.sh). Some extra configuration in the
system might be needed for cameras that were off-line for a long time.
I'm happy to do that remotely once you let me know. It is very KISS, the
capture parameters are set in the script. I can modify it if we needed
it to be more fancy - eg set the capture time as command-line parameter.

These params are passed (i think the names are self-explanatory):

`runTimeSeconds=120 videoGain=29 expTime=33000 imgWidth=1080 imgHeight=1080 imgStartX=420 imgStartY=60`

Best way to use it is to put line like this into crontab:

`0 20,21,22 5,6,7 12 * /usr/local/bin/freeture_continuous_video.sh`

(will run on Dec 5, 6, and 7 at 20:00, 21:00 and 22:00; records 120
seconds of video frames. The frame rate should be default from config
file 30Hz.) Please check the freeture watchdog is not acting during the
continuous video capture period. This watchdog is just a simple debug
on-liner restart ing freeture service if it is not running (for the case
it crashed). It is in crontab just at the end. The best way to make sure
it is not going to ruin the valuable timed capture is to comment that
line(s) out completely:

` ### restart freeture if crashed`
` #0-50/10 * * * * systemctl status freeture.service | grep running || systemctl restart freeture.service`
` #0-50/10 7-19 * * * systemctl status freeture.service | grep running || systemctl restart freeture.service`

The recorded frames go to /data0/video_frames/; a subfolder is created
for each night.

example file with path:

`/data0/video_frames/2020_10_30/DFNEXT015_2020_10_30_allskyvideo_frame-009617.fits.gz`

(Note: the frame no is generated as n+1 the highest number found in the
directory - will not re-write anything if scheduled multiple times a
day)

Logs are saved to folder `/data0/video_frames/log`.