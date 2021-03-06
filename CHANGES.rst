
TODO:
-----

These are all Open Tasks... HELP WANTED!

  * add python version testing to test suite (2.4, 2.5, 2.6)
  * add LUN test procedure to DEVELOPERS.txt
  * looking at firmware based LUN creation/deletion & format.
  * disk pack status.
  * alerts.
  * fix make_tarball to exclude .git and debian, like it is supposed to.
  * review getSubPageStatus, DMIP seems better now.
  * review unitstatus vs. diskpack status.
  * make a daemon. reasons:
            -- updated for relay out progress.
            -- alerting 
	    -- unified JSON interface for GUI & WUI.
            -- managed queueing for device (not five GUI's querying in parallel.)
            -- preserve option settings across reboot? (one report.)
            -- periodically synch. the drobo clock to the server.
  
0.6.2.2: 2010/02/06
-------------------

   * fix for Redhat/python 2.4 wanting to partition everything!

0.6.2.1: 2009/12/30
-------------------

   * fix for Drobo S slotinfo firmware bug (reports 8 instead of 5.)

0.6.2:  2009/12/26
------------------

  * GUI Option setting, thresholds, etc...
  * changed command line syntax to more consistently set <item>
  * documented option setting in CLI
  * more input checking to CLI.
  * improved LUN info display accuracy.
  * debugged DroboPRO support (debugged via google group. Thanks All!)
  * added scsi to info printing options in CLI.

0.6.1: October 7th, 2009
------------------------

  * DroboPro identification fix by spobole, 
  * parted fix from Patrick Bills
  * Looked at newly released DMIP. Found option2 commands for settings.
  * Option query/set implemented (but cannot test for DroboPRO)
  * migrated source code managers from subversion to git
  * minor: consistency of default location to look for fw between lib and gui.
  * fixed that inability to read lun size on old firmwares crashed GUI.
    GUI now works all the way back to the beginning.

0.6.0
-----

  * Major: GUI heavily re-vamped.  Behaves better. 
  * Major: DroboPRO support added (thanks to lemonizer on the google group!)
  * Major: GUI gets online documentation.
  * GUI: layouts replaced fixed dimensions. 
  * GUI: added Rename support 
  * GUI: added Load firmware button 
  * GUI: Show Diagnostics (equiv. to diagprint.)
  * GUI: Name display to main display. Easier to tell which Drobo is which.
  * GUI: added mount point display to space used tool tip.
  * api: added ability to detect un-documented feature bit settings.
  * ddiag.c becomes drobom diagprint
  * catlee fixes the packaging again. 

0.5.0 2009/01/30
----------------

  * replaced DroboDMP C extension by DroboIOctl python class.  no need
    for architecture dependent packages. this also means you can just 
    try it out straight after download, no need to install anything.
    much easier. fewer build dependencies as well.
  * device detection completely re-written.  It works better now.
  * Now displays mount points where it makes sense to do so.
  * status command output better/different. shows all devices and mount points
    for each drobo.
  * full Multiple LUN support.    should do what is expected.
  * full multiple Drobo support.  should do what is expected.
  * added modular info printing with csv list of outputs.
  * firmware 1.3.0, has enabled formerly broken functionality which
    name makes the sync command rename every drobo to 'hi there'.
    fixed.
  * added 'name' command, to name Drobo. (functionality of Sync)
  * corrected detection logic, which depended on formerly immutable name. 
    (affects fw >= 1.3.0) now use same mechanisms as sg_scan from sg3_utils.
  * documentation moved into restructured text. now shows up on the web.
    web site and documntation now consistent.

0.4.0 Release: 2008/12/28
-------------------------

  * IMPORTANT: firmware 1.3.0 support added.  Earlier drobo-utils 
     versions will not recognize a drobo running 1.3.0 or later.
  * CLI is more normal...  use getopts (got rid of device as fixed place arg.)
     now allows command to be an option as well.  
  * Changed DEBUG from a flag requiring code modification, to a bitmap
     honored as a command-line argument (--verbose)
  * misc. improvements in documentation.
  * droboview now only launches for first drobo found in list, and stays foreground
     always.
  * drobom list returns a list that is easier to parse (suitable for use in backticks.)
  * droboview is now just a stub for drobom view. reduced overhead.
  * now reports VERSION id in usage.  There is a -V option too.


0.3.3 release: 2008/11/05
-------------------------

  * lunsize display bug fixed.
  * Chris's man pages & help improvements.
  * more messaging fixups for when you don't invoke as root.
  * fix for ubuntu Intrepid deciding all disks are Drobos. !!
  * fixed all the firmware info parsing issues.
  * firmware load issues should all be gone.
  * Brad's fix for ddiag.c, done properly this time (my bad!)

0.3.2 release 2008/10/25
------------------------

  * for v1, still don't understand index.txt file.
  * after testing, some more fixes, v2 downloads now work.
  * fixes received from Brad Guillory for v2/tdz firmware downloading.
     they don't do any harm afaict, don't have a v2 to test with.
  * added fwload directive to drobom.
  * added root user check to give a bigger hint.

0.3.1 release 2008/10/01
------------------------

  * OK I know checking for firmware updates doesn't work
     right now, but I need to find out why.  code seems correct.
     there is a bugfix for getting rid of the 'licensed' part.

  * fixes for firmware validation:
       * header CRC on 32bit was broken on 64bit.
       * 32 vs. 64 bit kludge needed.

0.3.0 release 2008/09/04
------------------------

  * added ability to format Drobo
  * added ability to set lun sizes
  * bugfix for firmware:
        * CRC32 on 32bit intel. (use signed, instead of unsigned.)
        * fixed header CRC not validating.
  * added ability to set time on Drobo.
  * added simulation mode to Drobo.py, development aid.
  * some work done towards lunsize setting, but incomplete.
  * some work towards indicating relay out progress, not tested...

0.2.2 Released 2008/08/08
-------------------------

  * firmware uploads work with .tdz as well
  * tested with older firmwares, contains a few fixes to improve compatibility.
     works well enought upgrade old firmwares.

0.2.1 Released: 2008/08/06 
--------------------------

  * uploaded to drobospace & sourceforge.
  * firmware upload works for .tdf's 
     
0.1.1 Initial version uploaded  april 2008
------------------------------------------

  * to drobospace.com and sf.net
  * confirmed to work with firmware 1.1.1
  * shows status, disk slots filled, model, etc..
  * shows fw version loaded.
  * blink & standby commands work.
