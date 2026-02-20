# Baron Samedit Write-up

#### Author: https://github.com/picomve

## 1. Enumeration & Detection
This vulnerability, known as Baron Samedit, affects Sudo versions prior to 1.9.5p2. It is a Heap-Based Buffer Overflow that allows any local user (even without sudo rights) to gain root privileges.

I identified the target system as Ubuntu 18.04.5 running a vulnerable Sudo version (1.8.21).

## 2. Exploitation Path
I used the sudo-hax-me-a-sandwich exploit developed by Qualys / Blasty.

# Steps:
## 1. Compilation:
Since the source code was available (hax.c, lib.c), I compiled the exploit using the make command to generate the necessary shared libraries and the executable.

```
cd ~/Exploit
make
# Output: gcc -o sudo-hax-me-a-sandwich hax.c ...
```

## 2. Target Selection & Execution:
I ran the exploit to see available targets. Since the system is Ubuntu 18.04, I selected target 0.
`./sudo-hax-me-a-sandwich 0`

## 3. Results
The exploit successfully smashed the heap and spawned a root shell immediately.
```
[+] bl1ng bl1ng! We got it!
# whoami
root
# id
uid=0(root) gid=0(root) groups=0(root)
```

<img width="654" height="593" alt="Image" src="https://github.com/user-attachments/assets/9f9af3bf-b72a-4ce1-9441-66566f49ca1e" />

Conclusion: Successfully escalated privileges from a low-level user to root using the Heap Overflow vulnerability in Sudo command parsing.
