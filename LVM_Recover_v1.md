Title: Recovering Deleted Physical Disk and Logical Volume

Problem:

The Logical Volume /dev/newvg/newlv, which was created on /dev/sdb1 and mounted at /mnt/mylvm, is no longer available after a system reboot.

The associated Volume Group and Physical Volume are also missing, and commands like pvs, vgs, and lvs show no related LVM information

Steps

	1. Identify the physical disk unique ID
	
		a. cd /etc/lvm/archive
		
			i. cat newvg_00002-1011115100.vg
			
				□ Find the id and device fields under the physical volume section.

	


	2. Recreate the physical ID with the same unique ID from backup 

		a. pvcreate --uuid "ux2CHm-noOr-9oCX-n3uQ-cXk9-TrB9-22w00l" --restorefile /etc/lvm/backup/newvg /dev/sdb1
	
	


	3. Confirm available restore version from /etc/lvm/archive

		a. vgcfgrestore -l newvg

	

	4. Restore the volume group metadata to the preferred version
	
		a. vgcfgrestore -f /etc/lvm/backup/newvg newvg
	
	5. Deactivate and Activate the newvg

		a. vgchange -an newvg
		b. vgchange -ay newvg

	6. Mount the volume to mylvm directory

		a. mount /dev/newvg/newlv /mnt/mylvm
	
	

Root Cause:

After a reboot, the system failed to detect the LVM metadata due to the accidental loss or corruption of the Physical Volume's header on /dev/sdb1. This caused the Volume Group and Logical Volume to become inaccessible.
	
Solution:

Recovered the LVM setup using metadata stored in /etc/lvm/backup/ and /etc/lvm/archive/, by:

	• Recreating the Physical Volume with the original UUID
	• Restoring Volume Group metadata from backups
	• Re-activating the Volume Group and mounting the Logical Volume

	
Outcome:

The Logical Volume /dev/newvg/newlv was successfully recovered and mounted at /mnt/mylvm. Data is now accessible as before the incident.
