# PCAP Analysis

We have captured this traffic from P13's computer. Can you help him?

File Location: /root/Desktop/ChallengeFile/Pcap_Analysis.pcapng

---



## 1. In network communication, what are the IP addresses of the sender and receiver? Answer Format:SenderIPAddress,ReceiverIPAddress

Using the `frame contains "P13"` filter, we find packets containing this word in any layer of the packet (including the payload); since this filter works protocol-independently, it works with any cleartext protocol. When we right-click on one of the found packets and select Follow → TCP Stream, the entire conversation between P13 and Cu713 appears in a single window; the Source IP we see in the title or packet list of the stream window is the sender (P13), and the Destination IP is the receiver. Since the response format is requested as SenderIPAddress,ReceiverIPAddress, it is sufficient to enter the two IPs we found in this order.

<img width="1633" height="830" alt="image" src="https://github.com/user-attachments/assets/e524c776-75a7-4c6c-a9d1-8c9dad18940b" />

## 2. P13 uploaded a file to the web server. What is the IP address of the server?

The standard method for sending data/files in the HTTP protocol is the POST method (GET only requests information and is not suitable for carrying the body), so we start with the filter `http.request.method == "POST"`; we can add `&& frame contains "upload"` to narrow down the result. When this filter is applied, we get a POST request originating from P13's IP address and going to a destination; the Destination IP field of this packet is the address of the web server where the file is uploaded.

<img width="1640" height="852" alt="image" src="https://github.com/user-attachments/assets/0b067582-bed8-4c58-b8ad-08daaa5741e9" />


## 3. What is the name of the file that was sent through the network?

In file uploads, browsers/clients typically use an HTTP body format called multipart/form-data, which includes a line `Content-Disposition: form-data; name="..."; filename="..."` for each uploaded file; the `filename` field directly provides the file name. We can access this line by tracing the TCP/HTTP stream of the POST packet we found in Question 2; alternatively, Wireshark's File → Export Objects → HTTP menu automatically lists all captured files, allowing us to do this much faster than manually reading the stream. In this challenge, we see that the file name is simply "file" — the same technique is used in malware analysis to identify the names of payloads downloaded from the C2 server.

<img width="1672" height="748" alt="image" src="https://github.com/user-attachments/assets/4e106f1a-bde3-4851-b937-742d74827014" />

<img width="1641" height="806" alt="image" src="https://github.com/user-attachments/assets/17fbdc49-2923-421b-beb2-00f4a944302e" />


## 4. What is the name of the web server where the file was uploaded?

When a web server responds to an HTTP request, it typically includes a "Server:" line in the response header, indicating the software it's running (e.g., Server: Apache/2.4.x). This information is located in the response packet, not the request itself. Scrolling down the HTTP stream reveals this line, identifying Apache as the server. In the real world, these "Server" headers are often misused by attackers during reconnaissance because the version number is visible, allowing attackers to directly search for known vulnerabilities (CVEs) associated with that version. Therefore, in production environments, this header is usually hidden.

<img width="1682" height="788" alt="image" src="https://github.com/user-attachments/assets/eed54773-c2eb-4143-83e6-7020ec7c487d" />

## 5. What directory was the file uploaded to?

When a file upload is successful, the server usually notifies the user with a text message, and this message is included in the response body. Looking at this section in the same HTTP stream, we see an expression similar to "file uploaded at uploads/file," from which we deduce that the directory name is "uploads." Such "success messages" can lead to information disclosure in real web applications because they tell the attacker exactly which path they can use to access the uploaded file (e.g., a web shell). Therefore, such detailed (verbose) responses are risky from a security standpoint in production systems and are generally not recommended.

<img width="1641" height="821" alt="image" src="https://github.com/user-attachments/assets/05b55a8b-2ca0-4eac-9df4-29d3ba8b0a54" />

## 6. How long did it take the sender to send the encrypted file?

Instead of looking at individual packets to answer this question, we use Wireshark's Statistics → Conversations tool; this tool summarizes each unique IP-port pair in the captured traffic, displaying information such as total packet count, data volume, and duration in a single table. Switching to the TCP tab and finding the row between P13 and the server IP, the Duration column gives us the total duration of this conversation in seconds; in this challenge, that duration is approximately 0.0073 seconds, indicating that the file is small and likely transmitted at local area network (LAN) speeds.

<img width="1626" height="819" alt="image" src="https://github.com/user-attachments/assets/ca5e393e-b637-4f87-8ed9-672a59a305a9" />
