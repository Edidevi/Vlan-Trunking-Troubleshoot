# Vlan-trunking-troubleshoot 
Cisco Packet Tracer lab exploring troublehsooting scenarios for 802.1Q trunking, access ports, and L2 network segmentation.

## 📌 Project Objective
The objective of this lab is to show my troubleshooting acumen for a VLAN topology


## 🛠️ Topology & Hardware Setup
- **Switches:** 2x Cisco Catalyst 2960 (S1 and S2)
- **Hosts:** 4x End Devices (PCs)
- **Inter-Switch Link:** Connected via `FastEthernet 0/1` on both switches using a **copper cross-over cable**.

## 🧠 Troubleshooting Steps
To troubleshoot, i made an effort to go layer by layer, as there is no poing pinging if you haven't checked for possible physcial layer issues


## Scenario 1 
TICKETID-2026-001: VLAN issue

<img width="523" height="266" alt="image" src="https://github.com/user-attachments/assets/30317f1c-37d8-4a1f-a08d-53382cb93466" />

Firstly, I avoided using hover since it is not reflective of a real world scenario.

- **Physical:** I Checked at physical layer: I could see that all connections where green- meaning link is active and up on both ends
- **Layer 2:**  I then logged into switch 1 and typed "Show VLAN" to idenitfy any VLAN abnormalities
- **Problem found:** In the results this message came up `%CDP-4-NATIVE_VLAN_MISMATCH: Native VLAN mismatch discovered on FastEthernet0/1 (10), with S2 FastEthernet0/1 (1)`. I also saw that for the supposed trunk port, port `fa0/1` was explicity under VLAN 10, relfelcting the behaviour of an access port.
- I then typed `show interfaces fa0/1 switchport` to confirm my suspicions. Under both admin and operational access rows, it was listed as static accesss.
- I entered global conf mode, entered `interface fa0/1`, then used `switchport mode trunk`. After running `show interfaces fa0/1 switchport` again, static access had now been replaced by trunk.
- I typed `Show vlan` and behold, `fa0/1` was no longer explicitly in VLAN 10. The native VLAN mismatch command also was no longer present.
- I then pinged both opposing computers PC1 to PC3 and PC2 to PC4 and both pings were 100% succesfull.

## Scenario 2  
TICKETID-2026-001: PC1 can reach PC3 but PC2 can't reach PC4

<img width="523" height="266" alt="image" src="https://github.com/user-attachments/assets/30317f1c-37d8-4a1f-a08d-53382cb93466" />


- **Physical:** I Checked at physical layer: I could see that all connections where green- meaning link is active and up on both ends. Because of this, I moved on to layer 2.
- **Layer 2:**  I then logged into switch 1 and typed "Show VLAN" to idenitfy any VLAN abnormalities. THe right ports were assigned to the right VLAN & the trunk port was not explicitly aligned to a VLAN like the last troubleshoot.
- MAC addresses are another layer 2 feature, so i pinged PC3 from PC1, then typed `show mac-address-table` on both switches to check if the mac address of VLAN 20 (PC2 & PC4) would show up, to ensure that the packets crossed both switches.

<img width="40%" height="163" alt="image" src="https://github.com/user-attachments/assets/fd5217cc-ed92-42a0-821a-e12d610222fc" />
<img width="40%" height="164" alt="image" src="https://github.com/user-attachments/assets/0dbb47ef-8442-4df5-8cda-0683fd799854" />

- This showed that the ping was getting to switch 1, but are not getting to switch 2. I then started to think, what could possible be happening at switch 2?
- Even though the green on the layout meant that there should not be any interface issues, i typed `show ip interface brief` and `show vlan` to see if there were any layer 1 and 2 issues that i missed
- It took me a while, I started checking the pcs themselves and reattaching lines, but in the end, I went back to what had been proven:
- PC1 and PC3 could ping between each other, the ports were assigned to the right VLANs and the ping was getting to S1 but not S2, so it HAD to be a swtich problem
- So i typed `show running-config` to see everything type in the switch

<img width="353" height="277" alt="image" src="https://github.com/user-attachments/assets/03b193ff-82ee-4540-ab7f-c39bed0142fb" />

And i saw the line `switchport trunk allowed vlan 10`. This explains the issue, because if you expliictly allow VLAN 10, it will blaock every other VLAN other than VLAN 10. So i entered the sub configuration `interface fa0/1`, and entered `switchport trunk allowed vlan add 20` to add vlan 20`. 
After writing this to memeory, i was able to successfully ping PC4 from PC2.





 

