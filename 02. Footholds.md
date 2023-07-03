# 02. Footholds
Once your spreadsheet is built with the relevant information, it's time to try and get a foothold.

|Service|Service|Service|
|---|---|---|
|[[21 - FTP]]|[[22 - SSH]]|[[23 - TELNET]]|
|[[25 - SMTP]]|[[53 - DNS]]|[[80 - WEB]]|
|[[88 - KERBEROS]]|[[111 - NFS]]|[[135 - RPC]]|
|[[445 - SMB]]|[[161 - SNMP]]|[[389 - LDAP]]|
|[[3389 - RDP]]|[[5985 - WINRM]]|[[SQL DBs]]|

---

## What to look for

* Look at the service version of ports and see if there is any low-hanging fruit or public exploits
* If nothing easy is found, look deeper into the services (FTP,SMB,NFS,SMTP,WEB)
* Check if there's a way to upload files
* Check if there's a way to read sensitive information
* Check if there's any files that give contextual hints or point towards a vulnerable service running on an unknown port
* Open each service note and dig deep starting with FTP, SNMP, SMB, HTTP

---


### [[21 - FTP]]

* Check version using `searchsploit` for public exploits
* Check for `anonymous` login
* Check for hints within the directory (i.e. `minniemouse.exe`)
* Download the directory `wget -m ftp://anonymous:anonymous@192.168.215.245`
* Check if there's anything that points towards uploads going to the web directory

### [[80 - WEB]]

* Check version using `searchsploit` for public exploits (Traversal, SQLi, RCE)
* Check to see if anything else is running using `whatweb http://10.10.10.10` (searchsploit, wordpress)
* Fully enumerate with directory brute-forcing
	* Run multiple tools and check for file extensions, try from deeper directories
* Visit site in the browser and look for any context clues
	* See if there's any hint for FQDN and put it in `/etc/hosts`
	* See if there's any hints to valid users or software in pages or source code
* Test everything for default credentials or username being the password

### [[161 - SNMP]]
- Enumerate community strings on v1 and v2
	- `sudo nmap -sU -p 161 --script snmp-brute 192.168.194.149`
- Try to get useful information from accessible communities
	- `snmpwalk -v 1 -c public 192.168.194.149 NET-SNMP-EXTEND-MIB::nsExtendObjects`
	- `snmpwalk -v2c -c public 192.168.194.149 | grep <string>`
