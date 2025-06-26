Title: Recover Deleted File 

Problem:
An important file was accidentally deleted from a Linux system using the command line. With no available backups, the user must recover the file using appropriate Linux recovery techniques and tools

Troubleshooting/Diagnose

Method 1: Recover from an Open File (Held by a Running Process)

1 . List open files and search for the deleted file:
		
		lsof | grep -i satthya
		
2 . Identify the process ID (PID) that still has the file open.

		Example: PID = 1152
		
3 . List open file descriptors for that process:

		ls -l /proc/1152/fd

4 . Find the descriptor pointing to the deleted file (usually marked with (deleted)).

5 . Copy the file from the process’s memory to a safe location:

		cp /proc/1152/fd/3 /tmp/satthya

Remark :- Here, 3 is the file descriptor number corresponding to the deleted file.


Solution:

• Used the lsof command to identify a running process that still had the deleted file open.

• Retrieved the file content directly from the /proc/<pid>/fd/<fd> file descriptor.

• Successfully copied the file to a new location (/tmp) for recovery.

Outcome:

The user was able to recover the deleted file from memory without using any backup tools. System integrity and data were preserved
