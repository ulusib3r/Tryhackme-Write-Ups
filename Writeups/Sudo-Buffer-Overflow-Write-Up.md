# Sudo Buffer Overflow Write-Up

# CVE-2019-18634 Sudo Privilege Escalation Writeup

#### Author: https://github.com/picomve
## 1. Enumeration & DetectionFirst, I checked my current privileges and looked for misconfigurations:

<img width="550" height="109" alt="Image" src="https://github.com/user-attachments/assets/53baed20-9fe0-4242-9c12-35347a2b5194" />

Finding: I noticed pwfeedback was enabled in the Defaults entry. This specific setting in Sudo versions prior to 1.8.26 is vulnerable to a stack-based buffer overflow.

<img width="489" height="171" alt="Image" src="https://github.com/user-attachments/assets/9ad11799-1e7b-48e8-ad45-7bcbcab55b81" />

## 2. Exploitation Path
Since the target was vulnerable, I used the exploit developed by [@saleemrash1d.](https://github.com/saleemrashid/sudo-cve-2019-18634)

Transferring the exploit:
I downloaded the exploit.c file to the target machine.

Compilation:
Using GCC to compile the C code:


`gcc exploit.c -o exploit`

### Execution:
Running the binary to trigger the overflow in the tty ticket management:

## 3. Results
After execution, the shell prompt changed from $ to #.
<img width="507" height="122" alt="Image" src="https://github.com/user-attachments/assets/9ec2aa0f-3f26-4b49-b17e-3c0869d4a551" />

# uid=0(root) gid=0(root) groups=0(root)

<img width="473" height="97" alt="Image" src="https://github.com/user-attachments/assets/51a0bb80-b984-4d79-abfb-a8bb9f91c215" />

Final Step: Successfully read the flag at /root/root.txt.
