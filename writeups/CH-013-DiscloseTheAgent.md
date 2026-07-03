# Disclose The Agent
We reached the data of an agent leaking information. You have to disclose the agent.

Log file: /root/Desktop/ChallengeFile/smtpchallenge.pcap

Note: pcap file found public resources.

---

## 1. What is the email address of Ann's secret boyfriend?

To solve this problem, the PCAP file is opened with Wireshark and an SMTP filter is applied. Then, the email session is viewed as a single stream using the Follow → TCP Stream option in any SMTP packet. The recipient's email address is identified by examining the RCPT TO: command in the SMTP conversation or the To: field in the email headers.

<img width="1619" height="864" alt="image" src="https://github.com/user-attachments/assets/416ff136-dcf3-422b-ae77-d4ab73e8f807" />


## 2. What is Ann's email password?

The AUTH LOGIN command is found in the SMTP traffic, and the Base64 encoded username and password values ​​sent afterward are examined. The Base64 data obtained via the Follow TCP Stream is decoded using CyberChef, base64 -d, or a similar tool to obtain Ann's email password in plain text. This question demonstrates the security risk posed by unencrypted SMTP authentication.

<img width="1641" height="832" alt="image" src="https://github.com/user-attachments/assets/1c7f68ea-4955-4748-9c11-91c153a6ec9a" />
<img width="871" height="729" alt="image" src="https://github.com/user-attachments/assets/236e1955-8fa0-4a07-8b67-1ef08cf0b7da" />


## 3. What is the name of the file that Ann sent to his secret lover?

After examining the SMTP session using Follow TCP Stream, the Content-Disposition: attachment and filename= fields within the MIME headers are searched. Alternatively, the files attached to the email can be listed using the File → Export Objects → SMTP menu. The file name specified in these fields provides the answer to the problem.

<img width="1664" height="817" alt="image" src="https://github.com/user-attachments/assets/36eef12c-42de-42a7-b875-60458bbdef9d" />


## 4. In what country will Ann meet with her secret lover?

The email content is found in the message body following the DATA command in the SMTP session, or in an attached file. If the information is in an attached file, it is exported via File → Export Objects → SMTP and opened with the appropriate application to examine its contents. The meeting country mentioned in the file is the answer to the question.

<img width="750" height="558" alt="image" src="https://github.com/user-attachments/assets/91cf6501-f80a-4d95-97e3-e5be548e3228" />
<img width="859" height="746" alt="image" src="https://github.com/user-attachments/assets/83f49156-d929-472a-8d4a-8f4d8526ee70" />
<img width="756" height="338" alt="image" src="https://github.com/user-attachments/assets/fe7c03a4-7da8-4910-b2d4-295cce9a6c5d" />


## 5. What is the MD5 value of the attachment Ann sent?

First, the file attached to the email is exported using File → Export Objects → SMTP. Then, to verify the integrity of the file and obtain its unique identifier, the MD5 hash value is calculated using the `md5sum` command in Linux, `certutil -hashfile <file> MD5` in Windows, or `Get-FileHash -Algorithm MD5` command in PowerShell. This value is widely used as an IOC (Indicator of Compromise) in incident response processes.

<img width="1646" height="815" alt="image" src="https://github.com/user-attachments/assets/74f9d0ff-d1ab-49f1-99bc-2ce5232bc0f3" />


