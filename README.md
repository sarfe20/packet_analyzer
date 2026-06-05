# Packet Analyzer

Packet Analyzer is a C++ networking project that analyzes packets stored inside PCAP files. The goal of the project is to understand how network traffic is structured, which protocols are being used, and which application or domain the traffic may belong to.

The project is based on the basic idea of Deep Packet Inspection (DPI). A simple firewall usually checks only IP addresses or ports, while DPI looks deeper into useful packet information. This project reads packet headers, ports, protocols, and domain-related data to classify network traffic.

## Main Idea

Internet traffic moves in small packets. Each packet contains information such as source IP, destination IP, port, protocol, and payload. Packet Analyzer reads these packets and extracts meaningful details from them.

High-level flow:

```text
PCAP file -> Packet reading -> Header parsing -> Domain/app detection -> Rule checking -> Summary
```

## What The Project Analyzes

- Where a packet is coming from and where it is going.
- Whether the packet uses TCP or UDP.
- Source and destination ports.
- Whether the traffic looks like HTTP, HTTPS, DNS, or a known application.
- Whether an HTTPS packet exposes a domain through the TLS SNI field.
- Whether HTTP traffic contains a Host header.
- Whether a DNS query contains a domain name.
- Whether the traffic matches any blocking rule.

## Important Concepts

### PCAP File

A PCAP file is a packet capture file. It stores real or test network packets. This project reads that file and processes the packets one by one.

### Packet Parsing

Packet parsing means converting raw bytes into readable network information. In this project, Ethernet, IPv4, TCP, and UDP headers are parsed.

### FiveTuple

A FiveTuple identifies a network connection. It contains the source IP, destination IP, source port, destination port, and protocol. This helps the project understand which packets belong to the same connection.

### SNI Detection

HTTPS traffic is encrypted, but during the beginning of the TLS handshake, the SNI field may reveal the domain name. The project uses this information to map traffic to a possible application or domain.

Example:

```text
www.youtube.com -> YouTube
github.com -> GitHub
www.facebook.com -> Facebook
```

### Flow Tracking

A single connection can contain many packets. Flow tracking groups packets from the same connection and keeps track of their state.

### Rule Checking

The project also includes logic for allowing or blocking traffic based on rules. Rules can be based on IP address, port, application type, or domain pattern.

## Main Parts Of The Project

- `pcap_reader`: Reads packets from a PCAP file.
- `packet_parser`: Parses raw packet bytes into network headers.
- `sni_extractor`: Extracts domain-related data from HTTPS, HTTP, and DNS traffic.
- `types`: Defines common data structures and application classification.
- `rule_manager`: Manages blocking rules.
- `connection_tracker`: Tracks network flows.
- `load_balancer` and `fast_path`: Handle multi-threaded packet processing.
- `dpi_engine`: Coordinates the complete DPI system.

## Short Summary

Packet Analyzer is a learning-focused networking project. It reads packets, extracts protocol details, tries to identify domains and applications, tracks flows, and shows basic traffic filtering behavior through rules. The main focus is to demonstrate packet parsing, DPI concepts, flow tracking, and traffic classification in a simple way.
