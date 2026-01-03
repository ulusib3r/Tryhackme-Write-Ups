# Simple CTF write-up

# Simple CTF Write-Up
#### Author: https://github.com/elifnurozkaya

## How many services are running under port 1000? 

## What is running on the higher port? 

I started with an Nmap service scan to identify open ports and running services. 

<img width="902" height="245" alt="Image" src="https://github.com/user-attachments/assets/df5b6b9c-d6ee-45fb-8acc-8aa2e22e5090" />

There are 2 services running under port 1000 (FTP and HTTP). The higher port (2222) is running SSH. 

## What's the CVE you're using against the application? 

## To what kind of vulnerability is the application vulnerable? 

I made a gobuster scan to find hidden directories on the web server.When i checked the /simple directory, i found out “CMS Made Simple” used in the website at the footer.After a quick web-browsing here is our vulnerability and its CVE:

<img width="919" height="469" alt="Image" src="https://github.com/user-attachments/assets/85722295-2619-4bd3-be14-6589f85701ef" />

<img width="904" height="521" alt="Image" src="https://github.com/user-attachments/assets/17048842-9504-4009-97c7-bd079787982b" />

<img width="1347" height="405" alt="Image" src="https://github.com/user-attachments/assets/ee005ea8-1d04-4147-b35e-61acc304e4ab" />

I used searchsploit to find exploit id

<img width="919" height="922" alt="Image" src="https://github.com/user-attachments/assets/6bd98edf-4924-4011-855d-6c1ddc3c9831" />

## What's the password?

<img width="927" height="52" alt="Image" src="https://github.com/user-attachments/assets/dfd9b019-fe28-44fd-949b-0f066200cd22" />

<img width="840" height="85" alt="Image" src="https://github.com/user-attachments/assets/83748244-7248-4abc-98de-bc3dbabf872b" />

## Where can you login with the details obtained? 

Now, I have username and password.Also I know that ssh service is open from the nmap scan.So I started a ssh session. 

## What's the user flag?

<img width="858" height="476" alt="Image" src="https://github.com/user-attachments/assets/8a24cd23-751a-45be-8edd-3d55bc1d5d93" />

## Is there any other user in the home directory? What's its name? 

<img width="912" height="52" alt="Image" src="https://github.com/user-attachments/assets/4da33095-2967-4e6a-914a-df3b2db95b8d" />

## What can you leverage to spawn a privileged shell? 

<img width="912" height="64" alt="Image" src="https://github.com/user-attachments/assets/83efcfc7-3d01-4685-9737-50f7a2bc86a0" />

## What's the root flag? 

<img width="854" height="37" alt="Image" src="https://github.com/user-attachments/assets/c715846c-ee56-4058-8c5b-98b1cdaba724" />

<img width="912" height="139" alt="Image" src="https://github.com/user-attachments/assets/d1f79456-3d8e-45ea-adb6-802dc980dc64" />




