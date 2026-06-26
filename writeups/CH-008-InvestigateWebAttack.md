# Investigate Web Attack

Investigate Web Attack
We detected some web attacks and need to do deep investigation.

Challenge File: /root/Desktop/ChallengeFile/access.log

---

1. Which automated scan tool did attacker use for web reconnaissance?
<img width="1600" height="1009" alt="image" src="https://github.com/user-attachments/assets/698eb4dd-a655-4dab-bc07-09bd29b1a709" />


2. After web reconnaissance activity, which technique did attacker use for directory listing discovery?
Following the reconnaissance phase, the attacker used the **Directory Brute Force (Forced Browsing/Directory Enumeration)** technique to identify directories and files on the web server that are accessible but not linked to. In this technique, the attacker sends automated HTTP GET requests to numerous possible directories such as `/admin`, `/backup`, and `/uploads` using a pre-prepared wordlist, and attempts to determine the existing directories by analyzing the **200 OK**, **403 Forbidden**, or **404 Not Found** responses returned from the server. This phase is typical web attack behavior aimed at discovering potential targets for subsequent exploit attempts.
<img width="1672" height="967" alt="image" src="https://github.com/user-attachments/assets/4e6519bf-82a8-4fae-885a-6ff65ae5edd5" />


3. What is the third attack type after directory listing discovery?
after directory brute force, brute force attack starting for login page.
<img width="1635" height="1034" alt="image" src="https://github.com/user-attachments/assets/0ac676e4-5b64-4772-a354-52fdec253dce" />


4. Is the third attack successful?

<img width="1583" height="521" alt="image" src="https://github.com/user-attachments/assets/5c5c8d24-a027-41cf-9887-55694c0b149a" />


5. What is the name of fourth attack?
After successfully logined, attacker start code injection attack.
<img width="1607" height="1017" alt="image" src="https://github.com/user-attachments/assets/d46cb1fc-a228-4ec2-a705-4e76d5662557" />


6. What is the first payload for 4th attack?
first command is "whoami"
<img width="1607" height="1017" alt="image" src="https://github.com/user-attachments/assets/99655a56-f7a0-4964-9084-1807a6bb4e0a" />


7. Is there any persistency clue for the victim machine in the log file ? If yes, what is the related payload?

<img width="1605" height="1014" alt="image" src="https://github.com/user-attachments/assets/5e28859a-583c-4f04-b31c-daa90a2f6961" />
when we decode it
<img width="990" height="490" alt="image" src="https://github.com/user-attachments/assets/bf5492a9-e698-451e-ae53-d71b74839e4e" />
