# Remote Working
Analysis XLS File

File link: /root/Desktop/ChallengeFiles/ORDER_SHEET_SPEC.zip
Password: infected

---

<img width="1624" height="1034" alt="image" src="https://github.com/user-attachments/assets/4d303ca2-017f-448b-9d09-a60836f316b2" />

Upon reviewing the questions, it is assessed that the attack that occurred here was a malware distribution attack carried out via a maliciously crafted Excel file. It is believed that the attacker likely used a seemingly trustworthy file name, such as for remote work, ordering, or document sharing, to persuade the user to open the malicious Excel file. The attacker then executed the malicious content on the system via a macro or embedded malware within the file, and subsequently downloaded a spyware payload from the internet. This analysis will first examine the file's metadata to create a timeline, then analyze the Excel content and macro behavior to determine the attacker's methods, identify the files left on disk and extract their hash values, and finally analyze the download URL used by the malware to reveal the entire flow of the attack. The goal is not only to identify the malicious file but also to examine the steps the attacker followed from initial access to payload download and the traces left on the system from a DFIR perspective.

---

1. What is the date the file was created?



2. With what name is the file detected by Bitdefender antivirus?



3. How many files are dropped on the disk?



4. What is the sha-256 hash of the file with emf extension it drops?



5. What is the exact url to which the relevant file goes to download spyware?
