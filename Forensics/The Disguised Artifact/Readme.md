# 🕵️‍♂️ The Disguised Artifact
🔍 Challenge Overview
We are given a file named FLAG.hta, but it appears to be corrupted. Our task is to analyze and repair the file to uncover the hidden flag.

## 📌 Steps to Solve
### 1️⃣ Identify the True File Type
First, check the file type using the terminal:
file FLAG.hta
The output is data, indicating that the file type is not recognized. This suggests the file is corrupted or disguised. Given the .hta extension, it might be an HTML Application file, but further analysis is needed.

### 2️⃣ Analyze the File with a Hex Editor
Open the file using a hex editor like HxD or any similar tool. Upon inspection, you notice the word "PowerISO" repeated multiple times. This is a strong indication that the file is actually an ISO file, but the extension has been intentionally changed to .hta to disguise it.

### 3️⃣ Fix the File Corruption
To fix the corruption, you need to correct the file header. ISO files typically start with the bytes CD001. Replace the incorrect header bytes with CD001 using the hex editor. Save the changes, and you should now have a properly formatted ISO file.

### 4️⃣ Mount the ISO File
You can mount the ISO file using the terminal or a tool like PowerISO.

On Linux, use the following command:
```
sudo mount -o loop FLAG.iso /mnt/iso
```
On Windows, use a tool like PowerISO or WinCDEmu to mount the ISO file.

### 5️⃣ Fixing the Corrupted Image
After mounting the ISO, you will find a PNG file inside. This PNG file is also corrupted. To fix it:

Open the PNG file in a hex editor.

Ensure the PNG header is correct: 89 50 4E 47 0D 0A 1A 0A.

Locate the IEND chunk and the IDAT chunk. Swap these chunks if they are in the wrong order.

Save the changes.

### 6️⃣ Recover the Flag
Once the PNG file is fixed, open it to reveal the hidden flag.
