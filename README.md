# Penetration Testing Report: Footprinting & Network Scanning Phases

**W2-PM-FINAL | Cybersecurity | Networkwalks**

| Field | Details |
|---|---|
| **Pentester** | Ebere Precious Chibugo (Perry) |
| **Program / Batch** | Networkwalks |
| **Date** | 12 November 2026 |
| **Modules Completed** | W2-PM1 (Multiple Kali Tools) & W2-PM5 (Zenmap Scanning) |
| **Client / Target** | 1. Networkwalks (authorized educational scope) 2. Local LAN (192.168.1.0/24) |
| **Permission Secured** | Yes |
| **Phases Covered** | Phase 1 (Reconnaissance & Footprinting), Phase 2 (Scanning & Network Discovery) |

## 1. Introduction & Disclaimer
All testing was performed strictly within authorized educational boundaries. **No exploitation was carried out.**
The objective was to combine passive/active domain footprinting (W2-PM1) with local network discovery (W2-PM5) to map out an environment.

> ⚠️ This repository is for educational purposes only. Only scan systems you own or have explicit written permission to test.

## 2. Tools Used
- **Operating systems:** Kali Linux, Windows
- **Footprinting:** WHOIS, WhatWeb, nslookup, curl -I, wafw00f, dnsrecon
- **Scanning:** Zenmap (Nmap GUI), Windows CMD/PowerShell (`ipconfig`)

## 3. Activities Performed

### 3.1 Footprinting & Reconnaissance

| Tool | Command | Key Findings |
|---|---|---|
| WHOIS | `whois networkwalks.com` | Registrar GoDaddy; created 2019-11-06, expires 2027-11-06; name servers on HostGator/DomainControl; owner details hidden by privacy proxy |
| nslookup | `nslookup networkwalks.com` | Resolved to 192.........135 |
| dnsrecon | `dnsrecon -d networkwalks.com` | SOA, NS, MX, A, TXT, SRV records found; BIND version disclosed (9.16.23-RH); SPF uses `~all`; DNSSEC not signed |
| WhatWeb | `whatweb networkwalks.com` | Apache, WordPress, jQuery, Google Tag Manager, Bootstrap; HTTP redirects to HTTPS |
| curl -I | `curl -I https://networkwalks.com` | HTTP/2 200; cookie has `Secure` and `HttpOnly`; Apache server; no HSTS header observed |
| wafw00f | `wafw00f networkwalks.com` | Behind ModSecurity (SpiderLabs) WAF |

### 3.2 Network Scanning with Zenmap
1. Identified local IP (`192.......50`) and subnet (`192.......0/24`) using `ipconfig`.
2. Ran a ping scan: `nmap -sn 192.......0/24`
3. Discovered **2 live hosts**:
   - `192.......1`: Gateway/Router (MAC 34:A5:B4:04:C7:8E, Navtech PTE)
   - `192.......50`: Local host
4. Generated the network topology diagram and exported it as `Nmap - Zenmap.pdf`.

## 4. Risk Analysis

| # | Finding | Risk | Impact |
|---|---|---|---|
| 1 | WHOIS exposes registrar, dates and name servers | Low | Public by design; owner data is privacy-protected |
| 2 | DNS server discloses BIND version | Low | Helps attackers target known version issues |
| 3 | SPF set to softfail (`~all`) | Low | Weaker protection against email spoofing |
| 4 | Technology stack (WordPress, plugins, jQuery) fingerprinted | Medium | Enables targeted attacks if components are outdated |
| 5 | WAF identified; no HSTS header observed | Low | Attacker can plan around the WAF; missing header weakens HTTPS enforcement |
| 6 | 2 live hosts visible on local network | Medium | Reveals network layout to anyone on the LAN |

*No exploitation was performed. Ratings reflect information exposure only.*

## 5. Recommendations
1. Minimize public information exposure (hide version banners and verbose headers).
2. Keep the CMS, plugins and server software patched.
3. Implement security headers (HSTS, CSP, X-Content-Type-Options).
4. Maintain DNS hygiene: hide BIND version, tighten SPF to `-all`, add DKIM/DMARC, consider DNSSEC.
5. Monitor WAF logs for recon and probing activity.
6. Run regular internal network scans to maintain an asset inventory.
7. Investigate any unknown or unexpected devices immediately.
8. Document all findings, commands and results clearly.
9. Only test systems with explicit, documented authorization.

## 6. Conclusion
Footprinting maps what exists and scanning shows what is alive. Together they form the foundation of any assessment before exploitation. Clear documentation and operating strictly within authorized scope are what make the work professional and repeatable.

## 7. Evidence

| Evidence | File |
|---|---|
| WHOIS | `screenshots/whois-1.png`, `whois-2.png`, `whois-3.png` |
| WhatWeb | `screenshots/whatweb.png` |
| nslookup | `screenshots/nslookup.png` |
| curl -I | `screenshots/curl.png` |
| wafw00f | `screenshots/wafw00f.png` |
| dnsrecon | `screenshots/dnsrecon.png` |
| ipconfig | `screenshots/ipconfig.png` |
| Zenmap output (2 hosts up) | `screenshots/zenmap-output.png` |
| Zenmap topology | `screenshots/zenmap-topology.png` |

Full report: [`W2-PMFINAL.pdf`](./W2-PMFINAL.pdf) | Topology: [`Nmap - Zenmap.pdf`](./Nmap%20-%20Zenmap.pdf)
