---
authors:
  - ChatGPT
status: note
tags:
  - network
---

Xray is a modular proxy platform that separates **protocols** (such as VMess, VLESS, or Trojan) from the **transport layers** that carry them. This distinction matters because the transport layer determines how traffic is transmitted over the network, how it looks to observers, and how it performs under different conditions. Understanding these transport options helps users choose the most appropriate configuration for their environment.

## TCP

The Transmission Control Protocol (TCP) is the simplest and most widely supported transport layer in Xray. It provides reliable, ordered delivery of data and is recognized by virtually every network device. Because TCP is so common, traffic sent this way blends in with normal internet activity. However, it is more vulnerable to detection by deep packet inspection and may be less efficient on unreliable networks.

- Reliable and widely supported.
- Easy to deploy without special configuration.
- Can be throttled or flagged by censors in restrictive environments.

## WebSocket

WebSocket (WS) is a transport that packages proxy traffic inside what looks like ordinary web communication. It is particularly useful in environments where HTTPS traffic is common and rarely blocked. Because WebSocket traffic often runs over port 443, it is difficult to distinguish from secure web browsing.

- Resembles normal HTTPS traffic.
- Works well behind firewalls or content filters.
- Slightly more overhead than raw TCP.

## gRPC

gRPC is a newer transport built on HTTP/2. It allows multiplexing many streams over a single connection, which improves efficiency when running multiple sessions simultaneously. Like WebSocket, it can masquerade as ordinary web traffic, but with stronger support for multiplexed connections.

- Multiplexing improves performance with concurrent sessions.
- Appears as HTTP/2 traffic, common in modern web applications.
- Requires server environments that support HTTP/2 properly.

## QUIC

QUIC is a transport protocol built on UDP. It supports multiplexing, reduced latency, and better performance on unstable networks. Because it avoids some of TCP’s overhead, QUIC can deliver smoother streaming and faster recovery from packet loss. However, its reliance on UDP makes it more visible in certain restricted networks where UDP traffic is closely monitored.

- Low latency and fast reconnection.
- Strong performance on lossy or mobile networks.
- More easily targeted in environments where UDP is restricted.

## XTLS and Reality

XTLS is an extension of TLS that allows more efficient transmission of encrypted data by reducing redundant encryption layers. Reality builds on TLS obfuscation, making encrypted proxy traffic look indistinguishable from genuine HTTPS. These methods are not transport protocols themselves but are applied alongside transports to increase concealment and efficiency.

- Reduce overhead compared to double encryption.
- Make traffic mimic authentic HTTPS sessions.
- Provide resilience against deep packet inspection.

## Conclusion

Transport layers in Xray define not only how traffic is delivered but also how it appears to the outside world. TCP offers simplicity and compatibility, WebSocket and gRPC disguise proxy use as common web activity, QUIC delivers speed and resilience, while XTLS and Reality enhance both privacy and performance. The choice of transport depends on the environment: stable versus unstable networks, permissive versus restrictive censorship, and the need for stealth versus raw performance. Instead of a single “best” option, Xray offers a toolkit of transports that can be matched to the challenges of real-world internet use.
