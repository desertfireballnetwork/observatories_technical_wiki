Each DFN observatory has several disks with partitions for data. The
operating system and buffer for several nights of data is on a fast and
low power SSD drive, typically 500GB size. Most of it's size is
allocated to data0 partition. This one is mounted as /data0 in the
embedded PC OS filesystem. Last up to two nights of raw data plus all
history of log files are typically stored in /data0.

On top of that, to be able to hold moths or years of data, there are
removable drives. The number of drives depends on the observatory type.
DFNKITs have just one drive (permanently powered), DFNSMALLs two drives
and DFNEXTs three drives. These are mounted as /data1, /data2 and
/data3. Every day in late morning the SW in the observatory moves all
the data except last night from the /data0 buffer to the removable
(archive) drives. They fill up from \#1 to \#3.

For DFNSMALLs and DFNEXTs, one needs to power up and mount the removable
drives, eg [using web
GUI](Using_the_GUI_for_Regular_Maintenance#Accessing_data_from_earlier_observations "wikilink")
or commandline (ssh terminal, eg Putty in Windows, log in and copy these
commands)

commandline example:

`python /opt/dfn-software/enable_ext-hd.py`
`mount /data1`
`mount /data2`
`mount /data3`
*`... access data`*
`python /opt/dfn-software/disable_ext-hd.py`

*Note: It is essential to power off the HDDs when finished with data
transfer!*

*Note: When they get completely full, the SW starts compressing the old
data - converting raw (.NEF) images to .jpg, but only those images,
where event detection was successfully executed and did not detect any
likely fireball event. This compression functionality extends the time
before the disk need to be replaced or data remotely deleted.*

The data structure in /dataX folders is following:

`/data1`
`├── DFNEXT022`
`│   ├── 2018`
`│   │   ├── 01`
`│   │   │   ├── 2018-01-04_DFNEXT022_1515114584`
`│   │   │   ├── 2018-01-05_DFNEXT022_1515112201`
`                     ...`
`│   │   │   ├── 2018-01-30_DFNEXT022_1517353916`
`│   │   │   └── 2018-01-31_DFNEXT022_1517440320`
`│   │   ├── 02`
`│   │   │   ├── 2018-02-01_DFNEXT022_1517526718`
`│   │   │   ├── 2018-02-02_DFNEXT022_1517613118`
`│   │   │   ├── 2018-02-03_DFNEXT022_1517699520`

Under normal conditions there is one folder with images per night -
unless there is for example power cut or some other interruption during
the night. We call these chunks of data "camera sessions".

As you can see, the data storage is organised by time and the folders
have the date included in the name.

`2018-01-30_DFNEXT022_1517353916 ... YYYY-MM-DD _ observatory name _ UNIX epoch time at the time when folder was created`

Each of these folders contains up to over 1000 captured still images and
a few other files like text logs. (The number of images depends of the
duration of night and cloudiness.) The files (still images) have
time-based file names as well, see the example

`022_2018-01-31_143200_E_DSC_1644.NEF  ... NNN_YYYY-MM-DD_HHMMSS_T_DSC_XXXX.NEF`

where

**`NNN`**` is observatory serial number`
**`YYYY-MM-DD_HHMMSS`**` is UTC time of the beginning of the exposure`
**`T`**` is the camera type (missing or S: DFNSMALL, E: DFNEXT, K: DFNKIT)`
**`DSC_XXXX.NEF`**` is the original file name from the Nikon DSLR`

There are also full-res lossy compressed jpeg files extracted from the
NEF files, which are useful for quick check as it reduces data transfer
volume and not everyone have NEF viewer or converter.

`022_2018-01-31_143200_E_DSC_1644.thumb.jpg ... NNN_YYYY-MM-DD_HHMMSS_T_DSC_XXXX.thumb.jpg`

And if not, it is easy to create a few with the following command

`dcraw -e 022_2018-01-31_143200_E_DSC_1644.NEF`

*Note: If the removable disks get full, most of the NEF files will get
automatically converted to .JPG.*