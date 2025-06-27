---
id: embedded_fw
aliases:
  - CANopen
---

# CANopen

Extensive Guide: [YouTube: CANopen Course - EmSA CANopen](https://youtube.com/playlist?list=PLXc1T5NMSXQufsSlsN6unfDxT6ojiK_sn&si=6XUcZh8HnZZHbiwL)

Practical Guide: [YouTube:  CANOpen Node STM32 From basics to coding ](https://youtu.be/R-r5qIOTjOo?si=hjjJt6-FHXHmk5BB)

CANOpen is a Higher Layer Protocol (HLP) that is built on top of CAN Protocol to enable a standardisation and advance features over traditional CAN Protocol.

## Why is it needed?
- How do you detect device connectivity (Heartbeat in CANOpen)
- How do you figure out configuration change? (Configuration of Frequency in CANOpen)
- Standardisation to connect to any device off-market without requiring special configurations.
- Supports Device ID (Node ID) which allows P2P Communication (SDO)
- Allows One to Many Communication (PDO)
- Network Managament: Device Detection, Operational states, reset
- Service Access: Point to Point, any data size
- Data Access: Multicast, various trigger options
- Standard Documentation for various applications like Motors, Drives, etc.
 
---

## Important Concepts

- **Network Management (NMT)**
	- Used to execute NMT Services which are used for device initialization, started, monitored or stopped
	- Works in Master/Slave Structure with Node being NMT Master.
		- Bootup: Transmitted upon device initialisation to inform other nodes about its existence.
	- Master can control operating states of nodes:
		- Request state change
		- Request Reset
- **Synchronization (SYNC)**
- **Emergency (EMCY)**
		- Emergency: Triggered by the occurrence of CANopen device internal error and are transmitted once for each error
- **TimeStamp (TIME) (PDO)**
- **Process Data Objects (PDO)**
	- Real-time data transfer from single node to other nodes
	- No protocol overhead
	- Max 4 bytes
	- Use Case: Transmit/Receive real time value to and from other nodes
	- Allows mapping to specific variables in node for automatic transmission or reception of stack
	- Can be triggered by Event, Timer, Remote node or Sync Object
- **(Universal) Service Data Object (SDO)**
	- Provides access to all params of a object Dictionary
	- Used to Request changes/data and response is a confirmation to change or response to a data request.
	- Used to change configurations and run diagnostics
	- Can be of any parameter size.
	- Max 4 byte single request
	- Max 7 bytes per segment
	- Single Master/Client
	- Use Case: 
		- Peer to Peer reliable communication
		- Configuring a device at startup
		- Transferring large amount of data with acknowledgement
- **Node Monitoring (Heartbeat) (SDO)**
	- Heartbeat: Transmitted cyclically by node. Informs other node about connection status of the current producer node.

**CANOpen Initialisation Process on Node**

![[01_CANOpen_Init_Process.png]]

**Implementation Link**: [GitHub: C Implement for CANOpenNode](https://github.com/CANopenNode/CANopenNode)

---
## Data Definition

- CANOpen uses "Little Endian"
- Datatypes:
	- Unsignedxx(8,16,24,32)
	- Integerxx(8,16,24,32)
	- Floating, Time, Strings
	- Domain

## Node Monitoring (Heartbeat)

**Heartbeat / Node Guard State Codes**

These are the **1-byte values** you receive in the data field of a heartbeat message (`COB-ID = 0x700 + NodeID`):

| **Hex** | **Binary** | **NMT State**          | **Description**                                      |
| ------- | ---------- | ---------------------- | ---------------------------------------------------- |
| `00`    | `00000000` | **Initializing**       | Power-on or reset; node not yet booted               |
| `04`    | `00000100` | **Stopped**            | Node is stopped (doesn’t process PDOs, only SDO/NMT) |
| `05`    | `00000101` | **Operational**        | Fully active – sends/receives PDO, SDO, SYNC, etc.   |
| `7F`    | `01111111` | **Boot-Up**            | Node just booted – waiting for NMT command           |
| `7E`    | `01111110` | **Pre-Operational**    | Accepts SDOs, but does not process PDOs or SYNC      |
| `7D`    | `01111101` | **Unknown** (reserved) | Not standard – may indicate vendor-specific state    |


---
## Object Dictionary: Data Addressing
- Object dictionary is a look-up table type structure which contains the index and subindex which links to the datatype and the description for the object.
- It is predefined during the design of a component and follows ISO Standards. Can be generated using EDS Softwares.
- Index: 16 bits, SubIndex: 8 bits which allows upto 24bit addresses for objects.
- There are some predefined ranges:
	- 1000h - 1fffh: CANOpen Parameters to configure the CANOpen
	- 2000h - 5fffh: Manufacturer Specfic data
	- 6000h - Afffh: Profile defined data

---

## Process Data Object (PDO)
- Generally in Producer/Consumer format.
- Node is considered as server/producer with the client being considered as consumer.
- This is a predetermined table which is used to communicate data at a higher frequency as it allows to club multiple data objects in response/requests and also does not require feedback on successful requests.

---
## Nodes and Networks
Each device can be referred with Device ID: `<net id><node id>`

- Nodes:
	- Supports upto 127 nodes
	- No duplicate node ID allowed
- Networks:
	- Upto 255 networks can be connected via bridges

---
## Selected Profiles Available

Readily available standards are available.

CiA Device Profiles:

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

## Resources

### Theory

[YouTube: CANopen Course - EmSA CANopen](https://youtube.com/playlist?list=PLXc1T5NMSXQufsSlsN6unfDxT6ojiK_sn&si=6XUcZh8HnZZHbiwL)

### Practical Implementation Guide

- [YouTube:  CANOpen Node STM32 From basics to coding ](https://youtu.be/R-r5qIOTjOo?si=hjjJt6-FHXHmk5BB)
- [GitHub: C Implement for CANOpenNode](https://github.com/CANopenNode/CANopenNode)
