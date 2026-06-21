# Remote Working
Analysis XLS File

File link: /root/Desktop/ChallengeFiles/ORDER_SHEET_SPEC.zip
Password: infected

---

<img width="1624" height="1034" alt="image" src="https://github.com/user-attachments/assets/4d303ca2-017f-448b-9d09-a60836f316b2" />

Upon reviewing the questions, it is assessed that the attack that occurred here was a malware distribution attack carried out via a maliciously crafted Excel file. It is believed that the attacker likely used a seemingly trustworthy file name, such as for remote work, ordering, or document sharing, to persuade the user to open the malicious Excel file. The attacker then executed the malicious content on the system via a macro or embedded malware within the file, and subsequently downloaded a spyware payload from the internet. This analysis will first examine the file's metadata to create a timeline, then analyze the Excel content and macro behavior to determine the attacker's methods, identify the files left on disk and extract their hash values, and finally analyze the download URL used by the malware to reveal the entire flow of the attack. The goal is not only to identify the malicious file but also to examine the steps the attacker followed from initial access to payload download and the traces left on the system from a DFIR perspective.

---

## 1. What is the date the file was created?

Knowing the file's creation date is essential for tracing the event's timeline. Information on when a file was created allows us to correlate the attack's start time, relevant log records, and other artifacts. In DFIR processes, time information is critical for understanding how the attack occurred and what stage it was in.

We can retrieve this information using the exiftool command. Let's look at the create date section below.
<img width="1646" height="1056" alt="image" src="https://github.com/user-attachments/assets/79af8c89-6008-4ab1-b4f5-2d532b6624c8" />
Answer should be UTC format, so we will make it 2020-02-01 18:28:07
<img width="804" height="254" alt="image" src="https://github.com/user-attachments/assets/686cbd9b-5b43-46bf-917b-162b6036d292" />



## 2. With what name is the file detected by Bitdefender antivirus?
The purpose of learning the name under which Bitdefender detected the file is to determine the type of threat posed by the file and how the antivirus engine classified it. In malware analysis, a file's detection name provides important information about which malware family it might belong to, what behaviors it might exhibit, and whether it is a previously known threat. This information allows us to determine which methods to focus on during the analysis process, use the obtained malware name as an IOC (Initial Occur) to investigate whether the same threat exists on different systems, and more accurately assess the scope of the incident.
<img width="1655" height="1052" alt="image" src="https://github.com/user-attachments/assets/3f7ce566-0694-4e4f-806d-77ef83c6c615" />
<img width="2544" height="1247" alt="image" src="https://github.com/user-attachments/assets/9ef69f59-5087-48bc-8600-de289b06c620" />
<img width="1036" height="488" alt="image" src="https://github.com/user-attachments/assets/0d80af64-0a13-4df7-9021-5fcdbacac70e" />



## 3. How many files are dropped on the disk?
The goal of this question is to determine how many different files the attacker created on the system after executing the malicious Excel file. The first file that appears in malware analysis (in this example, an XLS file) is often just the initial stage; the actual malicious payload may be left on disk as separate files without the user noticing. Therefore, dropped files analysis is crucial for understanding the scope of the attack, identifying malicious artifacts on the system, and establishing an IOC (Instance of Attack). For example, finding only the Excel file may not be sufficient in an attack; we need to identify the .exe, .dll, .vbs, or other files created by the malware and look for similar traces on affected systems. This question teaches us which files the attacker left behind after gaining access to the system and how extensive the attack was.
<img width="1696" height="1233" alt="image" src="https://github.com/user-attachments/assets/d277fea6-6711-4f0e-9614-26c272968a9b" />



## 4. What is the sha-256 hash of the file with emf extension it drops?
<img width="1195" height="121" alt="image" src="https://github.com/user-attachments/assets/724ae79a-b294-45ee-b955-bb52786dea97" />



## 5. What is the exact url to which the relevant file goes to download spyware?
The goal of this problem is to identify the address that the malicious Excel file communicates with to download spyware. In malware attacks, the Excel file itself is usually not directly malicious; its macro or script mechanism connects to a remote server specified by the attacker and downloads the second-stage payload. Finding this URL is important for understanding the infrastructure used by the attacker, the malware distribution method, and possible C2 communications. SOC teams can use such URLs as IOCs to search firewall, proxy, DNS, and SIEM logs; they can also identify other users who accessed the same malicious link.
<img width="1535" height="231" alt="image" src="https://github.com/user-attachments/assets/8cb21809-a2d9-45db-80ae-6b500890633b" />
