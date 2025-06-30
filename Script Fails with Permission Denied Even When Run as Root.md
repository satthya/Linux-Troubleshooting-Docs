Title: Script Fails with "Permission Denied" Even When Run as Root

Problem:

A script fails to execute with a "permission denied" error, even though it is being run as the root user. 
Since root typically bypasses file permissions, this suggests a different underlying issue.


Troubleshooting/Diagnose

Method1

1. Check file execute permission:
	Ensure the script has the executable (x) permission

		ls -l /home/satthya/test
		
	if missing, add it:

		chmod +x /home/satthya/test/script.sh
		
2. Check file system mount options
   
		• mount | grep /home/satthya/test
	
	sample output indicating the issue:
	
		/dev/sdb1 on /home/satthya/test type ext4 (rw,noexec,relatime,seclabel)
	
	Here, noexec prevents execution of any script or binary in that directory — even for root.
	
3. Fix mount option in /etc/fstab
   
		• vi /etc/fstab
		
		change
		
		/dev/sdb1  /home/satthya/test  ext4  defaults,noexec  0 0
		
		to
		
		/dev/sdb1  /home/satthya/test  ext4  defaults  0 0

	

Method2 : Check Shebang and Interpreter

1 . Check shebang line
	The script may point to a non-existent or non-executable interpreter:

		• cat script.sh
		
	example of correct shebang : #!/bin/bash
	
2. Ensure the interpreter exists

		•  which bash
		
3.	Install bash if it's not Install

		• yum install bash
		
Conclusion

Root can still face permission denied errors if:

		• The file lacks execute permission

		• The script resides on a partition mounted with noexec

		• The script’s interpreter is missing or incorrect

Always check mount options and the shebang line first in such cases.