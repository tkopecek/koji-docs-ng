Release process
===============

Merging PRs
-----------

We're not using pagure's merge button as it doesn't do everything we want.
Instead `pg script <https://github.com/mikem23/pagure-tool>`__ is used.

::

    $ pg pr checkout <PR number>

will checkout given PR and it can be reviewed locally. It can be rebased in this
time or it can be done automatically in the moment of merging:

::

    $ pg pr merge -r

This will merge the changes, fetch info from pagure, and show you the current
git changelog. The changelog should be looked over for any issues, and it will
require special formatting so pagure will close out the release notes pull
request automatically.

For example, you'll want the 'Merges' and 'Fixes' lines.

The unit tests should be run before pushing if there were any new code changes
since the last tests. After that, it will be ready to push. Test with ``git push
-n`` first before the push to make sure it's correct.


Release Notes
-------------

There should be separate PR with release notes (and version bumps) for every
release.

Pushing the Code
----------------

Once Koji is ready to release, double check that the release notes are the only
open pull request. If that's the case, the release notes are ready to merge.

Once the code is pushed, make sure all the PRs in the release roadmap are
closed, including the release notes PR.

Now the release should be tagged. All Koji releases have been signed, using this
command:

::

    $ git tag -a -s -m 'Koji 1.18.0' koji-1.18.0

Then the tag must be pushed:

::

    $ git push origin refs/tags/koji-1.18.0

To create the tarball, run ``make tarball`` from the base koji directory. You
should then copy the tarball to some other directory, just to have a backup. The
tarball should be uploaded through the pagure interface, though the upload can
also be done via ssh.

Once it's uploaded, download it and check the shasum against your local tarball.
If there are no issues, Koji's code release is complete.

Updating the Site
-----------------

The next step is to update the Koji site with the new docs and release notes.
This content is based in a separate repo from the Koji one.

Simple bash script for this repo could be used, ``kdoc``. Using ``kdoc``, run
these commands in the docs repo:

::

    $ kdoc release-1.18.0:test
    $ kdoc

``kdoc`` checks out the master branch of the koji git repo, constructs docs from
that, and copies those changes into the doc repo. Once the changes are in place,
commit them and push them to your fork:

::

    $ git commit -m 'koji 1.18.0 release'
    $ git diff --stat test
    $ git push mikem

Check your fork to make sure everything looks good (for example, check against a
previous release), then push the changes with 'git push.'

.. code-block:: bash

    #!/bin/bash

    # kdoc shell script

    set -x
    set -e

    # TODO more sanity checks

    KDIR=~/cvs/koji
    DOCDIR=~/cvs/koji-docs
    DBRANCH=master
    SBRANCH=master

    branches=$1
    if test -n "$branches"; then
        dst=${branches#*:}
        src=${branches%:*}
        test -n "$src" && SBRANCH=$src
        test -n "$dst" && DBRANCH=$dst
    fi

    cd "$KDIR/docs"
    test -n "$SBRANCH" && git checkout $SBRANCH
    make dirhtml

    cd "$DOCDIR"
    test -n "$DBRANCH" && git checkout $DBRANCH
    rsync -avPH --delete --exclude .git "$KDIR/docs/build/dirhtml/" "$DOCDIR/"
    git status

    echo "Docdir is: $DOCDIR"

Pushing to PyPi
---------------

All releases should also go to PyPi repository, even if we've not publicly
announced, that it is the supported delivery channel.

::

    $ dnf install twine
    $ make pypi
    $ make pypi-upload

Release Email
-------------

The last step is to send out an email to koji-devel@lists.fedorahosted.org. See
previous release emails for how to format the message.

Generally the email should include a link to the release notes, list some
highlights from the release, links to the release roadmap and current roadmap,
and a link to the download.

Required Permissions
--------------------

* Merge permissions for the koji repo
* Merge permissions / access to the docs repo
* Key for signing
