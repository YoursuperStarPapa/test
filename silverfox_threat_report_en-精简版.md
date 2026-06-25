# 🦊 Silver Fox (银狐) Threat Report

> **Sources**: GitHub silverfox_report_reordered.md, CN-SEC "Big Data & AI-Driven Silver Fox Threat Intelligence and Forward Defense", Security Analysis & Research "Silver Fox Criminal Organization Latest Attack Trend Deep Analysis Report"
> **Report Date**: 2026-06-24
> **Intelligence Freshness**: Based on 2024–2026 public threat intelligence

---

## I. The Findings — Technical Breakdown

### 1.1 Threat Actor Profile

| Dimension | Detail |
|-----------|--------|
| **Threat Name** | SilverFox (银狐) / You She (游蛇) / Guo Duo Da Dao (谷堕大盗) / APT-Q-081 / SwimSnake / Void Arachne |
| **Actor Type** | Cybercrime organization driven by financial gain (evolved into a "spy + crime" dual model since 2025) |
| **Naming Origin** | "Silver Fox" string found in PDB paths of early samples |
| **Active Period** | 2020–present; large-scale surge in 2025-2026 |
| **Malware Foundation** | Rewritten from open-source Gh0st RAT 4.0, later evolved into independent families such as ValleyRAT |
| **Estimated Size** | 10–50 members, operating in a loosely coupled, rapid-iteration model |
| **Technical Capability** | Advanced (possesses APT-grade AV evasion and EDR bypass capabilities) |
| **Target Geography** | Mainland China → Greater China → Japan / Southeast Asia / India (continuously expanding) |


### 1.2 Six Core Threat Trends (2024–2026)

| # | Trend | Severity | Key Change |
|---|-------|----------|------------|
| 1 | **Escalation from "credential theft" to "real-time transaction tampering"** | 🔴 Critical | MitB (Man-in-the-Browser) techniques mature; capable of modifying payee accounts and amounts on online banking pages in real time |
| 2 | **AI deeply empowers the entire attack chain** | 🔴 Critical | LLM-generated phishing narratives, Deepfake executive video/audio impersonation, AI-driven AV evasion mutation |
| 3 | **Technical evolution to the 6.0 era** | 🔴 Critical | Kernel callback unhooking + AI-assisted evasion + Rust core rewrite; traditional security products face extreme challenges |
| 4 | **Cloud-native C2 and supply chain delivery become the new normal** | 🟠 High | C2 migrated to legitimate cloud service APIs; delivery shifted to software supply chain poisoning |
| 5 | **Attack targets spreading from tax/finance to all industries** | 🟠 High | Expansion into education, healthcare, manufacturing, Greater China, and Southeast Asia |
| 6 | **Convergence with ransomware ecosystems** | 🟡 Medium-High | Some variants now serve as initial access brokers for ransomware gangs |

#### Key Statistics

- Attack frequency in 2025 grew **150%+** year-over-year; variant count exceeded several thousand
- Attack incidents in 2026 grew **127%** year-over-year; over **12,000** enterprises affected
- Single fraud loss up to **¥5 million**
- Supply chain attack impacted over **200** financial institutions
- Deepfake financial fraud: A Hong Kong multinational lost **HK$200 million** due to an AI-fabricated CFO video conference call
- AV evasion lifespan extended from an average of 3 days to **7–14 days** (AI-assisted mutation)

### 1.3 Full Attack Chain

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                     Silver Fox Complete Attack Chain                        │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                             │
│  ① Delivery Entry        ② Initial Execution       ③ Environment Check     │
│  ┌──────────────┐     ┌──────────────┐      ┌──────────────┐                │
│  │ WeChat/WXWork│     │ Disguised    │      │ Anti-VM (50+)│                │
│  │ /QQ/Whatsapp │     │ Folder/PDF   │      │ Anti-Sandbox │                │
│  │Phishing Site │───→ │ Fake Error   │─────→│ Anti-AV      │                │
│  │/SEO Poisoning│     │ Prompt       │      │ Abort if     │                │
│  │Phishing Email│     │Legit Software│      │ detected     │                │
│  │Supply Chain  │     │ CHM/HTA Files│      │ Geo-fencing  │                │
│  └──────────────┘     └──────────────┘      └──────┬───────┘                │
│                                                     ↓                       │
│  ⑥ Lateral Movement     ⑤ C2 Established      ④ Persistence                │
│  ┌───────────────┐     ┌────────────────┐      ┌───────────────┐            │
│  │ IM Impersonate│     │ Port 8880      │      │ Fake Scheduled│            │
│  │ Executive     │←────│ AES-256-GCM    │←─────│ Tasks         │            │
│  │ SMB/Weak      │     │ Cloud-Native   │      │ Registry Run  │            │
│  │ Passwords     │     │ C2 API         │      │ Keys          │            │
│  │ GPO Lateral   │     │ Dynamic        │      │ WMI Event     │            │
│  │ Distribution  │     │ Domain Rotation│      │ Subscription  │            │
│  └───────────────┘     └────────────────┘      └───────────────┘            │
│                                                                             │
│  ⑦ Impact                                                                   │
│  ┌───────────────┐  ┌──────────────┐  ┌────────────────┐  ┌──────────┐      │
│  │ MitB Txn      │  │ Browser      │  │ AnyDesk Silent │  │ Screen   │      │
│  │ Tampering     │  │ Password     │  │ Install        │  │ Capture  │      │
│  │ Executive     │  │ /Cookie/     │  │ Desktop        │  │ Keylog   │      │
│  │ Impersonation │  │ Financial    │  │ Takeover       │  │ Data     │      │
│  │ Up to ¥5M/case│  │ Data Theft   │  │ Deepfake Verify│  │ Exfil    │      │
│  └───────────────┘  └──────────────┘  └────────────────┘  └──────────┘      │
└─────────────────────────────────────────────────────────────────────────────┘
```

### 1.4 Malware Family Matrix

| # | Variant Name | First Seen | Core Technology | Primary Function | Status |
|---|-------------|------------|-----------------|------------------|--------|
| 1 | **ValleyRAT** | 2020 | Go/Rust loader + in-memory shellcode | Full RAT (cmd exec, file mgmt, screenshot, keylog) | ✅ Foundational variant |
| 2 | **Ghost Variant (SilverFox RAT)** | 2022 | Improved from ValleyRAT | Keylogging, screenshots, file theft | ✅ Continuously active |
| 3 | **HackBrian RAT** | 2023 | Modular loading | Data theft, proxy tunnel setup | 🔄 Intermittent |
| 4 | **AtlasCross RAT** | 2024 | Novel RAT framework | Remote control, spear-phishing | ✅ Active |
| 5 | **AnyDesk Joint Variant** | 2025 | SilverFox + AnyDesk silent install | Desktop takeover + executive impersonation fraud | ✅ Most damaging |
| 6 | **HoldingHands** | 2025.10 | Gh0st RAT variant | Remote desktop, file ops, screen monitoring | ✅ High-value espionage |
| 7 | **ABCDoor** | Late 2025 | Python, dual persistence | Anti-EDR, advanced persistence | ✅ Advanced variant |
| 8 | **Python Stealer** | 2026.02 | Python + PyInstaller | Credential theft, self-delete after completion | ✅ New variant |
| 9 | **Chrome Variant** | 2026.05 | PowerShell AMSI bypass + reflective loading | Fileless attack, anti-sandbox | ✅ Latest variant |
| 10 | **SilverFox V3** | 2026 Q1 | Rust+Go hybrid + AI-assisted evasion | Kernel callback unhooking + code mutation | ✅ Most advanced |

### 1.5 AV Evasion Evolution: Six Generations

| Era | Timeframe | Core Technique | Bypass Capability |
|-----|-----------|---------------|-------------------|
| **1.0** | 2020–2021H1 | Simple packing, file property spoofing, macro obfuscation | Detectable by mainstream AV static + dynamic analysis |
| **2.0** | 2021H2–2022H1 | Living-off-the-Land (LotL) + DLL side-loading as standard | Significantly bypasses static detection; abuses trusted binary reputation |
| **3.0** | 2022H2–2023H1 | Direct Syscall + multi-layer shellcode encryption | Bypasses EDR userland hooks |
| **4.0** | 2023H2–2024H1 | Indirect Syscall + ETW/AMSI Patch + Unhooking | APT-grade EDR bypass |
| **5.0** | 2024H2–2025H1 | Full-stack fileless + callback execution + cloud-native C2 | Traditional security products face extreme detection challenges |
| **6.0** | 2025H2–2026 | Kernel callback unhooking + AI-assisted evasion + Rust core rewrite | Both userland and partial kernel-level monitoring bypassed |

### 1.6 Key Technical Capabilities

**Living-off-the-Land (LotL) Loading (White-Listing Bypass)**
- Delivery path: Payloads such as `log.dll` dropped into `C:\Program Files\Internet Explorer\`
- Legitimate `installer.exe` (whitelisted binary) used to load malicious `log.dll`
- System tool abuse: `mshta.exe`, `rundll32.exe`, `regsvr32.exe`, `powershell.exe`

**Process Injection & Fileless Execution**
- Reflective DLL Loading — fully in-memory execution without relying on `LoadLibrary`
- Injection targets: `explorer.exe`, `svchost.exe`, `sihost.exe`, `winlogon.exe`
- PowerShell in-memory .NET CLR loading; encrypted shellcode decrypted and executed in memory

**Persistence Mechanisms**
- Registry Run keys → WMI event subscription + AppInit_DLLs (V3+)
- BAT script watchdog: BAT files dropped into `C:\Windows\` to loop-check trojan process status
- Fake system services: `sc` command to create/maintain disguised services
- Scheduled tasks disguised as "Adobe Updater" / "Windows Update" / "System Check", executing every 1–5 minutes
- GPO (Group Policy Object) lateral distribution persistence (added 2025 Q3)

**C2 Communication**
- HTTP/HTTPS protocol, port 8880, mimicking normal business traffic
- DNS tunneling (2024.05+): Base64-encoded data embedded in DNS query subdomains
- AES-256-GCM encryption (2024.05+), replacing legacy RC4/XOR
- Cloud-native C2: Migrated to legitimate cloud service APIs (2025+), de-fingerprinted
- Dynamic domains + cloud services + relay nodes

**Counter-Detection Techniques**
- Anti-VM detection (50+ fingerprints: CPUID, hardware serial numbers, MAC addresses, etc.)
- Delayed execution + self-deletion
- "Fattening" technique: Inflating the payload to 50–100MB+ to evade cloud-based scanning
- Custom driver + unhooking dual-chain attack
- BYOVD: Abusing Microsoft-signed driver `amsdk.sys` to bypass vulnerable driver blocklist
- Kernel callback unhooking (6.0 era)

**Weaponization of Legitimate Remote Access Tools**
- Silent installation of AnyDesk with unattended access configured
- Abuse of HoldingHands (Gh0st RAT variant) for high-value espionage
- Security products observe "legitimate remote access software running" — signature/whitelist systems completely bypassed

**Filesystem Artifacts**
```
Malware resident paths:
  C:\Program Files\Internet Explorer\     ← LotL payload delivery path (log.dll)
  C:\Users\Public\Download\              ← Configuration files
  %APPDATA%                              ← Hidden files in user directory
  %ProgramData%                          ← Program data directory
  %TEMP%                                 ← Temporary files
  C:\Windows\                            ← BAT persistence scripts

Python Stealer artifacts:
  C:\WhatsAppBackup\WhatsAppData.zip
  %TEMP%\whatsapp_backup.lock

Chrome variant artifacts:
  PDB path: XiaobaoService.pdb
  Associated process: ABoxHeadless.exe
  Associated DLL: qimei.dll
```


---

## II. The Hunt — Threat Hunting

### 2.1 Core Hypothesis

> **If a Silver Fox-infected host is discovered in the environment, the attacker has very likely already completed secondary propagation via chat tools, and the infection scope far exceeds the initial victim.**

Silver Fox's IM worm characteristics mean: one compromised host = entire network at risk.

### 2.2 Hunt Hypotheses

| # | Hypothesis | Confidence | Rationale                                                   |
|---|-----------|------------|-------------------------------------------------------------|
| H1 | Users within the organization have received and executed files disguised as documents via IM tools | High | 90%+ of Silver Fox delivery is via WeChat/WeCom/QQ/Whatsapp |
| H2 | Anomalous DLL files (e.g., log.dll) exist under `C:\Program Files\Internet Explorer\` | High | Known LotL payload delivery path                            |
| H3 | Critical system processes (explorer.exe/svchost.exe) have anomalous outbound HTTPS connections to port 8880 | High | C2 callback signature port                                  |
| H4 | High-frequency scheduled tasks disguised as "Adobe Updater" / "Windows Update" exist | Medium | Silver Fox-preferred persistence method                     |
| H5 | AnyDesk/TeamViewer or other legitimate remote access tools silently installed on endpoints | Medium | AnyDesk joint fraud variant characteristic                  |
| H6 | PowerShell processes exhibit anomalous in-memory .NET CLR loading behavior | Medium | Fileless attack characteristic                              |
| H7 | DLLs exist with high-entropy .text sections and minimal import functions | Medium | Chrome variant characteristic                               |
| H8 | DNS queries contain long subdomains (>30 chars) + Base64 encoding patterns | High | DNS tunneling C2 communication                              |
| H9 | Browser processes exhibit anomalous DOM modification on payee/amount fields | High | MitB transaction tampering characteristic                   |
| H10 | Unknown MSI/scripts distributed via GPO exist | Medium | 2025 Q3 new persistence method                              |

### 2.3 Telemetry Sources

- **EDR Process Tree & File Events** — Parent-child process relationships, DLL loading, file creation
- **Firewall/Proxy Logs** — Outbound connection destinations (IP/domain/port)
- **DNS Query Logs** — DGA domain identification, DNS tunneling detection
- **Windows Scheduled Task Logs** — Event ID 106/140/200/201
- **Windows PowerShell Logs** — Event ID 4103/4104 (Script Block Logging)
- **Windows Sysmon** — Event ID 1/3/7/11/12-13/19-21/22
- **Active Directory Authentication Logs** — Lateral movement detection
- **IM Tool Audit Logs** — File transfer records
- **GPO Audit Logs** — Group Policy change detection (new)
- **Browser Extension/Plugin Audit** — MitB detection (new)

### 2.4 Layered Detection Queries

> Each detection scenario provides queries for **Splunk SPL**, **QRadar AQL**, and **Cortex XDR XQL**.

---

#### Layer 1: Network — C2 Communication Detection

##### Query 1: Known IOC Match

**Splunk SPL**
```splunk
index=firewall OR index=proxy
| lookup silverfox_ioc_ip.csv ip AS dst_ip OUTPUT threat_name, campaign
| lookup silverfox_ioc_domain.csv domain AS dst_domain OUTPUT threat_name, campaign
| where isnotnull(threat_name) OR isnotnull(campaign)
| stats count values(threat_name) values(campaign) by src_ip, dst_ip, dst_domain
```

**QRadar AQL**
```sql
SELECT sourceip, destinationip, destinationport, domainname,
  COUNT(*) AS event_count, MIN(starttime) AS first_seen, MAX(starttime) AS last_seen
FROM events
WHERE
  (destinationip IN ('115.187.17.212','112.121.183.102','156.251.18.45','206.238.178.116',
    '45.119.55.66','69.30.250.99','103.228.12.151','154.91.84.3',
    '93.127.142.77','150.109.79.82','170.205.54.88','222.186.190.138',
    '103.203.48.174','130.250.191.46','220.167.103.145','220.167.103.160',
    '154.201.87.124','154.201.87.75'))
  OR domainname ILIKE '%360sdgg.com%'
  OR domainname ILIKE '%googlevip.icu%'
  OR domainname ILIKE '%oytdwzz.shop%'
  OR domainname ILIKE '%cocdex.cn%'
  OR domainname ILIKE '%xqwmwru.top%'
  OR domainname ILIKE '%gov.incometax.click%'
GROUP BY sourceip, destinationip, destinationport, domainname
ORDER BY event_count DESC
```

**Cortex XDR XQL**
```xql
dataset = network_story
| filter dst_action_external_ip in (
    "115.187.17.212","112.121.183.102","156.251.18.45","206.238.178.116",
    "45.119.55.66","69.30.250.99","103.228.12.151","154.91.84.3",
    "93.127.142.77","150.109.79.82","170.205.54.88","222.186.190.138",
    "154.201.87.124","154.201.87.75"
  )
  or dns_query_name contains "360sdgg.com"
  or dns_query_name contains "googlevip.icu"
  or dns_query_name contains "xqwmwru.top"
  or dns_query_name contains "gov.incometax.click"
| comp count(*) as hit_count, values(dst_action_external_ip) as c2_ips,
    min(_time) as first_seen, max(_time) as last_seen
  by agent_hostname, src_action_ip
| sort desc hit_count
```

---

##### Query 2: DNS Tunneling Detection (Long Subdomains + High-Frequency Queries)

**Splunk SPL**
```splunk
index=dns
| eval subdomain_len = len(mvindex(split(dns_query, "."), 0))
| eval is_base64 = if(match(dns_query, "^[A-Za-z0-9+/=]{30,63}\."), 1, 0)
| stats count as query_count dc(dns_query) as unique_queries avg(subdomain_len) as avg_len by src_ip
| where (query_count > 10 AND avg_len > 40) OR (is_base64 = 1 AND query_count > 3)
| sort -query_count
```

**QRadar AQL**
```sql
SELECT sourceip, domainname,
  COUNT(*) AS query_count, COUNT(DISTINCT domainname) AS unique_domains,
  AVG(LENGTH(SUBSTR(domainname, 1, INSTR(domainname, '.') - 1))) AS avg_subdomain_len
FROM events
WHERE eventdirection = 'L2R'
  AND (LOGSOURCETYPENAME(devicetype) = 'DNS Server' OR LOGSOURCETYPENAME(devicetype) LIKE '%DNS%')
  AND domainname IS NOT NULL
  AND LENGTH(SUBSTR(domainname, 1, INSTR(domainname, '.') - 1)) > 30
GROUP BY sourceip, domainname
HAVING COUNT(*) > 5
  OR (COUNT(*) > 3 AND AVG(LENGTH(SUBSTR(domainname, 1, INSTR(domainname, '.') - 1))) > 40)
ORDER BY query_count DESC
```

**Cortex XDR XQL**
```xql
dataset = xdr_data
| filter event_type = "DNS"
| alter
    subdomain = extract_regex(dns_query_name, "^([^.]+)", 1),
    subdomain_len = string_length(extract_regex(dns_query_name, "^([^.]+)", 1))
| filter subdomain_len > 30
| alter is_base64 = if(subdomain ~= "^[A-Za-z0-9+/=]{30,63}$", true, false)
| comp count(*) as query_count, count_distinct(dns_query_name) as unique_domains,
    avg(subdomain_len) as avg_sub_len, values(dns_query_name) as queried_domains
  by agent_hostname, src_ip
| filter (query_count > 10 and avg_sub_len > 40) or (is_base64 = true and query_count > 3)
| sort desc query_count
```

---

##### Query 3: C2 Callback Port 8880 Detection

**Splunk SPL**
```splunk
index=firewall OR index=proxy dest_port=8880
| stats count values(dest_ip) as dest_ips by src_ip
| where count > 3
```

**QRadar AQL**
```sql
SELECT sourceip, destinationip, destinationport,
  COUNT(*) AS connection_count, MIN(starttime) AS first_seen, MAX(starttime) AS last_seen
FROM events
WHERE destinationport = 8880 AND eventdirection = 'L2R'
GROUP BY sourceip, destinationip, destinationport
HAVING COUNT(*) > 3
ORDER BY connection_count DESC
```

**Cortex XDR XQL**
```xql
dataset = network_story
| filter dst_action_external_port = 8880
| comp count(*) as conn_count, values(dst_action_external_ip) as dest_ips,
    min(_time) as first_seen, max(_time) as last_seen
  by agent_hostname, src_ip
| filter conn_count > 3
| sort desc conn_count
```

---

##### Query 4: Anomalous User-Agent Communication

**Splunk SPL**
```splunk
index=proxy
| regex http_user_agent="(WhatsAppBackup|WhatsappSync|TelegramSync)/"
| stats count values(url) by src_ip, http_user_agent
```

**QRadar AQL**
```sql
SELECT sourceip, destinationip, httpuseragent, url,
  COUNT(*) AS hit_count, MIN(starttime) AS first_seen, MAX(starttime) AS last_seen
FROM events
WHERE httpuseragent ILIKE '%WhatsAppBackup%'
  OR httpuseragent ILIKE '%WhatsappSync%'
  OR httpuseragent ILIKE '%TelegramSync%'
GROUP BY sourceip, destinationip, httpuseragent, url
ORDER BY hit_count DESC
```

**Cortex XDR XQL**
```xql
dataset = xdr_data
| filter event_type = "HTTP"
| filter http_request_user_agent contains "WhatsAppBackup"
     or http_request_user_agent contains "WhatsappSync"
     or http_request_user_agent contains "TelegramSync"
| comp count(*) as hit_count, values(http_request_url) as urls
  by agent_hostname, src_ip, http_request_user_agent
| sort desc hit_count
```

---

##### Query 5: RMM Tool Anomalous Traffic (IP Naming Pattern)

**Splunk SPL**
```splunk
index=proxy OR index=firewall
| regex url="ClientSetup\.exe"
| regex url="\d{1,3}\.\d{1,3}\.\d{1,3}\.\d{1,3}ClientSetup"
| stats count values(url) by src_ip, dst_ip
```

**QRadar AQL**
```sql
SELECT sourceip, destinationip, url, filename, COUNT(*) AS hit_count
FROM events
WHERE (url ILIKE '%ClientSetup.exe%' OR filename ILIKE '%ClientSetup.exe%')
  AND url SIMILAR TO '%[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}ClientSetup%'
GROUP BY sourceip, destinationip, url, filename
ORDER BY hit_count DESC
```

**Cortex XDR XQL**
```xql
dataset = xdr_data
| filter event_type in ("ENUM_FILE", "HTTP", "NETWORK")
| filter action_file_name contains "ClientSetup.exe"
     or http_request_url contains "ClientSetup.exe"
| filter action_file_name ~= "\d{1,3}\.\d{1,3}\.\d{1,3}\.\d{1,3}ClientSetup"
     or http_request_url ~= "\d{1,3}\.\d{1,3}\.\d{1,3}\.\d{1,3}ClientSetup"
| comp count(*) as hit_count, values(action_file_path) as file_paths,
    values(action_external_ip) as c2_ips
  by agent_hostname, src_ip
| sort desc hit_count
```

---

#### Layer 2: Endpoint — Behavioral Artifact Detection

##### Query 6: Suspicious Process Injection Chain (System Process Spawning Anomalous Child)

**Splunk SPL**
```splunk
index=sysmon EventCode=1
| eval parent_name = mvindex(split(ParentImage, "\"), -1)
| eval child_name = mvindex(split(Image, "\"), -1)
| where parent_name IN ("explorer.exe", "svchost.exe", "winlogon.exe", "sihost.exe", "winevr.exe")
  AND child_name IN ("powershell.exe", "cmd.exe", "rundll32.exe", "wscript.exe", "cscript.exe", "mshta.exe", "regsvr32.exe")
| stats count values(CommandLine) by Computer, ParentImage, Image, User
```

**QRadar AQL**
```sql
SELECT hostname, username, parentprocessname, processname, commandline,
  COUNT(*) AS hit_count, MIN(starttime) AS first_seen, MAX(starttime) AS last_seen
FROM events
WHERE parentprocessname IN ('explorer.exe','svchost.exe','winlogon.exe','sihost.exe','winevr.exe')
  AND processname IN ('powershell.exe','cmd.exe','rundll32.exe','wscript.exe','cscript.exe','mshta.exe','regsvr32.exe')
  AND (LOGSOURCETYPENAME(devicetype) LIKE '%Sysmon%' OR LOGSOURCETYPENAME(devicetype) LIKE '%Windows%')
GROUP BY hostname, username, parentprocessname, processname, commandline
ORDER BY hit_count DESC
```

**Cortex XDR XQL**
```xql
dataset = xdr_data
| filter event_type = "process"
| filter action_process_image_name in (
    "powershell.exe","cmd.exe","rundll32.exe","wscript.exe","cscript.exe","mshta.exe","regsvr32.exe"
  )
| filter actor_process_image_name in (
    "explorer.exe","svchost.exe","winlogon.exe","sihost.exe","winevr.exe"
  )
| fields _time, agent_hostname, actor_effective_username, actor_process_image_name,
    actor_process_command_line, action_process_image_name, action_process_command_line,
    action_process_image_path
| sort desc _time
```

---

##### Query 7: Disguised File Execution Detection (Social Engineering Lures)

**Splunk SPL**
```splunk
index=sysmon EventCode=1
| where ParentImage IN ("*explorer.exe", "*wechat.exe", "*wxwork.exe", "*qq.exe")
| where (Image LIKE "%.exe" AND (
    match(OriginalFileName, "(?i)(名单|违纪|裁员|补偿|稽查|通报|调查)")
    OR match(CommandLine, "(?i)(名单|违纪|裁员|补偿|稽查|通报|调查)")
))
| stats count by Computer, User, ParentImage, Image, CommandLine, Hashes
```

---

##### Query 8: LotL Path Anomalous DLL Detection

**Splunk SPL**
```splunk
index=sysmon EventCode=7
| where ImageLoaded LIKE "C:\\Program Files\\Internet Explorer\\%"
| where NOT (ImageLoaded IN ("ieproxy.dll", "ieproxy64.dll", "ielowutil.exe"))
| stats count values(ImageLoaded) values(Hashes) by Computer, Image, User
```

---

##### Query 9: AnyDesk Silent Installation Detection

**Splunk SPL**
```splunk
index=sysmon (EventCode=1 AND (CommandLine LIKE "%AnyDesk%" OR CommandLine LIKE "%--install%"))
  OR (EventCode=13 AND TargetObject LIKE "%AnyDesk%")
| stats count values(CommandLine) values(TargetObject) by Computer, User, Image
```

---

##### Query 10: Anomalous GPO Distribution Detection

**Splunk SPL**
```splunk
index=sysmon EventCode=1
| where ParentImage LIKE "%gpscript.exe%" OR ParentImage LIKE "%gpupdate%"
| where Image IN ("cmd.exe", "powershell.exe", "msiexec.exe", "wscript.exe")
| stats count values(CommandLine) by Computer, User, Image, ParentImage
```

---

## III. The Highlight — The Attacker's Smartest Moves

### 3.1 🏆 Most Clever Attack Techniques

#### 1. Weaponizing Legitimate Tools — Making Security Products "Look the Other Way"

Silver Fox's most ingenious design is not writing novel malware, but **turning legitimate tools into weapons**:

- Silently installing AnyDesk with unattended access configured
- Using HoldingHands (Gh0st RAT variant) for high-value espionage
- Security products observe "legitimate remote access software running" — **signature/whitelist systems completely fail**

> Analogy: Not breaking down the door, but walking in wearing a property management uniform.

#### 2. MitB (Man-in-the-Browser) — From "Stealing Secrets" to "Tampering with Reality"

The most dangerous evolution of 2024–2026: Attack objectives upgraded from "stealing your password" to **"tampering with everything you see"**:

- Real-time modification of **payee account numbers** and **transfer amounts** on online banking pages
- Users see a perfectly normal page in the browser, but the actual transaction has been tampered with
- Victims only discover the anomaly after funds have been transferred — by then, recovery is impossible

> A Hong Kong multinational lost **HK$200 million** through AI-fabricated CFO video conference fraud.

#### 3. AI-Powered Full Chain — Entering the "AI vs. AI" Era

- **LLM-generated phishing narratives**: No longer template-based; personalized content generated based on target industry, position, and recent news
- **Deepfake verification bypass**: Fabricated executive video/audio for secondary social engineering confirmation, defeating "call-back verification" defenses
- **AI-driven AV evasion mutation**: Evasion lifespan extended from 3 days to 7–14 days with daily automatic mutation against signature detection

#### 4. "Fattening" to Deceive Cloud Scanning — Simple but Effective

Inflating payload files to **50–100MB+**, exploiting cloud scanning engines' tendency to skip deep scanning of large files. Crude but extremely effective.

#### 5. Supply Chain Delivery — From "Phishing" to "Poisoning"

In 2024, Silver Fox compromised a mainstream financial management system's update server, affecting over **200** financial institutions. Legitimate software update channels became an expressway for attack payloads.

### 3.2 🏆 Trickiest Data Analysis Challenges

1. **Fileless attacks**: Entire execution in memory with no file artifacts — traditional file-hash-based detection completely fails
2. **Legitimate traffic camouflage**: C2 communication over HTTPS port 443 or legitimate cloud service APIs, indistinguishable from normal business traffic
3. **High-frequency domain rotation**: Dozens to hundreds of new phishing domains daily — IOC blacklists can never keep up
4. **AI-assisted mutation**: Code signatures automatically modified with each propagation; variant hashes within the same family are completely different
5. **Kernel-level counter-detection**: 6.0 era introduces kernel callback unhooking, bypassing even some kernel-level monitoring

---

## IV. The Recommendations — Defense-in-Depth

### 4.1 Strategic Recommendations

| # | Recommendation | Priority | Description |
|---|---------------|----------|-------------|
| 1 | **Shift from IOC-driven to TTPs-driven detection** | 🔴 Critical | Silver Fox's high-frequency mutation renders IOCs stale quickly; detection should be based on behavioral and tactical patterns |
| 2 | **Build AI-vs-AI capabilities** | 🔴 Critical | Use AI-driven threat intelligence to counter AI-driven evasion mutation and phishing generation |
| 3 | **Implement "forward defense" strategy** | 🔴 Critical | Identify and block attacks before they land via mapping + DNS analysis + sample hunting |
| 4 | **Supply chain security audit** | 🟠 High | Integrity verification and signature validation for critical business software update channels |
| 5 | **Zero Trust architecture implementation** | 🟠 High | IM file transfers should be untrusted by default; all external files must pass sandbox inspection before release |

### 4.2 Tactical Recommendations

#### Endpoint Protection

- **Enforce AMSI and ETW**: Monitor for Patch/Unhooking behaviors
- **Restrict PowerShell execution policy**: Constrained Language Mode + Script Block Logging
- **Full Sysmon deployment**: Focus on Event ID 1/3/7/11/12-13/19-21/22
- **EDR rule updates**: Target reflective DLL loading, process hollowing, kernel callback unhooking
- **AnyDesk/TeamViewer allowlist control**: Only authorized installations permitted; all others alert

#### Network Protection

- **Outbound traffic baselining**: Establish baseline alerts for C2 port (8880) and User-Agent patterns
- **DNS tunneling detection**: Deploy real-time detection for long subdomains + high-frequency queries + Base64 patterns
- **DNS-over-HTTPS control**: Restrict unauthorized DoH service usage to prevent DNS bypass
- **Cloud service API traffic auditing**: Alert on anomalous usage patterns of legitimate cloud services (e.g., Azure Blob, AWS S3)

#### Identity & Access

- **IM file transfer policy**: Mandatory sandbox inspection for executables transferred via enterprise IM
- **Financial system dual-person verification**: Large transfers must be confirmed offline, not relying on browser-displayed content
- **GPO change auditing**: All group policy changes require approval + alerting
- **Hardware token / USB security key**: Critical transactions must be confirmed with hardware second factor

#### Security Awareness

- **Targeted training**: Regular Silver Fox-specific phishing drills for finance/tax/HR personnel
- **Deepfake recognition training**: Executive video/voice verification processes must include non-digital channel confirmation
- **"Folder icon" trap awareness**: Train users to identify executables disguised as PDFs/documents

### 4.3 Incident Response Recommendations

#### Response Playbook for a Compromised Host

```
1. Immediately isolate the host (disconnect network, do NOT shut down — preserve memory evidence)
2. Review IM chat history — determine if the attacker has already propagated via IM
3. Scan entire network for LotL paths — anomalous DLLs under C:\Program Files\Internet Explorer\
4. Investigate outbound port 8880 connections — determine C2 communication scope
5. Review scheduled tasks — high-frequency tasks disguised as "Adobe Updater"/"Windows Update"
6. Check AnyDesk installation records — look for silent installation
7. Audit GPO changes — identify any anomalous group policy distribution
8. Browser session inspection — check for MitB tampering artifacts
9. Internal lateral movement investigation — SMB weak passwords, IPC$ connections
10. Collect samples + report IOCs — submit to security vendors for correlation analysis
```


---

## Appendix

### A. Known IOCs (as of 2026-06-22)

**IP Addresses**
```
115.187.17.212, 112.121.183.102, 156.251.18.45, 206.238.178.116,
45.119.55.66, 69.30.250.99, 103.228.12.151, 154.91.84.3,
93.127.142.77, 150.109.79.82, 170.205.54.88, 222.186.190.138,
103.203.48.174, 130.250.191.46, 220.167.103.145, 220.167.103.160,
154.201.87.124, 154.201.87.75
```

**Domains**
```
360sdgg.com, googlevip.icu, oytdwzz.shop, cocdex.cn,
xqwmwru.top, gov.incometax.click
```

### B. Data Sources

ThreatBook, CVERC (China Computer Virus Emergency Response Center), ThreatBook Online, QiAnXin, Antiy, 360, PwC DarkLab, Sekoia, Kaspersky, CN-SEC, CSDN, Security Analysis & Research (pandazhengzheng), and others

---

> **Recommended Update Frequency**: Monthly
> **Next Update**: 2026-07-24
