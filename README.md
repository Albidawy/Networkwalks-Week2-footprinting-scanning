# 🔍 Week 2: Footprinting & Network Scanning
## Author: Ahmed Umar
**Networkwalks Cybersecurity Program | Batch B08 | Modules W2-PM1 & W2-PM5** 

> ⚠️ **Disclaimer:** Educational use only. All activity was performed on systems I own or have written permission to test (networkwalks.com and my own local LAN). Do not use anything here to break the law.

## 🎯 Scope
- **Phase 1:** Reconnaissance & footprinting of `networkwalks.com` (Kali Linux)
- **Phase 2:** Scanning & network discovery of my local network `192.168.56.0/24` (Zenmap on Windows)

## 🛠️ Tools Used
| Tool | Purpose |
|---|---|
| WHOIS | Domain registration details |
| WhatWeb | Web technology fingerprinting |
| nslookup | Resolve domain to IP |
| curl -I | Read HTTP response headers |
| wafw00f | Detect Web Application Firewall |
| dnsrecon | Enumerate DNS records |
| Zenmap | Discover live hosts and view topology |

---

## 📸 Phase 1: Footprinting

### 1. WHOIS
`whois networkwalks.com`
Registered via GoDaddy (created 2019-11-06, expires 2027-11-06). Name servers are NS6135/NS6136.HOSTGATOR.COM and DNSSEC is unsigned.



![WHOIS](screenshots/1_whois.png)



### 2. WhatWeb
`whatweb networkwalks.com`
Identified WordPress 7.1.4, WordPress Download Manager 3.3.76, Apache, jQuery 3.7.1 and Google Tag Manager. Also showed an HTTP to HTTPS redirect (301).



![WhatWeb](screenshots/2_whatweb.png)



### 3. nslookup
`nslookup networkwalks.com`
The domain resolves to `192.232.216.135`.



![nslookup](screenshots/3_nslookup.png)



### 4. curl -I
`curl -I https://networkwalks.com`
Returned `HTTP/1.1 200 OK` from an Apache server. Headers expose the `/wp-json/` REST API, cache headers and a secure HttpOnly cookie.



![curl](screenshots/4_curl.png)



### 5. wafw00f
`wafw00f networkwalks.com`
The site is protected by **ModSecurity (SpiderLabs)**, detected in 2 requests.



![wafw00f](screenshots/5_wafw00f.png)



### 6. dnsrecon
`dnsrecon -d networkwalks.com`
Enumerated SOA, NS, MX, A, SPF/TXT and SRV records. Both name servers disclose **BIND 9.16.23-RH**, and there was no answer to the DNSSEC query.



![dnsrecon](screenshots/6_dnsrecon.png)



---

## 📸 Phase 2: Network Scanning

### 7. Zenmap ping scan
`nmap -sn 192.168.56.0/24`
Scanned 256 IP addresses in 13.90 seconds and found **1 live host: `192.168.56.1`**.



![Zenmap output](screenshots/7_zenmap_output.png)



### 8. Network topology (with legend)
The topology shows `localhost` connected to the discovered host `192.168.56.1`.



![Zenmap topology](screenshots/8_zenmap_topology.png)



### 9. Topology view


![Zenmap topology view](screenshots/9_zenmap_topology_view.png)



---

## ⚖️ Risk Analysis

| # | Finding | Evidence | Risk |
|---|---|---|---|
| 1 | Web technology information exposed | WordPress 7.1.4, WP Download Manager 3.3.76, jQuery 3.7.1, Apache | 🟠 Medium |
| 2 | Server IP identifiable | nslookup resolved `192.232.216.135` | 🟡 Low |
| 3 | HTTP technical information exposed | `/wp-json/`, Apache server and cache headers | 🟡 Low |
| 4 | WAF technology identifiable | ModSecurity (SpiderLabs) | 🟡 Low |
| 5 | DNS and domain information exposed | NS, MX, SPF, SRV records, BIND 9.16.23-RH, unsigned DNSSEC | 🟠 Medium |
| 6 | Live host visible on local network | 1 live host (`192.168.56.1`) | 🟡 Low |

These are observations, not confirmed vulnerabilities. This work covered information gathering and host discovery only, and **no exploitation was performed**.

## ✅ Recommendations
1. Review what technology information is publicly exposed
2. Keep WordPress core and plugins updated
3. Review HTTP headers for unnecessary technical information
4. Review DNS records regularly, hide the DNS software version and consider enabling DNSSEC
5. Keep ModSecurity enabled and tuned
6. Perform regular internal network discovery
7. Investigate unknown devices
8. Maintain network documentation
9. Only test with proper authorization

## 💡 What I Learned
- Public information alone can reveal a lot about a target before any exploitation
- A version number is not a vulnerability by itself, and good reports separate observations from confirmed risks
- Reconnaissance and scanning must always stay within an authorized scope

## 📁 Repository Contents
```
├── README.md
├── W2-PM-FINAL_-_Networkwalks_Report.docx
└── screenshots/
```

## 📄 Full Report
See [`W2-PM-FINAL_-_Networkwalks_Report.docx`](W2-PM-FINAL_-_Networkwalks_Report.docx)

