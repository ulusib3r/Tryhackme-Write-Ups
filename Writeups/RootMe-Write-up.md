# RootMe Write-up

# Root Me - Writeup

### **Level:** Easy

## **Task 1: Deploy the machine**
The machine was started after connecting via **OpenVPN**.

---

## **Task 2: Reconnaissance**
The goal for this task is to gather machine information using tools like `nmap` and `gobuster`.

### **Nmap Scan**
An initial nmap scan was performed:
`nmap –sC –sV <ip>`

![nmapScan](https://github.com/user-attachments/assets/19c70714-74d8-4c0e-862d-9ed51a434a22)


* **-sC**: Scans using default nmap scripts.
* **-sV**: Pulls version information for open ports.

**Scan Results:**
* **Open Ports:** 2 ports are open: **22** (running **ssh**) and **80**.
* **Apache Version:** 2.4.41.

### **Directory Discovery**
**GoBuster** was used to find hidden directories on the web server:
`gobuster dir -u http://<ip> -w /usr/share/wordlists/dirb/common.txt`

![WhatsApp Image 2026-03-23 at 02 14 49 (2)](https://github.com/user-attachments/assets/0e5411ee-fb08-49bd-b4f0-1f8e6a96e116)


**Findings:**
Two subdomains appeared useful: `/panel` and `/uploads`. The hidden directory is **/panel**.

---

## **Task 3: Getting a Shell**

![WhatsApp Image 2026-03-23 at 02 15 04 (1)](https://github.com/user-attachments/assets/46eec5e7-4975-4a57-8832-4c0a85f9ac13)

The `/panel` page allows for file uploads, which can be exploited using a **PHP reverse shell**.

<img width="332" height="91" alt="Screenshot from 2026-03-23 00-56-22" src="https://github.com/user-attachments/assets/037d5e26-db8e-49a0-8ae1-c12e8fe3d07c" />

**Preparation:** The shell script was downloaded from [PentestMonkey](https://pentestmonkey.net/tools/web-shells/php-reverse-shell).
   
**Configuration:** The IP was updated to the local machine's IP and the port was set to **9999**.
 
![WhatsApp Image 2026-03-23 at 02 14 55](https://github.com/user-attachments/assets/d68d92a2-5dfc-482a-b85b-78087f213466)

![WhatsApp Image 2026-03-23 at 02 15 04](https://github.com/user-attachments/assets/5f9a6d87-513e-4f69-909d-2b9777577154)

**Bypass:** Since `.php` files were not accepted, the extension was changed to **.php5** to allow the upload.

![WhatsApp Image 2026-03-23 at 02 15 04 (2)](https://github.com/user-attachments/assets/586b5eba-2363-4e3b-b2c5-d725f385a76a)
    
![WhatsApp Image 2026-03-23 at 02 15 04 (3)](https://github.com/user-attachments/assets/5c4be927-74e4-46e2-8b47-f4e31c359a7b)


**Listener:** A listener was started using **netcat**:
`nc –nlvp 9999`
**Execution:** The script was executed by navigating to the `/uploads` directory and clicking the uploaded file.
 
![WhatsApp Image 2026-03-23 at 02 15 04 (4)](https://github.com/user-attachments/assets/67d8c583-64ef-411e-b0ff-b537023dc436)
    
![WhatsApp Image 2026-03-23 at 02 15 05](https://github.com/user-attachments/assets/3ad99d94-8936-41ba-a3dc-42aee588198a)


**User Flag:**
After gaining a shell, the flag was found at `/var/www/user.txt`.

![userFlag](https://github.com/user-attachments/assets/982dd646-207c-45b0-90d5-42e634f15755)


---

## **Task 4: Privilege Escalation**
To elevate privileges to **root**, a search for files with **SUID** permissions was conducted:
`find / -type f –user root –perm –4000 2> /dev/null`

![WhatsApp Image 2026-03-23 at 02 15 14](https://github.com/user-attachments/assets/dd250f92-71f9-46e8-97f1-44d29caa7b3b)

**Exploitation:**
**Python** was identified in the SUID list. Using [GTFOBins](https://gtfobins.org/gtfobins/python/#shell) as a reference, the following command was used to escalate privileges:

`python -c 'import os; os.execl("/bin/sh", "sh", "-p")'`
<img width="865" height="412" alt="Screenshot from 2026-03-23 01-59-18" src="https://github.com/user-attachments/assets/14b8bfbb-2a58-4cc5-a128-7ba40236072c" />

![WhatsApp Image 2026-03-23 at 02 15 14 (1)](https://github.com/user-attachments/assets/134ba31f-3f5e-4ee5-8797-b1a051ce1b60)

**Root Flag:**
After gaining root access, the final flag was located at `/root/root.txt`.

![rootFlag](https://github.com/user-attachments/assets/62f25bb2-4e71-4c91-8a38-4ab7be52af47)

And that was it, well done!
