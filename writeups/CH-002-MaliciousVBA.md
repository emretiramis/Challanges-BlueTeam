## Malicious VBA
One of the employees has received a suspicious document attached in the invoice email. They sent you the file to investigate. You managed to extract some strings from the VBA Macro document. Can you refer to CyberChef and decode the suspicious strings?

Please, open the document in Notepad++ for security reasons unless you are running the file in an isolated sandbox.

Malicious Macro: /root/Desktop/ChallengeFiles/invoice.vb

<img width="1622" height="1010" alt="image" src="https://github.com/user-attachments/assets/b8ae1017-2627-4f2e-84f3-a10e7d15772c" />


Hackers typically email an Office file that appears to be a fake invoice, CV, or report; when the user opens the file and clicks "Enable Macro/Content," the VBA code runs. This VBA code connects to the internet in the background, downloads a malicious payload from a command-and-control server controlled by the attacker, saves it to disk, and executes it. It does this by using HTTP connections, file-writing objects like ADODB.Stream, and Windows features like WMI.



1. The document initiates the download of a payload after the execution, can you tell what website is hosting it? <br>
Hackers hide these macros to remain undetected and make analysts' jobs more difficult. They achieve this hiding technique using methods like base64 and hexadecimal. We will use the Cybercheff platform to decrypt these hidden codes.<br>
<img width="1581" height="944" alt="image" src="https://github.com/user-attachments/assets/8b9576a9-1f42-42f0-a740-718aa37abb41" /><br>
If we combine the two hidden codes that I've highlighted in the red box above and decode them as charcode, we get the URL shown below.<br>
<img width="2552" height="1112" alt="image" src="https://github.com/user-attachments/assets/8503a901-bbd8-41e6-a86b-091093ab07ce" /><br>



2. What is the filename of the payload (include the extension)?<br>
<img width="1611" height="1000" alt="image" src="https://github.com/user-attachments/assets/c2f0ebbb-c9f9-4f6f-a034-5c4809af2674" /><br>
If we combine the two hidden codes that I've highlighted in the red box above and decode them as charcode, we get the file shown below.<br>
<img width="2556" height="1242" alt="image" src="https://github.com/user-attachments/assets/331ca4d6-d480-48ae-88c5-86ee911c7e28" /><br>



3. What method is it using to establish an HTTP connection between files on the malicious web server?<br>
<img width="1591" height="983" alt="image" src="https://github.com/user-attachments/assets/b454964c-d662-4e1e-9a76-a9520a14f891" /><br>
MSXML2.ServerXMLHTTP is a Windows connection object that allows a VBA macro to send an HTTP request to the internet and retrieve data/files from an attacker's server.<br>
<img width="2558" height="1228" alt="image" src="https://github.com/user-attachments/assets/9a3ba5e9-bfc3-403b-b2d4-ae793cc82631" /><br>


4. What user-agent string is it using?<br>
<img width="1599" height="983" alt="image" src="https://github.com/user-attachments/assets/d3b58884-5c56-4de4-9f23-a1d64d078cfe" /><br>
<img width="2560" height="1235" alt="image" src="https://github.com/user-attachments/assets/f701e3fd-7566-42e8-8973-6f5b69a6cf4a" /><br>


5. What object does the attacker use to be able to read or write text and binary files?<br>
<img width="1592" height="989" alt="image" src="https://github.com/user-attachments/assets/3f15f403-4d36-44e1-a0c1-1c3ca218ab48" /><br>
ADODB.Stream is a Windows object used in VBA for reading and writing file data. In this challenge, the attacker's goal is to first download the malicious file (payload) from their own server using an object like MSXML2.ServerXMLHTTP, and then convert this downloaded data into a file on the computer. Because data from the internet isn't a directly executable file, it needs to be written to disk. The attacker uses ADODB.Stream to retrieve the downloaded binary data (e.g., an .exe file) and save it to the computer using operations like SaveToFile. Therefore, in the attack chain, MSXML2.ServerXMLHTTP handles the task of retrieving the file from the internet, and ADODB.Stream handles the task of writing the file to the computer.<br>
<img width="1540" height="867" alt="image" src="https://github.com/user-attachments/assets/04d29c27-9b3e-4bff-a849-573079c37045" /><br>


6. What is the object the attacker uses for WMI execution? Possibly they are using this to hide the suspicious application running in the background.<br>
<img width="1603" height="1020" alt="image" src="https://github.com/user-attachments/assets/a2b452ff-2720-4469-896e-37f5a3411253" /><br>
winmgmts:\\.\root\cimv2:Win32_Process is an object that allows a VBA macro to connect to the Windows WMI service and create a new process (for example, the downloaded malware.exe). An attacker uses this to run malware more stealthily and to exploit Windows' own administrative features.<br>
<img width="1712" height="1015" alt="image" src="https://github.com/user-attachments/assets/3aebf2be-31cf-4c51-b30e-bdbdbb0cae45" /><br>





