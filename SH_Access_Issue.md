Problem:

A developer was unable to SSH into a remote Linux server and received the following error:

![SSH Error](Image/ssh_error.png)

Troubleshooting/Diagnose

	1. Logged in as root to investigate the issue.
	
	2. Checked the user's last successful login attempt:
		a. lastlog -u <username>
		
	3. Verified the user's login information:
		a. getent passwd <username>  or cat /etc/passwd | grep <username>
		

Root Cause:

The user's shell was set to /bin/false, which prevented SSH access.

Example entry from /etc/passwd:

	• linda:x:1001:1001::/home/linda:/bin/false

Solution:

Edited the /etc/passwd file to assign a valid shell:

	• vi /etc/passwd
	• linda:x:1001:1001::/home/linda:/bin/bash

Outcome:

After modifying the user's shell, the developer was able to successfully SSH into the server.

