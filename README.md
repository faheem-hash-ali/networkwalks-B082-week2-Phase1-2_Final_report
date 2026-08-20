# 📑 Penetration Testing Report: Footprinting, Reconnaissance & Network Scanning (Week 2)

![Cybersecurity](https://img.shields.io/badge/Track-Offensive_Security-red.svg)
![Type](https://img.shields.io/badge/Document-Final_Pentest_Report-blue.svg)
![Batch](https://img.shields.io/badge/Batch-B082_Networkwalks-green.svg)
![Scope](https://img.shields.io/badge/Scope-PM1_%7C_PM2_%7C_PM5-orange.svg)

---

## 📌 Executive Summary
Yeh document **Networkwalks Cybersecurity & Ethical Hacking Internship (Batch B082)** ke Week 2 practical modules ki comprehensive **Penetration Testing Report** hai. Is report mein Phase 1 (Reconnaissance & Passive Footprinting) aur Phase 2 (Scanning & Network Discovery) ko thoroughly execute aur document kiya gaya hai.

---

## 📋 Assessment Scope & Metadata

| Field | Detail |
| :--- | :--- |
| **Pentester Name** | Faheem Ali Wattoo |
| **Program / Batch** | B082-Networkwalks |
| **Report Date** | 20 August 2026 |
| **Modules Covered** | **W2-PM1:** Footprinting with Multiple Kali Tools<br>**W2-PM2:** Footprinting with Google Hacking Database (GHDB)<br>**W2-PM5:** Network Scanning with Zenmap |
| **Target Scope** | 1. `networkwalks.com` (Authorized Client Scope)<br>2. Local Subnet `192.168.56.0/24` (Host-Only Lab Environment) |
| **Testing Authorization** | Formally Secured (Letter of Authorization NW-LOA-B082-017) |

---

## 1. Liability Disclaimer
I performed all activities described in this report strictly against systems and targets explicitly permitted for this training: the public website `networkwalks.com` (passive footprinting only — no exploitation), publicly indexed search results (GHDB, passive/read-only), and my own local network (Zenmap scanning). No unauthorized access, denial-of-service, or intrusive testing was carried out at any point.

---

## 2. Tools & Infrastructure Matrix

| Tool / Resource | Category | Purpose |
| :--- | :--- | :--- |
| **Kali Linux** | Operating System | Primary CLI reconnaissance platform |
| **WHOIS** | Registry OSINT | Query domain ownership, registrar details, and name servers |
| **WhatWeb** | Web Fingerprinting | Identify web server, CMS frameworks, and active plugins |
| **Nslookup** | DNS Resolution | Map target hostname to direct public IPv4 address |
| **cURL (`-I`)** | Header Inspection | Analyze HTTP response headers and REST API endpoints |
| **Wafw00f** | Perimeter Detection | Fingerprint Web Application Firewall (WAF) vendor |
| **DNSRecon** | Zone Enumeration | Enumerate SOA, NS, MX, SPF, TXT, and cPanel SRV records |
| **GHDB (Exploit-DB)** | Search OSINT | Utilize Google dorks to discover exposed IoT devices and open directories |
| **Zenmap (Nmap GUI)** | Network Discovery | Scan local subnet, identify live hosts, and generate topology maps |

---

## 3. Hands-on Technical Activities & Verification

### 🔹 Module 1: Footprinting with Multiple Kali Tools (W2-PM1)

1. **WHOIS Lookup (`whois networkwalks.com`):**
   * **Registrar:** GoDaddy.com, LLC
   * **Name Servers:** `NS6135.HOSTGATOR.COM`, `NS6136.HOSTGATOR.COM`
   * **Creation / Expiry:** 2019-11-06 / 2027-11-06
   * *Evidence:* `whois (networkwalks.com).jpg`

2. **Web Stack Fingerprinting (`whatweb networkwalks.com`):**
   * **Web Server:** Apache
   * **CMS & Plugins:** WordPress 7.0.4, WP Download Manager 3.3.58
   * **Target IP:** `192.232.216.135`
   * *Evidence:* `whatweb (networkwalks.com).jpg`

3. **DNS Resolution (`nslookup networkwalks.com`):**
   * Resolved against Google DNS (`8.8.8.8`) to IP `192.232.216.135`
   * *Evidence:* `nslookup (networkwalks.com).png`

4. **HTTP Header Inspection (`curl -I https://networkwalks.com`):**
   * Status: `HTTP/2 200 OK`
   * Exposed REST API Endpoint: `/wp-json/`
   * Caching Layer: `x-nginx-cache: WordPress`
   * *Evidence:* `curl (networkwalks.com).png`

5. **WAF Detection (`wafw00f networkwalks.com`):**
   * Detected Active WAF: `ModSecurity (SpiderLabs)`
   * *Evidence:* `wafw00f (networkwalks.com).png`

6. **DNS Record Enumeration (`dnsrecon -d networkwalks.com`):**
   * Extracted MX (`mail.networkwalks.com`), SPF policy, BIND version `9.16.23-RH`, and 8x cPanel autodiscover SRV records.
   * *Evidence:* `dnsrecon (networkwalks.com).jpg`

---

### 🔹 Module 2: Footprinting with GHDB (W2-PM2)

* **Task 1 (10x Exposed Security Cameras):** Executed curated dorks (e.g., `intitle:"webcamXP" inurl:8080`, `intitle:"ContaCam"`) to discover live, unauthenticated webcam and CCTV portals globally.
  * *Evidence:* `Screenshot_cam_1.jpg`, `Screenshot_cam_2.jpg`
* **Task 2 (10x Open Directory Listings):** Executed `intitle:index of "parent directory" mathematics pdf` to identify unprotected web directories leaking academic books and documents.
  * *Evidence:* `Screenshot_ebook_1.png`, `Screenshot_ebook_2.png`

---

### 🔹 Module 3: Network Scanning with Zenmap (W2-PM5)

* **Target Subnet:** `192.168.56.0/24` (VirtualBox Host-Only Adapter)
* **Command Executed:** `nmap -sn 192.168.56.0/24`
* **Scan Results:** 256 IP addresses scanned in 12.10 seconds.
* **Live Hosts Discovered:** 1 host active (`192.168.56.1`).
* **Topology Generation:** Interactive topology map generated with Legend and exported to PDF format (`1st_sacn.pdf`).
* *Evidence:* `Screenshot_ping_scan.png`, `Screenshot_1st_nmap_scan.png`

---

## 4. Risk Analysis & Threat Impact

| # | Finding / Observation | Technical Evidence | Potential Risk | Level |
| :---: | :--- | :--- | :--- | :---: |
| **01** | Public Version Disclosure | WordPress 7.0.4 & WP Download Manager 3.3.58 | Public CVE mapping for outdated components | `Medium` |
| **02** | Exposed REST API | Link header exposes `/wp-json/` | Automated user and route enumeration | `Low` |
| **03** | DNS & Infrastructure Disclosure | BIND 9.16.23-RH & HostGator NS visible | Targeted DNS exploit probing | `Medium` |
| **04** | Exposed Surveillance Endpoints | Unauthenticated webcamXP / ContaCam streams | Unauthorized physical surveillance | `High` |
| **05** | Open Directory Indexing | Web servers exposing `/parent directory/` | Intellectual property and document leakage | `Medium` |
| **06** | Local Host Discovery | Zenmap Ping Scan mapped active host | Internal discovery facilitates lateral movement | `Low` |

---

## 5. Remediation & Hardening Recommendations

1. **Suppress Server Headers:** Disable detailed server banners and WordPress signatures (`ServerTokens Prod`, `ServerSignature Off`).
2. **Implement IoT Access Controls:** Enforce mandatory authentication on all camera streams and web management consoles.
3. **Disable Directory Browsing:** Configure `Options -Indexes` across web servers to prevent indexing of internal file trees.
4. **WAF Tuning:** Continuously update and monitor ModSecurity WAF rulesets to drop automated reconnaissance traffic.
5. **Network Segmentation:** Enforce host firewall isolation to restrict discovery probes on local segments.

---

## 6. Conclusion
Week 2 practical tasks successfully demonstrated the end-to-end transition from passive OSINT collection (PM1 & PM2) to active internal network mapping (PM5). The assessment highlights that comprehensive information gathering is the critical prerequisite for identifying potential attack surfaces before executing any intrusive testing.

---

## 👨‍💻 Author & Project Information
* **Pentester Name:** Faheem Ali Wattoo
* **Program:** Networkwalks Cybersecurity & Ethical Hacking Internship
* **Batch:** B082 Networkwalks
* **Lead Instructor:** [Waqas Karim CCIE](https://www.linkedin.com/in/waqaskarim/)
