Title: Fixing "Web Server" file not accessible

Problem:

A user has deployed an application on a Red Hat Linux server with a web server. However, instead of loading the expected application, the browser displays the default Red Hat Test Page.

![SSH Error](Image/webserver_error_v1.png)


Troubleshooting/Diagnose

	1. Checked httpd Log files
		· Accessed the log files to identify any issues.
	
		a. /var/log/httpd
	
	2. Reviewed Error Log
		· Checked for any specific errors related to the issue.

		a. tail -f /var/log/httpd/error_log

![SSH Error](Image/webserver_error_v2.png)


	3. Checked index.html permission level
		· Verified the permissions on the index.html file.

		a. cd /var/www/html
		b. ls -l index.html
	
	4. Checked SELinux policy
		· Reviewed the SELinux audit logs to check for any denied access related to SELinux.

		a. cat /var/log/audit/audit.log | grep avc
	
	5. Checked SELinux status
		· Verified ether SELinux was enabled and enforcing any restrictions.
		
		a. sestatus
		
	6. Temporary Disable SELinux 
		· To confirm whether SELinux was the root cause, temporarily disable it.
		
		a. setenforce 0
	
	7. Checked SELinux file contexts
		· Verified that the files in /var/www/html had the correct SELinux context
		
		a. ls -lZ /var/www/html

		○ httpd_sys_content_t (for static files)
		○ httpd_sys_rw_content_t (if write access is required)



Root Cause:

The issue is caused by incorrect SELinux context settings, preventing the web server from serving the files from /var/www/html

Solution:

Update SELinux Context
Changed SELinux context from admin_home_t to httpd_sys_content_t:

	• restorecon -RV /var/www/html
	• systemctl restart httpd

Outcome:

After implementing the resolution steps:

	• Correct SELinux contexts were restored for web server files.
	• The application loaded successfully, replacing the default Red Hat Test Page.
	• SELinux remained enabled, ensuring security policies were enforced without blocking web content.
