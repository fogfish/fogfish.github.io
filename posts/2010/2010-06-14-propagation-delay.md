# Signal propagation and network processing latencies at the world-wide scale

## Propagation delay

Propagation delay (tᵖ) is the time taken by the head of signal to travel from the sender to the receiver. It is constrained by propagation speed (speed of light) and length of the link.

```
tᵖ = L/S
```

`L` is the distance (length of the link); `S` is the wave propagation speed over medium. Wireless communication speed = 299,792,458 meters per second, Copper is about 67% from speed of light, which is 200,860,946 meters per second.

**Propagation delay is 5 microsecond per kilometer.**

Table below summarizes the overall effect of increasing distances. You can minimize this effect by reducing the distance data must travel.

| Region   | Distance (km) | tᵖ (ms) |
| -------- | ------------- | ------- |
| Europe   | 2000          | < 10    |
| US       | 3000          | 10 - 15 |
| Asia     | 4000          | 15 - 20 |
| Atlantic | 5000          | 20 - 30 |
| America  | 6000          | 30 - 40 |
| Russia   | 10000         | 40 - 50 |
| Pacific  | 11000         | > 50    |


**Solution** The physical distances and speed of light are major constraints Unfortunately, there is not any solution except to minimize the distance required by the signal to travel, measure and deal with this latency.
* minimize a distance between client and data center
* co-locate dependencies; data provider and algorithms
* service provision on edge and/or Internet exchange
* measure and try to deal with it


## Processing delay

Each network element in the data path adds finite amount of delay for layer 2 forwarding, segmentation and reassembly, firewalls, NAT, etc.

The packet handling time of hardware-assisted network elements is in range of 4-20 microsecond, on averages 25 microsecond per hop. Using features that are supported with hardware assistance will greatly reduce latency.

Store-and-forward vs cut-through switching
* A forwarding decision on a packet is made at store-and-forward switch when whole frame is received and integrity is validated.
* Cut-through switch operates on incoming frame as soon destination MAC is received. A switching decision is made in order of few microsecond, regardless of the packet size.

![World-wide network delays](/assets/images/2010-06-14-world-wide-network-delays.png)


## Transmission delay

* time required to serialize packet bits into the link.
* constrained by carrier signal, modulation and used frequencies.
* expressed as bits per second (bandwidth), often bandwidth is shared among network users.

Table below summarizes the overall effect of transmission delay for various technologies

| Name             | Bandwidth | TXD 64 bytes (ms) | TXD 1500 bytes (ms) |
| ---------------- | --------- | ----------------- | ------------------- |
| WCDMA            | 144 Kbps  | 3.56              | 83.3                |
| ADSL2+           | 24 Mbps   | 0.02              | 0.50                |
| WLAN 802.11g     | 54 Mbps   | 0.01              | 0.22                |
| Fast Ethernet    | 100 Mbps  | 0.005             | 0.120               |
| OC12             | 622 Mbps  | <0.001            | 0.019               |
| Gigabit Ethernet | 1 Gbps    | <0.001            | 0.012               |
