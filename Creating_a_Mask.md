The default mask is in file /opt/dfn-software/basic_mask.png - the white
(pixel value \[255,255,255\]) circle area is the default field of view
of the lens, which is passed to fireball event detection. The black
corners area (pixel value \[0,0,0\]) is masked out.

It is up to an operator of each specific camera system to decide whether
the mask needs to be modified. Typically, the black area would be
extended to cover objects in the field of view that cause numerous false
positives. These could be antenna poles, trees, buildings etc.
Photoshop, Gimp or similar are recommended tools to create site-specifc
mask. The new mask needs to be the same pixel size and format (png). It
needs to be saved into /data0 directory and its filename has to be
'\*mask\*.png' (example: '/data0/Perenjori_mask_01.png'). The most
recent (file timestamp) mask in /data0 is used by the event detection
software. If there is no '\*mask\*.png' file in /data0, which is the
initial state of each camera system, the default mask file is used.

Please note that when the camera system is removed from the existing
site where it got a specific mask installed, and later deployed on
another site, the operator needs to delete the mask or replace it with a
new one suitable for the new site.