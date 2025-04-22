Title: Setting Up an iSCSI Target and Initiator on RHEL

Problem:

A storage administrator needs to configure centralized storage that can be shared across multiple Linux clients over a standard IP network, without requiring specialized Fibre Channel hardware. The objective is to set up a storage server (iSCSI Target) on RHEL and make it accessible to client systems (iSCSI Initiators) using standard TCP/IP networking. The solution must support creating and exposing block-level storage using tools native to RHEL.

The administrator must:

	• Configure and expose storage using the targetcli utility.
	• Ensure firewall settings allow iSCSI traffic on port 3260.
	• Configure a Linux client as an iSCSI initiator to discover and access the target.
	• Use Access Control Lists (ACLs) to securely allow only specific initiators to connect.

Steps 

iSCSI Target (Server Side)

	1. Install targetcli

		○ yum install targetcli
	
	2. Start and enable the target service

		○ systemctl start target
		○ systemctl enable target
	
	3. Launch targetcli

		○ targetcli
	
	4. Create backstore

	• Option (a) : Block Backstore (using a block device)
	
		○ /backstores/block> create "storage" /dev/sdX  # Replace sdX with actual device
	
	• Option (b) : FileIO Backstore (using a file)
	
		○ /backstores/fileio> create name=LUN_3 /root/disk1.img 5G
	
	5. Create iSCSI target
	
	(This generates an IQN like: iqn.2007-02.org.linux-iscsi.user.x8664:sn.XXXX)

		○ /iscsi> create
	
	6. Open firewall port

		○ firewall-cmd --permanent --add-port=3260/tcp
		○ firewall-cmd --reload

	7. Create a Lun and link to backstore

		○ /iscsi/iqn.../tpg1/luns> create /backstores/block/storage1
	
	8. Create ACL for initiator
	
		○ /iscsi/iqn.../tpg1/acls> create iqn.2008-05.com.clientname:uniqueid
	
iSCSI Initiator (Client Side)

	1. Install initiator utils
	
		○ yum install iscsi-initiator-utils
	
	2. Start iscsid service

		○ systemctl start iscsid
		○ systemctl enable iscsid
	
	3. Discover targets on the server
	
		○ iscsiadm -m discovery -t sendtargets -p <target_ip>
	
	4. Log in to the target
	
		○ iscsiadm -m node -T <target_iqn> -p <target_ip> -l

