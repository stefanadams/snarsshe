-----------------------------------------------------------------------
      			SnaRSShE  v11.10.12
 	   2011- by Stefan Adams <stefan@cogentinnovators.com>
-----------------------------------------------------------------------

SnaRSShE (Snapshots via Rsync in a Simple Shell Environment) is a simple,
lightweight (both in size and system requirements) server data archiving
package designed for secure and reliable archiving of critical data of
defined network systems.



Requirements for SnaRSSHe:
	* Unix Shell (Bourne-SH, BASH, Busybox)
	* standard Unix text tools (fgrep, cut, head, mail, time, date,
	  paste, sed, readlink, find, df...)
	* rsync
	
Hardware requirements:
	* An existing directory for snapshots to be stored: /backup/snapshots
	  This is intentially hard-coded, the location MUST be this
	* It is advised that /backup/snapshots be a mount point and on a
	  completely separate set of disks than where the original data is

KNOWN ISSUES:
	* rsync sometimes craps out in such a way that the script does not
	  continue

FEATURE WISH LIST / ROADMAP:
	* none yet


Updates will be available at   http://www.cogent-it.com/software/snarsshe/
Please check there for updates prior to submitting patches!

There is currently NO user/developer mailing list available. Stay tuned.

For bug reports and suggestions or if you just want to talk to me
please contact me at stefan@cogentinnovators.com


-----------------------------------------------------------------------
Usage
-----------------------------------------------------------------------

Just run the script with pathes to repos*, it'll install in cron and keep the
trend.

	* /backup/snapshots/Group/fqdn.system.local

Environment Variables, set in .config starting at /backup/snapshots, with
deeper directories' values overwriting that of shallower:

	NOINSTALL	Don't install SnaRSShE into cron
	NOPARTCHECK	Don't check that /backup/snapshots is its own
			partition
	NOACLCHECK	Don't verify that /backup/snapshots supports acls
	MINFREE		Require that /backup/snapshots has MINFREE bytes
	KEEP		Keep at least this many snapshots
	DAYS		Auto-purge snapshots older than this
	LASTRUN		Don't run again too soon (minutes)

SSH Keys

	Only supports Rsync via an SSH tunnel.  Generate your key combo as
	root and install the public key in the data servers' super-user
	authorized_keys file, e.g.
	- ssh-keygen -t rsa
	- ssh-copy-id -i /root/.ssh/id_rsa.pub root@dataserver

Disabling archives

	Set the sticky bit on the archive root, e.g.
	- chmod +t /backup/snapshots/Group/system.local

Adding a repo

	Run the script with the full path to the non-existent repo, e.g.
	- snarsshe /backup/snapshots/NewGroup/new.system.local
	Running the script in this fashion will also run updates of all
	snapshots

Rotating

	Snapshots are deleted after DAYS days but at least KEEP archives are
	kept
	Failed attempts and empty archives are automatically purged

Snapshots

	Snapshots are stamped with YYYYmmddHH:MM:SS-X-Y, where
	- X is one of ok,partial,err, and
	- Y is the return code of the rsync process

Rsync code map

	ok	0
	partial	23,24
	err	6,20,25,30,120
	fail	* (snapshot not kept)


-----------------------------------------------------------------------
Shortcut: Distributable under  GPL
-----------------------------------------------------------------------
Copyright (C) 2011- Stefan Adams

This program is free software; you can redistribute it and/or modify
it under the terms of the GNU General Public License as published by
the Free Software Foundation; either version 2 of the License, or (at
your option) any later version.

This program is distributed in the hope that it will be useful, but
WITHOUT ANY WARRANTY; without even the implied warranty of
MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the GNU
General Public License for more details.

You should have received a copy of the GNU General Public License
along with this program; if not, write to the Free Software
Foundation, Inc., 59 Temple Place - Suite 330, Boston, MA 02111-1307,
USA. or on their website http://www.gnu.org/copyleft/gpl.html

-----------------------------------------------------------------------

