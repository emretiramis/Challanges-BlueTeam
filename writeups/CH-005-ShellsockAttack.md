# Shellshock Attack
You must to find details of shellshock attacks

Log file: /root/Desktop/ChallengeFile/shellshock.pcap

Note: pcap file found public resources.

---

## 1. What is the server operating system?
After detecting the suspicious HTTP request in Wireshark, we examine the relevant packet using the "Follow → TCP Stream" feature to look at the HTTP response headers returned from the server. The Server: header in the response directly provides information about the operating system running on the web server; because web servers like Apache usually explicitly specify both the application name/version and the underlying operating system (e.g., (Unix), (Debian), (Ubuntu), (CentOS)) in this header. Therefore, by reading the Server: line in the packet, we determine the operating system of the target server:
<img width="1633" height="1040" alt="image" src="https://github.com/user-attachments/assets/d7d2edfd-52dc-4be7-8150-c4c83797419f" />


## 2. What is the application server and version running on the target system?
<img width="1674" height="1053" alt="image" src="https://github.com/user-attachments/assets/be4f1a51-6fc1-4b72-a843-4a638ee82597" />


## 3. What is the exact command that the attacker wants to run on the target server?
When we examine the suspicious header in the HTTP request (the packet we found using the `http contains "() { :; };"` filter), we see that the payload begins with the expression `() { :; };`, which is a classic Shellshock exploit pattern that allows Bash to close the function definition and execute the following part as a normal command. The part that follows this point is the command that the attacker actually wants to execute on the target server.
<img width="1610" height="1032" alt="image" src="https://github.com/user-attachments/assets/c2054403-ff0e-4aef-b1c4-842c1569398f" />
