
# Ubuntu Server & Pi-hole Deployment Log

## Project Overview
This home lab project documents the clean installation of a dedicated Ubuntu Linux environment configured to run a network-wide Pi-hole DNS ad-blocker. The purpose of this lab is to practice headless Linux administration, static network configuration, and local traffic optimization.

## Phase 1: Bare-Metal Ubuntu OS Installation
- **Platform:** Dedicated Laptop (Host Name: `blackhole`)
- **OS:** Ubuntu Linux (Minimal Server/Desktop footprint)
- **Installation Media:** Bootable USB flash drive initialized via Rufus/Etcher.
- **Hardware Boot Sequence:**
  1. Triggered a physical hardware reboot.
  2. Accessed BIOS/Diagnostics menu via physical hardware keys (`F2`/`F12`).
  3. Modified boot priority to launch directly from the external USB storage media.
  4. Followed localized language, partitioning, and clean account initialization steps.

**Access & Security:** Initialized a standard non-root administrative user account and enabled the OpenSSH server component during setup to allow for immediate headless terminal management.

## Phase 2: Core Networking & Static IP Assignment
To ensure the Pi-hole remains reachable as a reliable local DNS gateway, a fixed network footprint must be established:
- **Interface:** Hardwired Ethernet connection for maximum uptime and zero wireless latency.
- **Configuration:** Configured the system to bypass dynamic DHCP pooling and assigned a permanent static local IP address.
- **Upstream Connectivity:** Verified active gateway routing to ensure uninterrupted outbound traffic while local resolution is active.

**Interface Status Validation:** Utilized standard command-line tools to confirm active link state and ensure the interface correctly recognized the assigned static parameters without lease conflicts.

27  ## Phase 3: Pi-hole Automated Deployment
28  Once the baseline operating system was secure and networked, the core binaries were deployed:
29  ```bash
curl -sSL [https://install.pi-hole.net](https://install.pi-hole.net) | bash
```
