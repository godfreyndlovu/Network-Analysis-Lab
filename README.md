# Packet Capture Analysis (Wireshark & tcpdump)
In this project, I am conducting a network analysis on packet capture files using Wireshark and tcpdump. This is a Qwiklabs-based lab hosted on Google Cloud.

### Skills Learned

- 

### Tools Used


**The project scenario is stated below**:

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

I then, double-clicked the first packet that lists TCP as the protocol, opening the packet's details window pane shown:

![Z3](https://github.com/godfreyndlovu/Network-Analysis-Lab/assets/102636518/796f5fd2-8d0b-4b0e-baa2-a2539faeee74)

The upper section of the window pane contains subtrees where Wireshark provides an analysis of the various parts of our network packet. The lower section of the window contains the raw packet data displayed in hexadecimal and ASCII text. There is also placeholder text for fields where the character data does not apply, as indicated by the dot `“.”`.

