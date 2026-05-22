# Redundant Network Infrastructure

This directory documents the implementation of a high-availability, dual-node DNS architecture within my home lab. By deploying a secondary recursive DNS server, I have eliminated single points of failure, ensuring consistent network resolution and service continuity.

## Architecture Overview
* **Primary Node:** Lab Server (`192.168.1.226`) - Handles standard DNS resolution and blocking.
* **Secondary Node (RN):** Toshiba Laptop (`192.168.1.238`) - Serves as an active failover DNS resolver.
* **Recursive Engine:** [Unbound](https://unbound.net/) installed on both nodes to bypass third-party upstream providers (Google/Cloudflare), enhancing privacy and performance.

## Key Features
* **Zero-Downtime Design:** Devices are configured to failover automatically to the secondary node, maintaining internet access if the primary server goes offline.
* **Recursive Resolution:** Both nodes function as full recursive resolvers, bypassing ISP-level tracking and increasing query speed through local caching.
* **Standardized Configuration:** Both nodes utilize identical Pi-hole filtering lists and Unbound security parameters for consistent performance across the network.

## Configuration Highlights
* **Unbound Port:** Configured to `5335` to avoid conflicts with `systemd-resolved`.
* **Failover Implementation:** Secondary DNS addresses assigned via router DHCP/client settings to ensure immediate transition during primary node outages.
* **Environment:** Built on Linux-based architecture with hardware-optimized performance settings.

## Validation & Testing
* **Verification:** Successfully routed traffic from a high-bandwidth device through the secondary RN node.
* **Failover Testing:** Confirmed system responsiveness and query resolution during simulated primary node downtime.
* **Performance:** Observed consistent, low-latency DNS resolution and accurate block-list application across multiple client types (mobile and gaming console).
