Deployment and operations
=================================

The existing handoff requires a separate-device scheduled backup, periodic
off-site scientific-data and database copies, integrity checking, and a tested
restore procedure. Monitoring includes storage capacity, database health,
backup age, failed jobs/ingestion, certificate expiry, and node last contact
and delivery status. RAID is not a backup.

Continuous acquisition depends on explicit capacity, retention, backup, and
restore requirements. Destinations, intervals, acceptable data loss, recovery
times, quotas, and alert policies remain OD-11 through OD-15.

João and Henrique's administrative access uses a private network such as
Tailscale or WireGuard and individual credentials. Disable password SSH and
public port 22 only after key/private-network access is verified. No network,
SSH, firewall, container, or service configuration is changed here.

Do not commit credentials, tokens, private keys, or production secrets.
