# Corridor write-up

I began by navigating to the target IP address in my browser. Upon opening the website, I was greeted with an image featuring several doors. 

<img width="1437" height="818" alt="Image" src="https://github.com/user-attachments/assets/7484b6e8-f679-4ecb-89f2-a80c3f237ae6" />

Clicking on these doors opened new pages, but they appeared to be empty.

<img width="1442" height="821" alt="Image" src="https://github.com/user-attachments/assets/6ff544c1-1674-4638-a3cf-a464d3308526" />

The provided hint suggested examining the URL endpoints for hexadecimal values that resemble hashes. Following this lead, I extracted the hashes from the URLs and used CrackStation to identify them. The results revealed that the hashes were MD5 encryptions of numbers ranging from 1 to 13.

<img width="1443" height="820" alt="Image" src="https://github.com/user-attachments/assets/90f6487e-5ebe-4207-9d2b-9fea4caa9e4f" />

Recalling the second hint, "Can you find your way back to where you came?", I decided to test the number "0" (representing a potential 'original' or 'previous' door). I used CyberChef to generate an MD5 hash for "0".

<img width="1438" height="825" alt="Image" src="https://github.com/user-attachments/assets/d93706a0-4b91-4781-9c8b-b21bcb46c627" />

 After appending this new hash to the URL endpoint, I successfully accessed a hidden page and captured the flag.

<img width="1433" height="818" alt="Image" src="https://github.com/user-attachments/assets/d0b21125-73c2-4863-9e88-0d5fa204625d" />
