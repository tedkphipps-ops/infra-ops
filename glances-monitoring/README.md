# Glances Infrastructure Monitoring

## Overview

This module documents the deployment and operational integration of Glances within the `infra-ops` infrastructure environment.

Glances provides lightweight, real-time infrastructure observability across core infrastructure nodes, enabling centralized monitoring of system performance, resource utilization, and service health.

---

## Infrastructure Role

Glances functions as the primary lightweight monitoring platform for operational visibility across the infrastructure environment.

The platform provides monitoring for:

- CPU utilization
- Memory usage
- Disk activity
- Network throughput
- Running processes
- Service health
- System uptime and resource availability

---

## Monitored Infrastructure Nodes

| Node | Role | Access Endpoint |
|------|------|-----------------|
| `infra-hub` | Primary Infrastructure Node | `<infra-hub-ip>:61208` |
| `redundant-net` | Secondary Infrastructure Node | `<redundant-net-ip>:61208` |

---

## Deployment Objectives

- Establish centralized infrastructure visibility
- Monitor system resource utilization in real time
- Improve operational awareness across infrastructure nodes
- Support troubleshooting and performance analysis
- Prepare for future monitoring expansion and alerting integration

---

## Installation & Deployment

### System Preparation

Update system packages and install required Python dependencies:

```bash
sudo apt update && sudo apt install python3-pip -y
```

---

### Install Glances

Install Glances with web interface support:

```bash
sudo pip3 install glances[web] --break-system-packages
```

---

## Firewall Configuration

Allow inbound access to the Glances monitoring web interface:

```bash
sudo ufw allow 61208/tcp
```

---

## Systemd Service Configuration

To ensure Glances automatically starts during system boot, create the following service file:

```bash
sudo nano /etc/systemd/system/glances.service
```

Insert the following configuration:

```ini
[Unit]
Description=Glances Infrastructure Monitoring
After=network.target

[Service]
ExecStart=/usr/local/bin/glances -w
Restart=always

[Install]
WantedBy=multi-user.target
```

---

## Service Initialization

Reload the systemd daemon and enable the service:

```bash
sudo systemctl daemon-reload
sudo systemctl enable glances
sudo systemctl start glances
```

---

## Validation & Testing

Verify the monitoring service is operational:

```bash
systemctl status glances
```

Access the monitoring dashboard through a web browser:

```text
http://<infra-hub-ip>:61208
```

---

## Infrastructure Integration

Glances operates alongside other infrastructure services within the `infra-ops` ecosystem, including:

- `pihole-setup`
- `unbound`
- `redundant-net`
- `samba-nas`

The monitoring platform provides operational visibility into the overall health and performance of the infrastructure environment.

---

## Future Improvements

Planned future enhancements include:

- Centralized alerting integration
- Expanded node monitoring coverage
- Historical metrics collection
- Dashboard aggregation
- Infrastructure visualization integration
- Container monitoring support

---

## Operational Notes

The deployment of Glances marked the introduction of centralized operational observability across the infrastructure environment.

This service supports proactive infrastructure management, troubleshooting, and long-term operational scalability across the `infra-ops` ecosystem.
This monitoring layer establishes the foundation for future centralized infrastructure observability and operational analytics.
