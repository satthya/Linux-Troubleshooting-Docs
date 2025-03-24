Title: FTP Connectivity Issue

Problem:

The user reported being unable to access the FTP server. The following error message was encountered:

![SSH Error](Image/ftp_error_v1.PNG)


Troubleshooting/Diagnose

	1. Check FTP Service Status: To verify whether the FTP service is running, use the following command:

		○ systemctl status vsftpd
	
	2. Enable and Start FTP Service: If the service is not running, enable and start the vsftpd service with:

		○ systemctl enable vsftpd
		○ systemctl start vsftpd
	
	
	3. Check Logs for Errors: Review system logs for any FTP-related errors:

		○ journalctl -u vsftpd
		○ cat /var/log/messages
	
	4. Stop Firewall Temporarily to Test Connection: Temporarily stop the firewall to check if it's causing the issue:

		○ systemctl stop firewalld
	
	5. Check Firewall Rules: Run the following command to list the current firewall settings:

		○ firewall-cmd --list-all
	
	6. Add Firewall Rule to Allow FTP Traffic: If FTP is not listed in the allowed services, add the rule to allow FTP traffic:

		○ firewall-cmd --add-service=ftp --permanent

Root Cause:

	• vsftpd Service Disabled: The FTP service (vsftpd) was not enabled or started, causing the FTP server to be unavailable.
	
	• Firewall Blocking FTP Traffic: The firewall was blocking FTP traffic, preventing external access to the server.
	
Solution:

	1. Enable and Start the vsftpd Service: Enable and start the FTP service to ensure it runs on boot:
	
		a. systemctl enable vsftpd
		b. systemctl start vsftpd

	2. Configure Firewall Rules to Allow FTP: Add the FTP service to the firewall rules:
	
		a. firewall-cmd --add-service=ftp --permanent

Outcome:

After following the above steps, FTP connectivity was successfully established, and the issue was resolved.
