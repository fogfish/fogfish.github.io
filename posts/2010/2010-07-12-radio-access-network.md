# Impact of the radio access network on the communication latency of distributed applications

UTRAN Radio Interface protocol architecture. Transport Channels, Logical Channels, Radio Bearers.

![Radio Network Stack](/assets/images/2010-07-12-radio-network-stack.svg)

## Protocol Functions

**Radio Resource Control (RRC) protocol**
* Network-to-device control and signalling
* Establish, reconfigure and release transport channels and radio bearers
* Mobility functions
* Power control

**Packet Data Convergence (PDC) protocol**
* transfers of user data
* header compression

**Radio Link Control (RLC) protocol**
* Acknowledged data transfer
* Unacknowledged data transfer
* Segmentation and Concatenation
* Error Recovery
* Data transfer suspend/resume if requested by RRC

**Medium Access Control (MAC) protocol**
* Logical channel multiplexing
* Assignment, reconfiguration and release of shared radio resources
* MAC configuration is done by RRC layer.
* Asymmetrical protocol, different up/down links
* Unacknowledged transfer
* Reporting of Measurements: Traffic volume measurements are performed in MAC and reported to RRC

**Physical Layer**
Combination of frequency channels and time slots. Before discussing anything further, we should look at the channel structures for Layer 1, MAC and RLC.

## Channels

**Physical channel**
* defines exact physical characteristics of the radio channel. It uses a combination of frequency and time slots.
* the data is sent by transport block from MAC layer to physical every 10 ms.
* multiple transport channels are multiplexed to physical layer frame.

**Transport channel**
* defines how data is transferred by the physical channels.
* dedicated channel is reserved for a single user only (it is support fast power control and soft handover)
* common channel can be used by any user at any time. Don’t support soft handover but some support fast power control.

**Logical channel**
* defines the class of transported data (e.g. signaling, voice, packet data)

## Latency on the mobile network

The latency over 3G mobile network varies depending on the number of transmitted packets. The diagram below shows the latency (y axis) for each transmission such as first packet, second packet and so on. 

![Radio Network Latency](/assets/images/2010-07-12-radio-network-latency.png)

The power management and bandwidth allocation protocols slows down the One-way delay for first packets, which makes it slow to establish connection. Use smart "connection" management techniques to warm up the "modem" before the usage. 

![Radio Network Mitigate Latency](/assets/images/2010-07-12-radio-network-mitigate.png)

The network latency is not only the feature of the state of terminal equipment. It significantly varies from the overall network condition and amount of terminals within the cell. For example, the latency at fixed position varies significantly over the day.

![Radio Network Daily Latency](/assets/images/2010-07-12-radio-network-daily-latency.png)


The tower handover is another significant source of the latency. For example, driving through city for 1 hour shows latency spikes when terminal equipment executed handover protocol.

![Radio Network On-the-go Latency](/assets/images/2010-07-12-radio-network-mobility.png)

