# Echoed Mysteries - Wireshark Analysis  

## 🕵️‍♂️ Challenge Overview  
In this challenge, we need to analyze network traffic using **Wireshark** .

## 📌 Steps to Solve  


1. **Apply a Filter for Destination Unreachable Packets**  
   - Use the following Wireshark display filter to locate **ICMP Destination Unreachable** packets:  
     ```
     icmp.type == 3 && frame.len ==61
     ```
   - This will filter out all **ICMP Type 3 (Destination Unreachable)** messages with length 61 .  

2. **Inspect the Data Section**  
   - Click on an **ICMP Destination Unreachable** packet.  
   - Expand the **Packet Details** pane.  
   - Look inside the **data** section—sometimes, part of the original request or additional data is included in these packets.  

3. **Extract the Hidden Flag**  
   - Extract the flag in the Data section 
