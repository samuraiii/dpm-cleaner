# dpm-cleaner
Dpm-cleaner is script designed to clean inconsitencies from DPM (http://lcgdm.web.cern.ch/dpm) storage cluster.

##How it works?
Script works on one file system per time.
###Script workflow 
	 1. Check if file system and server exists (using dpm-qryconf)
	 2. Check if file system is not disabled
	 3. Check if mountpoint on server is writable
	 4. Check if file system is set as read-only on head node (and store state)
	 5. Set file system as read-only on headnode
	 6. Wait 3 minutes (hoping for all writes to finish)
	 7. Ask for credentials if not supplied through file or script edit
	 8. Dump data about files from database (or fail)
	 9. Fail also when returned data is empty (with recomandation)
	10. Find and check all files on file system against dumped data (and remove all non-matching)
	11. Fail if data found in step 10 is empty (again with recomandation) 
	12. Try to find all data from database dump and remove all without disk counter part (using dpm-del-replica)
	13. Set file system read-write (if it was marked as such in step 4)
	14. Clean all temporary files used by script
###Logs
Almost all transactions are echoed on terminal and all are written to log file (/var/log/dpm-cleaner/dpm-cleaner.DATEANDTIME.log).
###Requirements
Script was tested on Scientific Linux 6.8.

It needs standard set of system utilities such as ssh, scp ...

Also working instance of DPM is needed ;-)

Headnode should have root ssh access (ideally passwordless) to servers which you need to clean.

Read write acces to database is needed.

###Usage
/path/to/dpm-cleaner <storage server as in dpm-qryconf output> <file system as in dpm-qryconf output>

Example:

	/path/to/dpm-cleaner dpm1.example.tld /mnt/dpmfs1
	
	
##License
This script is licencsed unde MIT License. (See LICENSE file)
