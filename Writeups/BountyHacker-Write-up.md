# BountyHacker Write-up

### **Cowboy Hacker (Bounty Hacker) Write-Up!**

First, I started by scanning the target IP address with ‘nmap’. I saw that ports 21, 22, and 80 were open. Looking a little more closely, I saw that the ftp (21) port allowed anonymous access without a password. 

<img width="974" height="631" alt="Image" src="https://github.com/user-attachments/assets/9687b571-86e4-4d05-a479-9fb45ab62365" />

After connecting to FTP, I downloaded the .txt files in the directory to my system and examined them. There were two files containing someone named Lin's task list and possible passwords.


<img width="975" height="431" alt="Image" src="https://github.com/user-attachments/assets/a47a7228-5ca0-4a58-824c-54d7b17a8639" />

I tried the Hydra tool to guess the possible passwords of the Lin user over SSH. I connected to the SSH system using the username and the password I found.

<img width="975" height="264" alt="Image" src="https://github.com/user-attachments/assets/7d6451de-c273-473c-8021-40807be6d314" />

I scanned the directory and opened the user.txt file containing the first flag. After verifying the flag's validity, I searched for vulnerabilities within the system using ‘sudo -l’.  I searched the web to see if there were any vulnerabilities associated with the vulnerability I found, and used the vulnerability I found to gain root access on the system.

<img width="975" height="317" alt="Image" src="https://github.com/user-attachments/assets/b9d7f785-fdba-4005-9c27-4a4a23c842e0" />

After that, I searched for the root.txt file on the system using the ‘find’ command. I navigated to the directory that appeared, retrieved the second flag from the .txt file, exited the system, and completed the room.

<img width="738" height="536" alt="Image" src="https://github.com/user-attachments/assets/621d9dea-5465-48b6-a51b-4b3889d3be1d" />
