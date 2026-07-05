# Brute Force Attacks

Our web server has been compromised, and it's up to you to investigate the breach. Dive into the system, analyze logs, dissect network traffic, and uncover clues to identify the attacker and determine the extent of the damage. Are you up for the challenge?

File Location: /root/Desktop/ChallengeFile/BruteForce.7z

File Password: infected

---

## 1. What is the IP address of the server targeted by the attacker's brute-force attack?

In this problem, the HTTP traffic was isolated by applying the `tcp.port == 80` filter in Wireshark to determine the IP address of the target server. Then, by opening the **Statistics → Endpoints → TCP** section, the endpoints communicating over port 80 were displayed. By examining the IP addresses in the list, it was determined that the target IP address to which HTTP requests from the client were redirected was the web server that was subjected to a brute-force attack.

<img width="1639" height="759" alt="image" src="https://github.com/user-attachments/assets/586afc34-f1f6-4949-9a46-a5a18b43e99d" />



## 2. Which directory was targeted by the attacker's brute-force attempt?

In this problem, to identify the targeted directory, only HTTP POST requests were viewed using the `http.request.method == "POST"` filter in Wireshark. Since brute-force attacks typically send POST requests for authentication purposes, the Request URI field of the relevant packets was examined, and it was determined that the requests were directed to the index.php file, thus identifying the directory targeted by the attack.

<img width="1681" height="826" alt="image" src="https://github.com/user-attachments/assets/d00d6569-7fec-4e0b-ad98-178885092c39" />


## 3. Identify the correct username and password combination used for login. (Answer Format: username:password) 

In this problem, to identify a successful authentication request, the expression >Correct</p> was searched in the packet content using Ctrl+F in Wireshark. Since this expression indicates the HTTP Response packet corresponding to a successful login, the HTTP POST request immediately preceding that packet was examined. By analyzing the username and password parameters in the POST data, the username and password combination used by the attacker for the successful login was determined.

<img width="1668" height="809" alt="image" src="https://github.com/user-attachments/assets/c01bd695-09e6-4abc-ba46-9c589209cd12" />

<img width="1652" height="791" alt="image" src="https://github.com/user-attachments/assets/2b31f57b-00fe-433d-bd04-bd2be3bfe419" />


## 4. How many user accounts did the attacker attempt to compromise via RDP brute-force?

In this question, the `tcp.port == 3389 && frame contains "mstshash"` filter was applied in Wireshark to determine the number of user accounts targeted in an RDP brute-force attack. Since the **mstshash** field in RDP connections contains the user account to which the client is trying to connect, by examining the different user names in the filtered packets, it was determined that the attacker targeted a total of **7** different user accounts.

<img width="1674" height="883" alt="image" src="https://github.com/user-attachments/assets/abfd14b9-00de-43ad-a206-75770b6abed5" />


## 5. What is the “clientName” of the attacker's machine?

In this problem, to determine the name of the client machine used by the attacker, only RDP traffic was viewed by applying the `tcp.port == 3389` filter in Wireshark. Then, the `clientName` field was searched within the packet using **Ctrl+F**. By examining the `clientName` value in the relevant RDP packet, it was determined that the name of the client machine the attacker connected to was **t3m0-virtual-ma**.

<img width="1581" height="775" alt="image" src="https://github.com/user-attachments/assets/c74ba44b-f51c-4812-a7b5-b616783a7413" />


## 6. When did the user last successfully log in via SSH, and who was it? (Answer Format: username:time (HH:MM:SS))


<img width="1639" height="873" alt="image" src="https://github.com/user-attachments/assets/bef6dca8-b283-46fc-be55-5f75b3887a86" />

In this question, the command `cat auth.log | grep -i "accepted"` was run on the `auth.log` file to identify the last successful SSH session. This command filtered the successful authentication records of the SSH service, listing only those logs containing the word **Accepted**. By examining the resulting logs, it was determined that the last successful SSH login was performed by the user **mmox** at **11:43:54**.

## 7. How many unsuccessful SSH connection attempts were made by the attacker?

In this problem, the command `cat auth.log | grep -i "sshd" | grep -i "failed password for" | wc -l` was used to determine the number of failed SSH connection attempts. The command first read the log file with `cat auth.log`, then filtered only the records related to the sshd service with the first `grep`, and selected the "Failed password for" records, which indicate failed authentication attempts, with the second `grep`. Finally, the `wc -l` command calculated the number of filtered rows, revealing that the attacker made a total of 7480 failed SSH connection attempts.

<img width="1651" height="888" alt="image" src="https://github.com/user-attachments/assets/3b3a297b-298d-4e04-a20e-5a968d6184f0" />


## 8. What technique is used to gain access? (Answer Format: MitreID)

In this incident, the attack technique used is classified as **T1110 - Brute Force** within the MITRE ATT&CK framework, as the attacker attempted to access the system by trying numerous username and password combinations. The repeated failed authentication attempts to access Web, RDP, and SSH services, followed by successful logins, exhibit the characteristic behavior of a brute-force attack, and therefore the incident is matched with the **T1110** technique.

<img width="2560" height="1231" alt="image" src="https://github.com/user-attachments/assets/23ce3f88-41e8-493b-86a4-0a891600ca1d" />


