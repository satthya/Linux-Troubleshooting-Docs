🐘 Title: FTP Server Troubleshooting – Server Side

🎯 Objective:

To identify and resolve FTP connection issues from the server side by verifying key configurations and services.

📋 Problem Statement:

The FTP user is unable to connect to the server. Perform the following server-level troubleshooting steps to resolve the issue:

🔸 Check if the vsftpd service is running:

 	🛠️ sudo systemctl status vsftpd

🔸 Restart the FTP service if needed:

 	🛠️ sudo systemctl restart vsftpd

🔸 Verify firewall settings and open FTP ports (20 & 21):

 	🛠️ sudo firewall-cmd --list-all

	🛠️ sudo firewall-cmd --add-port=20-21/tcp --permanent

 	🛠️ sudo firewall-cmd --reload

🔸 Check if port 21 is listening on the server:

 	🛠️ sudo netstat -tulnp | grep 21

🔸 Review the vsftpd configuration file for syntax or missing parameters:

 	🛠️ sudo vi /etc/vsftpd.conf

  		Ensure parameters like:

  		• chroot_local_user=YES

  		• allow_writeable_chroot=YES

  		• local_enable=YES, write_enable=YES

🔸 Verify that the FTP user is listed (if using userlist_enable=YES):

	 🛠️ cat /etc/vsftpd.userlist

🔸 Check the user's home and ftp folder permissions:

	 🛠️ /home/ftpuser → root:root with chmod 755

	 🛠️ /home/ftpuser/ftp → ftpuser:ftpuser with chmod 755/775

🔸 Check passive mode port settings (if using PASV):

	 🛠️ Confirm port range in config and allow them in firewall

	  (e.g., pasv_min_port=30000, pasv_max_port=31000)

🔸 Check logs for any FTP-related errors:

	 🛠️ sudo tail -f /var/log/vsftpd.log

	 🛠️ sudo journalctl -xeu vsftpd

🔸 Confirm that the client is using FTP, not SFTP (vsftpd doesn’t support SFTP)