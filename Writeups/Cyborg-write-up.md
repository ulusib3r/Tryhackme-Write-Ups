# Cyborg write-up
#### Author: https://github.com/Nazlii07
Cyborg Writeup  
Nmap Scan 
We start with a basic port scan to identify open services on the target machine: 
nmap -sC -sV 10.80.163.166 

<img width="750" height="304" alt="Image" src="https://github.com/user-attachments/assets/0f8f37d9-b36d-4bef-83e5-afca1ac85faa" />

Scan Results
The scan revealed two open ports:
•	Port 22 (SSH) – Indicates that the machine allows remote login via SSH.
•	Port 80 (HTTP) – Indicates that a web server is running.
This information defines the initial attack surface. Since SSH typically requires valid credentials, the logical next step was to enumerate the web service running on port 80 in order to identify potential information leaks, misconfigurations, or exposed credentials.
________________________________________
Web Enumeration (Port 80)
Navigating to the web service via a browser:
http://10.80.163.166

<img width="1012" height="898" alt="Image" src="https://github.com/user-attachments/assets/469652ac-5661-4408-8a03-0c34610e7897" />

The page displayed the Apache2 Ubuntu Default Page, confirming:
•	Apache2 as the web server
•	Ubuntu as the operating system
•	Default web root location (/var/www/html)
Although no sensitive information was directly exposed, default Apache pages often indicate incomplete configurations and the potential existence of unsecured directories.
________________________________________
Directory Enumeration with Gobuster
Since the default page did not provide useful information, directory enumeration was performed using Gobuster:
gobuster dir -u http://10.80.163.166 -w /usr/share/wordlists/dirb/common.txt
Results
Gobuster revealed multiple accessible directories, most notably:
•	/admin
•	/etc
These directories should never be publicly accessible, especially /etc, which usually contains sensitive system and service configuration files. This confirmed a serious web server misconfiguration.
________________________________________
/admin Directory – Information Disclosure
Navigating to:
http://10.80.163.166/admin

<img width="1896" height="873" alt="Image" src="https://github.com/user-attachments/assets/8d15865a-a23b-4a34-9d12-7257dbd073cf" />

revealed an Admin Shoutbox containing internal messages between users.

<img width="1827" height="783" alt="Image" src="https://github.com/user-attachments/assets/84c8bf05-9c05-4bf9-83cf-ea9de380b8d6" />

One message from Alex was particularly important. In this message, Alex stated that:
•	He had been experimenting with a Squid proxy
•	The proxy was likely misconfigured
•	Configuration files were left exposed
•	A backup named “music_archive” existed
At the bottom of the page, a file named archive.tar was available for download.
Based on Alex’s message, this archive became a primary target for further analysis.
________________________________________
/etc Directory – Exposed Squid Configuration
Following the hint from the admin page, the /etc directory was accessed via the browser:
http://10.80.163.166/etc

<img width="592" height="320" alt="Image" src="https://github.com/user-attachments/assets/2cbf81cc-eb47-4375-80e0-3673fe59f0b6" />

Directory indexing was enabled, revealing a squid/ directory.
Navigating to:
http://10.80.163.166/etc/squid/passwd
revealed the following entry:

<img width="481" height="29" alt="Image" src="https://github.com/user-attachments/assets/7e521018-8b7d-4ec5-a174-2e3dc9762df7" />

This format indicates:
•	Username: music_archive
•	Hash type: Apache MD5 ($apr1$)
This confirmed that Squid authentication credentials were publicly exposed via the web server.
________________________________________
Cracking the Squid Password Hash
The hash was copied into a file and cracked offline using John the Ripper with the rockyou.txt wordlist:
john --wordlist=/usr/share/wordlists/rockyou.txt passwd.txt
John successfully recovered the plaintext password:
music_archive : squidward
Given the reference to the music_archive backup, it was clear that this password was related to the downloaded archive.
________________________________________
Analyzing archive.tar
The downloaded archive.tar file was extracted:
tar -xvf archive.tar
Upon inspection, it became clear that the contents were not regular files but a Borg Backup repository.
This was confirmed by the presence of typical Borg files and a README stating:
This is a Borg Backup repository.
See https://borgbackup.readthedocs.io/
Borg backups often contain highly sensitive data such as:
•	User home directories
•	Passwords
•	SSH keys
•	Notes and configuration files
________________________________________
Extracting the Borg Backup
The Borg repository was extracted using the cracked password:
borg extract ./archive::music_archive
When prompted, the passphrase squidward was entered.
The extraction revealed a filesystem structure containing user home directories.

________________________________________
Credential Discovery
After extraction, the following directory was explored:
cd home/field/dev/final_archive
While browsing the files, the following path stood out:
/home/alex/Documents/note.txt
note.txt Contents
Wow I'm awful at remembering Passwords so I've taken my Friends advice and noting them down!
alex:S3cretP@s3
This file directly exposed the SSH credentials for the user Alex.
________________________________________
Gaining SSH Access
Recovered credentials:
•	Username: alex
•	Password: S3cretP@s3
SSH access was obtained using:
ssh alex@10.80.163.166
The login was successful.

<img width="548" height="226" alt="Image" src="https://github.com/user-attachments/assets/bf977ef1-2408-4127-9147-f0be6f3321ba" />

Retrieving the User Flag
After logging in, the home directory was checked:
cd /home/alex
cat user.txt
 The user flag was successfully obtained.
 

<img width="327" height="34" alt="Image" src="https://github.com/user-attachments/assets/a91efa53-dae5-4149-b4df-048a6511436d" />

Privilege Escalation
Sudo permissions were enumerated:
sudo -l
Output
User alex may run the following commands on ubuntu:
    (ALL : ALL) NOPASSWD: /etc/mp3backups/backup.sh
This indicates that the script can be executed as root without a password.

<img width="946" height="97" alt="Image" src="https://github.com/user-attachments/assets/4835db24-4214-473d-807e-dd3bf8b0deaa" />

Analyzing backup.sh
The script was inspected:
cat /etc/mp3backups/backup.sh
Analysis revealed:
•	The script accepts a -c parameter
•	User-supplied input is passed directly to a system command
•	No input sanitization is performed
Because the script is executable with sudo and NOPASSWD, any command supplied to -c is executed with root privileges.
This results in a command injection vulnerability.
________________________________________
Gaining Root Access
The vulnerability was exploited to read the root flag:
sudo /etc/mp3backups/backup.sh -c "cat /root/root.txt"
The command executed successfully, and the root flag was obtained.

<img width="1212" height="556" alt="Image" src="https://github.com/user-attachments/assets/fe5ac73e-e8b2-438d-b763-471a1c22f4a8" />

________________________________________
Conclusion
The Cyborg machine was fully compromised due to multiple critical security misconfigurations.
Key Issues Identified
•	Public exposure of the /etc directory
•	Squid authentication files accessible over HTTP
•	Backup repository stored on the web server
•	Weak backup passphrase
•	Plaintext password storage
•	Insecure sudo script allowing arbitrary command execution
Key Takeaways
•	Thorough web enumeration is essential
•	Backup files often contain extremely sensitive information
•	Credential reuse can lead to full system compromise
•	Sudo permissions must always be carefully audited
•	Scripts executed as root must strictly validate user input


