## Introduction

There are several options how to access data recorded by the DFN
observarory systems.

Easiest, especially when the intention is to archive the data on some
media anyway, is to buy same or similar drives that are in the DFN
camera system, visit the camera site and replace the drive(s) with data
with empty ones.

*Note: we recommend NAS grade low power / low RPM drives for a better
reliability compared to desktop grade drives and to minimize power
consumption; we use WD RED drives.*

It is useful to identify and sort the data files on the removable drives
that are selected for archiving remotely before visiting the site.

It is also possible to connect eg USB drive and move data on-site.
However, that may take many hours, or even days, depending on the
transfer speed. Therefore it is not practical for remote locations with
limited time allocated for each visit on site, unless you are interested
in collecting / copying only a small subset of data.

Once drives are collected, it is fairly easy to attach them to whatever
PC or MAC. They are standard 3.5" SATA drives, which should be readable
in most SATA/USB, SATA/FireWire or SATA/Thunderbolt enclosures/adapters,
providing they address the drive(s) same way as if attached to a regular
SATA port inside a PC box rather than using some proprietary format of
storing data. One example an incompatible device is WD My Book USB drive
(various models), sold with the HDDs. In general, better chance for
compatibility is with enclosures which are sold without HDDs. We have
great experience with enclosures offered by \[OWC
[1](https://eshop.macsales.com/shop/storage)\], who ship world-wide. As
a less expensive option with acceptable quality we can recommend
\[Orico[2](https://www.orico.shop/en/data-storage/)\] enclosures.

Ext4 filesystem is used for all the drives in DFN observatory systems.
There are 3rd party filesystem drivers for both MS Windows and Mac OS,
while in Linux or \*BSD ext4 is natively supported. Some of the drivers
may support only read-only access.

*TODO: add links to drivers*

## General steps for accessing removable drives in DFN observatory box

*Note: valid both for remote and on-site access.*

1\. power on removable drives in DFN camera

2\. mount all or specific drives

3\. inspect/manipulate/download/delete data (including eg moving from
SSD buffer partition /data1 to removable(s))

4\. umount all previously mounted drives

5\. power off removable drives

Please refer to links below regarding detailed instructions.

All the operations can be executed from commandline
<https://wiki.dfn.net.au/index.php/Manual_Maintenance>, some of that can
be done also using web GUI
<https://wiki.dfn.net.au/index.php/Using_the_GUI_for_Regular_Maintenance>.

## Access to level 2 data via Pawsey buckets

[L2 bucket data access](L2_bucket_data_access.md "wikilink")

## Other related howtos

[DFN observatory data folders and files
structure](DFN_observatory_data_folders_and_files_structure.md "wikilink")

[Downloading Images](Downloading_Fireball_Images.md "wikilink") from your
observatory (transferring files from the camera system to a laptop or
PC).

[Dealing with RAW images](Dealing_with_RAW_images.md "wikilink") (e.g. NEF)

## Automated collection of data files from removable drives /data1../data3

The approach we use in DFN to retrieve older files automatically from
the remote camera systems is following: the central processing server
uploads (to each camera) a lists of regular expressions matching
specific files, which are then once a day (as a part of the data move
job) automatically searched in the directory tree of /dataX and if
found, copied to "outgoing" folder on /data0, where the server task can
grab them. If we need something immediately, we do it manually.

## sync video data only back to server

rsync -marvu --no-perms --exclude='\*_stack.fits.gz'
--include='\*\*/\*_allskyvideo/\*'
--include='\*\*/\*_allskyvideo/\*\*' --include='\*/' --include='\*.csv'
--exclude='\*' root@dfnext034:/data1/DFNEXT034/ /data/allskyvideo/