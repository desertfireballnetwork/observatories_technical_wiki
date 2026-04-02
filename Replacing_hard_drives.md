## How-to replace HDDs

Replacing magnetic hard drives is easy, as long as the drives are not
powered on (eg 'python /opt/dfn-software/disable_ext-hd.py'), the camera
box can stay powered ON. Just be careful when touching any wiring inside
the box. Please follow [HDDs replace
checklist](Media:Checklist_HDDs_replace.pdf.html), which includes
instructions regarding formatting.

## Recommended HDD models (2024)

All of the drives we used so far have lower spin rate to reduce power &
heat (5600RPM or so) and typically are cheaper to buy. We use 10TB WDC
WD100EFAX now in most of DFN cameras in Australia (that is an older 10TB
model, very reliable). We had some issues with the 8TB model
(specifically WD80EFAX), which was drawing higher spin up current and
consequently some of the drives were "disappearing" during use causing
data integrity problems, but the 6TB (WD60EFRX) has been perfectly OK.
Often a combination of one WD80EFAX and two WD60EFRX works pretty well
too.

There is now (2023-2024) a new WD Red Pro model line
[1](https://documents.westerndigital.com/content/dam/doc-library/en_us/assets/public/western-digital/product/internal-drives/wd-red-pro-hdd/product-brief-western-digital-wd-red-pro-hdd.pdf).
However, those are spinning faster at 7200RPM.

These are obviously better because faster, although that is not really
needed for the use in DFN camera systems. More important is the
wattage - I would select a type that **uses less power** (in watts),
same as has **lower peak 12V current** (in amps).

WD80EFAX has 12VDC peak current 1.85Amp and max power 8.8W, which causes
issues with both DFNEXT and DFNSMALL camera system types.
WD100EFAX/WD60EFRX have 12VDC peak current 1.79Amp/1.75Amp and max power
5.7W/5.3W and both are okay.

Another key parameter is recording technology - definitely select drives
with **CMR**, *not SMR*.

As of 2024, we would recommend **WD Red Plus** line
[2](https://documents.westerndigital.com/content/dam/doc-library/en_us/assets/public/western-digital/product/internal-drives/wd-red-plus-hdd/product-brief-western-digital-wd-red-plus-hdd.pdf)
rather than the Pro line.

For example, the Swiss team now uses this model in Oman: 8TB WD Red Plus
(WD80EFZZ) with 5640 RPM, 1.75 Amps and 6.2 Watt, and they are happy
with the drives.

8TB/6TB models WD80EFPX/WD60EFPX use even less power, looking at the
datasheet. The larger 14/12TB models WD140EFGX WD120EFBX also seem to be
well power optimised, we suppose these should be OK for DFN cameras and
two of these per camera system shall provide heaps of disk space
compared to 3x6TB or similar as 3x8TB.

We also recommend to optimise cost per TB of capacity, it might be more
economical to buy just two larger drives per camera and get more
capacity than by using three smaller drives. Or one can extend the
capacity at little extra cost, reducing the maintenance efforts needed
to manage (eg delete) the data.