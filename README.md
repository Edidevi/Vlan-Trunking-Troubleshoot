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


## Troubleshoot 1 
<img width="523" height="266" alt="image" src="https://github.com/user-attachments/assets/30317f1c-37d8-4a1f-a08d-53382cb93466" />

Firstly, I avoided using hover since it is not reflective of a real world scenario.

- **Physical:** I Checked at physical layer: I could see that all connections where green- meaning link is active and up on both ends
- **Layer 2:**  I then logged into switch 1 and typed "Show VLAN" to idenitfy any VLAN abnormalities
- **Problem found:** In the results this message came up `%CDP-4-NATIVE_VLAN_MISMATCH: Native VLAN mismatch discovered on FastEthernet0/1 (10), with S2 FastEthernet0/1 (1)`. I also saw that for the supposed trunk port, port `fa0/1` was explicity under VLAN 10, relfelcting the behaviour of an access port.
- I then typed `show interfaces fa0/1 switchport` to confirm my suspicions. Under both admin and operational access rows, it was listed as static accesss.
- I entered global conf mode, entered `interface fa0/1`, then used `switchport mode trunk`. After running `show interfaces fa0/1 switchport` again, static access had now been replaced by trunk.
- I typed `Show vlan` and behold, `fa0/1` was no longer explicitly in VLAN 10. The native VLAN mismatch command also was no longer present.
- I then pinged both opposing computers PC1 to PC3 and PC2 to PC4 and both pings were 100% succesfull.

## Troubleshoot 2  
<img width="523" height="266" alt="image" src="https://github.com/user-attachments/assets/30317f1c-37d8-4a1f-a08d-53382cb93466" />

Firstly, I avoided using hover since it is not reflective of a real world scenario.

- **Physical:** I Checked at physical layer: I could see that all connections where green- meaning link is active and up on both ends
- **Layer 2:**  I then logged into switch 1 and typed "Show VLAN" to idenitfy any VLAN abnormalities
- **Problem found:** In the results this message came up `%CDP-4-NATIVE_VLAN_MISMATCH: Native VLAN mismatch discovered on FastEthernet0/1 (10), with S2 FastEthernet0/1 (1)`. I also saw that for the supposed trunk port, port `fa0/1` was explicity under VLAN 10, relfelcting the behaviour of an access port.
- I then typed `show interfaces fa0/1 switchport` to confirm my suspicions. Under both admin and operational access rows, it was listed as static accesss.
- I entered global conf mode, entered `interface fa0/1`, then used `switchport mode trunk`. After running `show interfaces fa0/1 switchport` again, static access had now been replaced by trunk.
- I typed `Show vlan` and behold, `fa0/1` was no longer explicitly in VLAN 10. The native VLAN mismatch command also was no longer present.
- I then pinged both opposing computers PC1 to PC3 and PC2 to PC4 and both pings were 100% succesfull.


 

 

