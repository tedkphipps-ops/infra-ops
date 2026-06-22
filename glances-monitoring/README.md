# Glances Infrastructure Monitoring

## Overview

This module documents the deployment and operational integration of Glances within the `infra-ops` infrastructure environment.

Glances provides lightweight, real-time host monitoring across core infrastructure nodes, delivering visibility into system performance, resource utilization, process activity, and service health.

Glances serves as a complementary monitoring platform alongside Grafana, Prometheus, Loki, Promtail, Node Exporter, Pi-hole Exporter, and Uptime Kuma.

---

## Infrastructure Role

Glances functions as a lightweight host-level monitoring platform that provides immediate operational visibility into infrastructure resources.

The platform provides monitoring for:

* CPU utilization
* Memory usage
* Disk activity
* Network throughput
* Running processes
* Service health
* System uptime and resource availability

---

## Monitored Infrastructure Nodes

| Node            | Role                          | Access Endpoint            |
| --------------- | ----------------------------- | -------------------------- |
| `infra-hub`     | Primary Infrastructure Node   | `<infra-hub-ip>:61208`     |
| `redundant-net` | Secondary Infrastructure Node | `<redundant-net-ip>:61208` |

---

## Deployment Objectives

* Establish centralized infrastructure visibility
* Monitor system resource utilization in real time
* Improve operational awareness across infrastructure nodes
* Support troubleshooting and performance analysis
* Prepare for future monitoring expansion and alerting integration

---

## Installation & Deployment

### System Preparation

Update system packages and install required Python dependencies:

```bash
sudo apt update && sudo apt install python3-pip -y
```

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

Create the Glances service:

```bash
sudo nano /etc/systemd/system/glances.service
```

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

```bash
sudo systemctl daemon-reload
sudo systemctl enable glances
sudo systemctl start glances
```

---

## Validation & Testing

Verify service status:

```bash
systemctl status glances
```

Access the web interface:

```text
http://<infra-hub-ip>:61208
```

---

## Infrastructure Integration

Glances operates alongside the broader observability stack:

* Grafana
* Prometheus
* Loki
* Promtail
* Node Exporter
* Pi-hole Exporter
* Uptime Kuma

Glances also supports visibility into infrastructure services including:

* Pi-hole
* Unbound
* Samba NAS
* Docker workloads
* Infrastructure host resources

---

## Screenshot References

Current monitoring screenshots are maintained within:

```text
docs/monitoring/screenshots/
```

Available monitoring screenshots include:

* hub-glances-dashboard.png
* rn-glances-dashboard.png
* infra-hub-grafana-dashboard.png
* rn-grafana-dashboard.png

---

## Future Improvements

Planned future enhancements include:

* Additional alerting integrations
* Expanded infrastructure monitoring coverage
* Future VLAN and subnet monitoring
* Additional service health checks
* Monitoring standardization across future infrastructure nodes
* Long-term observability and analytics enhancements

---

## Operational Notes

Glances was the first monitoring platform deployed within the homelab and served as the foundation for the broader observability stack that now includes Grafana, Prometheus, Loki, Promtail, Node Exporter, Pi-hole Exporter, and Uptime Kuma.

The platform continues to provide fast host-level visibility for operational troubleshooting, performance validation, and infrastructure health monitoring across both infrastructure nodes.
