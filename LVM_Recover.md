Title: Recovering Deleted Logical Volume Metadata Using vgcfgrestore

Problem:

The user accidentally removed the Logical Volume /dev/new_vg/new_lv, which was created on 
/dev/sdb1 and mounted at /mnt/mylvm.

Steps

	1. Check the Volume Group and Logical Volume status

		a. lsblk
		b. vgs
		c. lvs
	
	2. Restore the Volume Group metadata from the archive

		a. locate backup:
		
			i. ls /etc/lvm/archive
			
		b. List the available archived metadata backup for the volume group
		
			i. vgcfgrestore -l new_vg
		
		c. Restore using:
		
			i. vgcfgrestore -f /etc/lvm/archive/new_vg_00000-xxxxxx.vg new_vg

	3. Reactivate the logical volume
	
		a. lvchange -an /dev/new_vg/new_lv
		b. lvchange -ay /dev/new_vg/new_lv
		
	4. Try mounting again
	
		a. mount dev/new_vg/new_lv /mnt/mylvm
	
	5. Confirm the mount

		a. df -h

Root Cause:

	Accidental lvremove command deleted the Logical Volume metadata, making the volume inaccessible.
	
Solution:

	• Restored the missing Logical Volume metadata using vgcfgrestore
	• Re-activated the Logical Volume using lvchange
	• Successfully mounted the volume after recovery
	
	
Outcome:

The Logical Volume was recovered, and the data remained intact. The filesystem was mounted back to /mnt/mylvm successfully.
