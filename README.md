# Footprinting & Network Scanning – Networkwalks Internship (Week 2)

**Author:** Ahmed Hussien Said Hossam Eldin
**Program:** Cybersecurity & Ethical Hacking internship, Networkwalks Academy (BO84)
**Modules:** W2-PM1 (footprinting with Kali Linux tools) and W2-PM5 (network scanning with Zenmap)

This repo has my Week 2 report and the screenshots I collected as evidence. The full write-up is in [`report/`](report).

> All activities were done as part of the assigned lab: footprinting was run against networkwalks.com, the target given in the internship task, and the Zenmap scan was run on my own home network. Don't run these tools against systems you don't have permission to test.

## Part 1 – Footprinting networkwalks.com

| Tool | Command | What I found |
|---|---|---|
| WHOIS | `whois networkwalks.com` | Registered through GoDaddy (created 2019-11-06, expires 2027-11-06). Name servers are NS6135/NS6136.HOSTGATOR.COM. DNSSEC is unsigned and the registrant is hidden behind a privacy service. |
| WhatWeb | `whatweb networkwalks.com` | WordPress 7.1.4 and WordPress Download Manager 3.3.76 on Apache, with jQuery 3.7.1 and Google Tag Manager. HTTP redirects to HTTPS. A contact e-mail address is exposed on the page. |
| Nslookup | `nslookup networkwalks.com` | Resolves to 192.232.216.135. |
| curl | `curl -I https://networkwalks.com` | HTTP/2 200 from Apache, `x-nginx-cache: WordPress` header, a `__wpdm_client` cookie, and the WordPress REST API endpoint `/wp-json/` in the `link` header. |
| Wafw00f | `wafw00f networkwalks.com` | The site is behind ModSecurity (SpiderLabs). |
| DNSRecon | `dnsrecon -d networkwalks.com` | SOA/NS records, mail server `mail.networkwalks.com`, SPF and Google verification TXT records, 8 SRV records for cPanel e-mail autodiscovery, BIND version 9.16.23-RH on both name servers, and no DNSSEC answer. |

## Part 2 – Network scanning with Zenmap

- Found my IP address and subnet with `ipconfig /all`: `192.168.1.4`, mask `255.255.255.0`, gateway `192.168.1.1`.
- Ran a Ping scan in Zenmap (`nmap -sn 192.168.1.0/24`, Nmap 7.98).
- Result: 256 addresses scanned, **4 hosts up** in 3.87 seconds. They are the router, my PC and two other devices on the network.
- One device uses a randomised MAC address, so Nmap could not identify its vendor.
- Saved the network topology as a PDF: [`topology/zenmap-topology.pdf`](topology/zenmap-topology.pdf).

IP and MAC details for each host are in the report.

## Findings and recommendations (short version)

These are observations, not confirmed vulnerabilities. No exploitation was done in these modules.

- Software names and versions (WordPress, plugin, Apache) are visible to anyone, so they should be kept updated and reviewed against security advisories.
- Response headers and DNS records expose some technical detail that is worth reviewing.
- The WAF (ModSecurity) is doing its job and should stay enabled and tuned.
- On the local network, any unknown device found by a scan should be investigated, and network documentation should be kept up to date.

## Repo contents

```
.
├── README.md
├── report/        Week 2 report (PDF and Word)
├── screenshots/   Evidence screenshots for each tool
└── topology/      Zenmap topology exported as PDF
```

## Related

- [Week 1 – Lab setup (VirtualBox, Kali Linux, Windows 10, Android)](https://github.com/ahmedhussienhossam-droid/networkwalks-lab-setup)
- [Wireshark SOC labs](https://github.com/ahmedhussienhossam-droid/wireshark-soc-labs)
