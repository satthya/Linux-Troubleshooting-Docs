Title:  User Cannot Log in via SSH Despite Correct Credentials

Problem:

A user is unable to log in via SSH, even though their username and password (or key) are correct. 
This suggests the issue may lie elsewhere—such as SSH configuration, account status, network access, or security policies.


Troubleshooting/Diagnose

Method1 : Check SSH Server Status

1 . Ensure SSH service is running:

		systemctl status sshd
		
2 . Start or restart if inactive:

		systemctl start/restart sshd 


Method2 : Check SSH Port and Connectivity

1 . Test if port (default 22) is reachable:

		netstat -tuldn | grep 22
		
	look for
	
		LISTEN  0  128  0.0.0.0:22

2 . Check firewall:

		firewall-cmd --list-all
		
	if SSH not allowed:	
		
		firewall-cmd --add-service=ssh --permanent
		firewall-cmd --reload
		
		
Method3 : Check Authentication Logs		
		
1 . View SSH login attempts:		
		
		tail -f /var/log/secure
		
	look for:

	• Permission denied
	
	• Invalid User
	
	• Key authentication failure	
		
		
Method4 : Check User Account Status	

1 . Verify if the user is locked or expired:

		passwd -S username 
	
	• If status shows L (locked) or E (expired), user can't log in.

2 . Unlock or reset password:

		passwd username
		chage -l username

3 . Check user's login shell:

		grep username /etc/passwd

	• Ensure shell is valid (/bin/bash, not /sbin/nologin or /bin/false)
	

Method5 : Check SSH Configuration

1 . Open SSH config file:

		vi /etc/ssh/sshd_config
		
2 . Look for these directives:

		PermitRootLogin yes
		PasswordAuthentication yes
		AllowUsers username

3 . Apply changes:

		systemctl reload sshd
	
		
Conclusion

Even with valid credentials, SSH login may fail due to:

	• SSH service not running or misconfigured

	• Port 22 blocked by firewall or not listening

	• User account locked, expired, or incorrect shell

	• Password authentication disabled in config

Always start by checking SSH logs, user status, and SSH config.

