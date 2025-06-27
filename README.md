Exam Objective
  ▪ Discover a private SSH key stored online
  ▪ Used on tools like Nmap, Gobuster, Searchsploit, MySQL, etc.
  ▪ Document the vulnerability and the path to discovery
![Image](https://github.com/user-attachments/assets/c24a3be8-7d24-4c90-af5f-4d478f222ea1)
![Image](https://github.com/user-attachments/assets/637dca44-37a8-4a13-aec7-0661bbb8b1ba)
![Image](https://github.com/user-attachments/assets/ab907f17-da1e-435c-ad53-f91a324868d1)
![Image](https://github.com/user-attachments/assets/27b13494-065c-437c-a416-45307ba9abd7)
![Image](https://github.com/user-attachments/assets/aab32a8f-67e6-4b34-9a8d-866841ab3058)
![Image](https://github.com/user-attachments/assets/681b8f13-def5-4e01-b16d-b49869c4c0e3)
![Image](https://github.com/user-attachments/assets/62ff053f-15ea-4d7e-bd3a-0b37caa7cd4b)
![Image](https://github.com/user-attachments/assets/eda096d3-4ed1-44ac-955e-db55798a0a77)
![Image](https://github.com/user-attachments/assets/86f3aa1b-19b8-4a10-966f-e0f6bad289e8)
![Image](https://github.com/user-attachments/assets/8c34207c-e472-4baf-b304-9f3e75ed41ba)
![Image](https://github.com/user-attachments/assets/9d474046-a19d-42b5-9180-8840b8e6d6d2)
![Image](https://github.com/user-attachments/assets/e8fd6b10-6451-4d4b-9feb-d2d7b0f04978)
![Image](https://github.com/user-attachments/assets/157b492d-6f9a-44e6-98bc-fc0e17e1252c)
![Image](https://github.com/user-attachments/assets/4046526a-03fe-47af-b089-079d11f51614)
![Image](https://github.com/user-attachments/assets/33dc070d-3089-47df-b18a-a8b460ba2bd7)
![Image](https://github.com/user-attachments/assets/1dc3d46f-fd28-461c-ae09-bf794e493d9e)
![Image](https://github.com/user-attachments/assets/f270e9b8-5c37-4951-9209-a419cbd81100)
![Image](https://github.com/user-attachments/assets/4c91e27d-bfcc-4cf6-8af0-8c02251d75b6)
![Image](https://github.com/user-attachments/assets/16e46a27-4708-421f-972b-0892695fc3ea)
✅Step-by-step summary of my reconnaissance process 
🔍Step 1: Initial scan with active nmap scan.  
    1.Command: sudo nmap -p- 185.218.124.165 –vv
    2.Command: sudo nmap –sC –sV 185.218.124.165 –p 80 -vv
    Determine the version of the server hosted on port 80. Then report a                  vulnerability for an old version of the server that was hosted.

🧭Step 2: 
✅1.Initial Observation
When visiting the website http://185.218.124.165 I noticed that the connection is made only via HTTP protocol - there is no automatic redirection to HTTPS. The address does not start with "https://".
✅2.Verification with Nmap and Browser
When scanning with nmap, port 443 (HTTPS) was not found open:
sudo nmap -p 443 185.218.124.165
Result: port 443 closed/unavailable.


🧑‍💻Step 3: 
Search for the login page at:
http://185.218.124.165/login
When manually trying to enter incorrect usernames and passwords, I noticed that there is no limit to the number of attempts (no temporary blocking or maintenance after several incorrect entries).
Attempts to make several consecutive unsuccessful logins without getting an error when blocking the account or saving.
Consequences:
  ▪ Allows unlimited login attempts with different passwords (brute force or             dictionary attacks).
  ▪ Can be used to fill in credentials - automatically testing leaked passwords from     other sites.
  ▪ For various errors when entering a username, an enumeration (enumeration) of         valid accounts can be done.

🧨Step 4:
Methodology:
I used nmap to detect and analyze the SSH service running on port 22 of the IP address 185.218.124.165.
The command executed:
  sudo nmap -sC -sV 185.218.124.165 -p 22 -vv
  -sC for default scripts scan
  -sV for service/version detection
  -p 22 to scan only port 22 (SSH)
  -vv for verbose output

✅Step 5:
Methodology:
I tested if the MySQL service was accessible on port 3306 by attempting to connect directly to the database.
Command used:
mysql -h 185.218.124.165 -u root -p
At the password prompt, I entered admin.
Findings:
  ▪ Successfully logged into the MySQL database using the root user with the password     admin.
  ▪ This indicates that the main administrative account uses a weak and easily            guessable password.

✅Step 6:
I accessed the admin panel in http://185.218.124.165/login . Then I went into all the files. I saw that there was text with a hash 2af9b1ba42dc5eb01743e6b3759b6e4b. After I unhash the text I noticed that this was the password that I needed. I started digging around the site and found a file called "Example.md". There was a pastebin link there, which I accessed. The unhash password that I found helped me log in to the pastebin link. From there I received another hashed message, which was the private 🔐ssh key that they required from us to pass the exam.


🧰Tools Used: • gobuster • nmap • Browser • MySQL • searchsploit
The engagement concluded with direct discovery of an SSH private key, completing the exam objective. Thank you, Lachezar – this was a very interesting
