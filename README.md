# Quality of service (QoS)

using the Modular QoS CLI (MQC)

## Application on the Networks

1. Latency: This refers to the time it takes for a packet to travel from its source to its destination. High latency can result in delays in data transmission and can be particularly problematic for real-time applications like voice and video conferencing, where a delay of a few hundred milliseconds can lead to poor quality of service.

2. Jitter: This refers to the variation in delay of received packets. In other words, it's the inconsistency in latency. If packets arrive at irregular intervals, it can cause problems for certain types of applications. For instance, in a voice call, high jitter can result in choppy audio or gaps in conversation.

3. Loss: This refers to the number or percentage of packets that are sent from the source but do not reach the destination. Packet loss can occur for a variety of reasons, including network congestion, faulty hardware, or poor signal quality. High packet loss can lead to significant degradation in the quality of service.

4. Bandwidth: This refers to the maximum data transfer rate of a network or Internet connection, measured in bits per second (bps). It indicates the capacity of the connection. The greater the bandwidth, the more data can be sent over the connection in a given amount of time.

### Batch Applications

Applications such as FTP and TFTP are considered batch applications. Both are used to send and receive files. Typically, a user selects a group of files that need to be retrieved and then starts the transfer. Once the download starts, no additional human interaction is required. The amount of available bandwidth determines the speed at which the download occurs. While bandwidth is important for batch applications, it is not critical. Even with low bandwidth, the download is completed eventually.

### Interactive Applications

Interactive applications are applications in which the user waits for a response—for example, inventory lookup. Because these applications require human interaction, response times are more important than for batch applications, but they are still not critical. The transaction will still be completed if the appropriate amount of bandwidth is not available, but it may take longer. Examples of the interactive applications are used in plant automation, physical security, sport, and retail.

### Real-Time Applications

Real-time applications such as voice and video applications also involve human interaction. Because of the amount of information that is transmitted, bandwidth is critical. In addition, because these applications are time-critical, a delay on the network can cause a problem. Timely delivery of the data is crucial. It is also important that data is not lost during transmission because real-time applications, unlike other applications, do not retransmit lost data. Therefore, sufficient bandwidth is mandatory, and the quality of the transmission must be ensured by implementing QoS. QoS is a way of granting higher priority to certain types of data, such as VoIP.

### Ipv4

If you would like to learn more about the IPv4 header fields, go to http://tools.ietf.org/html/rfc791.

### Traffic Characteristics
#### Data
1. Smoth/Burst
2. Benign/Greedy
3. Drop-Insensitive
4. Delay-Insensitive
5. TCP Retransmits

#### Voice
1. Smooth
2. Benign
3. Drop-Sensitive
4. Delay-Sensitive
5. UDP Priority
Requirements :
1. Latenvy <= 150 ms
2. Jitter <= 30 ms
3. Loss <= 1%
4. Bandwidth (30-128 Kbps)

#### Video
1. Bursty
2. Greedy
3. Drop-Sensitive
4. Delay-Sensitive
5. UDP Priority
Requirements :
1. Latenvy <= 150 ms
2. Jitter <= 30 ms
3. Loss <= 0.1 - 1%
4. Bandwidth (384 Kbps - 20+ Mbps)

## QoS Mechanisms Overview

1. Classification and marking tools: These tools analyze sessions to determine which traffic class they belong to and therefore which treatment the packets in the session should receive. Classification should happen as few times as possible because it takes time and uses up resources. For that reason, packets are marked after classification, usually at the ingress edge of a network. A packet might travel across different networks to its destination. Reclassification and re-marking are common at the handoff points upon entry to a new network.

3. Policing, shaping, and re-marking tools: These tools assign different classes of traffic to certain portions of network resources. When traffic exceeds available resources, some traffic might be dropped, delayed, or re-marked to avoid congestion on a link. Each session is monitored to ensure that it does not use more than the allotted bandwidth. If a session uses more than the allotted bandwidth, traffic is dropped (policing), slowed down (shaped), or re-marked (marked down) to conform.

4. Congestion management or scheduling tools: When traffic exceeds available network resources, traffic is queued. Queued traffic will await available resources. Traffic classes that do not handle delay well are better off being dropped unless there is guaranteed delay-free bandwidth for that traffic class.

5. Link-specific tools: There are certain types of connections, such as WAN links, that can be provisioned with special traffic handling tools. One such example is fragmentation.

### Trust Boundary

A trusted domain is a part of the network with only administrator-managed devices such as switches, routers, and so on.

A trust boundary is where packets are classified and marked. For example, IP phones, boundary between ISP and enterprise network.

Untrusted domains are usually devices with user access, such as PCs and printers. The trusted part of the network includes devices that only the network administrators have access to, such as routers and switches.

A trust boundary may also be established between an enterprise network and a service provider network.

### Classification and Marking

![Classification and Marking](https://raw.githubusercontent.com/deliawolf/QoS/main/Classification%20and%20Marking.png)

1. IP Precedence (IPP): This is a Layer 3 QoS method that uses a 3-bit field in the IP header to classify traffic into eight priority levels (0-7). IPP is older and less granular compared to DSCP.

2. Differentiated Services Code Point (DSCP): This is also a Layer 3 QoS method, but it uses a 6-bit field in the IP header, allowing for up to 64 different classes of traffic. This gives you more granularity and flexibility in classifying traffic compared to IPP.

3. Per-Hop Behavior (PHB): This isn't a classification method itself, but rather describes the treatment that packets receive based on their classification (IPP or DSCP). PHB determines how routers or switches handle packets at each hop in the network.

4. Class of Service (CoS): This is a Layer 2 QoS method used in VLAN-tagged frames (802.1Q). CoS uses a 3-bit field in the Ethernet frame header to classify traffic into eight priority levels (0-7).

### Classification Tools

The three most common ways to classify traffic include:

1. Markings: Classification is done on existing Layer 2 or Layer 3 settings.

2. Addressing: Classification is done based on the source and destination interface, or Layer 2 destination address, or Layer 3 source and destination address, or source and destination Layer 4 port. Using an IP address classifies traffic by a group of devices. Using a port number classifies traffic by traffic type.

3. Application signatures: Classification is done based on application content inside the packet payload. This classification is also called deep packet inspection.

#### NBAR Advanced Classification Tool

Network Based Application Recognition (NBAR) is a Layer 4 to Layer 7 advanced classification tool. It offers deep packet inspection classifier. NBAR is more CPU-intensive than marking that is done by the existing markings, addresses, or access control lists (ACLs).

NBAR has two different modes of operation:

1. Passive mode: Provides real-time statistics on applications per protocol or per interface and gives bidirectional statistics such as bit rate, packet, and byte counts.
2. Active mode: Classifies applications for traffic marking, so that QoS policies can be applied.

### Policing, Shaping, and Re-Marking

Policers and shapers are tools that identify and respond to traffic violations. They usually identify traffic violations in a similar manner, but they differ in their response:

1. Policers perform checks for traffic violations against a configured rate. The action they take in response is to either drop or re-mark the excess traffic. Policers do not delay traffic; they only check traffic and take action if needed.

2. Shapers are traffic-smoothing tools that work in cooperation with buffering mechanisms. A shaper does not drop traffic, but smooths it out so it never exceeds the configured rate. Shapers are usually used to meet SLAs. Whenever the traffic spikes above the contracted rate, the excess traffic is buffered and thus delayed until the offered traffic goes below the contracted rate.

### Managing Congestion

Congestion management includes queuing (or buffering). It uses a logic that reorders packets into output buffers. It is only activated when congestion occurs. When queues fill up, packets can be reordered so that the higher-priority packets can be sent out of the exit interface sooner than the lower-priority packets.

Scheduling is a process of deciding which packet should be sent out next. Scheduling occurs regardless of whether there is congestion on the link.

Low-latency queuing takes the previous model and adds a queue with strict priority (for real-time traffic).

Different scheduling mechanisms exist. The following are three basic examples:

1. Strict priority: The queues with lower priority are only served when the higher-priority queues are empty. There is a risk with this kind of scheduler that the lower-priority traffic will never be processed. This situation is commonly referred to as traffic starvation.

2. Round-robin: Packets in queues are served in a set sequence. There is no starvation with this scheduler, but delays can badly affect the real-time traffic.

3. Weighted fair: Queues are weighted, so that some are served more frequently than others. This method thus solves starvation and also gives priority to real-time traffic. One drawback is that this method does not provide bandwidth guarantees. The resulting bandwidth per flow instantaneously varies based on the number of flows present and the weights of each of the other flows.

Here are two examples of newer queuing mechanisms that are recommended for rich-media networks:

1. CBWFQ is a combination of bandwidth guarantee with dynamic fairness of other flows. It does not provide latency guarantee and is only suitable for data traffic management.

2. LLQ is a method that is essentially CBWFQ with strict priority. This method is suitable for mixes of data and real-time traffic. LLQ provides both latency and bandwidth guarantees.

### Tools for Congestion Avoidance

Cisco IOS Software does not support pure RED but uses weighted random early detection (WRED). The principle is the same as with RED, except that traffic weights skew the randomness of the packet drop. In other words, traffic that is more important will be less likely to be dropped than less important traffic.

## Define an Overall QoS Policy

1. Class Map and ACL (class-map)
   "Which traffic do we care about?"
2. Policy Map (policy-map)
   "What will be done to this traffic?"
3. Service map (service-policy)
   "Where will this policy be implemented?"

### Example IPP, DSCP(L3 Classification)

IP Precedence (IPP):
Voice Traffic:
```
access-list 100 permit udp any any range 16384 32767
class-map match-all VOICE
 match access-group 100
policy-map IPP_POLICY
 class VOICE
  set precedence 5
```
Video Traffic:
```
access-list 101 permit ip 192.0.2.0 0.0.0.255 any
class-map match-all VIDEO
 match access-group 101
policy-map IPP_POLICY
 class VIDEO
  set precedence 4
```
Data Traffic:
```
access-list 102 permit tcp any any eq 80
access-list 102 permit tcp any any eq 443
class-map match-all DATA
 match access-group 102
policy-map IPP_POLICY
 class DATA
  set precedence 1
```
Differentiated Services Code Point (DSCP):
Voice Traffic:
```
access-list 100 permit udp any any range 16384 32767
class-map match-all VOICE
 match access-group 100
policy-map DSCP_POLICY
 class VOICE
  set dscp ef
```
Video Traffic:
```
access-list 101 permit ip 192.0.2.0 0.0.0.255 any
class-map match-all VIDEO
 match access-group 101
policy-map DSCP_POLICY
 class VIDEO
  set dscp af41
```
Data Traffic:
```
access-list 102 permit tcp any any eq 80
access-list 102 permit tcp any any eq 443
class-map match-all DATA
 match access-group 102
policy-map DSCP_POLICY
 class DATA
  set dscp default
```
Per-Hop Behavior (PHB):
PHB is a concept rather than a specific configuration command. In the context of IP traffic, PHB refers to the treatment that each packet receives as it traverses a network, based on its classification (e.g., IPP or DSCP). So, the "set precedence" and "set dscp" commands in the above examples are essentially defining the PHB for the packets.

Remember to apply the policy map to the interface after configuring it:
```
interface GigabitEthernet0/0
 service-policy input DSCP_POLICY  ! Replace with IPP_POLICY or PHB_POLICY as needed
```
These examples should give you a good starting point for configuring QoS on your network. The specific ACLs, classes, and values you use will depend on your network's requirements and the types of traffic you have.

### Example COS(L2 Classification)

At Layer 2, traffic classification for QoS is done using the Class of Service (CoS) value, which is part of the VLAN tag in an Ethernet frame. The CoS value is a 3-bit field that allows for 8 different priority levels (0-7).

Devices such as IP phones or video conferencing systems typically assign a CoS value to their traffic to ensure proper handling by network devices. This value is usually specified in the device's technical documentation.

Here's a simple example of how to configure CoS in a Cisco switch:
```
class-map match-all CLASS1
 match cos 5  ! Matches all frames with a CoS value of 5

policy-map POLICY1
 class CLASS1
  set cos 3  ! Changes the CoS value to 3

interface GigabitEthernet0/0
 switchport mode trunk
 service-policy output POLICY1
```
In this example, all frames entering the switch on the trunk interface GigabitEthernet0/0 with a CoS value of 5 will have their CoS value changed to 3.

Voice Traffic with CoS 5:
CoS values range from 0 (lowest priority) to 7 (highest priority). Voice traffic is often given a high priority because it is sensitive to delay and packet loss.
```
class-map match-all VOICE
 match cos 5  ! Classify voice traffic with CoS 5

policy-map QOS_POLICY
 class VOICE
  priority percent 50  ! Give voice traffic high priority (low latency)

interface GigabitEthernet0/0
 service-policy output QOS_POLICY  ! Apply the QoS policy to the interface
```
Video Traffic with CoS 4:
Video traffic is also sensitive to delay and packet loss, but it's often given a slightly lower priority than voice traffic.
```
class-map match-all VIDEO
 match cos 4  ! Classify video traffic with CoS 4

policy-map QOS_POLICY
 class VIDEO
  bandwidth percent 30  ! Reserve a portion of the bandwidth for video traffic

interface GigabitEthernet0/0
 service-policy output QOS_POLICY  ! Apply the QoS policy to the interface
```
Data Traffic with CoS 1:
Data traffic is typically more tolerant of delay and packet loss, so it's often given a lower priority.
```
class-map match-all DATA
 match cos 1  ! Classify data traffic with CoS 1

policy-map QOS_POLICY
 class DATA
  bandwidth percent 20  ! Reserve a portion of the bandwidth for data traffic

interface GigabitEthernet0/0
 service-policy output QOS_POLICY  ! Apply the QoS policy to the interface
```

### Example Policing, Shaping, and Re-Marking

1. Policing: This is a method used to control the maximum rate of traffic sent or received on an interface. It's often configured at the network edge to prevent traffic from exceeding a specified rate. Here's an example configuration:
```
policy-map POLICY_MAP
 class CLASS_MAP
  police rate 1000000 conform-action transmit exceed-action drop
```
In this example, traffic that conforms to the rate limit (1 Mbps) is transmitted, while traffic exceeding the rate limit is dropped.

2. Traffic Shaping: Traffic shaping, unlike policing, buffers excess packets in a queue and then schedules them for later transmission over increments of time. This prevents packet drop and allows for smoother traffic flow. Here's an example configuration:
```
policy-map POLICY_MAP
 class CLASS_MAP
  shape average 1000000
```
In this example, traffic is shaped to an average rate of 1 Mbps.

3. Re-Marking: This allows you to change the classification of packets as they pass through a network device. This is often used when you want to change the priority of traffic at different points in your network. Here's an example configuration:

In this example, we classify HTTP traffic (port 80) and mark it with DSCP AF21:
```
! Define ACL to match HTTP traffic
access-list 100 permit tcp any any eq 80

! Define class-map to match the ACL
class-map match-all HTTP_TRAFFIC
 match access-group 100

! Define policy-map to set DSCP for HTTP traffic
policy-map MARKING_POLICY
 class HTTP_TRAFFIC
  set ip dscp af21

! Apply policy-map to the interface
interface GigabitEthernet0/0
 service-policy input MARKING_POLICY
```
Now, suppose later in the network, we want to remark this traffic with a different DSCP value. Here's how you might do it:
```
! Define class-map to match traffic with DSCP AF21
class-map match-all PREVIOUSLY_MARKED
 match ip dscp af21

! Define policy-map to remark DSCP
policy-map REMARKING_POLICY
 class PREVIOUSLY_MARKED
  set ip dscp af11

! Apply policy-map to the interface
interface GigabitEthernet0/1
 service-policy input REMARKING_POLICY
```
In this example, the HTTP traffic that was initially marked with DSCP AF21 is remarked with DSCP AF11 as it passes through a different interface.

### Example Managing Congestion with Queuing

Here's an example of how you might configure Class-Based Weighted Fair Queuing (CBWFQ) with Low Latency Queuing (LLQ):
```
class-map match-all VOICE
 match ip dscp ef

class-map match-all VIDEO
 match ip dscp af41

class-map match-all DATA
 match ip dscp default

policy-map QOS_POLICY
 class VOICE
  priority 500  ! LLQ: voice traffic goes to the priority queue
 class VIDEO
  bandwidth 1000  ! CBWFQ: video traffic is guaranteed 1000 kbps
 class DATA
  bandwidth 1500  ! CBWFQ: data traffic is guaranteed 1500 kbps

interface GigabitEthernet0/0
 service-policy output QOS_POLICY
```
In this example, voice traffic is given priority treatment with LLQ. Video and data traffic are handled with CBWFQ, with guaranteed minimum bandwidths of 1000 kbps and 1500 kbps respectively.

### Congestion Avoidance

weighted random early detection (WRED)

```
! Define a class-map to match the traffic
class-map match-all CLASS1
 match ip dscp default

! Define a policy-map to apply WRED
policy-map WRED_POLICY
 class CLASS1
  random-detect dscp-based

! Apply the policy-map to an interface
interface GigabitEthernet0/0
 service-policy output WRED_POLICY
```
