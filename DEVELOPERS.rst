
---------------
DEVELOPER NOTES
---------------


This software is copyright under GPL.  See near end file for details...

This is a pile of random useful bits for developers.  (aka notes to myself ;-)


Working With Source
-------------------

Project Source code is managed using a git repository provided by 
sourceforge.net.  Git ( http://git-scm.com/ ) provides a fully 
distributed development model, so one can exchange patches arbitrarily 
among developers.  On the sf.net repository, the 'master' branch is 
the final integration target for future releases. 


Getting a Source Tree 
---------------------

the following checks out the master branch of the source code tree and
puts it in the drobo-utils subdirectory of the current tree.

git clone ssh://username@drobo-utils.git.sourceforge.net/gitroot/drobo-utils/drobo-utils drobo-utils

Before doing Any commits, ensure that the author fields are 
appropriately set.  One can obtain patches applied to the branch 
with git pull, and commit patches for distribution with git push.


Build Dependencies
------------------

To build the package from source, there are a number of other packages needed::
 
 boule% sudo aptitude install debhelper python2.5-dev
 boule% sudo aptitude install python-docutils
 boule%

The second line only required to build documentation.  On the other hand,
to get a complete list of packages you need, it is best to use a shell window 
to grep in the Debian package control file (which defines what the 
dependencies are for the build system)::

     peter@pepino% grep Depend debian/control
     Build-Depends: debhelper (>= 5), python2.5-dev, python-docutils
     Depends: ${shlibs:Depends}, ${misc:Depends}, parted
     peter@pepino%



GIT Configure Patch Author
--------------------------

To ensure the correct author on patches created, make sure to set your 
author settings.  Something like the following is appropriate if you 
use a single identity for all your code contributions::

  boule% git config --global user.name "firstname lastname"
  boule% git config --global user.email "developer@sourceforge.net"
  boule% 

Others may wish for the identity to be associated with each project.

Using a Source Snapshot
-----------------------

Sometimes, when there are issues, the correction gets checked in, but there 
hasn't been time to do a full release process.  If you really need the fix, 
then your only option is to get it from the developers' git repository.  
If you are told 'it is in git', that refers to the git source code management 
system, and the server for that is at sourceforge.net.  How to get it::

 # git clone ssh://developer@drobo-utils.git.sourceforge.net/gitroot/drobo-utils/drobo-utils mine
 # cd mine                 # go into the source directory you downloaded
 # ./drobom status         # try it out...
 # python setup.py install # install it in system places...
 # drobom status           # try it out...
 # git pull # get any changes made since the git clone was done.
 # python setup.py install # install the changes in system places.

Sample checkout of a stable version.  To view available branches::

 % git branch -r
 origin/HEAD -> origin/master
 origin/master
 origin/noC
 origin/peter
 origin/r0.2.1
 origin/r0_3_3
 origin/r0_4_0
 origin/r0_5_0
 origin/r0_6_0

Where a version is something like r0_4_0.  then you can pick anyone to work with::

 % git branch r0_4_0 -r origin/r0_4_0
 Branch r0_4_0 set up to track remote branch r0_4_0 from origin.
 % git checkout r0_4_0
 Switched to branch 'r0_4_0'
 %

When you use git to get a tree, it keeps copies of metadata to be able to 
track changes.  If you want a copy that is contains no git cruft, 
rm -rf .git in the root of the source tree.

If you are mixing downloaded packages and source installs, check out the next 
section for gotchas.

Dpkg vs. Python Install
-----------------------

The 'setup.py' script, mentioned in the previous section, is a convention  
from the distutils python packaging system.  distutils installation is slightly 
different from installation from debian packages.  There doesn't seem to be a 
distutils way to remove a package. touch all the files, do an installation, then 
manually remove the files it installed.

drobo-utils has been picked up for inclusion in debian.  The "real" packaging 
for debian packages is kept in a separate tree, and maintained by debian 
developers.  

The debian/ setup puts stuff in /usr/sbin while setup.py puts things in /usr/bin.  Python install does not install man pages either, which the dpkg takes care of.  The libs are placed differently too.  haven't reviewed for other conflicts, least confusing to use one or the other method on a system.  

(if you do distutils install, then remove the debian package via: dpkg --purge drobo-utils)


Making a Release
----------------

Procedure::

  1 - make a branch
   # assuming you have a local repository...
   git branch <branch>  # creates the branch, from the cwd (ought to be master)
   git checkout <branch> # switches current dir to the branch.

  2 - Stamp the branch with version 
   vi CHANGES.txt          # complete change manifest for release
   vi debian/changelog     # copy manifest from txt, add signature.
   vi setup.py 		   # edit version
   vi Drobo.py             # edit VERSION

  3 - Build packages for testing & Install them. (see separate recipe.)

  4 - Run QA.
   Record results of release tests in the branch (QA.txt) As new tests are created, 
   modify QA.txt on trunk for to keep references for the next release.

  5 - Commit & Push QA'd branch

    git commit -a
    git push origin origin:refs/heads/<branch>


Quality Assurance (QA.txt)
==========================

QA.txt is a quality assurance log.  The version on the trunk of the releases 
indicates the QA procedure to be applied to the next version during the 
release process.  Since a branch is created for each release, the version 
of QA.txt acts as a quality log for that release.  so one can do a git 
checkout, or git export to get the quality log for any release (QA.txt 
introduce in version 0.4.0)


Building Debian & Ubuntu Packages
=================================

Assumes you have installed the Build dependencies::

 # obtain a fresh tree 
 % git clone ssh://user@drobo-utils.git.sourceforge.net/gitroot/drobo-utils/<version> drobo-utils-<version>
 % cd drobo-utils-<version>
 % rm -rf .git  # get rid of Git cruft, yielding a raw source tree.
 % chmod 755 debian/rules  # I dunno why the permissions are wrong...

 # this debian/ config is just for non-distro packages.
 # builds for debian and Ubuntu.

 % dpkg-buildpackage -rfakeroot
 % cd ..
 # rename it for whatever distro is appropriate...
 % mv drobo_utils_0.3.3-1_i386 --> droboutils_0.3.3-1_i386_ubunutuIntrepid.deb

 # rebuild the source tar because it will have the 'debian' link in it.
 % cd drobo-utils-0.99.9
 % rm debian
 % cd ..
 % tar -czvf drobo-utils-0.3.3-1.tgz drobo-utils-0.99.9

apply QA tests. as per QA.txt recording results there.


Updating Documentation
----------------------

use the restructured text tools (from the python-docutils package.)
to build things using:

 % make doc

Have a look at Makefile for how that works.
update the web site:

 % scp README.html <user>,drobo-utils@web.sourceforge.net:htdocs

Droboshare
----------

Droboshare is not directly supported by drobo utils running on a linux host.  
However, the droboshare itself is a linux host, and it is possible to run
drobo-utils un-modified on the droboshare itself.  There is download called 
the Droboshare Augmented Root File system (DARFS), which includes a python 
interpreter and drobo-utils.

Open Task: Reverse Engineer Dashboard <-> Droboshare Protocol
=============================================================

Why isn't there full support in host based drobo-utils itself?  Digital 
Robotics hasn't released details of the protocol used by the proprietary 
dashboard to communicate with a droboshare, so it would be a lot of work to 
reverse engineer that.  So support of a droboshare from a linux GUI on a 
host system is not likely in the near future.  

If someone wants to figure that out, it might be a good thing (tm)
After that is figured out, the next step would be to understand
how to flash the firmware remotely.  That would eliminate the last function
that cannot be done with open source.

Building DARFS
==============

DARFS - Droboshare Augmented Root File System. A pile of stuff that can
be run on a droboshare.

Have a look here:

http://groups.google.com/group/drobo-talk/web/building-droboshare-apps-on-debianish-os?hl=en

TRIM/DISCARD
------------

Drobo is the only consumer-level storage unit that does `Thin Provisioning`_ (allocating a device larger than the physical space available, allowing space upgrades without
OS changes.)  Drobo does this by understanding the file system blocks, which is
why it only supports a very limited set of file systems and cannot support full 
disk encryption.

There is considerable industry activity about adding `ATA TRIM`_ and corresponding 
`SCSI UNMAP`_ commands.  These commands, for their respective command sets, add 
the ability for the operating systems' file system code to indicate blocks that
are not in use to storage units.  Drobo would work with any file system that 
uses these commands, with far less firmware.  On linux, that file systems that 
are starting to support TRIM/DISCARD are:  ext4, btrfs, and xfs.  It may also 
help with the inherent limitations around full disk encryption. 

These commands are still maturing in support.  Long term, they seem like 
The right thing to do.

(2009/12/30)


.. _`ATA TRIM`: http://en.wikipedia.org/wiki/TRIM
.. _`Thin Provisioning`: http://en.wikipedia.org/wiki/Thin_provisioning
.. _`SCSI UNMAP`: http://www.t10.org/ftp/t10/document.08/08-149r4.pdf
.. _`Andy Grover on TRIM`: http://blogs.oracle.com/linuxnstuff/2009/04/drobo_and_linux.html

Administrivia
-------------

Revision date: 2009/12/27

copyright:

Drobo Utils Copyright (C) 2008,2009  Peter Silva (Peter.A.Silva@gmail.com)
Drobo Utils comes with ABSOLUTELY NO WARRANTY; For details type see the file
named COPYING in the root of the source directory tree.

 version 9999, somewhen

==============
Stuart's Notes
==============

I am Stuart Blake Tener, a Computer Scientist and Amateur Radio Operator (N3GWG) that has
begun to work on the Drobo-Utils package in order to modernize it and bring back into the
best functional state that I can.

From this point forward the versioning shall start with Version 1.0.0.0 as of 07 DEC 2020
and going forward from there in my repo with my fork of the product.

