Tasks
=====

appliance (children: createAppliance)
-------------------------------------

buildMaven
----------

buildNotification
-----------------

chainbuild (children: build, waitrepo)
----------------------------

chainmaven (children: maven)
----------------------------

image (children: createImage)
-----------------------------

newRepo (children: createrepo)
------------------------------

build (children: rebuildSRPM, buildSRPMFromSCM, buildArch, tagBuild)
--------------------------------------------------------------------


distRepo
image
indirectionimage
livecd
livemedia
runroot
saveFailedTree
tagBuild
tagNotification
vmExec
waitrepo
winbuild
wrapperRPM
createKiwiImage
kiwiBuild
