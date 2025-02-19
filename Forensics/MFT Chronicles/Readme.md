# 🔍 MFT Chronicles - Unveiling the Attacker’s Footprints
###  What is MFT?
- The Master File Table (MFT) is a critical component of the NTFS file system. It keeps records of all files and directories on an NTFS-formatted disk, including their names, timestamps, attributes, and data location. Every file has a corresponding entry in the MFT, making it a valuable forensic artifact for investigating file activities, deletions, and modifications.

## 📌 Steps to Solve
- We use MFTEcmd.exe, a tool from Eric Zimmermann, to parse the MFT file into a CSV file for easy searching and analysis.

### 🔹 Command to extract MFT data:
```
MFTEcmd.exe -f "$mft" --csv "file.csv"
```
This generates file.csv, containing detailed metadata about all files recorded in the MFT.

###  1. Identify the malicious .lnk file.
Answer: malware.exe.lnk
- Explanation: .lnk files are Windows shortcut files. Attackers often use .lnk files to execute malicious payloads while disguising them as legitimate shortcuts. In this case, malware.exe.lnk likely points to an executable file used in the attack.

###  2. What was the last modification time of the malware?
Answer: 2025/01/28 20:33:27
- Explanation: The MFT records timestamps for file creation, modification, and last access. The modification timestamp tells us when the attacker last altered the malware file, which can help determine the timeline of the attack.

###  3. What was the malicious IP from where the attacker downloaded a malicious archive file?
Answer: 192.168.100.57
- Explanation: The attack involved downloading a malicious archive from an external server. The IP address 192.168.100.57 represents the source of the file, which can help track the attacker's infrastructure.

###  4. The attacker downloaded a tool used for detailed system activity. What was its name?
Answer: Sysmon
- Explanation: Sysmon (System Monitor) is a Windows system monitoring tool used to log detailed system activity. While it’s commonly used by defenders for security monitoring, attackers can also install Sysmon to track system activity for persistence, privilege escalation, or defense evasion.

###  5. What was the path of the server-side application run in Python?
Answer: /Users/Zyyz/Downloads/server.py
- Explanation: The attacker executed a Python script named server.py, located in the Downloads directory of the user Zyyz. This script likely acted as a malicious server or command-and-control (C2) infrastructure.

###  6. The attacker created a memory dump. What was its creation time?
Answer: 2025/01/23 09:51:16
- Explanation: Memory dumps are used to capture the contents of RAM for analysis. Attackers often create memory dumps to extract sensitive data like passwords, encryption keys, or session tokens. The timestamp tells us exactly when the attacker performed this action.

###  7. The attacker downloaded a script for privilege escalation. Name it.
Answer: UAC_BYPASS.ps1
- Explanation: UAC Bypass refers to techniques used to escalate privileges and gain administrative access on a Windows system. The PowerShell script UAC_BYPASS.ps1 likely exploited User Account Control (UAC) mechanisms to execute high-privilege commands without user consent.

###  8. Which executable did the attacker use to retrieve security keys?
Answer: mimikatz.exe
- Explanation: Mimikatz is a well-known post-exploitation tool used to extract credentials, security keys, and NTLM hashes from memory. It allows attackers to perform pass-the-hash, pass-the-ticket, and other credential theft techniques.

#  Conclusion
### By carefully analyzing the MFT records, we successfully reconstructed the attack timeline, identified key artifacts, and retrieved forensic evidence. This highlights the importance of MFT analysis in incident response and digital forensics. 🚀
