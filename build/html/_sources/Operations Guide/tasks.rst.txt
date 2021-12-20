Tasks Description
=================

Here is a list of all tasks which can be spawned by koji. Most of them are
available via CLI, some are just child tasks of others.

appliance (children: createAppliance)
-------------------------------------

buildMaven
----------

buildNotification
-----------------

Koji sends build notifications to the package owner and the user who submitted
the build. For BuildNotifications to work successfully, the package owner's
username needs to match a valid username for an e-mail address, because kojihub
sends to ``username@domain`` in ``kojihub.conf``.

For SSL authentication, this means that your CN must be valid as the user
portion of an e-mail address.


chainbuild (children: build, waitrepo)
--------------------------------------

chainmaven (children: maven)
----------------------------

image (children: createImage)
-----------------------------

newRepo (children: createrepo)
------------------------------
This task is intended for regenerating buildroots. It can be triggered manually
via ``koji regen-repo <buildtag_name>`` or automatically via ``kojira``.

build (children: rebuildSRPM, buildSRPMFromSCM, buildArch, tagBuild)
--------------------------------------------------------------------

distRepo
--------

image
-----

indirectionimage
----------------

livecd
------

livemedia
---------

runroot
-------

saveFailedTree
--------------

tagBuild
--------

tagNotification
---------------

vmExec
------

waitrepo
--------

winbuild
--------

wrapperRPM
----------

createKiwiImage
---------------

kiwiBuild
---------
