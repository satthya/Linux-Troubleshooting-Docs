Title:  Allow User to Run Only a Specific Command Using Sudo

Problem:

You want to restrict a user so they can only execute one specific command using sudo, without giving them full root access.
This is common for delegating limited administrative tasks while maintaining security


Troubleshooting/Diagnose

1 . Open the sudoers file safely using visudo:

		visudo
		
2 . Add the following line at the end of the file:

		username ALL=(ALL) NOPASSWD: /full/path/to/command

	• username with the actual user's name.
	• /full/path/to/command with the exact command the user is allowed to run.
	
	Example: deployuser ALL=(ALL) NOPASSWD: /usr/bin/systemctl restart nginx

3 . Test the Configuration:

		sudo /usr/bin/systemctl restart nginx
	
		
Conclusion

Restricting a user to run only one command with sudo improves security by:

	• Preventing access to full root privileges.

	• Allowing delegation of specific tasks (e.g., restarting a service).

	• Avoiding misuse of powerful commands like bash, vi, or sh.

