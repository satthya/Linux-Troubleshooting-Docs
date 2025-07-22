
🐘 PostgreSQL Database Administration


🎯 Objective

To demonstrate essential PostgreSQL administration skills, including:

	• Database and user management  
	
	• Privilege assignment  
	
	• Password expiry handling  
	
	• User session monitoring  
	
	• Backup and restoration using SQL commands and pgAdmin

---

📋 Problem Statement

You are acting as a PostgreSQL Database Administrator. Complete the following tasks:

	• Create two databases: `db1` and `db2`, each with sample tables and data  
	
	• Create two users: `user1` and `user2`  
	
	• Set and verify password expiry dates for both users  
	
	• Monitor currently logged-in users  
	
	• Grant:
		Full access on `db1` to `user1`
		Full access on `db2` to `user2`  
	
	• Connect via **pgAdmin** and verify:
		Databases exist
		Users have correct access  
	
	• Take dump backups of `db1` and `db2`  
	
	• Delete both databases  
	
	• Restore them from backups  
	
	• Verify:
		Sample data is restored  
		User privileges remain intact  

---

🔹 Scenario 1: Database Creation

1. Create databases:

   CREATE DATABASE db1;
   CREATE DATABASE db2;

2. List databases:

   \l

3. Connect to a database:

   \c db1


4. Create sample table and insert data:

   CREATE TABLE list (s_no SERIAL PRIMARY KEY, name TEXT, email TEXT);
   INSERT INTO list (s_no, name, email) VALUES (1, 'Satthya', 'satthya@gmail.com');

5. Verify:

   \dt
   SELECT * FROM list;

*Repeat steps 3–5 for `db2`.*

---

🔹 Scenario 2: User Management

1. Create users with passwords:

   CREATE USER user1 WITH PASSWORD 'Welcome@1';
   CREATE USER user2 WITH PASSWORD 'Welcome@2';


2. Set password expiry:

   ALTER ROLE user1 VALID UNTIL '2025-08-12';
   ALTER ROLE user2 VALID UNTIL '2025-08-12';


3. Verify roles:

   \du

---

🔹 Scenario 3: Monitor Logged-In Users

SELECT usename, client_addr, backend_start FROM pg_stat_activity;

---

🔹 Scenario 4: Assign Privileges

1. Grant access on `db1` to `user1`:

   GRANT ALL PRIVILEGES ON DATABASE db1 TO user1;
   GRANT ALL PRIVILEGES ON ALL TABLES IN SCHEMA public TO user1;


2. Grant access on `db2` to `user2`:

   GRANT ALL PRIVILEGES ON DATABASE db2 TO user2;
   GRANT ALL PRIVILEGES ON ALL TABLES IN SCHEMA public TO user2;


---

🔹 Scenario 5: Configure Remote Access and Connect Using pgAdmin

1. vi `postgresql.conf`:

   listen_addresses = '*'
   port = 5432


2. vi `pg_hba.conf`:

   host    db1    user1    192.168.0.10/24    scram-sha-256
   host    db2    user2    192.168.0.10/24    scram-sha-256


3. Restart PostgreSQL:

   systemctl restart postgresql-16


4. Open firewall ports:

   firewall-cmd --add-service=postgresql --permanent
   firewall-cmd --add-port=5432/tcp --permanent
   firewall-cmd --reload


5. Launch **pgAdmin** on a client machine:
   - Register server:
     - **Name:** db1
     - **Host:** PostgreSQL server IP
     - **Database:** db1
     - **Username:** user1
     - **Password:** Welcome@1

---

🔹 Scenario 6: Take Backups Using `pg_dump`

1. Create backup directory:

   mkdir -p /pg_backup/dump_backup
   chown -R postgres:postgres /pg_backup/dump_backup


2. As `postgres` user, back up the databases:

   su - postgres
   pg_dump -d db1 -f /pg_backup/dump_backup/db1_backup.sql
   pg_dump -d db2 -f /pg_backup/dump_backup/db2_backup.sql


---

🔹 Scenario 7: Restore Databases Using `psql`

1. Drop existing databases:

   DROP DATABASE db1;
   DROP DATABASE db2;


2. Recreate databases:

   CREATE DATABASE db1;
   CREATE DATABASE db2;


3. Restore from dumps:

   su - postgres
   psql -d db1 -f /pg_backup/dump_backup/db1_backup.sql
   psql -d db2 -f /pg_backup/dump_backup/db2_backup.sql


4. Verify:

   \c db1
   \dt
   SELECT * FROM list;

*Repeat verification for `db2`.*
