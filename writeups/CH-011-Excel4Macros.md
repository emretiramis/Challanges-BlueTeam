# Excel 4.0 Macros

One of the employees has received a suspicious document attached in the email. When the e-mail flow is examined, it is seen that there is a suspicious Office file. Employees forward the email to the security team for analysis.

When L1 analysts scan the suspicious file with several different scanning tools, they see that it does not contain VBA macros. Since the file format is similar to phishing, they forwarded the suspicious Office file to you for detailed analysis.

** Since the 2nd payload download addresses are closed, the 2nd payload is in the zip. Please start your analysis from the Office file.

Malicious Office Document (Zip password: infected): /root/Desktop/ChallengeFiles/11f44531fb088d31307d87b01e8eabff.rar

---
Basic analysis

<img width="1651" height="926" alt="image" src="https://github.com/user-attachments/assets/be4810d1-62ce-4d10-a567-aab49ac8aad3" />

I used file command,

<img width="1624" height="388" alt="image" src="https://github.com/user-attachments/assets/2c7775a4-d441-4891-a61a-78d108df0213" />

Lets check the hash.

<img width="2560" height="1147" alt="image" src="https://github.com/user-attachments/assets/58c15378-c110-46f7-a7a2-f2262174fa12" />


seems malicious.

<img width="1635" height="626" alt="image" src="https://github.com/user-attachments/assets/e2721729-e1cc-4c86-9712-ae5ffa3a6865" />

I used command ```oleid research-1646684671.xls``` and tool says file hasnt macro. 

<img width="823" height="531" alt="image" src="https://github.com/user-attachments/assets/19c334d1-d41c-4b70-b08d-bfefa53e2c5b" />

olebva tool says same.

<img width="821" height="533" alt="image" src="https://github.com/user-attachments/assets/46881a06-d66f-4dc0-a5d1-b1ef63ce9dbd" />

I want to use another tool for XLM based detection.

<img width="1683" height="854" alt="image" src="https://github.com/user-attachments/assets/ad443438-1f34-493d-a6eb-4f5a79566dfe" />


1. Attackers use a function to make the malicious VBA macros they have prepared run when the document is opened. What do attackers change the cell name to to make Excel 4.0 macros work to provide the same functionality?

auto_open

In XLM macros, automatic execution is achieved by using a defined name named "Auto_Open"—created within the Name Manager (Defined Names)—instead of the VBA `Auto_Open()` function. This name points to the initial cell containing the macros (e.g., `Macro1!A1`), and Excel automatically executes the XLM commands in that cell when the document is opened. Therefore, during analysis, it is essential to examine not only VBA macros but also Defined Names and the cells to which they refer.

<img width="1624" height="677" alt="image" src="https://github.com/user-attachments/assets/1e639168-4ac8-40ae-8c04-b4d341de588e" />


2. What is the address of the first cell where Excel 4.0 macros will run in the malicious Office document you are analyzing? (Example: {doc1!ab3})

In XLM macros, the first cell to execute when the document is opened is the one referenced by the defined name "Auto_Open" (e.g., Sheet1!A1). During analysis, identifying this cell allows for a step-by-step trace of the commands that initiate the macros, how execution branches to subsequent cells, and how malicious behavior is triggered. Therefore, identifying the initial execution cell serves as the starting point for analyzing the macro's execution chain.
points to a specific cell (row 7, column BA) in the worksheet Doc4. That cell (BA7) contains the macros that will be executed automatically.
<img width="849" height="439" alt="image" src="https://github.com/user-attachments/assets/4d8e99ae-99f0-43b9-b052-66bf232135bf" />


3. Which function is used to start a process in the operating system in the document you are analyzing?

The purpose of this problem is to teach how to identify the function used in Excel 4.0 (XLM) macros to launch operating system-level commands or processes. The most commonly used function for this purpose in XLM macros is EXEC(). The EXEC() function enables the execution of malicious commands by running tools such as cmd.exe, powershell.exe, regsvr32.exe, or other system utilities. Therefore, examining EXEC() calls during analysis is critical for determining which process the attacker initiated and which payload or LOLBAS tool was employed in the subsequent stage.
EXEC
<img width="623" height="41" alt="image" src="https://github.com/user-attachments/assets/9bc90fdd-3943-4f9d-9510-e60c10faaca0" />


4. Which LOLBAS tool was used in the Excel 4.0 macros you analyzed? (Format: {xxxx.exe})

The objective of this problem is to understand how attackers conceal malicious activities within Excel 4.0 (XLM) macros by leveraging built-in Windows tools—specifically LOLBAS (Living Off The Land Binaries And Scripts). In this scenario, rather than directly executing a malicious binary, the XLM macro triggers a legitimate Windows executable (such as regsvr32.exe, mshta.exe, or rundll32.exe) via the EXEC() function. Consequently, the attack traffic and process chain appear to the system as a "legitimate process," making detection difficult. Therefore, identifying the native Windows tools invoked within the macro is critical for detecting the specific LOLBAS technique employed.

<img width="621" height="43" alt="image" src="https://github.com/user-attachments/assets/26c67eab-6ad6-4c31-b1e5-97668784c588" />


5. What is the name of the registered DLL?

The objective of the problem is to teach how to identify the specific DLL used to facilitate persistence and the execution of a second-stage payload within Excel 4.0 (XLM) macros. Attackers typically use the `EXEC()` function to invoke a LOLBAS tool (such as `regsvr32.exe`) to execute or register a remote or local DLL file on the system. The name of the DLL involved in this step appears—either in plain text or in an obfuscated form—within the macro's command-line arguments and represents a critical component of the attack's payload chain. Consequently, accurately extracting the DLL name is a fundamental DFIR step for understanding the malicious component loaded by the attacker and the code slated for execution in the subsequent stage.

<img width="623" height="42" alt="image" src="https://github.com/user-attachments/assets/4c27ddc3-3605-4ae7-83ac-255d47aa98d7" />


6. What is the username that made the last change to the malicious document?

<img width="1576" height="120" alt="image" src="https://github.com/user-attachments/assets/e7735113-0025-42d9-ae9e-000b9b518b31" />
