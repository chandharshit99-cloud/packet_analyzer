You can replace your current README with this:

# 🚀 DPI Engine — Deep Packet Inspection System

A high-performance **Deep Packet Inspection (DPI) engine built with C++17** for analyzing network traffic from PCAP files. The system parses network packets, tracks connections, identifies applications using HTTP/TLS metadata, and applies configurable blocking rules.

The project also includes a **multi-threaded processing architecture** designed to handle packet analysis efficiently.

---

## ✨ Features

- 🔍 Deep inspection of network packets
- 🌐 Ethernet, IPv4, TCP and UDP packet parsing
- 🔗 Five-tuple based connection tracking
- 🔐 TLS SNI and HTTP Host extraction
- 📱 Application identification
- 🚫 IP, domain and application-based blocking
- ⚡ Multi-threaded packet processing
- 📊 Traffic statistics and processing reports
- 📦 PCAP input and filtered PCAP output

---

## 🧠 How It Works

The DPI engine processes network traffic through a simple pipeline:

```text
PCAP File
    ↓
Packet Reader
    ↓
Protocol Parser
    ↓
Flow Tracking
    ↓
SNI / HTTP Host Extraction
    ↓
Application Classification
    ↓
Blocking Rules
    ↓
Forward / Drop
    ↓
Processing Report

The system can identify traffic such as YouTube, Facebook, Google and other applications using available domain information from network traffic.

🔍 Deep Packet Inspection

Traditional packet filtering generally relies on information such as:

Source IP
Destination IP
Port
Protocol

DPI goes further by inspecting available packet payload information to identify the application or destination associated with a connection.

For HTTPS traffic, the project extracts the Server Name Indication (SNI) from the TLS Client Hello when it is available.

This allows the engine to identify domains such as:

www.youtube.com
www.facebook.com
www.google.com

The detected domain can then be mapped to an application and evaluated against the configured blocking rules.

🌐 Network Information Used

The engine tracks connections using the five-tuple:

Source IP
Destination IP
Source Port
Destination Port
Protocol

This allows packets belonging to the same network connection to be tracked together.

🚫 Traffic Blocking

The system supports multiple types of blocking rules:

Rule Type	Example	Purpose
IP	192.168.1.50	Block traffic from an IP
Application	YouTube	Block an application
Domain	facebook	Block matching domains

Once a flow is identified as blocked, subsequent packets belonging to that flow are dropped.

⚡ Multi-Threaded Architecture

The project includes a multi-threaded implementation for processing larger packet captures.

              ┌───────────────┐
              │ Reader Thread │
              └───────┬───────┘
                      ↓
              ┌───────────────┐
              │ Load Balancer │
              └───────┬───────┘
                      ↓
        ┌─────────────┼─────────────┐
        ↓             ↓             ↓
    Fast Path      Fast Path     Fast Path
        │             │             │
        └─────────────┼─────────────┘
                      ↓
              ┌───────────────┐
              │ Output Writer │
              └───────────────┘

A hash based on the connection's five-tuple is used to distribute packets while keeping packets from the same flow together.

This helps maintain flow state while allowing multiple packets to be processed concurrently.

🛠️ Tech Stack

Languages

C++17
Python

Networking

TCP/IP
Ethernet
IPv4
TCP
UDP
TLS
HTTP

Concepts

Deep Packet Inspection
Network Protocol Parsing
Flow Tracking
Multi-threading
Producer-Consumer Pattern
Hash-based Load Distribution

Tools

Wireshark
PCAP
📁 Project Structure
packet_analyzer/
│
├── include/
│   ├── pcap_reader.h
│   ├── packet_parser.h
│   ├── sni_extractor.h
│   ├── types.h
│   ├── rule_manager.h
│   ├── connection_tracker.h
│   ├── load_balancer.h
│   ├── fast_path.h
│   ├── thread_safe_queue.h
│   └── dpi_engine.h
│
├── src/
│   ├── pcap_reader.cpp
│   ├── packet_parser.cpp
│   ├── sni_extractor.cpp
│   ├── types.cpp
│   ├── main_working.cpp
│   └── dpi_mt.cpp
│
├── generate_test_pcap.py
├── test_dpi.pcap
└── README.md
💻 Getting Started
Prerequisites
Linux or macOS
C++17 compatible compiler
g++ or clang++
Python 3

No external C++ libraries are required for the basic implementation.

🔨 Build
Simple Version
g++ -std=c++17 -O2 -I include \
    -o dpi_simple \
    src/main_working.cpp \
    src/pcap_reader.cpp \
    src/packet_parser.cpp \
    src/sni_extractor.cpp \
    src/types.cpp
Multi-Threaded Version
g++ -std=c++17 -pthread -O2 -I include \
    -o dpi_engine \
    src/dpi_mt.cpp \
    src/pcap_reader.cpp \
    src/packet_parser.cpp \
    src/sni_extractor.cpp \
    src/types.cpp
▶️ Running the Project

Basic usage:

./dpi_engine test_dpi.pcap output.pcap

With blocking rules:

./dpi_engine test_dpi.pcap output.pcap \
    --block-app YouTube \
    --block-app TikTok \
    --block-ip 192.168.1.50 \
    --block-domain facebook

Configure the multi-threaded version:

./dpi_engine test_dpi.pcap output.pcap --lbs 4 --fps 4
📊 Example Output

The engine generates information such as:

Total Packets: 77
Total Bytes: 5738

TCP Packets: 73
UDP Packets: 4

Forwarded: 69
Dropped: 8

Application Breakdown:
HTTPS      39
YouTube     4
Facebook    3
DNS         4

It can also display detected domains/SNIs from the analyzed traffic.

🔮 Future Improvements

Some possible extensions for the project include:

🌐 QUIC / HTTP3 support
📊 Real-time traffic dashboard
🚦 Bandwidth throttling
💾 Persistent blocking rules
📈 More detailed traffic analytics
🔎 Additional application signatures
🎯 Project Highlights

This project demonstrates practical concepts in:

Network programming
Packet parsing
TCP/IP networking
Deep Packet Inspection
TLS inspection
Flow-based traffic analysis
Multi-threaded programming
Producer-consumer architecture
👨‍💻 Author

Harshit Chand

B.Tech — Computer Science & Engineering

⭐ If you find this project interesting, feel free to explore the code and experiment with the DPI engine.


