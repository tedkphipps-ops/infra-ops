# Pi-hole Exporter Performance and Connectivity Recovery

## Incident Date

October 31, 2026

## Overview

A secondary Linux infrastructure node experienced
significant memory pressure, excessive swapping,
and intermittent disk I/O delays.

Investigation identified two separate problems:

1. Excessive Pi-hole Exporter resource consumption.
2. Blocked Docker-to-host API connectivity.

Both problems were addressed and functionality restored.

## Performance Investigation

### Initial Observations

- Host RAM utilization: approximately 73-80%.
- Exporter resident memory: approximately 233 MiB.
- Exporter swap consumption: approximately 516 MiB.
- CPU I/O wait periodically approached 90%.
- The system used a mechanical hard drive.

These conditions degraded infrastructure responsiveness.

### Immediate Recovery

The affected Docker container was restarted.

Historical command:

    docker restart pihole-exporter

### Results

- Container returned to running status.
- Exporter resident memory decreased to about 14.2 MiB.
- Exporter swap consumption decreased to zero.
- Host RAM utilization decreased to approximately 36%.
- CPU I/O wait subsequently dropped below 1%.

RAM utilization remained below 40% during
the following observation period.

The restart resolved the immediate resource pressure.

However, the underlying cause of the exporter's
previous memory growth was not conclusively identified.

## Docker Connectivity Investigation

### Symptoms

Following the restart, the exporter experienced
timeouts when requesting data from the Pi-hole API.

Testing established that:

- The host could reach the application API.
- The exporter container could not reach the same API.
- The container used Docker bridge networking.
- The host INPUT firewall blocked the required traffic.

This isolated the connectivity failure to the
container-to-host network path.

### Resolution

A narrowly scoped IPv4 firewall exception was
introduced for the required application traffic.

The exception was positioned before the existing
inbound DROP rule.

The broader firewall restrictions remained in place.

Actual source addresses, destination addresses,
and operational firewall rules remain private.

## Firewall Persistence

The correction was incorporated into the persistent
firewall configuration.

Validation included:

- Live firewall rule inspection.
- Firewall packet counter verification.
- Saved rule ordering inspection.
- Successful iptables-restore syntax validation.
- Confirmation that netfilter-persistent was enabled.

The exception was also verified after reboot.

## Functional Validation

After restoring connectivity:

- Pi-hole API communication succeeded.
- Pi-hole Exporter metrics returned HTTP 200.
- Docker remained operational.
- DNS services remained active.
- Samba storage services remained operational.
- The NAS filesystem remained mounted.
- Grafana and related monitoring services remained active.
- Availability monitoring remained healthy.

The exporter metrics endpoint returned HTTP 200
during the final connectivity validation.

## Outcome

The immediate performance degradation was reduced,
and Pi-hole Exporter functionality was restored.

The Docker-to-host firewall correction was validated,
persisted, and verified following a reboot.

No exporter software upgrade was performed
during this recovery.

Long-term exporter memory stability requires
continued observation because the original
memory-growth mechanism was not established.

## Lessons Learned

- Container health depends on both resource usage
  and network connectivity.
- Host connectivity does not guarantee that a
  container can reach the same destination.
- Firewall hardening must account for Docker
  bridge and container-to-host traffic.
- Persistent firewall changes require validation.
- Reboot testing confirms configuration persistence.
- Short-term recovery does not establish root cause.

## Documentation Security

Operational IP addresses, firewall exceptions,
private network configuration, and access details
are intentionally excluded.

See docs/security/public-repository-policy.md
for publication requirements.
