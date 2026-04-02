The long-exposure camera in DFN observatories (usually a Nikon D800E or
D810) record image in Nikon RAW format (NEF).

This guarantees as much information as possible is kept about the
fireball, producing the best result for the data reduction steps.
However this file format is not practical taking a quick look at the
image.

There are several ways to extract a RAW file.

Note that here we use NEF and RAW inter-changeably. In principle
everything described here for Nikon NEF format is valid for RAW coming
from other camera manufacturers (Sony ARW, Canon CF2...).

## Converting NEF to JPG

The easiest is to use dcraw [1](https://www.dechifro.org/dcraw/).

On Unix-like systems it is usually available via your package manager
(apt install dcraw). Or it is easily compiled from source with gcc (the
source is a single C file).

There is also a Windows packaged version of dcraw (although we have not
tested it).

Once installed dcraw cannot invoked from the command line:

dcraw -e /path/to/image.NEF

This will create a file called file image.thumb.jpg that is almost the
same resolution as the RAW, but lossy compressed (from ~45 MB to ~5 MB).
Note that this particular command does not truly extract the RAW, it
just extracts the JPG thumbnail that is embedded in the NEF file. Dcraw
has options to re-process the RAW images, but we do not go into details
here, please refer to dcraw's documentation.

Most image editing software (Photoshop, GIMP...) will also let you open
and export RAW images.

The other option for Microsof Windows and/or Apple MAC computers is to
use free Nikon software to work with NEFs - ViewNX
([2](https://downloadcenter.nikonimglib.com/en/products/166/ViewNX_2.html))
or just codec to add NEFs support to windows:
([3](https://downloadcenter.nikonimglib.com/en/download/sw/190.html)).
With the codec in Windows installed, the NEF should open and then can be
saved as JPG.

## Science grade

We have developed a converter for these images, that enables conversion
to FITS and TIFF. Head to the public github repository for more info:
[4](https://github.com/desertfireballnetwork/RAW2FITS)