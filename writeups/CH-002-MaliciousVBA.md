## Malicious VBA
One of the employees has received a suspicious document attached in the invoice email. They sent you the file to investigate. You managed to extract some strings from the VBA Macro document. Can you refer to CyberChef and decode the suspicious strings?

Please, open the document in Notepad++ for security reasons unless you are running the file in an isolated sandbox.

Malicious Macro: /root/Desktop/ChallengeFiles/invoice.vb


Hackers typically email an Office file that appears to be a fake invoice, CV, or report; when the user opens the file and clicks "Enable Macro/Content," the VBA code runs. This VBA code connects to the internet in the background, downloads a malicious payload from a command-and-control server controlled by the attacker, saves it to disk, and executes it. It does this by using HTTP connections, file-writing objects like ADODB.Stream, and Windows features like WMI.
