# Brooklyn Nine Nine write-up

# Brooklyn Nine Nine CTF Write-up 🕵️‍♂️

First, I performed a network scan using `nmap` to identify open ports and services:
- **Port 21:** FTP
- **Port 22:** SSH
- **Port 80:** HTTP

<img width="903" height="455" alt="Image" src="https://github.com/user-attachments/assets/fb02ed8c-e206-4838-8773-2ae6def93e55" />

I logged into the FTP server anonymously and discovered a file named `note_to_jake.txt`. The note mentioned that Jake's password was "too weak."

<img width="900" height="494" alt="Image" src="https://github.com/user-attachments/assets/b05e9096-0766-4b3e-a86b-bd7f2a3dacf7" />

<img width="909" height="179" alt="Image" src="https://github.com/user-attachments/assets/b3a1992d-992b-4567-a74c-4323e62ec598" />

Using the hint from the FTP file, I performed a brute-force attack against the `jake` user using `Hydra` and the `rockyou.txt` wordlist.

<img width="918" height="325" alt="Image" src="https://github.com/user-attachments/assets/12582868-6df9-460a-b292-75c3874300b5" />

I logged in via SSH and navigated through the home directories. I found the `user.txt` flag in Holt's directory.

<img width="697" height="83" alt="Image" src="https://github.com/user-attachments/assets/b69eeeb8-d99d-44cc-b18f-1830380a5403" />

<img width="616" height="274" alt="Image" src="https://github.com/user-attachments/assets/f34e848e-8096-447a-a54c-5d2f51b89c63" />

I checked for sudo permissions using `sudo -l` and found that the user could run `/usr/bin/less` as root without a password.

<img width="918" height="199" alt="Image" src="https://github.com/user-attachments/assets/b2f80738-7875-45a7-b5b1-58260abf3046" />

<img width="1272" height="282" alt="Image" src="https://github.com/user-attachments/assets/400a7612-1a66-4684-8270-a8276e9b3e1a" />

<img width="748" height="381" alt="Image" src="https://github.com/user-attachments/assets/4e3a15fb-3776-4b7a-b174-f115af81e769" />


