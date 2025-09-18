# Security Assessment Report – itsecgames.com  

##  Overview  
This repository contains the security assessment report created as part of the **Security Officer Trainee Job Assessment**.  
The task was to evaluate the security posture of a publicly hosted endpoint and document vulnerabilities, risks, and mitigation strategies.   

---

##  Objectives  
- Identify vulnerabilities using publicly available tools.  
- Detect potential misconfigurations, outdated software, and CVEs.  
- Assess SSL/TLS configuration and certificate health.  
- Highlight exposed information (headers, banners, error messages).  
- Provide a prioritized list of findings with mitigation recommendations.  

---

##  Tools & Techniques Used  
- **WhatWeb** → Fingerprinting  
- **WafW00f** → Firewall detection  
- **Subfinder / Amass** → Subdomain discovery  
- **Dirb** → Directory enumeration  
- **Nmap** → Port scanning & service detection  
- **SSL Labs** → TLS/SSL configuration check  
- **curl** → Header inspection  
- **Manual Browsing**  

---

##  Key Findings  
- **No robots.txt file** → Potential for unrestricted crawling.  
- **No WAF detected** → Site lacks protection from malicious traffic.  
- **SSL/TLS not enforced** → Site only accessible over HTTP, exposing data in transit.  
- **Missing security headers** → X-Frame-Options, X-XSS-Protection, Content-Security-Policy not present.  
- **Open ports (22, 80, 443)** → Minimal exposure but requires hardening.  
- **DoS vulnerability** → Website became unavailable under multiple requests.  
- **JavaScript directory found** → Could contain sensitive API endpoints (further inspection blocked due to downtime).  

---

## 🛡 Risk Levels  
| Area                  | Observation                                  | Risk Level |
|------------------------|----------------------------------------------|------------|
| SSL/TLS Configuration  | No HTTPS, Apache headers exposed             | **High**   |
| Denial of Service      | Site unstable under load                     | **High**   |
| Firewall / WAF         | No WAF detected                              | Medium     |
| Directories            | Index.htm accessible                        | Medium     |
| JavaScript/API         | Potential for exposed endpoints              | Medium     |
| Robots.txt / Subdomains| Not found / none detected                    | Low        |

---

##  Mitigation Recommendations  
- Enforce HTTPS with TLS 1.2+ and HSTS.  
- Deploy a Web Application Firewall (WAF).  
- Remove or restrict access to sensitive directories.  
- Add essential security headers (CSP, X-Frame-Options, X-XSS-Protection).  
- Protect against DoS with rate limiting, caching, and traffic filtering.  
- Regularly monitor for subdomains and hidden endpoints.  

##  Sample Commands Used 

nmap -sV -sC -Pn websiteURL
→ Scanned for open ports and services.Collects OS details, traceroute, service versions (useful for banner grabbing).
--

dirb websiteURL
→ Enumerates hidden directories/files like /index.htm, /js/, /admin/. 
--

wafw00f websiteURL
→ Checks if the site is protected by a WAF
--

curl -IL websiteURL
→ Looked for disallowed paths.
→ Revealed missing security headers (X-Frame-Options, CSP, etc.) and exposed server info (Server: Apache).
--

Whatweb websiteURL
→ Identifies web technologies, server type, CMS, frameworks, etc. Useful for detecting outdated software versions.
--

subfinder -d itsecgames.com
→ Finds subdomains that could expose hidden services or staging environments.

