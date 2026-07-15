<div align="center">

# SOC Analyst Toolkit

**A curated arsenal of blue team and detection engineering tools, mapped to the workflows a SOC analyst actually runs.**

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)](CONTRIBUTING.md)
[![MITRE ATT&CK](https://img.shields.io/badge/MITRE-ATT%26CK-red.svg)](https://attack.mitre.org)

Maintained by [@WilliamInCyber](https://github.com/WiLL75G)

</div>

---

Every entry here is something I use, have tested in a lab, or would reach for on shift. No dead links, no filler. Where a tool appears in my own lab work, the **Lab** column links to the writeup.

---

## Contents

1. [SIEM and Log Management](#1-siem-and-log-management)
2. [Endpoint Detection and Response](#2-endpoint-detection-and-response)
3. [Network Detection and Traffic Analysis](#3-network-detection-and-traffic-analysis)
4. [Intrusion Detection and Prevention](#4-intrusion-detection-and-prevention)
5. [Threat Intelligence](#5-threat-intelligence)
6. [Detection Engineering and Rule Development](#6-detection-engineering-and-rule-development)
7. [Malware Analysis and Sandboxing](#7-malware-analysis-and-sandboxing)
8. [Digital Forensics and Incident Response](#8-digital-forensics-and-incident-response)
9. [OSINT and Reconnaissance](#9-osint-and-reconnaissance)
10. [Vulnerability Management](#10-vulnerability-management)
11. [Automation and SOAR](#11-automation-and-soar)
12. [Purple Team and Adversary Emulation](#12-purple-team-and-adversary-emulation)
13. [Reference and Learning](#13-reference-and-learning)

---

## 1. SIEM and Log Management

| Tool | What it does | Lab |
|------|--------------|-----|
| [Splunk](https://www.splunk.com/) | Industry-standard SIEM. SPL for search, dashboards, correlation searches. | 28-day SOC lab |
| [Microsoft Sentinel](https://learn.microsoft.com/en-us/azure/sentinel/) | Cloud-native SIEM/SOAR on Azure. KQL, analytics rules, workbooks. | sentinel-soc-lab |
| [Wazuh](https://wazuh.com/) | Open-source SIEM/XDR with agent-based collection and built-in ruleset. | Wazuh SSH brute-force lab |
| [Elastic Security (ELK)](https://www.elastic.co/security) | Elasticsearch, Logstash, Kibana. Detection rules and hunting. | — |
| [Graylog](https://graylog.org/) | Open-source log management with streams, pipelines, alerting. | — |

## 2. Endpoint Detection and Response

| Tool | What it does | Lab |
|------|--------------|-----|
| [Sysmon](https://learn.microsoft.com/en-us/sysinternals/downloads/sysmon) | Deep Windows event logging. EID 1 process creation, EID 3 network, EID 4104 scriptblock. | PowerShell forensics on JAMES-VM |
| [Microsoft Defender for Endpoint](https://www.microsoft.com/en-us/security/business/endpoint-security/microsoft-defender-endpoint) | Enterprise EDR with process trees and hunting. | — |
| [Velociraptor](https://docs.velociraptor.app/) | Endpoint visibility and hunting via VQL. | — |
| [OSQuery](https://osquery.io/) | Query your endpoints like a SQL database. | — |
| [LimaCharlie](https://www.limacharlie.io/) | SecOps cloud with free-tier EDR telemetry. | — |

## 3. Network Detection and Traffic Analysis

| Tool | What it does | Lab |
|------|--------------|-----|
| [Wireshark](https://www.wireshark.org/) | The packet analysis standard. Deep protocol dissection. | wireshark-pcap-deep-analysis-lab |
| [tshark](https://www.wireshark.org/docs/man-pages/tshark.html) | CLI Wireshark. Scriptable capture and field extraction. | Tier 1 tshark drills |
| [Zeek](https://zeek.org/) | Network security monitor producing rich connection logs. | — |
| [Arkime](https://arkime.com/) | Full-packet capture with indexed search. | — |
| [tcpdump](https://www.tcpdump.org/) | Lightweight CLI capture on any host. | — |

## 4. Intrusion Detection and Prevention

| Tool | What it does | Lab |
|------|--------------|-----|
| [Suricata](https://suricata.io/) | High-performance IDS/IPS with custom rule support. | Suricata custom rules (sid 1000001/1000002) |
| [Snort](https://www.snort.org/) | Classic signature-based IDS/IPS. | — |
| [Security Onion](https://securityonionsolutions.com/) | Full monitoring distro bundling Suricata, Zeek, and the ELK stack. | — |
| [Fail2ban](https://github.com/fail2ban/fail2ban) | Log-driven ban of brute-force sources. | — |

## 5. Threat Intelligence

| Tool | What it does | Lab |
|------|--------------|-----|
| [MISP](https://www.misp-project.org/) | Threat intel sharing platform. IOCs, sightings, feeds. | — |
| [OpenCTI](https://www.opencti.io/) | Structured CTI knowledge management on STIX2. | — |
| [VirusTotal](https://www.virustotal.com/) | File/URL/IP reputation across dozens of engines. | — |
| [AbuseIPDB](https://www.abuseipdb.com/) | Community IP reputation and abuse reporting. | — |
| [URLScan.io](https://urlscan.io/) | Sandboxed URL analysis and screenshotting. | Malware URL Dashboard |
| [GreyNoise](https://www.greynoise.io/) | Separates targeted attacks from internet background noise. | — |

## 6. Detection Engineering and Rule Development

| Tool | What it does | Lab |
|------|--------------|-----|
| [Sigma](https://github.com/SigmaHQ/sigma) | Generic detection rule format, portable across SIEMs. | SigmaHQ issues 6056/6057 |
| [sigma-cli / pySigma](https://github.com/SigmaHQ/sigma-cli) | Convert Sigma rules to SPL, KQL, EQL, and more. | — |
| [MITRE ATT&CK Navigator](https://mitre-attack.github.io/attack-navigator/) | Visualize detection coverage against the matrix. | — |
| [Atomic Red Team](https://github.com/redcanaryco/atomic-red-team) | Small tests per ATT&CK technique to validate detections. | — |
| [Detection Lab](https://github.com/clong/DetectionLab) | Pre-built lab for practicing detection engineering. | — |

## 7. Malware Analysis and Sandboxing

| Tool | What it does | Lab |
|------|--------------|-----|
| [CyberChef](https://gchq.github.io/CyberChef/) | The "cyber swiss army knife" for decode/deobfuscate. | base64 deobfuscation |
| [Any.Run](https://any.run/) | Interactive online malware sandbox. | — |
| [Cuckoo Sandbox](https://cuckoosandbox.org/) | Automated dynamic malware analysis. | — |
| [PEStudio](https://www.winitor.com/) | Static triage of Windows PE files. | — |
| [Ghidra](https://ghidra-sre.org/) | NSA's open-source reverse engineering suite. | — |

## 8. Digital Forensics and Incident Response

| Tool | What it does | Lab |
|------|--------------|-----|
| [Autopsy](https://www.autopsy.com/) | GUI disk forensics on the Sleuth Kit. | — |
| [Volatility](https://www.volatilityfoundation.org/) | Memory forensics framework. | — |
| [KAPE](https://www.kroll.com/en/services/cyber/incident-response-litigation-support/kroll-artifact-parser-extractor-kape) | Fast targeted artifact collection and parsing. | — |
| [Chainsaw](https://github.com/WithSecureLabs/chainsaw) | Fast Windows event log hunting with Sigma support. | — |
| [Hayabusa](https://github.com/Yamato-Security/hayabusa) | Windows event log timeline and threat hunting. | — |

## 9. OSINT and Reconnaissance

| Tool | What it does | Lab |
|------|--------------|-----|
| [Shodan](https://www.shodan.io/) | Search engine for internet-exposed devices. | — |
| [Nmap](https://nmap.org/) | Network mapping and port scanning. | CorpOps-Shell-Suite |
| [theHarvester](https://github.com/laramies/theHarvester) | Email, subdomain, and host OSINT gathering. | Recon Automation Toolkit |
| [Maltego](https://www.maltego.com/) | Link analysis and OSINT graphing. | — |
| [Amass](https://github.com/owasp-amass/amass) | In-depth attack surface and subdomain mapping. | — |

## 10. Vulnerability Management

| Tool | What it does | Lab |
|------|--------------|-----|
| [OpenVAS / Greenbone](https://www.greenbone.net/) | Open-source vulnerability scanning. | — |
| [Nessus Essentials](https://www.tenable.com/products/nessus/nessus-essentials) | Free-tier industry vuln scanner. | — |
| [Nuclei](https://github.com/projectdiscovery/nuclei) | Template-based fast vulnerability scanning. | — |
| [Trivy](https://github.com/aquasecurity/trivy) | Container and IaC vulnerability scanning. | — |

## 11. Automation and SOAR

| Tool | What it does | Lab |
|------|--------------|-----|
| [Shuffle](https://shuffler.io/) | Open-source SOAR for playbook automation. | — |
| [TheHive](https://thehive-project.org/) | Case management and incident response collaboration. | — |
| [Cortex](https://github.com/TheHive-Project/Cortex) | Observable analysis engine paired with TheHive. | — |
| [n8n](https://n8n.io/) | General workflow automation, handy for security glue. | — |

## 12. Purple Team and Adversary Emulation

| Tool | What it does | Lab |
|------|--------------|-----|
| [Caldera](https://caldera.mitre.org/) | MITRE's automated adversary emulation platform. | — |
| [NetExec (nxc)](https://github.com/Pennyw0rth/NetExec) | Network protocol post-exploitation and lateral movement. | WMI remote execution demo |
| [Atomic Red Team](https://github.com/redcanaryco/atomic-red-team) | Technique-level detection validation tests. | — |
| [Kali Linux](https://www.kali.org/) | The offensive tooling distro for generating test attacks. | Live Kali attack scenarios |

## 13. Reference and Learning

| Resource | What it is | Lab |
|----------|------------|-----|
| [MITRE ATT&CK](https://attack.mitre.org/) | The adversary tactics and techniques knowledge base. | — |
| [The DFIR Report](https://thedfirreport.com/) | Real intrusion writeups mapped to ATT&CK. | — |
| [LOLBAS](https://lolbas-project.github.io/) | Living-off-the-land binaries abused by attackers. | — |
| [GTFOBins](https://gtfobins.github.io/) | Unix binaries abused to bypass restrictions. | — |
| [OverTheWire](https://overthewire.org/wargames/) | Hands-on security wargames. | Bandit Levels 1-31 |

---

## Contributing

PRs welcome. See [CONTRIBUTING.md](CONTRIBUTING.md). One rule: every tool must be something you would actually reach for on shift. No filler, no dead links.

## License

[MIT](LICENSE)

---

<div align="center">

Built and maintained by [@WilliamInCyber](https://github.com/WiLL75G) — detection engineering, in the trenches.

</div>
