# Port Scan Activity
Can you determine evidences of port scan activity?

Log file: /root/Desktop/ChallengeFile/port_scan.pcap

Note: pcap file found public resources.

---

<img width="1625" height="1042" alt="image" src="https://github.com/user-attachments/assets/4c4eb188-8408-4897-a57c-ab8b148787cf" />


## 1. What is the IP address scanning the environment?
To solve this problem, it's first necessary to understand TCP SYN scanning, one of the most commonly used methods in port scanning. TCP connections are normally established with a three-way handshake: SYN → SYN/ACK → ACK. An attacker or scanning tool (such as Nmap) sends numerous SYN packets to target systems to identify open ports on the network. Therefore, by applying the filter *tcp.flags.syn == 1 && tcp.flags.ack == 0* in Wireshark, only SYN packets sent for connection initiation purposes can be viewed. Then, it is examined which IP address sent numerous SYN packets to different targets or different ports. The source IP that sends a large number of SYN packets in a short period of time is considered the system performing the port scan. Therefore, SYN traffic is analyzed to find the IP address performing the scan. 10.42.42.253 is scanner.
<img width="1617" height="719" alt="image" src="https://github.com/user-attachments/assets/dd5e00ec-57c3-44c6-bae7-bd5beb50c24f" />


## 2. What is the IP address found as a result of the scan?
To solve this problem, first the IP address of the system performing the scan was identified, and then the systems that responded to the SYN packets sent by this system were examined. In TCP communication, a SYN packet sent to an open port is answered by the target system with a SYN/ACK packet. Therefore, by analyzing the SYN/ACK packets returned to the attacker's IP address, the active systems discovered as a result of the scan can be determined. Since the purpose of port scanning is to identify accessible systems and open services on the network, the target IP address that gave a SYN/ACK response was considered as the system found as a result of the scan. 
<img width="1590" height="659" alt="image" src="https://github.com/user-attachments/assets/365d4efb-ed7f-4711-97f2-f7e1083cd771" />


## 3. What is the MAC address of the Apple system it finds?
To solve this problem, the relevant network traffic was examined by filtering the IP address belonging to the Apple system. Then, the source MAC address was identified by checking the Ethernet header in the packet details. Since Wireshark automatically decodes the OUI (Organizationally Unique Identifier) ​​information of the MAC address, it also shows the device manufacturer. The fact that the source address is displayed as "Apple" in the Ethernet header confirmed that this MAC address belongs to an Apple system. Thus, the MAC address of the Apple device was determined, and the problem was solved.
<img width="1640" height="1032" alt="image" src="https://github.com/user-attachments/assets/eeb09531-f57a-40fc-9d47-a786d9da3eec" />



## 4. What is the IP address of the detected Windows system?
you can determine OS by looking at the TTL of the packet. Normally this is 64 = Linux based. 128 = windows. And it is based off of the source OS, not the receiving OS. You can see the only TTL of 128 is from the source IP of 10.42.42.50.
<img width="1608" height="912" alt="image" src="https://github.com/user-attachments/assets/9d430f68-5f4d-4aa0-bbea-a8f026e3da99" />
