Title:  Install PostgreSQL on a Red Hat-Based Server.

Problem:

You need to install PostgreSQL on a Red Hat-based Linux server using the root user and ensure the service is enabled and running after installation.


Steps :-


⚙️ Installation
----------------

1) Install the Rpository RPM :

	• yum install -y https://download.postgresql.org/pub/repos/yum/reporpms/EL-9-x86_64/pgdg-redhat-repo-latest.noarch.rpm

2) Disable the built-in PostgreSQL module : 
		
	• yum -qy module disable Postgresql	
	
3) Install Postgresql16-server :

	• yum install -y postgresql16-server
	
4) Initialize the database and enable/start the service :

	• /usr/pgsql-16/bin/postgresql-16-setup initdb
	• systemctl enable postgresql-16
	• systemctl start postgresql-16


🛠️ Configuration
-----------------
		
1) Verify that the service is enable and running :

	• systemctl status postgresql16.service 

2) Check configuration file path and port :
	
	• ps -ef | grep postgres		
	
	📝 Note: This step requires the PostgreSQL service to be running.

3) Edit Configuration file

	• vi /var/lib/pgsql/16/data/postgresql.conf 

		• Set listen_addresses to allow external access : 
			
			listen_addresses = 'your_server_ip'
			
		• Confirm or change the port :
			 
			 port = 5432
			
4) Restart postgresql service 

	• systemctl restart postgresql.16.service
