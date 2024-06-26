# Packet Capture Analysis (Wireshark & tcpdump)
In this project, I am conducting a network analysis on packet capture files using Wireshark and tcpdump. This is a Qwiklabs-based lab hosted on Google Cloud.

### Skills Learned

- Network filtering and traffic analysis
- Packet inspection
- Utilization of specific filters for targeted network traffic investigation

### Tools Used

- Wireshark
- tcpdump
- Qwiklabs


#### The project scenario is stated below:

## Scenario 1 -  Network filtering using Wireshark
_In this scenario, you’re a security analyst investigating traffic to a website._

_Analyze a network packet capture file that contains traffic data related to a user connecting to an internet site. The ability to filter network traffic using packet sniffers to gather relevant information is an essential skill as a security analyst._

_You must filter the data in order to identify the source and destination IP addresses involved in this web browsing session, examine the protocols that are used when the user makes the connection to the website, and analyze some of the data packets to identify the type of information sent and received by the systems that connect to each other when the network data is captured._


### Task 1. Explore data with Wireshark

In this task, I opened a network packet capture file that contained data captured from a system that made web requests to a site. I needed to open this data with Wireshark to get an overview of how the data was presented in the application.

I opened the packet capture file by double-clicking the sample p.cap file called 'sample' on the Windows desktop to launch Wireshark. I then maximized the Wireshark application window  by double-clicking the Wireshark title bar next to the `sample.pcap` filename. 
The Wireshark window displays the contents of the p.cap file as shown below:

![Z1](https://github.com/godfreyndlovu/Network-Analysis-Lab/assets/102636518/623cc9b2-3344-4b79-96cf-873c4d436b6e)

A lot of network packet traffic is listed, I’ll apply filters to find the information I need.

The overview of the key property columns listed for each packet:

- **No.** : The index number of the packet in this packet capture file
- **Time:** The timestamp of the packet
- **Source:** The source IP address
- **Destination:** The destination IP address
- **Protocol:** The protocol contained in the packet
- **Length:** The total length of the packet
- **Info:** Some infomation about the data in the packet (the payload) as interpreted by Wireshark

On scrolling we observe all that not data packets are the same color. Coloring rules provide high-level visual cues that will help in quickly classify the different types of data. Given that network packet capture files can contain large amounts of data, using coloring rules helps in quickly identifying relevant data. Our `sample.pcap` lists a group of light blue packets containing DNS traffic, then green packets containing a mixture of TCP and HTTP protocol traffic.

Upon observation, ICMP is the protocol type listed for the first (and all) packets that contain 'Echo (ping) request' in the info column.

### Task 2. Apply a basic Wireshark filter and inspect a packet

In this task, I engage in a detailed analysis of the network packet and I filter the data to inspect the network layers and protocols contained in the packet.

I start by applying the display filter `ip.addr == 142.250.1.139` in the text box and select enter.

![Z2](https://github.com/godfreyndlovu/Network-Analysis-Lab/assets/102636518/c0cdd65b-4e2c-49ec-9ec1-b1b142d45d35)

The previous query significantly reduced the list of packets displayed and only shows packets where either the source or the destination IP address matches the address I entered. At this point, only two packet colors are used: light pink for ICMP protocol packets and light green for TCP (and HTTP, which is a subset of TCP) packets. 

---
I then, double-clicked the first packet that lists TCP as the protocol, opening the packet's details window pane shown:

![Z3](https://github.com/godfreyndlovu/Network-Analysis-Lab/assets/102636518/796f5fd2-8d0b-4b0e-baa2-a2539faeee74)

The upper section of the window pane contains subtrees where Wireshark provides an analysis of the various parts of our network packet. The lower section of the window contains the raw packet data displayed in hexadecimal and ASCII text. There is also placeholder text for fields where the character data does not apply, as indicated by the dot `“.”`.

---
Double-clicking **Frame** - the first subtree in the upper section, provides details about the overall network packet, or frame, including the frame length and the arrival time of the packet. At this level, we’re viewing information about the entire packet of data.
The window pane displays as shown below:

![Z4](https://github.com/godfreyndlovu/Network-Analysis-Lab/assets/102636518/d3486505-f079-4a68-8295-d92d972d618f)

---
Collapsing the first subtree and double-clicking **Ethernet II**, the second subtree - we observe details about the packet at the Ethernet level, including the source and destination MAC addresses and the type of internal protocol that the Ethernet packet contains. The window pane displays as shown below:

![Z5](https://github.com/godfreyndlovu/Network-Analysis-Lab/assets/102636518/7dedc356-1e6c-49b9-a7e2-f2ad4ffff0b4)

---
Collapsing the second subtree and double-clicking **Internet Protocol Version 4**, the third subtree - provides packet data about the Internet Protocol (IP) data contained in the Ethernet packet. It contains information such as the source and destination IP addresses and the Internal Protocol (for example, TCP or UDP), which is carried inside the IP packet. The source and destination IP addresses shown here match the source and destination IP addresses in the summary display for this packet in the main Wireshark window.
The window pane displays as shown below:

![Z6](https://github.com/godfreyndlovu/Network-Analysis-Lab/assets/102636518/2d2e3f84-6fb5-44d5-af9c-83d35c4f609e)

---
Collapsing the third subtree and double-clicking **Transmission Control Protocol**, the fourth subtree - provides detailed information about the TCP packet, including the source and destination TCP ports, the TCP sequence numbers, and the TCP flags.

![Z7](https://github.com/godfreyndlovu/Network-Analysis-Lab/assets/102636518/fcd2e3b7-ce35-4c01-a8d7-4a72aa219384)

The source port and destination port listed here match the source and destination ports in the info column of the summary display for this packet in the list of all of the packets in the main Wireshark window. 
In the window pane displayed, We also observe that Port 80 is the TCP destination port for this packet. It contains the initial web request to an HTTP website that will typically be listening on TCP port 80.

---

In the **Transmission Control Protocol** subtree, scrolling down and double-clicking **Flags**.
This provides a detailed view of the TCP flags set in this packet - as shown below:

![Z8](https://github.com/godfreyndlovu/Network-Analysis-Lab/assets/102636518/e188a8d7-d047-4cfe-8e83-44d0139ef193)


### Task 3. Use filters to select packets

In this task, I use filters to analyze specific network packets based on where the packets came from or where they were sent. l explore select packets using either their physical Ethernet Media Access Control (MAC) address or their Internet Protocol (IP) address.

I started by applying a filter to select traffic from a specific source IP address by querying `ip.src == 142.250.1.139`. This query returns a filtered list with fewer entries than before. It only contains packets that came from 142.250.1.139.

![Z1](https://github.com/godfreyndlovu/Network-Analysis-Lab/assets/102636518/82187bd3-14cd-4c3f-8791-6d11b7ee8e73)


I then moved on to filter to select traffic for a specific destination IP address only  by querying `ip.dst == 142.250.1.139`. This query returns a filtered list is that contains only packets that were sent to 142.250.1.139.

![Z2](https://github.com/godfreyndlovu/Network-Analysis-Lab/assets/102636518/c464cfdd-33b4-411b-afd6-1c3928ae7b76)


To filter and select traffic to or from a specific Ethernet MAC address, I queried `eth.addr == 42:01:ac:15:e0:02`. This filters traffic related to one MAC address, regardless of the other protocols involved:

I then opened the first packet in the list and opened the **Ethernet II** subtree - the MAC address specified in the filter is listed as either the source or destination address in the expanded Ethernet II subtree.

I then moved to open the **Internet Protocol Version 4** subtree and scrolled down to the fields **Time to Live** and **Protocol**.

The **Protocol** field in the Internet Protocol Version 4 subtree indicates which IP internal protocol is contained in the packet.

![Z3](https://github.com/godfreyndlovu/Network-Analysis-Lab/assets/102636518/274ece45-cb2c-4006-b894-53630d147640)

From the Wireshark window pane above, we observe that ICMP is the internal protocol contained in the first packet from MAC address `42:01:ac:15:e0:02.`
`

### Task 4. Use filters to explore DNS packets

In this task, I filter to select and examine DNS traffic. Once I've selected the DNS traffic, l drill down into the protocol to examine how the DNS packet data contains both queries (names of internet sites that are being looked up) and answers (IP addresses that are being sent back by a DNS server when a name is successfully resolved).

To filter and select UDP port 53 traffic, I queried `udp.port == 53`. DNS traffic uses UDP port 53, so it only lists traffic related to DNS queries and responses, as shown below: 

![4](https://github.com/godfreyndlovu/Network-Analysis-Lab/assets/102636518/49b0b86c-12ee-491a-b31d-f39efdd6f9b3)

On double-clicking the first packet in the list, the detailed packet is displayed on the window.

Expanding the **Domain Name System (query)** and selecting **Queries.**

We notice that the name of the website that was queried is _opensource.google.com_ - as shown below:

![Z5](https://github.com/godfreyndlovu/Network-Analysis-Lab/assets/102636518/4fbef1a6-1652-4b15-8dac-e49e649cf708)

We also note that the IP address 142.250.1.139 is displayed when the **Answers** section for the DNS query for opensource.google.com is expanded.


## Task 5. Use filters to explore TCP packets
In this task, l use additional filters to select and examine TCP packets. It involves searching for text that is present in payload data contained inside network packets. This locates packets based on something such as a name or some other text that is of interest.

To filter for TCP port 80 traffic, I queried `tcp.port == 80`, and the results are as shown below:

![Z6](https://github.com/godfreyndlovu/Network-Analysis-Lab/assets/102636518/de8f9aba-8091-4b8d-9958-5e2213264a13)
TCP port 80 is the default port that is associated with web traffic and we observe that quite a few packets were created when the user accessed the web page http://opensource.google.com.

On selecting the first packet in the list - with the Destination IP address of 169.254.169.254, we observe that the **Time to Live value** of the packet as specified in the Internet Protocol Version 4 subtree is `64.`
![Z7](https://github.com/godfreyndlovu/Network-Analysis-Lab/assets/102636518/6513bf7c-86a3-4cc0-a7e5-26e80b027e0f)

The Frame Length of the packet as specified in the Frame subtree is `54` bytes
![Z8](https://github.com/godfreyndlovu/Network-Analysis-Lab/assets/102636518/23e8b97c-b3b9-411d-83f0-1f1dac430cf8)

Applying the `tcp contains "curl"`filter selects TCP packet data that contains specific text data - in this case, "curl."
This filters packets containing web requests made with the curl command in this sample packet capture file - as shown below:

![Z9](https://github.com/godfreyndlovu/Network-Analysis-Lab/assets/102636518/442b409d-8fee-4030-9d4d-f96da1f17cbd)


## Conclusion

In this project, I effectively analyzed network traffic using Wireshark and tcpdump. By applying various filters, I efficiently sifted through data packets to identify relevant information. I also identified source and destination IP addresses, examined the protocols used during web browsing sessions, and analyzed payload data within packets.

I explored network layers and protocols in detail, gaining insights into the structure and flow of data. I specifically analyzed DNS traffic to understand domain name queries and their corresponding IP addresses, and inspected TCP packets to identify web requests and responses.

Through this project, I enhanced my proficiency in network traffic analysis and developed a solid foundation in using Wireshark and tcpdump for real-world network security and troubleshooting scenarios. These skills are crucial for any security analyst or network professional, enabling effective monitoring and analysis of network communications.

