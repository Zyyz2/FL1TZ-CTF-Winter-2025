# 🕵️ RAMDive - Memory Forensics Investigation

- Special thanks to my partners in cyber-crime, Bitraven and Ghr4b, for inspiring this chaotic, malware-filled challenge. Without you, I wouldn’t have had the pure joy of digging through memory dumps and malware. You two are the real MVPs! 😎💻

## Background: You’ll want to read this for the full ride and to enjoy the writeup !!!

- This challenge is based on a real incident: my friend, Bitraven (the crypto wizard behind FL1TZ CTF), managed to turn his PC into a digital disaster when malware—yep, from Cloudflare of all places—came in and completely wrecked it.
- As a lover of malware and trojans particularly i was inspired by this iconic malware so, this got me thinking: “What if I turn this chaos into a memory forensics challenge?” So, we did the most logical thing ever.
- Bitraven, extracted the malicious executable, and bam, I dove deep into a memory forensics investigation. What did we find? A network trojan with backdoor functionality—basically a little digital gremlin that pokes around your system, steals everything, and calls home to its attacker in Amsterdam. Classic, right? 😈

## 🚨 Malware Analysis Summary
- The malware is a network-based remote access trojan (RAT).
- It carves the file system, opens a TCP connection, and exports user files.
- It specifically targets Bitcoin wallets and temporary files.
- It loads jsc.exe, a .NET-compiled payload, as part of its attack chain.
- 🛠 Forensic Analysis using Volatility 3
- To analyze the memory dump, we used Volatility 3, a powerful memory forensics tool. Below are the findings from different Volatility plugins, along with explanations.

## Quick explanation about volatility tool :
- Volatility is like your digital detective—but instead of a magnifying glass, it uses RAM dumps!
- It lets you peer into the chaos of a computer's memory, uncovering everything that was happening while the system was in action.
- Think of it like digging through the memory drawers of your computer to find the juicy details: what processes were running, what secret network connections were happening, and even what your computer was doing behind your back (like running malware!). 
- Using Volatility, you can find out if a malicious process was lurking in the shadows, what user accounts were snooping around, and even the last-minute panic activity before things went south.
- Basically, it’s like CSI for your computer's brain—but instead of solving murders, you're catching cybercriminals! 


## 📌 Key Findings
### 🔹 1. Path of the malicious process
- Answer: C:\Windows\Microsoft.NET\Framework\v4.0.30319\jsc.exe
```
python3 vol.py -f memdump.mem windows.cmdline
```
📝 Explanation:
- The command line of the running process (jsc.exe) shows that it resides inside the .NET Framework directory, which is suspicious.
This suggests that the malware is abusing .NET components to execute its payload.

### 🔹 2. Find the SID and the username the malware was running under
- Answer: S-1-5-21-1261973874-1698488304-1739942524-1001_Zyyz
```
python3 vol.py -f memdump.mem windows.getsids.GetSIDS
```
📝 Explanation:
- SID (Security Identifier) identifies the user account under which the process was executed.
- Here, the malware ran under the user Zyyz, meaning the attacker executed it with user-level privileges.

### 🔹 3. NTLM hash of the user
- Answer: 7eb59e280c8fbc878955e0269cbe2ae9
```
python3 vol.py -f memdump.mem windows.getsids.hashdump
```
📝 Explanation:
- The NTLM hash represents the user's password hash stored in memory.
- Attackers can use Pass-the-Hash (PtH) attacks to authenticate without knowing the plaintext password.

### 🔹 4. City of the attacker
- Answer: Amsterdam
```
python3 vol.py -f memdump.mem windows.netscan
```
- lookup (for IP related to jsc.exe)
📝 Explanation:
- The malware established a network connection to an attacker-controlled server.
- Reverse lookup of the IP address associated with jsc.exe revealed that it was linked to a server in Amsterdam.

### 🔹 5. Ending address of the executable
- Answer: 0xf8dfff
```
python3 vol.py -f memdump.mem windows.vadinfo
```
📝 Explanation:
- The ending address of jsc.exe in memory helps determine the memory range allocated to the executable.
- This is useful for identifying code injection or memory-resident malware.

### 🔹 6. Memory protection flag of the process
- Answer: PAGE_EXECUTE_WRITECOPY
```
python3 vol.py -f memdump.mem windows.vadinfo
```
📝 Explanation:
- This protection flag means that memory pages can be executed and modified.
- This is often used by malware to inject shellcode into processes.

### 🔹 7. The process manipulated a DLL for cryptographic functions
- Answer: \Windows\SysWOW64\cryptbase.dll
```
python3 vol.py -f memdump.mem windows.vadinfo
```
📝 Explanation:
- cryptbase.dll is a Windows DLL used for encryption and decryption functions.
- The malware likely used it to steal sensitive data like passwords, encryption keys, or Bitcoin wallets.

### 🔹 8. Name of the computer
- Answer: DESKTOP-AJM6HKU
```
python3 vol.py -f memdump.mem windows.envars
```
📝 Explanation:
- The malware retrieves the computer name to fingerprint the infected system.
- This is useful for the attacker to identify unique victims.

### 🔹 9. The process used a temporary variable. Give its path.
- Answer: C:\Users\Zyyz\AppData\Local\Temp
```
python3 vol.py -f memdump.mem windows.envars
```
📝 Explanation:
- Many malware strains drop temporary files in the AppData\Local\Temp directory.
- These are often deleted after execution to cover tracks.

### 🔹 10. Creation time of the process
- Answer: 2025-02-04 03:21:09 UTC
```
python3 vol.py -f memdump.mem windows.pstree
```
📝 Explanation:
- Knowing when the malware was created helps in timeline analysis.
- This timestamp tells us exactly when the malware was executed on the system.

### 🔹 11. The process interacted with a Windows user registry. Give its path.
- Answer: HKEY_USERS\S-1-5-21-1261973874-1698488304-1739942524-1001\SOFTWARE\MICROSOFT\WINDOWS NT\CURRENTVERSION
```
python3 vol.py -f memdump.mem windows.handles
```
📝 Explanation:
- This registry path stores system settings.
- Malware often modifies registry keys for persistence or evasion.

### 🔹 12. The 9th privilege related to the process
- Answer: SeTakeOwnershipPrivilege
```
python3 vol.py -f memdump.mem windows.privileges.Privs
```
📝 Explanation:
- SeTakeOwnershipPrivilege allows a process to take ownership of files and objects.
- This can be used to bypass file access restrictions, allowing the malware to exfiltrate files.

### 🔹 13. The secret message hidden in memory
- Answer: FL1TZ{U_G0T_M3!}
```
python3 vol.py -f memdump.mem windows.strings | grep -i FL1TZ
```
📝 Explanation:
- Searching for ASCII and Unicode strings in memory led to discovering this flag inside the malware’s memory space.
- Likely an embedded challenge left by the attacker.

### 🔹 14. Number of threads related to the process
- Answer: 5
```
python3 vol.py -f memdump.mem windows.thrdscan
```
📝 Explanation:
- Threads indicate active execution paths within the process.
- More threads often mean parallel execution, typical for backdoors and network-based malware.

### 🔹 15. The process used a DLL for system calls
- Answer: ntdll.dll
```
python3 vol.py -f memdump.mem windows.dlllist | grep -i jsc.exe
```
📝 Explanation:
- ntdll.dll is a critical Windows DLL responsible for system calls and low-level OS interactions.
- Malware often hooks ntdll.dll to hide malicious activities.
