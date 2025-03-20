Title: Fixing "Web Server" file not accessible

Problem:

A web server running on a Red Hat Linux system has stopped serving the expected web page. Instead, users are experiencing  the following issues:




Troubleshooting/Diagnose

	1. Checked httpd service status 
	
		a. systemctl status httpd

	2. Start the httpd service (if not running)

		a. systemctl start httpd
	
	3. Reviewed journal logs for service errors

		a. journalctl -u httpd




	4. Examined httpd log file for error
	
		a. cd /var/log/httpd
		b. cat error_log
		


	5. Verified existence of index.html in the web root directory
	
		a. ls -l /var/www/html/
	
	6. Checked index.html file permission
	
		a. ls -l /var/www/html/
	
	7. Verified SELinux status
	
		a. sestatus
	
	8. Examined SELinux audit logs for denied access


		
		a. /var/log/audit
		b. cat audit.log | grep avc
	
	9. Disable SELinux temporary to test the application
	
		a. setenforce 0
	
	10. Checked SELinux file context for web content

	Verified that the files in /var/www/html had the correct SELinux context
		
		a. ls -lZ /var/www/html

		○  httpd_sys_content_t (for static files)
		○  httpd_sys_rw_content_t (if write access is required)


Root Cause:

The issue was traced to one or more of the following factors:

	1. The HTTPD service was inactive due to a missing configuration file: /etc/httpd/conf/httpd.conf

	2. The index.html file was missing from /var/www/html/

	3. Incorrect SELinux context settings prevented the web server from accessing content.

Solution:

	1. Restored Missing HTTPD Configuration File
	
		a. mv /etc/httpd/conf/httpd.conf.back /etc/httpd/conf/httpd.conf
	
	2. Replaced the Missing index.html File
	
		a. Downloaded from GitHub and moved to /var/www/html/
	
	3. Corrected SELinux Context
	
		a. restorecon -Rv /var/www/html
	
		a. Restarted the web server: systemctl restart httpd
	
	


Outcome:

After implementing the resolution steps:

	• The HTTPD service was restored and is now running as expected.

	• The web server successfully serves the intended web page.

SELinux policies are correctly enforced, ensuring secure access to web content.