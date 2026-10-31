# RN Pi-hole Exporter Performance Recovery

## October 31, 2026

### System

- Host: redundant-net
- Address: REDACTED_PRIVATE_IP
- Docker container: pihole-exporter
- Image at time of repair: ekofr/pihole-exporter:latest

## Incident

RN experienced sustained memory pressure, excessive swapping, and high CPU I/O wait.

Observed symptoms included:

- RAM consumption approximately 73-80%.
- Pi-hole Exporter process RSS approximately 233 MiB.
- Pi-hole Exporter process swap approximately 516 MiB.
- CPU I/O wait periodically approaching 90%.
- Slow I/O involving RN's mechanical system HDD.

## Performance Recovery

The Pi-hole Exporter container was restarted using:

    docker restart pihole-exporter

Immediately afterward:

- Container status: running.
- Exporter RSS: approximately 14.2 MiB.
- Exporter swap: 0 MiB.
- Glances RAM utilization: approximately 36%.
- CPU I/O wait subsequently observed below 1%.

RAM utilization remained below 40% during the following day's observation.

The restart relieved the immediate resource pressure. Long-term memory stability was not established by this observation alone.

## Exporter Connectivity Repair

After restarting, the exporter could not complete requests to the RN Pi-hole API.

Logs showed repeated timeouts contacting:

    http://REDACTED_PRIVATE_IP:80/api/auth

Testing confirmed:

- RN host to Pi-hole API: HTTP 400, reachable.
- Exporter network namespace to Pi-hole API: connection timeout.
- Cause of connectivity failure: Docker-to-host traffic was not permitted by RN's INPUT firewall allowlist.

The exporter was connected to Docker's default bridge network.

A targeted IPv4 firewall exception was installed:

    -A INPUT -i docker0 -s REDACTED_PRIVATE_IP/16 -d REDACTED_PRIVATE_IP/32 -p tcp -m tcp --dport 80 -j ACCEPT

The exception precedes the existing INPUT DROP rule.

This permits containers on the default Docker bridge to reach RN's local Pi-hole API on TCP port 80. It does not grant unrestricted inbound LAN access.

## Firewall Persistence

The verified exception was added to:

    /etc/iptables/rules.v4

A backup was created:

    /etc/iptables/rules.v4.rn-exporter-backup

Validation completed:

- Live firewall exception verified with packet counters.
- Saved exception verified immediately before INPUT DROP.
- iptables-restore --test passed.
- netfilter-persistent confirmed enabled.

The firewall exception was subsequently verified present after the October 31 reboot using iptables -C. Pi-hole Exporter metrics also returned HTTP 200 after reboot.

## Final Functional Validation

After connectivity was restored, Pi-hole Exporter was restarted again.

The metrics endpoint returned:

    HTTP 200 in approximately 0.259 seconds

The October 31 RN health check confirmed:

- pihole-FTL: active
- unbound: active
- smbd: active
- docker: active
- /mnt/rn-nas: mounted
- Pi-hole Exporter: HTTP 200
- Uptime Kuma: healthy
- Grafana: running
- Promtail: running
- Loki: running

## Result

RN's immediate performance degradation was substantially reduced, Pi-hole Exporter metrics were restored, and the firewall correction was saved and validated.

The precise cause of the exporter's prior memory growth was not conclusively established. No exporter software upgrade was performed during this repair.
