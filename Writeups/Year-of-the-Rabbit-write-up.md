# Year of the Rabbit write-up
#### Author: https://github.com/nitibyre

I started by scanning the target machine with nmap. Seeing that ports 21 (FTP), 22 (SSH), and 80 (HTTP) were open, I turned my attention to the web side.

<img width="974" height="609" alt="Image" src="https://github.com/user-attachments/assets/0d685715-a3fd-4ae6-8ed3-337c30e5e311" />

When I couldn't find much in the HTML sections of the website on the given IP, I found the /assets folder while scanning the directories with Gobuster. 

<img width="973" height="428" alt="Image" src="https://github.com/user-attachments/assets/154e10f9-2835-4ebe-81f5-013ef2471d93" />

I added “/assets” to the end of the IP address. When I added the hidden extension between the comment lines in the style.css file on the site that appeared to the end of the IP address, it redirected me to a website with auto-redirect. I opened the browser's inspect mode and focused on the network section. While reloading the page, the “secret directory” section in the network section caught my attention.

<img width="973" height="589" alt="Image" src="https://github.com/user-attachments/assets/2807bddd-f6bc-429d-a37c-12608c1f7a23" />

When I pasted the given directory to the end of the IP address, the page that opened only contained hot_babe.png. After failing to extract anything by downloading the file, changing its extension, and checking its metadata, I used strings to extract the ASCII/Unicode characters within the image to the shell. I was faced with a long jumble of meaningless characters, but among them was a small compressed note containing a long list of possible usernames and passwords needed to connect to the FTP network. I saved the passwords to a txt file and tried them all one by one with the hydra tool to connect to the FTP. Once I found the correct password, I connected to the FTP and checked what was in the directory. I downloaded the text file I found and opened it on my system. When I decoded the BrainFuck language code inside the file on the website, the username and password “eli” appeared.

<img width="973" height="420" alt="Image" src="https://github.com/user-attachments/assets/c41f490e-8566-4294-bd57-c61108882261" />

When I connected to the system as an administrator, the system displayed a message from root to Gwendolin. The message requested that root check Gwendolin's “s3cr3t” file. 

<img width="975" height="385" alt="Image" src="https://github.com/user-attachments/assets/07675598-177c-434a-bdd5-e90de945d947" />

I navigated up to the “/home” directory and ran the command find / -name “s3cr3t” to locate the directory.

<img width="975" height="341" alt="Image" src="https://github.com/user-attachments/assets/bb2898a6-6bb4-40d4-99aa-4d9c3a367a61" />

I navigated to the directory and scanned it with ls -la, noticing a hidden file. I read the file using the “cat” command. This allowed me to obtain gwendolin's FTP password. I then closed my FTP connection and connected as gwendolin. By scanning the directory with `ls -la`, I found the txt file containing the flag, opened it with `cat`, and found the first flag.

<img width="975" height="502" alt="Image" src="https://github.com/user-attachments/assets/a2b3faf0-5029-409e-9e98-bde4a4a4492b" />

After that, I ran sudo -l to see how much authority I had in the system. When I saw that I could use vi without entering a password, I researched vulnerabilities on the web. 

<img width="975" height="109" alt="Image" src="https://github.com/user-attachments/assets/ee0ebfa9-aa37-428f-821c-00d44893656a" />

As a result of my research, I opened the user.txt file in the editor with a non-existent user ID using the command “sudo -u#-1 (specifying the user ID as -1) /usr/bin/vi /home/gwendolin/user.txt”. This confuses the system and automatically elevates me to the highest level of authority. When the vi editor opened, I entered the command “:!/bin/bash,” which allowed me to run commands from within the editor. Since vi runs with root privileges, the terminal it opened also had root privileges. After verifying this with “whoami,” I navigated to the root directory, scanned it, opened the root.txt file containing the final flag, retrieved the flag, and exited the system.

<img width="975" height="371" alt="Image" src="https://github.com/user-attachments/assets/7138ac87-bb9d-44d9-85fe-43e0b42d6a6f" />
