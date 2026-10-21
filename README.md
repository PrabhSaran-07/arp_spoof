# ARP Spoofing Using Python and Scapy

## Overview

This project demonstrates the fundamentals of the Address Resolution Protocol (ARP) and explores how ARP spoofing can affect IP-to-MAC address mappings on a local network.

The project uses Python 3 and Scapy to construct and inspect ARP packets in a controlled virtual network environment using Kali Linux and VMware.

It also explores ARP cache behavior, packet transmission, and the importance of restoring correct network mappings after a security experiment.

## Objectives

- Understand how ARP resolves IP addresses to MAC addresses.
- Learn how ARP requests and replies work.
- Explore packet manipulation using Python and Scapy.
- Understand ARP spoofing and its potential security implications.
- Observe ARP cache entries using Windows CMD and Kali Linux.
- Learn about network isolation and virtual networking with VMware.
- Understand the importance of network restoration and defensive measures.

## Technologies Used

- **Programming Language:** Python 3
- **Packet Manipulation:** Scapy
- **Operating System:** Kali Linux
- **Target Environment:** Windows
- **Virtualization:** VMware Workstation
- **Networking Protocol:** Address Resolution Protocol (ARP)

## Project Structure

```text
arp_spoof/
├── arp_spoof.py
└── README.md
```

## Prerequisites

Before running the project, ensure you have:

- Kali Linux installed in VMware.
- Python 3 installed.
- Scapy installed.
- Basic knowledge of IP addressing, MAC addresses, and ARP.
- An isolated virtual network configured for authorized testing.

## Installation

### 1. Install Scapy

On Kali Linux, run:

```bash
sudo apt update
sudo apt install python3-scapy
```

### 2. Verify Python and Scapy

Check the Python version:

```bash
python3 --version
```

Verify that Scapy is available:

```bash
python3 -c "import scapy.all as scapy; print(scapy.__version__)"
```

### 3. Run the Python Script

Run the script using Python 3:

```bash
sudo python3 arp_spoof.py
```

Administrative privileges may be required for network packet operations.

## Network Configuration

The initial VMware NAT configuration used the following addresses:

| Device | IP Address | Description |
|---|---|---|
| Windows Host | `192.168.83.1` | Windows VMnet8 adapter |
| Kali Linux | `192.168.83.140` | Kali network interface |
| VMware NAT Gateway | `192.168.83.2` | Virtual network gateway |

These addresses are specific to the initial lab configuration and may differ on other systems.

## Understanding ARP

The Address Resolution Protocol (ARP) maps an IPv4 address to a MAC address within a local network.

For example, when a device needs to communicate with another device on the same subnet, it can broadcast an ARP request asking which device owns a particular IP address.

The device with that IP address can respond with its MAC address.

### ARP Spoofing

ARP spoofing is a technique in which forged ARP messages advertise an incorrect association between an IP address and a MAC address.

Potential consequences include:

- Incorrect ARP cache entries.
- Traffic interception under certain network conditions.
- Communication disruption.
- Man-in-the-middle attacks.

ARP spoofing does not change the physical MAC address of a router. It attempts to influence the IP-to-MAC mappings stored by other devices.

## Observations and Findings

During the experiment:

- Kali Linux successfully communicated with the VMware NAT gateway.
- Windows and Kali successfully exchanged ICMP ping packets.
- ARP neighbor entries were inspected using Kali Linux.
- Windows ARP entries were inspected using the `arp -a` command.
- Scapy generated warnings concerning Ethernet destination addresses when sending ARP replies.
- The expected gateway mapping change was not confirmed in the Windows ARP table.

These observations highlight that successful script execution does not necessarily mean that another device accepted the transmitted ARP messages.

VMware NAT networking can also behave differently from a shared Ethernet network, affecting what can be observed during an ARP experiment.

## Troubleshooting

### Issue 1: IndexError in `get_mac()`

**Cause:** The script attempts to access an ARP response when no response was received.

**Resolution:** Check whether the response list is empty before accessing its first element.

### Issue 2: Scapy Ethernet Destination Warnings

**Cause:** Scapy warns that an Ethernet destination MAC address should be provided when sending an ARP reply.

**Resolution:** Review Scapy's Layer 2 and Layer 3 packet-sending behavior and inspect the packet structure.

### Issue 3: Gateway Missing from the Windows ARP Table

**Cause:** Windows may not have a cached ARP entry for the gateway. VMware NAT can also affect gateway visibility.

**Resolution:** Verify the virtual network configuration and distinguish between the Windows host's ARP cache and Kali's neighbor table.

### Issue 4: Dynamic Printing

The packet counter uses carriage-return printing to update the same terminal line:

```python
print(
    "\r[+] Packets sent: " + str(sent_packets_count),
    end="",
    flush=True
)
```

- `\r` returns the cursor to the beginning of the line.
- `end=""` prevents a newline.
- `flush=True` displays the output immediately.

## Security Recommendations

Organizations can reduce ARP spoofing risks through appropriate network security measures, including:

- Dynamic ARP Inspection (DAI), where supported.
- DHCP snooping and appropriate switch configurations.
- Network segmentation.
- Monitoring unexpected ARP changes.
- Static ARP entries for selected critical systems where operationally appropriate.
- Encryption protocols such as HTTPS to protect application data.

## Learning Outcomes

This project provides practical exposure to:

- ARP fundamentals and address resolution.
- Python-based packet analysis.
- Scapy packet construction.
- ARP cache inspection.
- VMware virtual networking.
- Common network security risks.
- Troubleshooting network experiments.

## Disclaimer

This project is intended for educational purposes and authorized security testing only.

Conduct experiments exclusively in isolated lab environments or on systems for which you have explicit permission.

Do not use ARP spoofing to intercept, manipulate, or disrupt communications on networks without authorization.

## Author

**Prabhleen Kaur**

Cybersecurity and Forensics Student

## License

This project is available for educational and learning purposes. 
