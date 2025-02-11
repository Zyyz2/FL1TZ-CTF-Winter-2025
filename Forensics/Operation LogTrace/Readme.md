# 🕵️‍♂️ Operation LogTrace: A Forensic Adventure
## 🔍 Challenge Overview
In this challenge, you'll need to investigate a cyber attack by analyzing Windows event logs. The task involves reviewing security logs, system logs, and PowerShell scripts to trace the actions of an attacker. The goal is to uncover what happened during the attack, from how the attacker got in to what they did and how they tried to cover their tracks. Special thanks to Stoura, my first-year professor, who inspired this whole challenge. 😎

## 💡 Task Background
### In this investigation, I executed several actions, including running PowerShell commands, opening HTTP and TCP connections, downloading malicious files, escalating privileges, viewing hashes, and even trying Pass-the-Hash. This is a deep dive into event logs, uncovering suspicious activities and malicious actions from the attack.

## 📝 Challenge Questions & Answers with Explanations
### 1. When was the new user created?
Answer: 2025-01-30 00:24:29

Log Type: Security logs (Event ID 4720)
Explanation: This is the exact timestamp when a new user account was created in the system, and you can find this in the Security logs. The event ID 4720 signifies a "user account created" event. Knowing when the attacker created a new user is important because it can show when they gained access to the system.
### 2. What is the credential key identifier for the new user?
Answer: 0x422304C2319BA5917BD4EC2B6A7E9FE61D95D1E773EE0886CE36BB51AFFDC22E

Log Type: Crypto-DPAPI Operation
Explanation: This is the unique identifier for the credential key associated with the new user. The DPAPI (Data Protection API) is used by Windows to encrypt sensitive information, like credentials. This key identifier is crucial for decrypting the credentials associated with the new user.
### 3. Name the script that the new user used for privilege escalation.
Answer: UAC_BYPASS.ps1

Log Type: PowerShell Operation (Event ID 4104 → Script Block Logging)
Explanation: This PowerShell script is used to bypass User Account Control (UAC), which is a common technique for privilege escalation. By executing this script, the attacker elevated their privileges to gain more control over the system.
### 4. Which private IP address encountered an error while attempting to connect to the network, as indicated in the event logs?
Answer: 10.0.2.15

Log Type: DHCP Logs (Event ID 1002)
Explanation: This IP address encountered an error while trying to connect to the network. It was logged in the DHCP logs, indicating that the machine had issues obtaining a valid IP address. This could signify a failed attempt to connect by a compromised device or an attacker’s device.
### 5. What's the socket IP used to download the malware?
Answer: 192.168.100.57:8000

Log Type: PowerShell Operation (Event ID 4104)
Explanation: The attacker used this IP address and port (192.168.100.57:8000) to download the malware to the compromised system. This is the IP address the malware was retrieved from, indicating an exfiltration point or a server controlled by the attacker.
### 6. Which registry key was modified to persist a backdoor in the system that refers to a specific user profile?
Answer: HKU\S-1-5-21-1261973874-1698488304-1739942524-1002\SOFTWARE\Microsoft\Windows\CurrentVersion\Run\Backdoor

Log Type: Event ID 13 in Sysmon (Filter with Malware)
Explanation: This registry key is used to ensure the persistence of the backdoor. It makes sure that the backdoor program runs every time the user logs in to the system. Attackers often modify the Run registry key to achieve persistence after gaining access.
### 7. What technique did the attacker use to gain access to a remote system by bypassing the need for a plaintext password?
Answer: Pass-the-Hash

Log Type: PowerShell Operation (Event ID 4104)
Explanation: In Pass-the-Hash attacks, attackers use the hash of a user’s password (instead of the actual plaintext password) to authenticate. This is a common technique when attackers steal password hashes from a system and use them to gain access to other systems or services.
### 8. Name the device Stoura was trying to connect to remotely but failed.
Answer: LAPTOP-3CU59SBJ

Log Type: Event ID 32784 (Windows Remote Management Error)
Explanation: Stoura tried to connect to this device remotely via Windows Remote Management (WinRM) but failed, which was logged in the event logs as an error. This failure could be a sign that the attacker was attempting to pivot or move laterally within the network but encountered an obstacle.
### 9. What is the primary domain of the attack, where the attacker exfiltrated sensitive files?
Answer: attackerserver.com

Log Type: PowerShell Operation (Event ID 4104)
Explanation: The attacker exfiltrated sensitive files to the domain attackerserver.com. This could be the attacker's server or a location where the stolen data is being uploaded. It's essential to know this domain to understand the scope of the data exfiltration.
### 10. Stoura used a tool to dump the user's passwords. What’s its process GUID?
Answer: {2f06b238-dd55-679a-7d08-000000000900}

Log Type: PowerShell Operation (Event ID 4104)
Explanation: This is the Process GUID of the tool Stoura used to dump passwords from the system. This GUID helps identify the exact process in the event logs, which is crucial for tracking what tools were used to compromise the system.
### 11. What is the content of the secret file?
Answer: FL1TZ{w4n3z8u9fgh32m2i}

Log Type: Event ID 15 in Sysmon
Explanation: The content of the secret file was uncovered as FL1TZ{w4n3z8u9fgh32m2i}. This is a flag typically used in CTF challenges to show that a specific file or operation was successfully accessed during the investigation.
### 12. What is the name of the ADS file created?
Answer: hidden_stream.txt

Log Type: Event ID 15 in Sysmon
Explanation: The attacker created an Alternate Data Stream (ADS) named hidden_stream.txt to hide files on the system. ADS is a feature in NTFS file systems that allows files to have multiple streams of data, making it easier to hide malicious content.
### 13. What’s the password inside the hidden file?
Answer: trustno1

Log Type: Event ID 15 in Sysmon
Explanation: The password stored inside the hidden file hidden_stream.txt was trustno1. This file might be a key to accessing other systems or data, and this password could be used by the attacker for further access or lateral movement within the network.
## 🛠️ Tools Used:
Event Logs Analysis: To trace the actions of the attacker by reviewing security logs, system logs, and PowerShell logs.
PowerShell Scripts: Investigating PowerShell scripts used for privilege escalation and downloading malware.
Hash Analysis: Understanding credential theft through pass-the-hash and identifying key password hashes.
Registry Monitoring: To identify persistence mechanisms set by the attacker through registry modifications.
## 🚨 What You'll Learn:
Log Forensics: How to analyze and trace attacker activity from event logs, focusing on user creation, privilege escalation, and credential dumping.
Credential Dumping & Pass-the-Hash: Learn how attackers use stolen password hashes to bypass authentication systems.
Persistence Mechanisms: Understand how attackers maintain access through registry modifications and hidden files.
# 🎉 Conclusion
### By investigating these event logs and analyzing the attacker’s actions, you’ll uncover the full scope of the attack, from privilege escalation to file exfiltration. It's like piecing together the mystery in a cyber-thriller, one event log at a time.

### Enjoy the challenge, and remember—trust no one, especially when dealing with hidden streams! 🕵️‍♂️

### May your logs be clean, your hashes cracked, and your investigations always successful! 😎
