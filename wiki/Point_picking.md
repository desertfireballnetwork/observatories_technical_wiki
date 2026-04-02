**Installation**

Get Linux.

Get access to the code repo, and go to:

`dfn_repo/server/data_processing/src/de_bruijn_sequence_tools/point_picker`

Package the point picking software (unless someone has done it for you):

`./pp_packager.sh`

This creates q folder like pp_package_DD-MM-YYYY, which contains
everything you need.

Install <https://www.anaconda.com/download/> (preferably 3.X for the
rest of the pipeline).

Create a Python 2.7 environment:

`conda create -n py27 python=2.7 anaconda`

Before point picking, switch to that environment:

`source activate py27`

Install python packages, preferably using conda:

`conda install future astropy pyyaml seaborn`

The usage function is helpful, read it:

`$ python point_picker.py -h`

In particular:

`    -i IMAGE_PATH, --image_path IMAGE_PATH`
`                          Path to fireball image (REQUIRED)`
`    -e PREVIOUS_POINTS_PATH, --previous_points_path PREVIOUS_POINTS_PATH`
`                          Previously picked ecsv (OPTIONAL, if you want to repick a fireball)`
`    -n STARTSEQINDEX, --startSeqIndex STARTSEQINDEX (OPTIONAL)`
`                          - auto attempts to find existing point picking for that event, give you  about those, and lets you interactively choose.`
`                          - an integer will jump straight to the corresponding De Bruijn sequence number `

**Use - controls**

p ... switches between pan and point picking mode

*Note: the DeBruijn sequence position will scroll (mouse wheel) only
when over the image and in point picking mode*

g ... show plot (graph) with timing to verify the picking

w ... write picked points in ecsv format

mouse left button ... drop point

mouse right button ... delete last dropped point

**DeBuijn sequence**

`00000000011111111101111111001111110101111110001111101101111101001111100101111100001111011101111011001111010101111010001111001101111001001111000101111000001110111001110110101110110001110101101110101001110100101110100001110011001110010101110010001110001101110001001110000101110000001101101101001101100101101100001101011001101010101101010001101001001101000101101000001100110001100101001100100101100100001100010101100010001100001001100000101100000001010101001010100001010010001010001001010000001001001000001000100001`

**Troubleshooting**

if you get this error:

`$ python point_picker.py -i /home/NAS_clone_m/events_trello/DN161031_01/16_Perenjori/16_2016-10-31_120328_S_DSC_0326-G.tiff`
`This application failed to start because it could not find or load the Qt platform plugin "xcb"`
`in "".`
`Available platform plugins are: minimal, offscreen, xcb.`
`Reinstalling the application may fix this problem.`
`Aborted (core dumped)`

Set the following environment variable each time before running the
point picker:

`LD_LIBRARY_PATH=$HOME/anaconda3/lib; python point_picker.py -i /home/NAS_clone_m/events_trello/DN161031_01/16_Perenjori/16_2016-10-31_120328_S_DSC_0326-G.tiff`

(assuming you have anaconda installed in directory
"\$HOME/anaconda3/lib")

Bottom line: Anaconda is compiled against specific QT version which gets
installed with it, but there is another version installed in the OS,
which is used by default. By LD_LIBRARY_PATH we explicitly and locally
point anaconda to libs it requires.

Works with Kubuntu LTS 16.04.x. Reference
[1](https://github.com/ContinuumIO/anaconda-issues/issues/1182)