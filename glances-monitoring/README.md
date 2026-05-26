# Server Monitoring with Glances

This directory contains the documentation and service configuration for deploying Glances across the infrastructure nodes. Glances provides a real-time, browser-based dashboard to monitor system health, CPU, memory, and network performance.

## Monitored Nodes
* blackhole: 192.168.1.226:61208
* redundantnetwork: 192.168.1.238:61208

## Deployment Guide

### 1. Installation
Update your package list and install the necessary Python tools, then install Glances with web support:

# Update and install pip
sudo apt update && sudo apt install python3-pip -y

# Install Glances with web dependencies
sudo pip3 install glances[web] --break-system-packages

### 2. Configure Firewall
Allow traffic through the Glances web port:

sudo ufw allow 61208/tcp

### 3. Automate as a System Service
To ensure Glances starts on boot, create a systemd service file:

sudo nano /etc/systemd/system/glances.service

Paste the following configuration into the file:

[Unit]
Description=Glances
After=network.target

[Service]
ExecStart=/usr/local/bin/glances -w
Restart=always

[Install]
WantedBy=multi-user.target

Save and exit (Ctrl+O, Enter, Ctrl+X), then enable and start the service:

sudo systemctl daemon-reload
sudo systemctl enable glances
sudo systemctl start glances

### 4. Verification
Verify the service is running:

systemctl status glances

Access the dashboard via your browser at http://<server-ip>:61208.
