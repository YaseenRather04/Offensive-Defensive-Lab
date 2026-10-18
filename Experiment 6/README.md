# Experiment 6: Website Risk Assessment with OWASP ZAP

**Course:** Ethical Hacking & Network Security Lab, Model Institute of Engineering and Technology (MIET), Jammu  
**Date performed:** 18 October 2026  
**Tools:** OWASP ZAP 2.17.0, Kali Linux, Metasploitable 2, DVWA 1.0.7, VirtualBox

## 1. Aim

To assess a deliberately vulnerable web application with OWASP ZAP, discover its pages and requests, identify potential security weaknesses, and review the resulting alerts by risk and confidence.

## 2. Scope and lab environment

| System | Lab role | Address / details |
|---|---|---|
| Kali Linux | Scanner workstation; runs ZAP | Hostname shown as `kali` |
| Metasploitable 2 | Isolated practice target; hosts DVWA | `192.168.1.100` |
| DVWA | Web application assessed | `http://192.168.1.100/dvwa` |

The scan was performed against the intentionally vulnerable virtual machine in the lab. The lab handout recommends a host-only network (`192.168.56.x`); the target address used in this run was `192.168.1.100`. Because a deliberately vulnerable VM should not be exposed to an untrusted network, keep it on an isolated lab network and shut it down when the exercise is complete.

## 3. Procedure

### 3.1 Confirm target reachability

I checked that Kali could communicate with Metasploitable before starting the web scan:

```bash
ping -c 4 192.168.1.100
```

Four replies were received with no packet loss. The ping screenshot was not included with the available images.

### 3.2 Prepare DVWA

Log in at `http://192.168.1.100/dvwa/login.php` and set DVWA's security level to **Low**. The corresponding screenshot was not included with the available images.

### 3.3 Install and start OWASP ZAP

If the configured Kali package mirror is unreachable, select a working mirror, then install and launch ZAP 2.17.0:

```bash
sudo apt update
sudo apt install zaproxy -y
zaproxy
```

The captures show the ZAP launch and the initial ZAP workspace. [Launch](113610.png) · [Welcome screen](113755.png) · [Workspace](114038.png)

### 3.4 Explore the authenticated application

DVWA requires a login, so I used ZAP's browser/manual exploration workflow to sign in and visit application pages. This helps ZAP record requests made after authentication, rather than limiting discovery to the login page. The ZAP Sites tree capture shows DVWA paths and requests, including login, security, and vulnerability-related endpoints. [Discovered site tree](114045.png)

### 3.5 Crawl with Spider

I set the DVWA address as the Spider starting point and left recursive crawling enabled to discover linked paths. ZAP's passive scanner can analyze traffic observed during crawling and browsing. [Spider configuration](114434.png)

### 3.6 Run an Active Scan

I configured an Active Scan for `http://192.168.1.100/dvwa` with the Default Policy and recursive scope, then ran it against the lab target. An active scan sends test requests, so it should only be used on systems where scanning is authorized. [Active Scan configuration](114454.png)

### 3.7 Review alerts and export the report

After scanning, I reviewed the ZAP Alerts panel and generated an HTML report using **Report → Generate Report**. The screenshots show the alert list and report overview. [Alerts](120626.png) · [Report risk/confidence summary](121001.png) · [Report cover and contents](121020.png)

## 4. Results

### 4.1 Alert totals

The supplied report summary records 23 alerts:

| Risk level | Alerts |
|---|---:|
| High | 2 |
| Medium | 6 |
| Low | 9 |
| Informational | 6 |
| **Total** | **23** |

The report's counts by risk and confidence are shown in [the summary screenshot](121001.png). These are scanner alerts, not proof that every issue is exploitable.

### 4.2 Alerts documented in the supplied report

The table below lists the findings recorded in the generated report. OWASP Top 10 mappings are an interpretation using the 2021 categories; verify an individual alert's ZAP tags and report details before treating a mapping as definitive.

| # | ZAP alert | Risk | Suggested OWASP Top 10 (2021) area |
|---:|---|---|---|
| 1 | Remote Code Execution (CVE-2012-1823) | High | A06: Vulnerable and Outdated Components |
| 2 | Source Code Disclosure (CVE-2012-1823) | High | A06: Vulnerable and Outdated Components |
| 3 | Absence of Anti-CSRF Tokens | Medium | A01: Broken Access Control |
| 4 | Application Error Disclosure | Medium | A05: Security Misconfiguration |
| 5 | Content Security Policy (CSP) Header Not Set | Medium | A05: Security Misconfiguration |
| 6 | Directory Browsing | Medium | A05: Security Misconfiguration |
| 7 | HTTP Only Site (plain HTTP) | Medium | A02: Cryptographic Failures |
| 8 | Missing Anti-clickjacking Header | Medium | A05: Security Misconfiguration |
| 9 | Cookie No HttpOnly Flag | Low | A05: Security Misconfiguration |
| 10 | Cookie without SameSite Attribute | Low | A01: Broken Access Control |
| 11 | Server Leaks Information via `X-Powered-By` | Low | A05: Security Misconfiguration |
| 12 | Server Leaks Version Information via `Server` header | Low | A05: Security Misconfiguration |
| 13 | `X-Content-Type-Options` Header Missing | Low | A05: Security Misconfiguration |

Other Low alerts were In Page Banner Information Leak, Private IP Disclosure, Timestamp Disclosure (Unix), and a second Application Error Disclosure alert. Informational alerts included Authentication Request Identified, Cookie Poisoning, Information Disclosure (Suspicious Comments), Modern Web Application, and Session Management Response Identified. Consult the exported ZAP report for the full alert details.

### 4.3 Interpretation

- The two High alerts reference CVE-2012-1823 and are associated with an outdated PHP-CGI component in the intentionally vulnerable target. They were recorded as scan findings; exploitation was not part of this experiment.
- Medium alerts point to missing request-forgery protection, directory listing, application error details, missing browser security headers, and use of unencrypted HTTP.
- Low alerts include cookie attribute gaps and server or application version disclosure.
- Risk levels help prioritize review, but they do not replace manual validation or context-specific impact assessment.

### 4.4 Coverage limits

SQL Injection and Reflected XSS are expected areas to examine in DVWA, but neither appeared in the final alert list. They are therefore not reported as detected findings here. Authenticated pages may require complete browsing or a configured session, and automated scanners can miss issues. A targeted follow-up would be to verify the relevant DVWA pages individually in the authorized lab.

## 5. Conclusion

OWASP ZAP was used to explore DVWA, crawl its routes, run an Active Scan, and produce an alert report. The supplied report records 23 alerts across High, Medium, Low, and Informational risk levels. The exercise demonstrates why authenticated coverage, scanner configuration, report review, and manual verification all matter when assessing a web application. In a real deployment, confirmed findings should lead to appropriate patching, safer configuration, stronger transport security, and removal of unnecessary exposure.

## 6. Screenshot index

All nine screenshots available with this task are embedded or linked above:

| Image | What it documents |
|---|---|
| `113610.png` | ZAP launch |
| `113755.png` | ZAP welcome screen |
| `114038.png` | ZAP workspace |
| `114045.png` | DVWA site tree and requests |
| `114434.png` | Spider scan setup |
| `114454.png` | Active Scan setup |
| `120626.png` | Alert list |
| `121001.png` | Report risk/confidence summary |
| `121020.png` | Report cover and contents |

The target-IP, ping, and DVWA security-level screenshots were not included with the available image files, so they are not linked here.

## 7. Repository layout

```text
Experiment/
├── README.md
├── 113610.png
├── 113755.png
├── ...
└── 121020.png  # Screenshot files are beside this README
```

If the exported ZAP HTML report is added later, it can be stored in a `reports/` directory.

