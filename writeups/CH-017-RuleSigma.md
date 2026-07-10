# Learn Sigma
Your organization has detected a ransomware infection on one of its critical systems, and it is imperative that you address this issue immediately. This type of malware searches for valuable files, such as sensitive documents and configuration files, and encrypts them using a strong encryption algorithm.

The investigation has revealed that the ransomware may have used the Windows utility bitsadmin.exe to download additional malicious payloads or communicate with its command-and-control (C2) server.

Your task is to carefully review the Sigma rule, answer the related questions, and understand how different rule sections (selection, condition, fields, tags, logsource) work together to detect malicious activity.

File Location: C:\Users\LetsDefend\Desktop\ChallengeFile\proc_creation_win_bitsadmin_download.yml

---

## 1. Which executable file was specifically targeted by this Sigma rule?

In the selection_img block of the rule (lines 22-24) the conditions Image|endswith: '\bitsadmin.exe' and OriginalFileName: 'bitsadmin.exe' are defined — so the answer is bitsadmin.exe. Authenticating the same target with these two separate fields provides additional assurance against the attacker's "masquerading" technique, as the OriginalFileName metadata remains constant from compilation time, even if the file is renamed (even if the name in the Image path changes).

<img width="1478" height="853" alt="image" src="https://github.com/user-attachments/assets/90023b0e-92de-49f4-bd01-6aaa9dbb7930" />

## 2. What command-line option is used to indicate a file transfer in this rule?

In the selection_cmd block on lines 25-26, CommandLine|contains: ' /transfer ' is defined. The /transfer parameter is the main command that tells bitsadmin.exe to create a new BITS transfer job; this job is the kernel instruction that starts the file download/upload process in the background, and it is where the rule answers the question "is a transfer operation taking place?".

<img width="1667" height="853" alt="image" src="https://github.com/user-attachments/assets/9fc397ae-2ab1-43f2-8c8e-35d061c6a567" />

## 3. What logical expression in the condition field combined the criteria to trigger this rule?

Line 33 states `condition: selection_img and (selection_cmd or all of selection_cli_*)`. This means that the process must be bitsadmin.exe (selection_img – mandatory), and either the `/transfer` command must be present (selection_cmd), or all conditions in the `selection_cli_1` and `selection_cli_2` blocks must be met simultaneously (all of selection_cli_*, i.e., both `/create+/addfile` and `http` must be present at the same time). This is a hybrid AND/OR logic that flexibly captures different bitsadmin usage scenarios (the classic `/transfer` syntax or the step-by-step `/create→/addfile` syntax) in a single rule.

<img width="1360" height="863" alt="image" src="https://github.com/user-attachments/assets/666b8b21-a864-4342-85fb-1d5513624337" />

## 4. Which specific field did this rule capture that shows the command being executed?

The `fields:` section (lines 34-36) lists the `CommandLine` and `ParentCommandLine` fields. The answer is specifically the `CommandLine` field, which indicates the command itself — this field contains all the parameters used when bitsadmin.exe was executed (which URL it was downloaded from, which file it was downloaded from) and is the most critical data source allowing the SOC analyst to understand the attacker's intent when examining the alarm.

<img width="1666" height="864" alt="image" src="https://github.com/user-attachments/assets/df89d4ae-c930-458a-bfc2-2db274a9ddd9" />

## 5. Which single ATT&CK tactic tag is listed first in this rule?

The tags list on lines 12-17 starts with `attack.defense_evasion`. This indicates that because bitsadmin.exe is a signed and legitimate Windows tool, attackers are aiming to evade antivirus/EDR products (evade defenses) by using this tool; it is followed by `attack.persistence`, `attack.t1197` (technical), `attack.s0190` (tool/software reference), and `attack.t1036.003` (masquerading sub-technique), but the first tactic is `defense evasion`.

<img width="1589" height="829" alt="image" src="https://github.com/user-attachments/assets/8042cbe9-552f-44ce-b273-77c1bb8d4757" />

## 6. What is the primary category of events that this Sigma rule was written to monitor?

In the logsource section on lines 18-20, category: process_creation and product: windows are defined. This means the rule monitors newly started processes on Windows (typically fed from Sysmon Event ID 1 or Windows Security Event ID 4688 logs) — that is, it identifies which program is run with which command line on the system.

<img width="1495" height="858" alt="image" src="https://github.com/user-attachments/assets/dd7afc38-3de0-4ba9-96ab-dd984435da0b" />

## 7. What specific command-line argument did this rule look for to identify HTTP-based downloads?

In the selection_cli_2 block on lines 31-32, CommandLine|contains: 'http' is defined. This condition checks whether the URL passed in the command line is a web address starting with http:// or https://; thus, the rule specifically targets the scenario of downloading files from the internet, not harmless uses of bitsadmin such as local file copying — which is the typical way ransomware pulls its second-stage payload.

<img width="1490" height="832" alt="image" src="https://github.com/user-attachments/assets/8655e8aa-f0d6-4178-bf54-380acf65733c" />

## 8. Which command-line option must be present to create a new transfer using bitsadmin?

In the selection_cli_1 block on lines 28-30, under CommandLine|contains, the `/create` and `/addfile` statements are listed together. The `/create` parameter creates a new BITS job (transfer task), while `/addfile` specifies which file (source URL and destination path) will be added to this job; when these two parameters are used together (as a step-by-step equivalent of the classic `/transfer` command), a new file transfer is defined, and the rule captures this alternative usage through the selection_cli_* blocks.

<img width="1610" height="822" alt="image" src="https://github.com/user-attachments/assets/fa36a51f-9480-4dd2-a6ee-5a949c379b5f" />
