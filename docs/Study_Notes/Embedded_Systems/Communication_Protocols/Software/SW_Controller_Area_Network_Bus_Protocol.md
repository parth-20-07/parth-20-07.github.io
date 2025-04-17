---
id: SW_Controller_Area_Network_Bus_Protocol
aliases:
  - Controller Area Network Bus Protocol
tags: []
---

# Controller Area Network Bus Protocol

## CAN Bus Types

These are lower level protocols which focus on the Physical and Data Link

CAN Physical Data Frame:

![[00_CAN_Data_Frame.png]]

Types:
- CAN 2.0A: 11bit ID, 8 byte Data, 1 Mbps
- CAN 2.0B: 11/29bit ID, 8 byte Data, 1 Mbps
- CAN FD: 11/29bit ID, 64 byte Data, 10+ Mbps
- CAN XL: 11/29bit ID, 1500+ byte Data, 10+ Mbps

## CAN Bus Protocols

These are higher level protocol that allow you to use a standard that is heavily adopted by an industry to make the development quicker and easily swapable.

Types:
- DeviceNet: Industrial I/O
- SAE J1939: Commercial Vehicles
- CANOpen: All Applications
- CANOpen FD: All Applications

---
## CANopen

Extensive Guide: [YouTube: CANopen Course - EmSA CANopen](https://youtube.com/playlist?list=PLXc1T5NMSXQufsSlsN6unfDxT6ojiK_sn&si=6XUcZh8HnZZHbiwL)
Practical Guide: [YouTube:  CANOpen Node STM32 From basics to coding ](https://youtu.be/R-r5qIOTjOo?si=hjjJt6-FHXHmk5BB)

CANOpen is a Higher Layer Protocol (HLP) that is built on top of CAN Protocol to enable a standardisation and advance features over traditional CAN Protocol.

Standard Benefits:
 - Network Managament: Device Detection, Operational states, reset
 - Service Access: Point to Point, any data size
 - Data Access: Multicast, various trigger options
 - Standard Documentation for various applications like Motors, Drives, etc.

SDO Features:
- Max 4 byte single request
- Max 7 bytes per segment
- Single Master/Client
PDO:
- Max 8 bytes
 
---
### Data Definition
- CANOpen uses "Little Endian"
- Datatypes:
	- Unsignedxx(8,16,24,32)
	- Integerxx(8,16,24,32)
	- Floating, Time, Strings
	- Domain

---
### Object Dictionary: Data Addressing
- Object dictionary is a look-up table type structure which contains the index and subindex which links to the datatype and the description for the object.
- Index: 16 bits, SubIndex: 8 bits which allows upto 24bit addresses for objects.
- There are some predefined ranges:
	- 1000h - 1fffh: CANOpen Parameters to configure the CANOpen
	- 2000h - 5fffh: Manufacturer Specfic data
	- 6000h - Afffh: Profile defined data

---
### Nodes and Networks
Each device can be referred with Device ID: `<net id><node id>`
- Nodes:
	- Supports upto 127 nodes
	- No duplicate node ID allowed
- Networks:
	- Upto 255 networks can be connected via bridges

---
### Network Management

- Nodes report basic state information:
	- Bootup message
	- Heartbeat Message
	- Emergencies
- Master can control operating states of nodes:
	- Request state change
	- Request Reset

---

### Service and Data Access Protocols

#### SDO/USDO

**(Universal) Service Data Object**

- Provides access to all params of a object Dictioinary
- Used to Request changes/data and response is a confirmation to change or response to a data request.
- Used to change configurations and run diagnostics
- Can be of any parameter size.
#### PDO

**Process Data Object**

- Optimized access to select entries of the Object Dictionary
- Used for self triggered broadcasts which can be triggered by time, event, sync message, etc.

---

### Selected Profiles Available

Readily available standards are available.

CiA Device Profiles
- CiA401 Generic I/O
 - CiA402 Dives and motion control
 - CiA406 Encoders
 - CiA410 Inclinometer
 - CiA418/419 Batteries/Charger
 - CiA420 Extruder
 - CiA450 Pumps

CiA Application Profiles:
- CiA417 Lift Control Systems
- CiA420/423/424/426/430/433 Train/Rail Vehicle

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

[YouTube: CANopen Course - EmSA CANopen](https://youtube.com/playlist?list=PLXc1T5NMSXQufsSlsN6unfDxT6ojiK_sn&si=6XUcZh8HnZZHbiwL)

[Texas Instruments - Introduction to the Controller Area Network (CAN)](https://www.ti.com/lit/an/sloa101b/sloa101b.pdf)

[Typhoon HIL Documentation - CAN Bus protocol](https://www.typhoon-hil.com/documentation/typhoon-hil-software-manual/References/can_bus_protocol.html)

[Can someone explain how to use CANBUS protocol?](https://stackoverflow.com/questions/33569507/can-someone-explain-how-to-use-canbus-protocol)

[CAN Bus: A Beginners Guide Part 1](https://youtu.be/YBrU_eZM110?si=aA5Myy0UNug5AIS1)

[CAN Bus: A Beginners Guide Part 2](https://youtu.be/z5CVljiLhvc?si=EGW0jlv_eU0BY4Zc)

[Arm Semiconductors - CAN Primer: Creating your own Network](https://grouper.ieee.org/groups/msc/upamd/pub_docs/CAN.pdf)

### Practical Implementation Guide

[YouTube:  CANOpen Node STM32 From basics to coding ](https://youtu.be/R-r5qIOTjOo?si=hjjJt6-FHXHmk5BB)
