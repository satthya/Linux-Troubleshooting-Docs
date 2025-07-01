Title: User Cannot Create Files in Directory Despite Write Permissions

Problem:

A user is unable to create files in a specific directory, even though ls -l confirms they have write (w) permission on that directory.
This suggests the issue lies beyond basic Linux file permissions


Troubleshooting/Diagnose

Method1 : Check for Missing Execute (x) Permission on the Directory

1. Check directory Permissions: 

		ls -ld /path/to/directory

	• Even if w is present, the user also needs x (execute) permission to access or modify contents in the directory

2. Fix it missing:

		chmod +x /path/to/directory
	
	
Method2 : Check Filesystem Mount Options

1 . Check if mounted as read-only:

		mount | grep /path/to/directory
		
	• Look for ro in the output:	

		/dev/sdb1 on /mount/point type ext4 (ro,relatime,seclabel)

2 . Fix by remounting as read-write:

		mount -o remount,rw /mount/point


Method3 : Check Disk Quota or Disk Space

1 . Check available disk Space:

		df -h 
		
2 . Check if the user exceeded their quota:

		quota -u satthya
		
3 . Fix: 

	• Free up Space
	• Increase user quota 
		
		
Method4 : Check ACLs (Access Control Lists)

1 . View ACLs:	

		getfacl /path/to/directory
		
	• The user may be explicitly denied access, even if regular permissions look fine

2 . Fix or remove ACLs:

		setfacl -x u:username /path/to/directory

		
Conclusion

A user may be unable to create files in a directory even with write permission due to:

	• Missing execute (x) permission on the directory

	• Filesystem mounted as read-only

	• Disk full or user quota exceeded

	• ACLs overriding normal permissions

Always check permission triad (rwx), mount options, disk space, and ACLs when diagnosing such issues.