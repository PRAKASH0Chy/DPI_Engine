# Deep Packet Inspection (DPI) Engine

A high-performance **C++17** based Deep Packet Inspection system that analyzes network traffic from PCAP files, extracts Server Name Indication (SNI) from TLS handshakes, classifies applications, applies blocking rules, and writes filtered packets to a new PCAP file with detailed reports.

## Features

- Multi-layer packet parsing (Ethernet → IPv4 → TCP/UDP)
- Deep Packet Inspection with **TLS SNI extraction** for HTTPS domain identification
- Application classification (YouTube, Facebook, etc.)
- Flexible blocking rules: by IP, by Application, or by Domain
- Flow tracking using 5-tuple (source/dest IP, ports, protocol)
- **Single-threaded** and **Multi-threaded** implementations
- Custom thread-safe queue, load balancer, and producer-consumer pattern
- Detailed statistics and per-thread performance report

## Project Structure

dpi-engine/
├── include/              # Header files
├── src/                  # Source files
│   ├── dpi_mt.cpp        # Multi-threaded main engine
│   ├── main_working.cpp  # Single-threaded version
│   ├── pcap_reader.cpp
│   ├── packet_parser.cpp
│   ├── sni_extractor.cpp
│   └── types.cpp
├── test_dpi.pcap         # Sample input PCAP
├── generate_test_pcap.py # Script to generate test data
├── CMakeLists.txt
└── README.md


## Technologies Used

- **Language**: C++17
- **Concurrency**: Multi-threading with pthreads
- **Networking**: TCP/IP stack, PCAP file format, TLS handshake
- **Data Structures**: Unordered maps, custom thread-safe queues, efficient buffer handling
- **Tools**: Wireshark (for validation)

## How to Build & Run

### Prerequisites
- Linux (recommended) or WSL on Windows
- g++ with C++17 support

### Build

**Single-threaded version:**
```bash
g++ -std=c++17 -O2 -I include -o dpi_simple \
    src/main_working.cpp src/pcap_reader.cpp src/packet_parser.cpp \
    src/sni_extractor.cpp src/types.cpp

### Run Example
./dpi_engine test_dpi.pcap output.pcap --block-app YouTube --block-domain facebook

### Generate Test Data
python3 generate_test_pcap.py