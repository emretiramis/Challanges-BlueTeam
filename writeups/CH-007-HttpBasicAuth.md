# Http Basic Auth

We receive a log indicating a possible attack, can you gather information from the .pcap file?

Log file: /root/Desktop/ChallengeFile/webserver.em0.pcap

Note: pcap file found public resources.

---

1. How many HTTP GET requests are in pcap?
We can find it with this filter = http.request.method=="GET"
<img width="1674" height="1039" alt="image" src="https://github.com/user-attachments/assets/296daf42-1c1c-4223-ad9a-fa763e6e6644" />

2. What is the server operating system?
We can find it in HTTP response. Attackers choose their exploit based on the information they obtain from this source.
<img width="1454" height="897" alt="image" src="https://github.com/user-attachments/assets/e956a22c-de57-416b-8a14-cb36c36dbf7d" />

3. What is the name and version of the web server software?
We can find the answer in the same place like before question
<img width="1477" height="926" alt="image" src="https://github.com/user-attachments/assets/12b13b96-c243-4e26-ac22-4a36fc99c3c7" />

4. What is the version of OpenSSL running on the server?

<img width="1467" height="912" alt="image" src="https://github.com/user-attachments/assets/82bfbaa8-98b8-4291-935e-42c38393352a" />

5. What is the client's user-agent information?
We can find the answer on http request.
<img width="1657" height="1016" alt="image" src="https://github.com/user-attachments/assets/2d60c2fb-b645-4801-80ff-144f8f26e526" />


6. What is the username used for Basic Authentication?
we can find this answer with "http.authorization" filter.
<img width="1615" height="1014" alt="image" src="https://github.com/user-attachments/assets/22c04229-1666-4c94-8bf5-97a7433f5e06" />
when I decode the base64 encode, I can get both username and password:
<img width="1309" height="874" alt="image" src="https://github.com/user-attachments/assets/62ec51af-006d-4575-a940-853f250c5044" />


7. What is the user password used for Basic Authentication?
<img width="1615" height="1014" alt="image" src="https://github.com/user-attachments/assets/60a5eddd-8fd2-4b5e-9773-c183d4a5f7e1" />
<img width="1309" height="874" alt="image" src="https://github.com/user-attachments/assets/ecd355ca-de51-4363-addf-fd37a7d97031" />
