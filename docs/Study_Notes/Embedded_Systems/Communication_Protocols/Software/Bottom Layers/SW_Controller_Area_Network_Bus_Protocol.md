---
id: SW_Controller_Area_Network_Bus_Protocol
aliases:
  - Controller Area Network Bus Protocol
tags: []
---
- [ ] 
# Controller Area Network Bus Protocol

## CAN Bus Types

These are lower level protocols which focus on the Physical and Data Link

CAN Physical Data Frame:

![[00_CAN_Data_Frame.png]]

### Types:
- CAN 2.0A: 11bit ID, 8 byte Data, 1 Mbps
- CAN 2.0B: 11/29bit ID, 8 byte Data, 1 Mbps
- CAN FD: 11/29bit ID, 64 byte Data, 10+ Mbps
- CAN XL: 11/29bit ID, 1500+ byte Data, 10+ Mbps

## CAN Bus Protocols

These are higher level protocol that allow you to use a standard that is heavily adopted by an industry to make the development quicker and easily swapable.

Types:
- DeviceNet: Industrial I/O
- SAE J1939: Commercial Vehicles
- CANOpen: [[SW_CANOpen]]
- CANOpen FD: All Applications

---

## CANOpen FD

USDO Features:

- Max 56 byte single request
- Max 60 bytes per segment
- Fully meshed

PDO:

- Max 64 bytes

---

## Resources

### Theory


[Texas Instruments - Introduction to the Controller Area Network (CAN)](https://www.ti.com/lit/an/sloa101b/sloa101b.pdf)

[Typhoon HIL Documentation - CAN Bus protocol](https://www.typhoon-hil.com/documentation/typhoon-hil-software-manual/References/can_bus_protocol.html)

[Can someone explain how to use CANBUS protocol?](https://stackoverflow.com/questions/33569507/can-someone-explain-how-to-use-canbus-protocol)

[CAN Bus: A Beginners Guide Part 1](https://youtu.be/YBrU_eZM110?si=aA5Myy0UNug5AIS1)

[CAN Bus: A Beginners Guide Part 2](https://youtu.be/z5CVljiLhvc?si=EGW0jlv_eU0BY4Zc)

[Arm Semiconductors - CAN Primer: Creating your own Network](https://grouper.ieee.org/groups/msc/upamd/pub_docs/CAN.pdf)



