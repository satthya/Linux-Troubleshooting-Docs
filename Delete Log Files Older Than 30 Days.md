Title: Delete Log Files Older Than 30 Days

Problem:

A system is accumulating .log files over time, potentially consuming large amounts of disk space. 
There is a need to identify and delete log files that are older than 30 days to maintain system performance and storage efficiency.


Troubleshooting/Diagnose

1. Identify Log File Location
   Determine the directory where log files are stored, commonly:

		/var/log
		
2. Dry run -list files older than 30 days
   Before deletion, verify which files would be affected:
   
		• find /var/log -type f -name "*.log" -mtime +30
	
3. Delete old log Files
   Use the find command to delete .log files older than 30days:
   
		• find /var/log -type f -name "*.log" -mtime +30 -exec rm -f {} \;


	
	
Conclusion

		• Old log files can accumulate and consume disk space over time.
		• The find command helps automate cleanup by targeting .log files older than a specific duration.
		• Always perform a dry run first to avoid accidental deletion.

