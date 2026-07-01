# MSHTML - 0-Day

2021's 0-Day MSHTML

file location: /root/Desktop/ChallengeFiles/Employee_W2_Form.docx
file location2: /root/Desktop/ChallengeFiles/Employees_Contact_Audit_Oct_2021.docx
file location3: /root/Desktop/ChallengeFiles/Work_From_Home_Survey.doc
file location4: /root/Desktop/ChallengeFiles/income_tax_and_benefit_return_2021.docx

This challenge prepared by @Bohan Zhang Malware samples: MalwareBazaar

---

## 1. Examing the Employees_Contact_Audit_Oct_2021.docx file, what is the malicious IP in the docx file?

Analysis of the Employees_Contact_Audit_Oct_2021.docx file using the oleobj tool revealed that the document contains an embedded OLE object with an external link pointing to the IP address 175[.]24[.]190[.]249. This IP address is an IOC (Indicator of Compromise) used to download or process malicious content remotely upon opening the document and is consistent with the CVE-2021-40444 (MSHTML Remote Code Execution) exploit chain. The attacker aimed to establish a connection to this server via Word's MSHTML component by exploiting the external reference mechanism in the Office document, thereby creating the necessary infrastructure for loading malicious HTML/ActiveX content and initiating the remote code execution process.

<img width="871" height="254" alt="image" src="https://github.com/user-attachments/assets/ea47d425-a30b-4980-b460-323cddc06027" />


## 2. Examing the Employee_W2_Form.docx file, what is the malicious domain in the docx file?

Analysis of the Employee_W2_Form.docx file using the oleobj tool revealed that an embedded OLE (Object Linking and Embedding) object within the document references an external resource and uses the domain name arsenal[.]30cm[.]tw. This indicates that Word attempts to load remote content upon opening the document, a characteristic feature of the CVE-2021-40444 (MSHTML Remote Code Execution) attack chain. The attacker used this external domain name to host malicious HTML or ActiveX content and initiate the execution of malicious code via the MSHTML engine.

<img width="1485" height="832" alt="image" src="https://github.com/user-attachments/assets/002bd136-3b07-4bf9-a26a-b4eb042428e2" />


## 3. Examing the Work_From_Home_Survey.doc file, what is the malicious domain in the doc file?

Analysis of the Work_From_Home_Survey.doc file using the oleobj tool revealed that the document contains an embedded OLE object with an external reference pointing to the domain name trendparlye[.]com. This domain name is considered part of the infrastructure used in the CVE-2021-40444 (MSHTML Remote Code Execution) exploit chain, which allows Word to load remote content when the document is opened. The attacker aimed to use the external OLE reference to pull malicious HTML or ActiveX content from a remote server via Word's MSHTML component, thereby executing the malicious code.

<img width="1637" height="978" alt="image" src="https://github.com/user-attachments/assets/a95645b1-6c3f-4af1-aabb-9f6031e118c1" />


## 4. Examing the income_tax_and_benefit_return_2021.docx, what is the malicious domain in the docx file?

Analysis of the income_tax_and_benefit_return_2021.docx file using the oleobj tool revealed that the document contains an embedded OLE object with an external reference to the hidusi[.]com domain. This domain is an IOC (Indicator of Compromise) that enables the loading of remote malicious content upon opening the Office document and is consistent with the CVE-2021-40444 (MSHTML Remote Code Execution) exploit chain. The attacker aimed to exploit the external OLE link in the Word document to allow the MSHTML component to process remote HTML or ActiveX content, thereby initiating the remote code execution process.

<img width="1645" height="950" alt="image" src="https://github.com/user-attachments/assets/28c93375-fc41-46e5-a49a-300525f7830f" />


## 5. What is the vulnerability the above files exploited?

External OLE objects and remote URL references detected in the analyzed Office documents indicate that the files were prepared to exploit the CVE-2021-40444 vulnerability. This critical vulnerability allows Microsoft Word to load malicious HTML content from a remote location via embedded OLE objects through the MSHTML (Internet Explorer) rendering engine, and under certain conditions, enables remote code execution (RCE) on the victim system. Using this method, attackers trick the user into opening the document, then the Word application connects to the external server, downloads the malicious content, and initiates the exploitation chain. Therefore, CVE-2021-40444 is a critical Office vulnerability that is commonly used in phishing campaigns, providing initial access and playing a significant role in malware distribution.
