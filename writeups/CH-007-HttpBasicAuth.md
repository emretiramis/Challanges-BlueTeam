# Http Basic Auth

We receive a log indicating a possible attack, can you gather information from the .pcap file?

Log file: /root/Desktop/ChallengeFile/webserver.em0.pcap

Note: pcap file found public resources.

---

1. How many HTTP GET requests are in pcap?

This question aims to filter HTTP traffic on Wireshark and analyze client activity on the network. By using the `http.request.method == "GET"` filter, all GET requests can be viewed, and it can be determined how many different requests an attacker or user has sent to the web server.

<img width="1674" height="1039" alt="image" src="https://github.com/user-attachments/assets/296daf42-1c1c-4223-ad9a-fa763e6e6644" />

2. What is the server operating system?

The purpose of this problem is to teach you how to gather information about a server by examining HTTP response headers. Specifically, the Server header sometimes includes operating system information, and attackers can use this information to search for vulnerabilities in the target system.
<img width="1454" height="897" alt="image" src="https://github.com/user-attachments/assets/e956a22c-de57-416b-8a14-cb36c36dbf7d" />

3. What is the name and version of the web server software?
This question allows the system to determine the type and version of the web server from the HTTP headers. The version of the server software (e.g., Apache or Nginx) is an important source of information for investigating known vulnerabilities and assessing the system's currency.
<img width="1477" height="926" alt="image" src="https://github.com/user-attachments/assets/12b13b96-c243-4e26-ac22-4a36fc99c3c7" />

4. What is the version of OpenSSL running on the server?
The goal of this question is to determine the OpenSSL version used by the server. Since older OpenSSL versions can contain critical vulnerabilities, this information is used in DFIR and vulnerability analysis to assess the potential causes or risk level of an attack.
<img width="1467" height="912" alt="image" src="https://github.com/user-attachments/assets/82bfbaa8-98b8-4291-935e-42c38393352a" />

5. What is the client's user-agent information?
This question teaches you how to gather information about a client by analyzing the User-Agent header in HTTP requests. The browser, operating system, or automation tools (such as curl or Python scripts) being used can be identified from this field, playing a significant role in detecting suspicious activity.
<img width="1657" height="1016" alt="image" src="https://github.com/user-attachments/assets/2d60c2fb-b645-4801-80ff-144f8f26e526" />


6. What is the username used for Basic Authentication?
The goal of this problem is to understand the HTTP Basic Authentication mechanism. When the Base64 encoded data in the Authorization: Basic header is decrypted, the username and password can be obtained. This will demonstrate practically why credentials sent over HTTP are not secure.
<img width="1615" height="1014" alt="image" src="https://github.com/user-attachments/assets/22c04229-1666-4c94-8bf5-97a7433f5e06" />
when I decode the base64 encode, I can get both username and password:
<img width="1309" height="874" alt="image" src="https://github.com/user-attachments/assets/62ec51af-006d-4575-a940-853f250c5044" />


7. What is the user password used for Basic Authentication?
<img width="1615" height="1014" alt="image" src="https://github.com/user-attachments/assets/60a5eddd-8fd2-4b5e-9773-c183d4a5f7e1" />
<img width="1309" height="874" alt="image" src="https://github.com/user-attachments/assets/ecd355ca-de51-4363-addf-fd37a7d97031" />
