# Echoed Mysteries - Wireshark Analysis  

## 🕵️‍♂️ Challenge Overview  
In this challenge, we need to analyze network traffic using **Wireshark** to extract a flag hidden within **Destination Unreachable** packets.  

## 🔧 Tools Required  
- **Wireshark** (Packet Analysis)  

## 📌 Steps to Solve  

1. **Open the Capture File**  
   - Load the provided `.pcap` file in **Wireshark**.  

2. **Apply a Filter for Destination Unreachable Packets**  
   - Use the following Wireshark display filter to locate **ICMP Destination Unreachable** packets:  
     ```
     icmp.type == 3 && frame.len ==61
     ```
   - This will filter out all **ICMP Type 3 (Destination Unreachable)** messages with length 61 .  

3. **Inspect the Data Section**  
   - Click on an **ICMP Destination Unreachable** packet.  
   - Expand the **Packet Details** pane.  
   - Look inside the **data** section—sometimes, part of the original request or additional data is included in these packets.  

4. **Extract the Hidden Flag**  
   - Extract the flag in the Data section 
