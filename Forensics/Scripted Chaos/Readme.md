# 🌀 Scripted Chaos : File Carving and decoding !
🔍 Challenge Overview
We are given a capture file and need to analyze HTTP packets to uncover the hidden flag.

## 📌 Steps to Solve

#### The capture file reveals two files:
- important.js
- no_need
### 2️⃣ Investigating the no_need File
- Opening no_need, we find that it contains JFIF (JPEG) data but appears incomplete. Instead of making assumptions, we move on to analyzing important.js.

### 3️⃣ Analyzing important.js
- Inside important.js, we find a lot of garbage data, but among it, there is hex-encoded text.
-Decoding the hex reveals obfuscated text.
- After thorough inspection, we identify it as a VBE-encoded (VBScript Encoded) file.

### 4️⃣ Deobfuscating the VBE Script
- We save the extracted data as a .vbe file and use an online VBE deobfuscator: 🔗 https://master.ayra.ch/vbs/vbs.aspx

- This successfully decrypts the VBE into a VBS script.

### 5️⃣ Decoding the VBS Script
- The VBS script is now readable but still unclear. We notice Base64-encoded data inside it.
- Concatenating and decoding the Base64 data reveals a PowerShell script.
- Inside the PowerShell script, there is another layer of Base64-encoded data.

### 6️⃣ Extracting the JSON Data
- Decoding the Base64 inside the PowerShell script gives us a JSON file.
- The JSON file contains yet another Base64-encoded string.

### 7️⃣ Reconstructing the JPEG File
- After decoding the final Base64 string, we get random data.
- Remembering the no_need file from earlier, we suspect this is the missing part of the JPEG file.
- We concatenate this data to no_need using a hex editor and reconstruct the image.

### 8️⃣ Finding the Pastebin Link
- The reconstructed image contains a Pastebin link: 🔗 https://pastebin.com/KJbQFy3z
- Accessing the link gives us the flag.

### 9️⃣ Extracting flag.tar
- We submit the flag , but it is still incorrect ??? 
- Oh ! We almost forgot that file we were given earlier flag.tar protected with a password, so this flag might be the password of the tar file !
- Using the flag from Pastebin as the password, we extract flag.tar.
- Inside, we find flag.txt, which contains the final flag. 
