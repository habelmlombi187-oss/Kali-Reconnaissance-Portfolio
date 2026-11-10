
# Kali Linux Reconnaissance & Footprinting Portfolio

A documented, authorization-based reconnaissance and local network host-discovery portfolio completed in a Kali Linux virtual machine running on VirtualBox. The work is presented as a learning and internship portfolio for the **Networkwalks Cybersecurity Internship (Batch B084)**.

> **Authorization and scope notice:** Perform tests only where you have explicit, current permission. The external-domain activities in this repository are limited to the scope and low-impact techniques recorded in the supplied portfolio. Zenmap/Nmap host discovery is restricted to the tester's own network. Do not expand the scope, run intrusive scripts, exploit services, or perform vulnerability testing without separate written approval.

## 1. Project Objectives

- Collect publicly available domain-registration and DNS information.
- Identify publicly observable web technologies and HTTP response headers.
- Check whether a web application firewall (WAF) is detectable using a fingerprinting tool.
- Perform DNS record enumeration within the approved scope.
- Use Zenmap/Nmap host discovery to identify responding devices on the tester's own local network.
- Preserve commands, outputs, screenshots, scope, and authorization evidence in a reproducible repository.

## 2. Authorization and Rules of Engagement

| Field | Details |
|---|---|
| Authorization Reference | `NW-LOA-B082-017` |
| Date Recorded | 17 August 2026 |
| Program | Networkwalks Cybersecurity Internship — Batch B082 |
| Client / Target Owner | Networkwalks |
| Client Representative | Sonia John, Managing Director |
| Authorized Tester |  |
| External Target | `networkwalks.com` |
| Local Network Scope | Tester-owned LAN only; source portfolio records `10.0.0.0/24` and `192.168.0.0/24` |
| Approved Activities | WHOIS, web fingerprinting, DNS lookups, HTTP header inspection, WAF fingerprinting, DNS enumeration, and local Zenmap ping sweeps |


## 3. Target and Environment

### 3.1 External Web Target

| Parameter | Value Recorded |
|---|---|
| Domain | `networkwalks.com` |
| Resolved IPv4 Address | `192.232.216.135` |
| Web Server | Apache |
| CMS | WordPress `7.1.4` |
| Additional Component | WordPress Download Manager `3.3.76` |
| WAF Fingerprint | ModSecurity (SpiderLabs) |
| Name Server | `ns6135.hostgator.com` (`50.87.144.87`) |
| Mail Host | `mail.networkwalks.com` |
| Other DNS Results | SPF TXT record and SRV/autodiscovery records |

These are findings transcribed from the supplied portfolio, not a claim that the records remain current. DNS, hosting, software versions, and security controls can change.

### 3.2 Local Network Discovery

| Address | Description |
|---|---|
| `127.0.0.1` | Loopback address of the Kali host; not a separate LAN device |
| `10.0.0.2` | Active virtual-network node reported in the NAT network |
| `10.0.0.3` | Active virtual-network node reported in the NAT network |
| `192.168.0.1` | Local Wi-Fi gateway/router reported in the portfolio |

The two `/24` ranges are historical scope details from the supplied document. Check your current interface addresses and routing before scanning; do not assume those subnets are still your own network.

## 4. Tools Used

| Tool | Purpose |
|---|---|
| `whois` | Public domain-registration and registrar information |
| `whatweb` | Web technology fingerprinting |
| `nslookup` | DNS name and address lookups |
| `curl -I` | Retrieve HTTP response headers without downloading the page body |
| `wafw00f` | Identify detectable web application firewall signatures |
| `dnsrecon` | DNS record enumeration |
| Zenmap / Nmap | Visual and command-line host discovery on the tester's own network |

## 5. Repository Structure

```text
kali-recon-portfolio/
│
├── README.md
├── REPORT.md
│
├── docs/
│   ├── AUTHORIZATION_EVIDENCE_README.md
│   └── NW-LOA-B082-017_Authorization.pdf
│
├── recon/
│   ├── whois_output.txt
│   ├── whatweb_output.txt
│   ├── nslookup_output.txt
│   ├── curl_headers.txt
│   ├── wafw00f_output.txt
│   └── dnsrecon_output.txt
│
├── scans/
│   ├── zenmap_activity.md
│   └── nmap_host_discovery.txt
│
└── screenshots/
    └── README.md
```

## 6. Reproducing the Reconnaissance

Run these commands only after confirming that the target and techniques are covered by written authorization. Save the terminal output to the corresponding file.

```bash
whois networkwalks.com | tee recon/whois_output.txt

whatweb https://networkwalks.com | tee recon/whatweb_output.txt

nslookup networkwalks.com | tee recon/nslookup_output.txt

curl -I https://networkwalks.com | tee recon/curl_headers.txt

wafw00f https://networkwalks.com | tee recon/wafw00f_output.txt

dnsrecon -d networkwalks.com | tee recon/dnsrecon_output.txt
```

These commands are examples for reproducing the documented low-impact checks. Use the exact scope and rate limits in the signed authorization. Do not add brute-force wordlists, zone-transfer attempts, exploit modules, or intrusive options unless expressly authorized.

## 7. Zenmap: Local Network Host Discovery

Zenmap is the graphical interface for Nmap. For a basic ping/host-discovery sweep on a network you own or are explicitly authorized to assess:

```bash
nmap -sn 192.168.0.0/24 -oN scans/nmap_host_discovery.txt
```

If your own VirtualBox NAT network is currently `10.0.0.0/24`, you may perform a separate discovery scan:

```bash
nmap -sn 10.0.0.0/24 -oN scans/nmap_nat_host_discovery.txt
```

### Zenmap Procedure

1. Open Zenmap in Kali Linux.
2. Enter the confirmed local subnet in the **Target** field.
3. Select **Ping scan** (equivalent to `-sn`) as the profile.
4. Confirm the target is your own network and that the range is correct.
5. Run the scan and review **Hosts**, **Host Details**, and **Topology**.
6. Save the scan output in `scans/` and take a screenshot for `screenshots/`.

A ping scan discovers hosts that respond to the probes available on the local network. A device may be online but not appear if it blocks or does not answer those probes. A host-discovery scan does not establish that a host is vulnerable.

## 8. Findings Summary

The supplied portfolio reports:

- **WHOIS:** GoDaddy.com, LLC as registrar; domain creation date `2019-11-06`; expiry date `2027-11-06`; privacy-protected registrant details.
- **WhatWeb:** HTTP `200 OK`; Apache; WordPress `7.1.4`; page title `Networkwalks Academy`; IP `192.232.216.135`.
- **HTTP Headers:** HTTP/2 `200`; Apache server header; `x-nginx-cache: WordPress`; `__wpdm_client` cookie.
- **WAFW00F:** ModSecurity (SpiderLabs) was identified.
- **DNSRecon:** A, SOA, TXT/SPF and SRV/autodiscovery information was reported.
- **Zenmap/Nmap:** The source portfolio lists `10.0.0.2`, `10.0.0.3`, and `192.168.0.1` as discovered local network nodes, plus `127.0.0.1` as loopback.

See [`REPORT.md`](REPORT.md) for the formal assessment report and [`scans/zenmap_activity.md`](scans/zenmap_activity.md) for the local discovery procedure.

## 9. Disclaimer

This repository is an educational record of scoped reconnaissance and local host discovery. It is not a penetration-test certification, a guarantee that the target is secure or insecure, or evidence of exploitation. All testing must remain within the written authorization and applicable law.
