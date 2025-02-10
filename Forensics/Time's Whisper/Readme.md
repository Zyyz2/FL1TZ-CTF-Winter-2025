# ⏳ Time's Whisper - Decoding the Hidden Message
🔍 Challenge Overview
We are given a capture file and need to analyze ICMP packets to uncover a hidden flag.

## 📌 Steps to Solve
### 1️⃣ Inspecting ICMP Packets
Using Wireshark, we analyze the capture file and observe that the TTL (Time-To-Live) values in the ICMP request packets are changing.

### 2️⃣ Identifying a Pattern
We notice that the TTL values range between 64 and 67. Converting these values to binary, we observe that only the two least significant bits (LSBs) are changing.

This suggests that the flag is encoded as the concatenation of the last two significant bits from each ICMP request packet.

### 3️⃣ Filtering Relevant Packets
To isolate the necessary packets, apply the following Wireshark display filter:
icmp && !(frame.len == 60) && icmp.type == 8

icmp → Captures only ICMP packets.
!(frame.len == 60) → Excludes packets with a frame length of 60 (common for irrelevant packets).
icmp.type == 8 → Filters only ICMP request packets.

### 4️⃣ Extracting the Flag
To automate the extraction process, we write a Python script that:

Reads the TTL values from the filtered ICMP request packets.
Extracts the last two significant bits from each value.
Concatenates the bits to form a binary string.
Converts the binary string to ASCII text to reveal the flag.
