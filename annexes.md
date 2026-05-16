# Annexes: Detailed Per-Host Nmap Scans

> These are the full per-host stealth scan outputs (`nmap -vvv -p <port> <ip>`) run against each service identified by RustScan. TTL values of 64 confirm Linux operating systems across all hosts.

---

## infra_net Host Scans

### FTP Server (10.10.30.10) — Port 21/tcp

![Nmap stealth scan of FTP server at 10.10.30.10 confirming port 21 open with Pure-FTPd](screenshots/annex-nmap-ftp-server.png)

---

### OpenLDAP Server (10.10.30.17) — Ports 389/tcp and 636/tcp

![Nmap scan of OpenLDAP server at 10.10.30.17 showing ports 389 and 636 open](screenshots/annex-nmap-openldap.png)

---

### Zabbix/Nginx Server (10.10.30.117) — Ports 80/tcp, 10051/tcp, 10052/tcp

![Nmap scan of Zabbix server at 10.10.30.117 confirming http and zabbix-trapper ports](screenshots/annex-nmap-zabbix.png)

---

### infra_net Full Stealth Scan Summary

![Nmap stealth scan summary across the full infra_net range confirming open ports and OS fingerprint](screenshots/annex-nmap-infra-stealth.png)

---

*Back to [README.md](README.md)*
