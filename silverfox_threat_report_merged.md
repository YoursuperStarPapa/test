# 🦊 Silver Fox（银狐）APT 组织深度分析报告 — 整合版

> **报告生成时间**：2026-06-22
> **情报时效**：基于 2025–2026 年公开威胁情报
> **建议更新频率**：每月
> **数据来源**：ThreatBook、国家计算机病毒应急处理中心（CVERC）、微步在线、奇安信、安天、360、PwC DarkLab、Sekoia、卡巴斯基、CN-SEC、CSDN 等

---

## 1. The Findings — 技术剖析

### 1.1 组织画像

| Field | Detail |
|---|---|
| **Threat Name** | SilverFox（银狐）/ 游蛇 / 谷堕大盗 / UTG-Q-1000 / SwimSnake / Void Arachne |
| **组织性质** | 黑灰产犯罪组织，以经济利益为驱动（2025 年后演变为"间谍+犯罪"双重模式） |
| **命名来源** | 早期样本 PDB 路径中存在"Silver Fox"字符串 |
| **首次披露** | 2020-2021 年间，微步在线、奇安信等安全厂商陆续披露 |
| **活跃时间** | 2020 年至今，2025-2026 年大规模活跃 |
| **木马基础** | 改写自开源 Gh0st RAT 4.0，后发展出 ValleyRAT 等独立家族 |
| **组织规模** | 推测 10-50 人规模的黑产团伙 |
| **攻击范围** | 初始以中国境内为主，2025 年后扩展至印度、日本、马来西亚、菲律宾等亚太地区 |

#### NCC Group 披露的四个子群组

| 子群组 | 职能 | 说明 |
|---|---|---|
| **金融组** | 资金窃取与诈骗变现 | 冒充领导转账、窃取银行凭据 |
| **新闻与情感组** | 社工诱饵制作与投放 | 伪造公文、制作钓鱼素材 |
| **设计与制造组** | 木马开发与免杀更新 | 核心开发者，负责 ValleyRAT 等恶意软件迭代 |
| **"黑水坑"组** | 基础设施运营与数据处理 | C2 服务器管理、域名轮换、窃取数据变现 |

### 1.2 攻击目标

**目标行业**：

- 财务/税务人员（首要目标）
- 人事/IT 运维人员
- 金融行业（约 42%）
- 医疗机构（约 28%）
- 政府机构（约 20%）
- 制造业/能源、央企/国企、中小企业（占比 73%）

**目标地域**（持续扩大）：

| 阶段 | 区域 |
|------|------|
| 核心区域 | 中国大陆、中国台湾 |
| 扩展区域（2025–2026） | 日本、马来西亚、菲律宾、泰国、印度尼西亚、新加坡、印度、越南、香港 |

### 1.3 攻击链全貌

```
┌──────────────────────────────────────────────────────────────────────────┐
│                        Silver Fox 完整攻击链                              │
├──────────────────────────────────────────────────────────────────────────┤
│                                                                          │
│  ① 投递入口            ② 初始执行              ③ 环境适配                │
│  ┌──────────────┐     ┌──────────────┐      ┌──────────────┐            │
│  │ 微信/企微/QQ  │     │ 伪装文件夹/PDF │      │ 反虚拟机(50+) │            │
│  │ 仿冒网站/SEO  │────→│ 虚假错误提示   │─────→│ 反沙箱/反杀软 │            │
│  │ 钓鱼邮件      │     │ 合法软件释放   │      │ 不通过则终止  │            │
│  └──────────────┘     └──────────────┘      └──────┬───────┘            │
│                                                     ↓                    │
│  ⑥ 横向扩散            ⑤ C2 建立              ④ 持久化                   │
│  ┌──────────────┐     ┌──────────────┐      ┌──────────────┐            │
│  │ IM冒充领导    │     │ 端口8880      │      │ 计划任务伪装  │            │
│  │ SMB/弱密码    │←────│ AES-256加密   │←─────│ 注册表Run键   │            │
│  │ 内网蠕虫传播  │     │ 动态域名轮换  │      │ 系统服务创建  │            │
│  └──────────────┘     └──────────────┘      └──────────────┘            │
│                                                                          │
│  ⑦ 影响                                                                    │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐  ┌──────────┐     │
│  │ 冒充领导诈骗  │  │ 浏览器密码窃取│  │ AnyDesk静默  │  │ 屏幕监控  │     │
│  │ 最高500万/笔  │  │ Cookie/财务数据│  │ 桌面接管     │  │ 键盘记录  │     │
│  └──────────────┘  └──────────────┘  └──────────────┘  └──────────┘     │
└──────────────────────────────────────────────────────────────────────────┘
```

### 1.4 2025–2026 三波攻击演进

| 阶段 | 时间 | 投递方式 | 载荷类型 | 目标扩展 |
|------|------|----------|----------|----------|
| **Wave 1** | 2025.01 | 税务审计主题 PDF 附件（台湾财政部公告仿冒） | ValleyRAT（DLL 侧加载） | 台湾 → 日本 |
| **Wave 2** | 2025.12 | 钓鱼邮件 → 虚假税务网站 → ZIP/RAR 下载 | 合法 RMM 工具（SyncFutureTec 签名） | 马来西亚、菲律宾、泰国、印尼、新加坡、印度 |
| **Wave 3** | 2026.02 | 钓鱼网站（马来语）→ 下载页 | Python 窃取器伪装 WhatsApp | 马来西亚及更广泛南亚 |

### 1.5 变种演化时间线（2020–2026）

```
2020 ──── 首次披露，聚焦财税行业
  │        └─ ValleyRAT 基础远控诞生（Go/Rust加载器 + 内存Shellcode）
  │
2021 ──── 显著上升，扩展至政务、IM投递
  │        ├─ 白加黑（DLL Side-Loading）成为标配
  │        └─ 免杀1.0时代：简单加壳 + 文件属性伪造 + 宏代码混淆
  │
2022 ──── 持续上升，百余变种
  │        ├─ 引入 Direct Syscall + Shellcode 多层加密
  │        ├─ SEO投毒增加（伪造Chrome下载站等）
  │        └─ 免杀2.0时代：白加黑+DLL侧载，大幅绕过静态检测
  │
2023 ──── 高频攻击，数百变种
  │        ├─ 全行业扩散（教育、医疗、制造业、物流）
  │        ├─ HackBrian RAT 引入
  │        └─ 免杀3.0时代：Direct Syscall + Shellcode多层加密，绕过EDR用户态Hook
  │
2024 ──── 爆发式增长，千余变种
  │        ├─ EDR深度对抗能力成熟，无文件攻击成为主流
  │        ├─ ABCDoor 高级持久化变种出现（Python编写，双重持久化）
  │        ├─ AtlasCross RAT 新型远控出现
  │        └─ 免杀4.0时代：Indirect Syscall + ETW/AMSI Patch + Unhooking
  │
2025 ──── 持续高位，技术范式转移
  │        ├─ AnyDesk联合诈骗变种成为主流（合法远控武器化）
  │        ├─ Python Stealer 窃密变种出现
  │        ├─ HoldingHands 合法远控工具被滥用
  │        ├─ "双重间谍"模式出现（间谍+犯罪并行）
  │        └─ 免杀5.0时代：全栈无文件 + Callback执行 + AI辅助免杀
  │
2026 ──── 当前——产业化巅峰
           ├─ 最新变种：Rust加载器 + ValleyRAT + Python后门ABCDoor
           ├─ 攻击事件同比增长127%，超1.2万家企业中招
           ├─ 单笔诈骗金额最高达500万元
           ├─ Chrome变种：PowerShell AMSI绕过 + 反射式加载
           └─ 国家病毒中心发布专项预警（2026-05-21）
```

### 1.6 恶意软件家族矩阵

| # | 变种名称 | 首次出现 | 核心技术 | 主要功能 | 活跃状态 |
|---|---|---|---|---|---|
| 1 | **ValleyRAT** | 2020 | Go/Rust加载器 + 内存Shellcode | 完整远控（命令执行、文件管理、屏幕截图、键盘记录） | ✅ 最基础变种 |
| 2 | **Ghost改良版（银狐RAT）** | 2022 | 基于ValleyRAT改良 | 键盘记录、截屏、文件窃取 | ✅ 持续活跃 |
| 3 | **HackBrian RAT** | 2023 | 模块化加载 | 数据窃取、代理隧道搭建 | 🔄 间歇活跃 |
| 4 | **AtlasCross RAT** | 2024 | 新型RAT框架 | 远程控制，鱼叉攻击 | ✅ 活跃 |
| 5 | **AnyDesk联合变种** | 2025 | 银狐+AnyDesk静默安装 | 桌面接管 + 冒充领导诈骗 | ✅ 危害最大 |
| 6 | **HoldingHands** | 2025.10 | Gh0st RAT 变种 | 远程桌面、文件操作、屏幕监控 | ✅ 高价值间谍 |
| 7 | **ABCDoor** | 2025底 | Python，双重持久化 | 对抗EDR，高级持久化 | ✅ 高阶变种 |
| 8 | **Python Stealer** | 2026.02 | Python编写，PyInstaller打包 | 专注窃密，完成后自删除 | ✅ 新变种 |
| 9 | **Chrome变种** | 2026.05 | PowerShell AMSI绕过 + 反射式加载 | 无文件攻击，反沙箱 | ✅ 最新变种 |

### 1.7 免杀技术演进五代

| 时代 | 时间段 | 核心手段 | 绕过能力 |
|---|---|---|---|
| **1.0** | 2020-2021H1 | 简单加壳、文件属性伪造、宏代码混淆 | 可被主流AV静态+动态检测 |
| **2.0** | 2021H2-2022H1 | 白加黑 + DLL侧载成为标配 | 大幅绕过静态检测，利用白程序信誉 |
| **3.0** | 2022H2-2023H1 | Direct Syscall + Shellcode多层加密 | 绕过EDR用户态Hook |
| **4.0** | 2023H2-2024 | Indirect Syscall + ETW/AMSI Patch + Unhooking | APT级EDR绕过 |
| **5.0** | 2024-2026 | 全栈无文件 + Callback执行 + AI辅助免杀 | 传统安全产品检测面临极大挑战 |

### 1.8 关键技术能力详解

**白加黑加载（White-Listing Bypass）**：
- 投放阶段：在 `C:\Program Files\Internet Explorer\` 下投放 `log.dll` 等载荷
- 利用合法的 `installer.exe`（白文件）加载恶意 `log.dll`
- 系统工具滥用：`mshta.exe`、`rundll32.exe`、`regsvr32.exe`、`powershell.exe`

**进程注入与无文件执行**：
- 反射式 DLL 加载（Reflective DLL Loading），不依赖 LoadLibrary，完全内存执行
- DLL 侧加载到合法签名程序
- 注入目标：`explorer.exe`、`svchost.exe`、`sihost.exe`、`winevr.exe`、`winlogon.exe`
- PowerShell 框架直接在内存中加载 .NET CLR，Shellcode 加密后内存解执行

**持久化机制**：
- 注册表 Run 键（早期）→ WMI 事件订阅 + AppInit_DLLs（V3+）
- BAT 脚本守护：`C:\Windows\` 下释放 BAT，循环检测木马进程状态
- 伪装系统服务：`sc` 命令创建/维护伪装服务
- 计划任务：伪装为"Adobe Updater"、"Windows Update"、"System Check"，每 1-5 分钟执行

**C2 通信**：
- HTTP/HTTPS 协议，端口 8880，模拟正常业务流量
- DNS 隧道（2024.05+）：Base64 编码数据嵌入 DNS 查询子域名
- AES-256-GCM 加密（2024.05+），取代旧版 RC4/XOR
- 动态域名 + 云服务 + 中转节点

**对抗技术**：
- 反虚拟机检测（50+ 特征：CPUID、硬件序列号、MAC地址等）
- 安全软件识别与回避
- 延迟执行 + 自删除
- "增肥"手法：母体膨胀至 50–100MB+，欺骗云查杀
- 2026 年新增：自编写驱动 + Unhooking 双链路攻击
- 利用微软签名驱动 amsdk.sys（BYOVD）绕过漏洞驱动黑名单

**合法远控工具武器化**：
- 静默安装 AnyDesk 并设置无人值守访问
- 利用 HoldingHands（Gh0st RAT 变种）进行高价值间谍活动
- 安全产品看到的是"合法远控软件在运行"，签名/白名单体系失效

**文件系统痕迹**：

```
恶意文件常驻路径：
  C:\Program Files\Internet Explorer\     ← 白利用载荷投递路径（log.dll）
  C:\Users\Public\Download\              ← 配置文件
  %APPDATA%                              ← 用户目录隐藏文件
  %ProgramData%                          ← 程序数据目录
  %TEMP%                                 ← 临时文件
  C:\Windows\                            ← BAT 持久化脚本

Python 窃取器痕迹：
  C:\WhatsAppBackup\WhatsAppData.zip
  %TEMP%\whatsapp_backup.lock

Chrome变种痕迹：
  PDB路径：XiaobaoService.pdb
  关联进程：ABoxHeadless.exe
  关联DLL：qimei.dll
```

### 1.9 "双重间谍"模式（2025-2026 新趋势）

| 维度 | 战略情报线 | 机会主义犯罪线 |
|---|---|---|
| **目标** | 特定目标（如台湾机构、印度政府部门） | 广泛的金融、税务相关行业 |
| **时机** | 特定时间窗口（如税务审计期） | 全年持续攻击 |
| **目的** | 获取敏感政治或经济情报 | 窃取资金或凭证变现 |
| **工具** | 定制化ValleyRAT + 高级持久化 | Python Stealer + 合法远控 |
| **投递** | 精准鱼叉式钓鱼 | 批量IM投递 + SEO投毒 |

### 1.10 MITRE ATT&CK 映射

| Tactic | Technique | ID | Description |
|---|---|---|---|
| **Initial Access** | Phishing: Spearphishing via Service | T1566.003 | 通过微信/QQ/企微发送钓鱼文件 |
| **Execution** | User Execution: Malicious File | T1204.002 | 用户双击伪装成文档的exe |
| **Execution** | Command and Scripting Interpreter: PowerShell | T1059.001 | 内存加载.NET CLR执行恶意代码 |
| **Defense Evasion** | Process Injection | T1055 | 反射式DLL注入/APC注入/进程空心化 |
| **Defense Evasion** | Masquerading | T1036 | 文件名伪装（加pdf后缀、图标伪装） |
| **Defense Evasion** | Signed Binary Proxy Execution | T1218 | 白加黑：利用installer.exe加载恶意DLL |
| **Defense Evasion** | Indicator Removal | T1070 | 攻击结束后卸载AnyDesk、清除痕迹 |
| **Defense Evasion** | Virtualization/Sandbox Evasion | T1497 | 检测50+虚拟机/沙箱特征 |
| **Persistence** | Scheduled Task/Job | T1053.005 | 创建伪装计划任务，每1-5分钟执行 |
| **Persistence** | Registry Run Keys | T1547.001 | HKCU/HKLM Run键持久化 |
| **Persistence** | Create or Modify System Process | T1543.003 | 创建随机名称系统服务 |
| **Discovery** | Remote System Discovery | T1018 | 内网存活主机扫描 |
| **Credential Access** | Credentials from Password Stores | T1555 | 窃取浏览器保存的密码/Cookie |
| **Collection** | Screen Capture | T1113 | 屏幕监控与截图 |
| **Collection** | Input Capture: Keylogging | T1056.001 | 键盘记录 |
| **C2** | Encrypted Channel | T1573 | AES-256加密C2通信 |
| **C2** | Non-Standard Port | T1571 | 端口8880 |
| **C2** | Remote Access Software | T1219 | 静默安装AnyDesk实现远控 |
| **Exfiltration** | Exfiltration Over C2 Channel | T1041 | 窃取数据通过C2上传 |

---

## 2. The Hunt — 威胁狩猎

### 2.1 爆炸半径评估假设

> **核心假设**：如果在环境中发现一台 Silver Fox 感染主机，攻击者很可能已通过聊天工具完成二次传播，感染范围远超初始受害者。

### 2.2 狩猎假设（Hunt Hypotheses）

| # | Hypothesis | Confidence | Rationale |
|---|---|---|---|
| H1 | 组织内存在通过IM工具接收并执行伪装成文档的可执行文件的行为 | High | 银狐90%以上通过微信/企微/QQ投递 |
| H2 | 存在`C:\Program Files\Internet Explorer\`目录下异常DLL文件（如log.dll） | High | 本次预警明确的白利用载荷路径 |
| H3 | 系统关键进程（explorer.exe/svchost.exe）存在异常出站HTTPS连接至端口8880 | High | C2回联特征端口 |
| H4 | 存在伪装成"Adobe Updater"/"Windows Update"的高频计划任务 | Medium | 银狐偏好持久化方式 |
| H5 | 终端静默安装了AnyDesk/TeamViewer等合法远控软件 | Medium | AnyDesk联合诈骗变种特征 |
| H6 | PowerShell进程存在内存加载.NET CLR的异常行为 | Medium | 无文件攻击特征 |
| H7 | 存在高熵.text节且导入函数极少的DLL | Medium | 银狐Chrome变种特征 |

### 2.3 遥测数据源（Telemetry Sources）

- [x] **EDR 进程树 & 文件事件** — 进程父子关系、DLL加载、文件创建
- [x] **防火墙/代理日志** — 出站连接目标IP/域名/端口
- [x] **DNS 查询日志** — DGA域名识别、DNS隧道检测
- [x] **Windows 计划任务日志** — Event ID 106/140/200/201
- [x] **Windows PowerShell 日志** — Event ID 4103/4104（脚本块日志）
- [x] **Windows Sysmon** — Event ID 1/3/7/11/12-13/19-21/22
- [x] **Active Directory 认证日志** — 横向移动检测
- [x] **即时通讯工具审计日志** — 文件传输记录

### 2.4 分层检测查询

> 以下每个检测场景均提供 **Splunk SPL**、**QRadar AQL**、**Cortex XDR XQL** 三种平台的查询。

---

#### Layer 1：网络层 — C2 通信检测

##### 查询 1：已知 IOC 命中

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
SELECT
  sourceip,
  destinationip,
  destinationport,
  domainname,
  COUNT(*) AS event_count,
  MIN(starttime) AS first_seen,
  MAX(starttime) AS last_seen
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
| alter
    hour = hour_of_day(_time),
    day_type = if(day_of_week(_time) in ("Saturday","Sunday"), "weekend", "weekday")
| comp count(*) as hit_count, values(dst_action_external_ip) as c2_ips, min(_time) as first_seen, max(_time) as last_seen by agent_hostname, src_action_ip
| sort desc hit_count
```

---

##### 查询 2：DNS 隧道检测（长子域名 + 高频查询）

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
SELECT
  sourceip,
  domainname,
  COUNT(*) AS query_count,
  COUNT(DISTINCT domainname) AS unique_domains,
  AVG(LENGTH(SUBSTR(domainname, 1, INSTR(domainname, '.') - 1))) AS avg_subdomain_len
FROM events
WHERE
  eventdirection = 'L2R'
  AND (LOGSOURCETYPENAME(devicetype) = 'DNS Server' OR LOGSOURCETYPENAME(devicetype) LIKE '%DNS%')
  AND domainname IS NOT NULL
  AND LENGTH(SUBSTR(domainname, 1, INSTR(domainname, '.') - 1)) > 30
GROUP BY sourceip, domainname
HAVING
  COUNT(*) > 5
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
| comp
    count(*) as query_count,
    count_distinct(dns_query_name) as unique_domains,
    avg(subdomain_len) as avg_sub_len,
    values(dns_query_name) as queried_domains
  by agent_hostname, src_ip
| filter (query_count > 10 and avg_sub_len > 40) or (is_base64 = true and query_count > 3)
| sort desc query_count
```

---

##### 查询 3：C2 回联端口 8880 检测

**Splunk SPL**
```splunk
index=firewall OR index=proxy dest_port=8880
| stats count values(dest_ip) as dest_ips by src_ip
| where count > 3
```

**QRadar AQL**
```sql
SELECT
  sourceip,
  destinationip,
  destinationport,
  COUNT(*) AS connection_count,
  MIN(starttime) AS first_seen,
  MAX(starttime) AS last_seen
FROM events
WHERE
  destinationport = 8880
  AND eventdirection = 'L2R'
GROUP BY sourceip, destinationip, destinationport
HAVING COUNT(*) > 3
ORDER BY connection_count DESC
```

**Cortex XDR XQL**
```xql
dataset = network_story
| filter dst_action_external_port = 8880
| comp
    count(*) as conn_count,
    values(dst_action_external_ip) as dest_ips,
    min(_time) as first_seen,
    max(_time) as last_seen
  by agent_hostname, src_ip
| filter conn_count > 3
| sort desc conn_count
```

---

##### 查询 4：异常 User-Agent 通信

**Splunk SPL**
```splunk
index=proxy
| regex http_user_agent="(WhatsAppBackup|WhatsappSync|TelegramSync)/"
| stats count values(url) by src_ip, http_user_agent
```

**QRadar AQL**
```sql
SELECT
  sourceip,
  destinationip,
  httpuseragent,
  url,
  COUNT(*) AS hit_count,
  MIN(starttime) AS first_seen,
  MAX(starttime) AS last_seen
FROM events
WHERE
  httpuseragent ILIKE '%WhatsAppBackup%'
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
| comp count(*) as hit_count, values(http_request_url) as urls by agent_hostname, src_ip, http_request_user_agent
| sort desc hit_count
```

---

##### 查询 5：RMM 工具异常流量（IP 命名模式）

**Splunk SPL**
```splunk
index=proxy OR index=firewall
| regex url="ClientSetup\.exe"
| regex url="\d{1,3}\.\d{1,3}\.\d{1,3}\.\d{1,3}ClientSetup"
| stats count values(url) by src_ip, dst_ip
```

**QRadar AQL**
```sql
SELECT
  sourceip,
  destinationip,
  url,
  filename,
  COUNT(*) AS hit_count
FROM events
WHERE
  (url ILIKE '%ClientSetup.exe%' OR filename ILIKE '%ClientSetup.exe%')
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
| comp count(*) as hit_count, values(action_file_path) as file_paths, values(action_external_ip) as c2_ips by agent_hostname, src_ip
| sort desc hit_count
```

---

#### Layer 2：终端层 — 行为痕迹检测

##### 查询 6：可疑进程注入链（系统进程产生异常子进程）

**Splunk SPL**
```splunk
index=sysmon EventCode=1
| eval parent_name = mvindex(split(ParentImage, "\\"), -1)
| eval child_name = mvindex(split(Image, "\\"), -1)
| where parent_name IN ("explorer.exe", "svchost.exe", "winlogon.exe", "sihost.exe", "winevr.exe")
  AND child_name IN ("powershell.exe", "cmd.exe", "rundll32.exe", "wscript.exe", "cscript.exe", "mshta.exe", "regsvr32.exe")
| stats count values(CommandLine) by Computer, ParentImage, Image, User
```

**QRadar AQL**
```sql
SELECT
  hostname,
  username,
  parentprocessname,
  processname,
  commandline,
  COUNT(*) AS hit_count,
  MIN(starttime) AS first_seen,
  MAX(starttime) AS last_seen
FROM events
WHERE
  parentprocessname IN ('explorer.exe','svchost.exe','winlogon.exe','sihost.exe','winevr.exe')
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
| fields _time, agent_hostname, actor_effective_username, actor_process_image_name, actor_process_command_line,
         action_process_image_name, action_process_command_line, action_process_image_path
| sort desc _time
```

---

##### 查询 7：伪装文件执行检测（社工诱饵）

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

**QRadar AQL**
```sql
SELECT
  hostname,
  username,
  parentprocessname,
  processname,
  commandline,
  COUNT(*) AS hit_count
FROM events
WHERE
  (parentprocessname ILIKE '%explorer.exe'
   OR parentprocessname ILIKE '%wechat.exe'
   OR parentprocessname ILIKE '%wxwork.exe'
   OR parentprocessname ILIKE '%qq.exe')
  AND processname ILIKE '%.exe'
  AND (commandline ILIKE '%违纪%' OR commandline ILIKE '%裁员%'
       OR commandline ILIKE '%补偿%' OR commandline ILIKE '%稽查%'
       OR commandline ILIKE '%通报%' OR commandline ILIKE '%调查%')
GROUP BY hostname, username, parentprocessname, processname, commandline
ORDER BY hit_count DESC
```

**Cortex XDR XQL**
```xql
dataset = xdr_data
| filter event_type = "process"
| filter actor_process_image_name ~= "(?i)(explorer|wechat|wxwork|qq)\.exe"
| filter action_process_image_name ~= "(?i).*\.exe$"
| filter action_process_command_line ~= "(?i)(名单|违纪|裁员|补偿|稽查|通报|调查)"
| fields _time, agent_hostname, actor_effective_username, actor_process_image_name,
         action_process_image_name, action_process_command_line, action_file_hash_sha256
| sort desc _time
```

---

##### 查询 8：白利用路径异常 DLL 加载

**Splunk SPL**
```splunk
index=sysmon EventCode=7
| where ImageLoaded LIKE "C:\\Program Files\\Internet Explorer\\%"
| where NOT match(ImageLoaded, "(?i)(ieproxy\.dll|iertutil\.dll)")
| stats count by Computer, Image, ImageLoaded, Signed, Signature
```

**QRadar AQL**
```sql
SELECT
  hostname,
  username,
  processname,
  filename,
  COUNT(*) AS hit_count
FROM events
WHERE
  (LOGSOURCETYPENAME(devicetype) LIKE '%Sysmon%')
  AND eventid = 7
  AND filename ILIKE 'C:\Program Files\Internet Explorer\%'
  AND filename NOT ILIKE '%ieproxy.dll%'
  AND filename NOT ILIKE '%iertutil.dll%'
GROUP BY hostname, username, processname, filename
ORDER BY hit_count DESC
```

**Cortex XDR XQL**
```xql
dataset = xdr_data
| filter event_type = "load_image"
| filter action_module_path contains "C:\Program Files\Internet Explorer\"
| filter not action_module_path ~= "(?i)(ieproxy\.dll|iertutil\.dll)"
| fields _time, agent_hostname, actor_process_image_name, action_module_path,
         action_module_signature_vendor, action_module_signature_status
| sort desc _time
```

---

##### 查询 9：WMI 事件订阅创建

**Splunk SPL**
```splunk
index=sysmon (EventCode=19 OR EventCode=20 OR EventCode=21)
| table _time, Computer, EventType, Name, Query, Operation
```

**QRadar AQL**
```sql
SELECT
  hostname,
  username,
  processname,
  commandline,
  COUNT(*) AS hit_count,
  MIN(starttime) AS first_seen
FROM events
WHERE
  (LOGSOURCETYPENAME(devicetype) LIKE '%Sysmon%')
  AND (eventid IN (19, 20, 21))
GROUP BY hostname, username, processname, commandline
ORDER BY first_seen DESC
```

**Cortex XDR XQL**
```xql
dataset = xdr_data
| filter event_type = "WMI"
| filter action_wmi_operation in ("WmiEventFilter","WmiEventConsumer","WmiEventConsumerToFilter")
| fields _time, agent_hostname, actor_effective_username, action_wmi_operation,
         action_wmi_query, action_wmi_consumer_command
| sort desc _time
```

---

##### 查询 10：注册表持久化检测

**Splunk SPL**
```splunk
index=sysmon (EventCode=12 OR EventCode=13)
| where TargetObject IN ("*\\CurrentVersion\\Run*", "*\\AppInit_DLLs*", "*\\CurrentVersion\\Windows*")
| eval suspicious = if(match(TargetObject, "(?i)(WinUpdate|SystemService|SecurityUpdate|svchost)"), "high", "medium")
| table _time, Computer, User, TargetObject, Details, suspicious
```

**QRadar AQL**
```sql
SELECT
  hostname,
  username,
  commandline,
  COUNT(*) AS hit_count,
  MIN(starttime) AS first_seen
FROM events
WHERE
  (LOGSOURCETYPENAME(devicetype) LIKE '%Sysmon%')
  AND (eventid IN (12, 13))
  AND (
    commandline ILIKE '%CurrentVersion\Run%'
    OR commandline ILIKE '%AppInit_DLLs%'
    OR commandline ILIKE '%CurrentVersion\Windows%'
  )
GROUP BY hostname, username, commandline
ORDER BY first_seen DESC
```

**Cortex XDR XQL**
```xql
dataset = xdr_data
| filter event_type = "registry"
| filter action_registry_key_name contains "\Microsoft\Windows\CurrentVersion\Run"
     or action_registry_key_name contains "\AppInit_DLLs"
     or action_registry_key_name contains "\Microsoft\Windows\CurrentVersion\Windows"
| alter suspicious = if(
    action_registry_key_name ~= "(?i)(WinUpdate|SystemService|SecurityUpdate|svchost)",
    "high", "medium"
  )
| fields _time, agent_hostname, actor_effective_username, action_registry_key_name,
         action_registry_value_name, action_registry_data, suspicious
| sort desc _time
```

---

##### 查询 11：可疑计划任务创建

**Splunk SPL**
```splunk
index=sysmon EventCode=1 Image="*schtasks.exe"
| regex(CommandLine, "(?i)(Adobe Updater|Windows Update|System Check|Security Update)")
| stats count by Computer, User, CommandLine, ParentImage
```

**QRadar AQL**
```sql
SELECT
  hostname,
  username,
  processname,
  commandline,
  COUNT(*) AS hit_count
FROM events
WHERE
  processname ILIKE '%schtasks.exe'
  AND commandline ILIKE '%/create%'
  AND (commandline ILIKE '%Adobe Updater%'
       OR commandline ILIKE '%Windows Update%'
       OR commandline ILIKE '%System Check%'
       OR commandline ILIKE '%Security Update%')
GROUP BY hostname, username, processname, commandline
ORDER BY hit_count DESC
```

**Cortex XDR XQL**
```xql
dataset = xdr_data
| filter event_type = "process"
| filter action_process_image_name ~= "(?i)schtasks\.exe"
| filter action_process_command_line contains "/create"
| filter action_process_command_line ~= "(?i)(Adobe Updater|Windows Update|System Check|Security Update)"
| fields _time, agent_hostname, actor_effective_username, action_process_command_line
| sort desc _time
```

---

##### 查询 12：BAT 脚本持久化检测

**Splunk SPL**
```splunk
index=sysmon EventCode=11
| where TargetFilename IN ("C:\\Windows\\*.bat", "C:\\Windows\\System32\\*.bat")
| stats count values(TargetFilename) by Computer, User, Image
```

**QRadar AQL**
```sql
SELECT
  hostname,
  username,
  processname,
  filename,
  COUNT(*) AS hit_count
FROM events
WHERE
  (LOGSOURCETYPENAME(devicetype) LIKE '%Sysmon%')
  AND eventid = 11
  AND (filename ILIKE 'C:\Windows\%.bat' OR filename ILIKE 'C:\Windows\System32\%.bat')
GROUP BY hostname, username, processname, filename
ORDER BY hit_count DESC
```

**Cortex XDR XQL**
```xql
dataset = xdr_data
| filter event_type = "ENUM_FILE"
| filter action_file_name ~= ".*\.bat$"
| filter action_file_path contains "C:\Windows\" and not action_file_path contains "C:\Windows\Temp"
| fields _time, agent_hostname, actor_effective_username, action_file_path, action_file_name, actor_process_image_name
| sort desc _time
```

---

##### 查询 13：异常驱动加载

**Splunk SPL**
```splunk
index=sysmon EventCode=7
| where ImageLoaded IN ("*\\drivers\\amsdk.sys") OR SignatureStatus != "Valid"
| table _time, Computer, ImageLoaded, Signature, SignatureStatus
```

**QRadar AQL**
```sql
SELECT
  hostname,
  username,
  processname,
  filename,
  COUNT(*) AS hit_count
FROM events
WHERE
  (LOGSOURCETYPENAME(devicetype) LIKE '%Sysmon%')
  AND eventid = 7
  AND (filename ILIKE '%amsdk.sys' OR filename ILIKE '%\drivers\%.sys')
GROUP BY hostname, username, processname, filename
ORDER BY hit_count DESC
```

**Cortex XDR XQL**
```xql
dataset = xdr_data
| filter event_type = "load_image"
| filter action_module_path contains "amsdk.sys"
     or action_module_signature_status != "Signed"
| fields _time, agent_hostname, actor_process_image_name, action_module_path,
         action_module_signature_vendor, action_module_signature_status
| sort desc _time
```

---

##### 查询 14：AnyDesk 静默安装检测

**Splunk SPL**
```splunk
index=sysmon EventCode=1
| where Image="*AnyDesk.exe" OR OriginalFileName="*AnyDesk*"
| regex(CommandLine, "(?i)(--install|--start-with-win|--silent|--get-id)")
| stats count by Computer, User, CommandLine, ParentImage
```

**QRadar AQL**
```sql
SELECT
  hostname,
  username,
  processname,
  commandline,
  COUNT(*) AS hit_count
FROM events
WHERE
  (processname ILIKE '%AnyDesk.exe' OR processname ILIKE '%TeamViewer.exe')
  AND (commandline ILIKE '%--install%' OR commandline ILIKE '%--start-with-win%'
       OR commandline ILIKE '%--silent%' OR commandline ILIKE '%--get-id%')
GROUP BY hostname, username, processname, commandline
ORDER BY hit_count DESC
```

**Cortex XDR XQL**
```xql
dataset = xdr_data
| filter event_type = "process"
| filter action_process_image_name ~= "(?i)(AnyDesk|TeamViewer)\.exe"
| filter action_process_command_line ~= "(?i)(--install|--start-with-win|--silent|--get-id)"
| fields _time, agent_hostname, actor_effective_username, action_process_image_name, action_process_command_line
| sort desc _time
```

---

##### 查询 15：聊天工具异常活动（二次传播）

**Splunk SPL**
```splunk
index=edr OR index=sysmon
| where Image IN ("*WeChat*", "*WXWork*", "*DingTalk*")
| eval cmd_len = len(CommandLine)
| where cmd_len > 200 OR match(CommandLine, "(?i)(group|create|send|file|zip|rar)")
| stats count values(CommandLine) by Computer, User, Image
```

**QRadar AQL**
```sql
SELECT
  hostname,
  username,
  processname,
  commandline,
  LENGTH(commandline) AS cmd_len,
  COUNT(*) AS hit_count
FROM events
WHERE
  (
    processname ILIKE '%WeChat%'
    OR processname ILIKE '%WXWork%'
    OR processname ILIKE '%DingTalk%'
    OR processname ILIKE '%Weixin%'
  )
  AND (
    LENGTH(commandline) > 200
    OR commandline ILIKE '%group%'
    OR commandline ILIKE '%create%'
    OR commandline ILIKE '%send%'
    OR commandline ILIKE '%.zip%'
    OR commandline ILIKE '%.rar%'
    OR commandline ILIKE '%.exe%'
  )
GROUP BY hostname, username, processname, commandline
ORDER BY hit_count DESC
```

**Cortex XDR XQL**
```xql
dataset = xdr_data
| filter event_type = "process"
| filter action_process_image_name ~= "(?i)(WeChat|WXWork|DingTalk|Weixin).*\.exe"
| alter cmd_len = string_length(action_process_command_line)
| filter cmd_len > 200
     or action_process_command_line ~= "(?i)(group|create|send|file|\.zip|\.rar|\.exe)"
| fields _time, agent_hostname, actor_effective_username, actor_process_image_name,
         action_process_image_name, action_process_command_line, cmd_len
| sort desc _time
```

---

#### Layer 3：行为关联层 — 攻击链还原

##### 查询 16：完整攻击链关联（非工作时间 + 多事件关联）

**Splunk SPL**
```splunk
index=sysmon (EventCode=1 OR EventCode=11 OR EventCode=12 OR EventCode=13 OR EventCode=22)
| eval hour = strftime(_time, "%H")
| eval is_off_hours = if(hour < 8 OR hour > 20, 1, 0)
| where is_off_hours = 1
| transaction Computer maxspan=30m startswith=(EventCode=1) endswith=(EventCode=22)
| where eventcount > 3
| table _time, Computer, User, Image, CommandLine, TargetObject, QueryName
```

**QRadar AQL**
```sql
SELECT
  hostname,
  username,
  processname,
  parentprocessname,
  commandline,
  COUNT(*) AS event_count,
  MIN(starttime) AS chain_start,
  MAX(starttime) AS chain_end
FROM events
WHERE
  HOUR(starttime) NOT BETWEEN 8 AND 20
  AND (
    (LOGSOURCETYPENAME(devicetype) LIKE '%Sysmon%' AND eventid IN (1, 11, 12, 13, 22))
    OR (LOGSOURCETYPENAME(devicetype) LIKE '%Windows%' AND eventid IN (4688, 4689))
  )
GROUP BY hostname, username, processname, parentprocessname, commandline
HAVING COUNT(*) > 3
ORDER BY event_count DESC
```

**Cortex XDR XQL**
```xql
dataset = xdr_data
| filter event_type in ("process", "file", "registry", "DNS")
| alter hour = hour_of_day(_time)
| filter hour < 8 or hour > 20
| comp
    count(*) as event_count,
    values(event_type) as event_types,
    min(_time) as chain_start,
    max(_time) as chain_end,
    values(action_process_image_name) as processes,
    values(dns_query_name) as dns_queries
  by agent_hostname, actor_effective_username
| filter event_count > 5
| alter chain_duration_sec = to_epoch(chain_end) - to_epoch(chain_start)
| sort desc event_count
```

---

### 2.5 狩猎检查清单

```
□ H1: IM工具是否传输并执行了伪装成文档的.exe文件？
□ H2: C:\Program Files\Internet Explorer\ 下是否存在异常DLL（log.dll等）？
□ H3: explorer.exe/svchost.exe 是否有出站连接至端口8880？
□ H4: 是否存在伪装为"Adobe Updater"/"Windows Update"的高频计划任务？
□ H5: 终端是否静默安装了AnyDesk/TeamViewer？
□ H6: PowerShell是否存在内存加载.NET CLR的异常行为？
□ H7: 是否存在高熵.text节且导入函数极少的DLL？
□ 已知 C2 IP/域名通信是否命中？
□ 是否存在 DNS 隧道特征（长子域名、高频、Base64 编码）？
□ 是否存在 WhatsAppBackup/1.0 等异常 User-Agent？
□ 是否有 ClientSetup.exe 命名模式的 RMM 工具下载？
□ 注册表 Run 键是否有可疑条目？
□ C:\Windows\ 下是否有近期创建的 BAT 文件？
□ 聊天工具进程是否有异常行为（自动发消息/建群）？
□ 感染主机用户是否在非工作时间段有异常活动？
```

---

### 2.6 爆炸半径评估

| 环境 | 暴露面 | 检查状态 | 备注 |
|---|---|---|---|
| 终端（办公PC） | 🔴 高 | ☐ 待检查 | 银狐首要攻击目标，重点排查财务/人事/税务岗位 |
| 终端（开发机） | 🟡 中 | ☐ 待检查 | 可能通过群聊文件波及 |
| 邮件网关 | 🟡 中 | ☐ 待检查 | 钓鱼邮件投递通道 |
| IM工具（微信/企微） | 🔴 高 | ☐ 待检查 | 90%+攻击通道，需审计文件传输记录 |
| 内网服务器 | 🟡 中 | ☐ 待检查 | 横向移动可能波及 |
| Active Directory | 🟡 中 | ☐ 待检查 | 凭据窃取后可能横向扩散 |
| 云环境 | 🟢 低 | ☐ 待检查 | 除非终端有云凭据 |

---

## 3. The Highlight — 攻击者最精妙之处

### 3.1 最聪明的战术

#### ① 白加黑的信任套娃

银狐将恶意DLL放在 `C:\Program Files\Internet Explorer\` 这个 Windows 自带、杀软天然信任的目录下，然后用合法的 `installer.exe`（带数字签名的白文件）去加载。安全产品看到的是"IE目录下的合法程序加载了一个DLL"——这在Windows系统中再正常不过了。**攻击者没有写任何恶意PE文件，他们只是把恶意代码放到了一个"不该被怀疑"的地方。**

#### ② 合法工具 + 配置错误的投递方式（Wave 2）

Silver Fox 在第二波攻击中做出了一个极其精妙的设计：**利用合法 RMM 工具的配置错误实现零恶意代码投递**。

1. 选择一个有数字签名（SyncFutureTec Company Limited）的合法中国 RMM 工具
2. 不修改二进制文件的任何一个字节，不破坏签名
3. **通过文件名传递 C2 参数**：将 C2 IP 地址嵌入文件名，格式为 `[ipv4]ClientSetup.exe`
4. RMM 工具在执行时自动解析文件名中的 IP 作为服务器地址

```
示例：45.119.55.66ClientSetup.exe
      └─┬──┘
        └→ 这个 IP 就是 C2 地址
```

- ✅ 二进制有合法数字签名 → 绕过签名验证
- ✅ 文件本身不是恶意软件 → 绕过杀毒引擎
- ✅ 不改变文件内容 → 哈希签名检测失效
- ✅ C2 参数在文件名而非代码中 → 静态分析无法发现

#### ③ 合法远控的武器化

AnyDesk 是一个被IT运维人员广泛使用、拥有合法数字签名的远程控制软件。银狐静默安装它并设置无人值守访问后，安全团队看到的是"一个合法远控软件在运行"——这不是恶意软件，这是一个被恶意使用的合法工具。**这让基于签名和黑白名单的安全体系彻底失效。**

#### ④ 社会工程学的精准打击

银狐的诱饵不是"恭喜你中奖了"这种低级骗术，而是精确复刻的公文格式——"2026年第二季度违纪名单""裁员补偿方案""税务稽查通知"。**它攻击的不是系统漏洞，而是人在面对权威信息（违纪、裁员、稽查）时的恐慌本能。** 高阶操纵者甚至会根据企业组织架构跳过安全部门，只针对财务和人事人员。

#### ⑤ 产业化分工的效率

从木马开发、免杀更新（按周迭代）、诱饵制作、批量投递到诈骗变现，每个环节都有专业团队。攻击者只需花几百元购买授权就能发起大规模攻击。**这不是技术对抗，这是商业模式对抗。**

### 3.2 最棘手的数据分析挑战

**① 无文件攻击的取证困难**
- Shellcode全程在内存中执行，不写入磁盘，传统基于文件的检测完全失效
- 进程注入到 `explorer.exe` / `svchost.exe` 后，从进程列表看完全合法
- 计划任务的命名模仿系统更新，触发频率高但每次执行时间极短
- C2通信采用AES-256全加密，流量分析无法获取明文内容

**② 攻击链碎片化**
- 需要关联 Sysmon 进程日志、DNS 日志、网络流量、Windows 事件日志、浏览器历史等至少 5 个数据源
- 单一日志源无法还原完整攻击路径

**③ "双重动机"带来的归因困难**
- 同一组织同时进行机会主义网络犯罪和定向 APT 攻击
- 从受害者视角难以判断自己是被"随机撒网"还是"定向打击"
- 从防御者视角难以判断应该投入多大的响应力度

**④ 变种迭代速度**
- 免杀按周迭代，2025年9月单月新增400+变种
- 基于签名的检测在这种速度下几乎无效，必须依赖行为分析

### 3.3 破解关键

**`C:\Program Files\Internet Explorer\` 目录下出现非系统自带的DLL文件（如 `log.dll`）**——这个目录正常情况下不应该有第三方DLL。**一个不该存在的文件，出现在一个不该出现的地方，这就是银狐的"阿喀琉斯之踵"。**

---

## 4. The Recommendations — 战略加固建议

### 4.1 立即执行（0–72 小时）

| 优先级 | 行动 | 负责人 | 状态 |
|--------|------|--------|------|
| 🔴 P0 | 全网排查 `C:\Program Files\Internet Explorer\` 目录下是否存在异常DLL（log.dll等） | SOC | ☐ |
| 🔴 P0 | 在防火墙/代理上封禁已知 C2 IP 和域名，阻断端口 8880 出站 | 网络安全 | ☐ |
| 🔴 P0 | 全网排查可疑计划任务（"Adobe Updater"/"Windows Update"等高频任务） | SOC | ☐ |
| 🔴 P0 | 检查终端是否静默安装 AnyDesk/TeamViewer，如有立即隔离 | IT运维 | ☐ |
| 🔴 P0 | 全网扫描爆破半径（执行 Layer 1–3 全部狩猎查询） | SOC | ☐ |
| 🟡 P1 | 排查财务/人事/税务岗位人员终端，优先级最高 | SOC | ☐ |
| 🟡 P1 | 通知全员警惕"违纪名单""裁员补偿"等主题文件 | 全员 | ☐ |
| 🟡 P1 | 部署 RMM 工具白名单，审计所有 `*ClientSetup.exe` 文件 | IT运维 | ☐ |

### 4.2 短期加固（1–4 周）

- [ ] **部署检测规则**：将第 2.4 节的 Splunk/QRadar/XDR 检测查询部署到 SIEM/EDR，开启告警
- [ ] **启用 Sysmon**：全网部署 Sysmon，开启进程创建(Event 1)、网络连接(Event 3)、DLL加载(Event 7)、文件创建(Event 11)、注册表(Event 12-13)、WMI(Event 19-21)日志
- [ ] **PowerShell 日志增强**：开启脚本块日志（Event 4103/4104）
- [ ] **IM 安全网关**：部署企业微信/钉钉文件传输审计，拦截 .exe/.scr/.bat 等可执行文件
- [ ] **EDR 行为检测**：开启计划任务创建监控、注册表 Run 键修改监控、进程注入检测、合法远控软件安装告警
- [ ] **网络层加固**：部署 DNS 安全网关（DNS 隧道检测），对财务/人事等部门实施网络微分段
- [ ] **邮件安全**：邮件网关启用仿冒域名识别，对 PDF 附件启用沙箱检测
- [ ] **全员安全意识培训**：专项讲解银狐木马识别方法，进行模拟钓鱼演练
- [ ] **财务流程加固**：所有转账操作必须多人审批+电话/当面核实，禁止仅通过 IM 下达转账指令

### 4.3 中期建设（1–3 个月）

**SIEM 规则体系**：
- 将本文全部 Splunk/QRadar/XDR 查询部署为告警规则
- 建立 Silver Fox 专属检测仪表板
- 配置自动封禁联动（SIEM → 防火墙 API）

**威胁情报集成**：
- 订阅 ThreatBook、微步在线等银狐专项情报
- 建立 IOC 自动更新管道（TAXII/STIX）
- 定期更新 YARA 规则、Suricata 规则

**应用控制**：
- 启用应用程序白名单（AppLocker / WDAC），禁止非授权 .exe 执行
- 建立内部软件仓库，统一分发常用办公软件

### 4.4 长期战略（3–6 个月）

**行为基线建模**：
```
正常行为基线：
  - 工作时间段（08:00–20:00）的进程活动
  - 合法软件更新流量模式
  - 聊天工具正常使用模式

告警阈值：
  - 非工作时间段的异常进程链 → 高危
  - 注册表 Run 键写入 + 网络外联（30min 内） → 高危
  - DNS 查询中 Base64 子域名占比 > 5% → 中危
  - 新建 WMI 事件订阅 → 中危（需人工确认）
  - C:\Program Files\Internet Explorer\ 下新增 DLL → 高危
  - 端口 8880 出站连接 → 高危
```

**自动化响应剧本（SOAR）**：
- 检测到 Silver Fox IOC → 自动隔离主机 + 封禁 IP + 通知 SOC
- 检测到可疑进程注入 → 自动采集内存快照 + 终止进程
- 检测到聊天工具异常 → 自动强制下线 + 通知用户

**红蓝对抗验证**：
- 每季度使用 Silver Fox TTP 进行模拟攻击
- 验证检测规则覆盖率和响应时效
- 根据演练结果迭代优化规则

### 4.5 持续运营

| 维度 | 频率 | 内容 |
|------|------|------|
| IOC 更新 | 每周 | 同步最新 C2 IP/域名/哈希 |
| 规则验证 | 每月 | 用最新样本验证检测规则命中率 |
| 狩猎任务 | 每两周 | 执行一次全量狩猎查询 |
| 情报复盘 | 每月 | 跟踪 Silver Fox 最新 TTP 变化 |
| 应急演练 | 每季度 | 模拟银狐入侵场景，验证响应流程 |

---

## 附录 A：安全设备检测规则

### A.1 网络层检测（Suricata/Snort）

```
# 检测已知 C2 域名通信
alert dns $HOME_NET any -> any any (msg:"SILVERFOX C2 Domain Query - valleyrat"; dns.query; content:"360sdgg.com"; nocase; sid:8000001; rev:1; classtype:trojan-activity;)
alert dns $HOME_NET any -> any any (msg:"SILVERFOX C2 Domain Query - googlevip"; dns.query; content:"googlevip.icu"; nocase; sid:8000002; rev:1; classtype:trojan-activity;)

# 检测 DNS 隧道特征
alert dns $HOME_NET any -> any any (msg:"SILVERFOX DNS Tunnel Suspect - Long Subdomain"; dns.query; pcre:"/^[A-Za-z0-9+\/=]{30,63}\..+\..+$/"; sid:8000003; rev:1; classtype:trojan-activity;)

# 检测 WhatsApp 伪装 C2 通信
alert http $HOME_NET any -> any any (msg:"SILVERFOX Fake WhatsApp C2 Beacon"; http.user_agent; content:"WhatsAppBackup/1.0"; sid:8000004; rev:1; classtype:trojan-activity;)

# 检测 RMM 工具异常命名模式
alert http $HOME_NET any -> any any (msg:"SILVERFOX RMM Tool Download Pattern"; http.uri; pcre:"/\d{1,3}\.\d{1,3}\.\d{1,3}\.\d{1,3}ClientSetup\.exe/i"; sid:8000005; rev:1; classtype:trojan-activity;)

# 检测 C2 端口 8880 通信
alert tcp $HOME_NET any -> any 8880 (msg:"SILVERFOX C2 Port 8880 Communication"; flow:established,to_server; sid:8000007; rev:1; classtype:trojan-activity;)

# 检测向已知恶意 IP 的外联
alert ip $HOME_NET any -> [115.187.17.212,112.121.183.102,156.251.18.45,206.238.178.116,45.119.55.66,69.30.250.99,103.228.12.151,154.91.84.3,93.127.142.77,150.109.79.82,170.205.54.88,222.186.190.138,154.201.87.124,154.201.87.75] any (msg:"SILVERFOX Known C2 IP Communication"; sid:8000006; rev:1; classtype:trojan-activity;)
```

### A.2 终端检测（YARA）

```
rule SilverFox_ValleyRAT {
    meta:
        description = "Detects Silver Fox ValleyRAT loader"
        author = "ThreatIntel"
        date = "2026-06"
    strings:
        $s1 = "WhatsAppBackup/1.0" ascii
        $s2 = "whatsapp_backup.lock" ascii
        $s3 = "WhatsAppData.zip" ascii
        $m1 = { 48 89 5C 24 08 48 89 6C 24 10 48 89 74 24 18 57 48 83 EC 20 48 8B FA 48 8B F1 }
        $pdb = /SilverFox|ValleyRAT|silverfox/ ascii nocase
    condition:
        uint16(0) == 0x5A4D and (2 of ($s*) or ($pdb and $m1))
}

rule SilverFox_RMM_Loader {
    meta:
        description = "Detects Silver Fox RMM tool delivery pattern"
    strings:
        $sig1 = "SyncFutureTec Company Limited" ascii
        $name1 = /[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}ClientSetup\.exe/ ascii
    condition:
        uint16(0) == 0x5A4D and ($sig1 or $name1)
}

rule SilverFox_Python_Stealer {
    meta:
        description = "Detects Silver Fox Python-based stealer"
    strings:
        $ua = "WhatsAppBackup/1.0" ascii
        $path1 = "WhatsAppBackup" ascii
        $path2 = "WhatsAppData.zip" ascii
        $c2_upload = "upload_large.php" ascii
        $c2_status = "upload_status.php" ascii
    condition:
        3 of them
}

rule SilverFox_Chrome_Variant {
    meta:
        description = "Detects Silver Fox Chrome variant with AMSI bypass"
    strings:
        $pdb = "XiaobaoService.pdb" ascii
        $proc = "ABoxHeadless.exe" ascii
        $dll = "qimei.dll" ascii
        $ps1 = "Add-MpPreference -ExclusionPath" ascii
    condition:
        uint16(0) == 0x5A4D and (any of ($pdb, $proc, $dll) or $ps1)
}
```

### A.3 Sysmon 配置

```xml
<Sysmon schemaversion="4.90">
  <EventFiltering>
    <ProcessCreate onmatch="include">
      <ParentImage condition="is">C:\Windows\explorer.exe</ParentImage>
      <Image condition="contains">powershell.exe</Image>
    </ProcessCreate>
    <WmiEvent onmatch="include">
      <Operation condition="is">Created</Operation>
    </WmiEvent>
    <RegistryEvent onmatch="include">
      <TargetObject condition="contains">CurrentVersion\Run</TargetObject>
    </RegistryEvent>
    <FileCreate onmatch="include">
      <TargetFilename condition="contains">C:\Program Files\Internet Explorer\</TargetFilename>
    </FileCreate>
    <ImageLoad onmatch="include">
      <ImageLoaded condition="contains">C:\Program Files\Internet Explorer\</ImageLoaded>
    </ImageLoad>
  </EventFiltering>
</Sysmon>
```

### A.4 QRadar 参考集与 Offense 规则

```
# Reference Set: SilverFox_C2_IPs（每周更新）
Reference Set Name: SilverFox_C2_IPs
  Type: IP
  Elements: 115.187.17.212, 112.121.183.102, 156.251.18.45, 206.238.178.116,
    45.119.55.66, 69.30.250.99, 103.228.12.151, 154.91.84.3, 154.201.87.124, 154.201.87.75
  TTL: 90 days

# Reference Set: SilverFox_C2_Domains（每周更新）
Reference Set Name: SilverFox_C2_Domains
  Type: ALNIC
  Elements: 360sdgg.com, googlevip.icu, oytdwzz.shop, cocdex.cn, xqwmwru.top, gov.incometax.click
  TTL: 90 days

# Reference Set: SilverFox_Malware_Hash（每月更新）
Reference Set Name: SilverFox_Malware_Hash
  Type: ALNIC
  Elements: (SHA256 hashes — 见附录 B.4)
  TTL: 180 days

# Offense Rules
Rule 1: Silver Fox - Known C2 IP Communication
  WHEN Flow Destination IP is in [SilverFox_C2_IPs]
  THEN Create Offense severity 10, Block IP

Rule 2: Silver Fox - C2 Port 8880
  WHEN Destination Port = 8880 AND Flow Count > 3
  THEN Create Offense severity 9

Rule 3: Silver Fox - DNS Tunnel Detection
  WHEN DNS Query matches regex "^[A-Za-z0-9+/=]{30,63}\..+" AND Count > 5/hour
  THEN Create Offense severity 8

Rule 4: Silver Fox - RMM Tool IP Filename
  WHEN URL matches regex "\d{1,3}\.\d{1,3}\.\d{1,3}\.\d{1,3}ClientSetup\.exe"
  THEN Create Offense severity 9, Block URL

Rule 5: Silver Fox - Fake WhatsApp User-Agent
  WHEN HTTP User-Agent contains "WhatsAppBackup"
  THEN Create Offense severity 8

Rule 6: Silver Fox - Process Injection Chain (Off-Hours)
  WHEN Parent Process in [explorer.exe, svchost.exe, winlogon.exe]
    AND Child Process in [powershell.exe, cmd.exe, rundll32.exe]
    AND Time between 20:00-08:00
  THEN Create Offense severity 9

Rule 7: Silver Fox - WMI Persistence
  WHEN Sysmon Event ID in [19, 20, 21]
  THEN Create Offense severity 7

Rule 8: Silver Fox - Suspicious BAT in Windows Dir
  WHEN Sysmon Event ID = 11 AND Target matches "C:\Windows\*.bat"
  THEN Create Offense severity 8

Rule 9: Silver Fox - AnyDesk Silent Install
  WHEN Process = "AnyDesk.exe" AND CommandLine contains "--install"
  THEN Create Offense severity 8

Rule 10: Silver Fox - Chat Tool Anomaly (Off-Hours)
  WHEN Process matches "(WeChat|WXWork|DingTalk).*\.exe"
    AND (CommandLine length > 200 OR contains ".zip"/.rar"/.exe")
    AND Time between 20:00-08:00
  THEN Create Offense severity 9
```

### A.5 Cortex XDR IOC 规则

```
Rule 1: Silver Fox - Known C2 IP Communication
  Condition: action_external_ip in silverfox_c2_ips AND event_type in (NETWORK, DNS, HTTP)
  Action: isolate_endpoint, notify SOC

Rule 2: Silver Fox - C2 Port 8880
  Condition: dst_action_external_port = 8880 AND conn_count > 3
  Action: block_ip, notify SOC

Rule 3: Silver Fox - DNS Tunnel Base64 Subdomain
  Condition: event_type = DNS AND dns_query_name matches "^[A-Za-z0-9+/=]{30,63}\..+"
  Action: alert, notify SOC

Rule 4: Silver Fox - RMM Tool IP Filename Pattern
  Condition: action_file_name matches "\d{1,3}\.\d{1,3}\.\d{1,3}\.\d{1,3}ClientSetup\.exe"
  Action: block_process, isolate_endpoint

Rule 5: Silver Fox - Fake WhatsApp User-Agent
  Condition: http_request_user_agent contains "WhatsAppBackup"
  Action: block_ip, notify SOC

Rule 6: Silver Fox - Process Injection Chain (Off-Hours)
  Condition: actor_process_image_name in (explorer.exe, svchost.exe, winlogon.exe)
    AND action_process_image_name in (powershell.exe, cmd.exe, rundll32.exe)
    AND hour < 8 OR hour > 20
  Action: isolate_endpoint, notify SOC

Rule 7: Silver Fox - WMI Event Subscription Persistence
  Condition: event_type = WMI AND action_wmi_operation in (WmiEventFilter, WmiEventConsumer)
  Action: alert, notify SOC

Rule 8: Silver Fox - AnyDesk Silent Install
  Condition: action_process_image_name = "AnyDesk.exe" AND command_line contains "--install"
  Action: block_process, notify SOC

Rule 9: Silver Fox - IE Directory Abnormal DLL
  Condition: event_type = load_image AND action_module_path contains "C:\Program Files\Internet Explorer\"
    AND NOT action_module_path matches "(ieproxy|iertutil)\.dll"
  Action: alert, isolate_endpoint

Rule 10: Silver Fox - Chat Tool Anomaly (Off-Hours)
  Condition: action_process_image_name matches "(WeChat|WXWork|DingTalk).*\.exe"
    AND (command_line length > 200 OR command_line contains ".zip"/.rar"/.exe")
    AND (hour < 8 OR hour > 20)
  Action: alert, notify SOC
```

### A.6 防火墙策略

```
# 封禁已知 C2 IP 段
154.201.87.0/24    # 钓鱼网站 + Python 窃取器 C2

# 封禁端口 8880 出站
deny tcp any any eq 8880

# 封禁已知钓鱼域名
googlevip.icu, oytdwzz.shop, cocdex.cn, fzdoor.incometax.click,
360sdgg.com, xqwmwru.top
```

---

## 附录 B：完整 IOC 列表

### B.1 C2 IP（RMM 相关 + 已知基础设施）

```
115.187.17.212    112.121.183.102   156.251.18.45     206.238.178.116
45.119.55.66      69.30.250.99      103.228.12.151    154.91.84.3
93.127.142.77     150.109.79.82     170.205.54.88     222.186.190.138
103.203.48.174    130.250.191.46    220.167.103.145   220.167.103.160
154.12.87.28      116.213.43.23     103.231.12.23     103.70.76.130
154.44.28.175     156.251.18.238    112.121.183.106   45.119.55.112
115.187.17.68     103.231.12.45     103.97.128.142    156.254.5.118
47.85.99.19       216.250.104.166   103.97.128.103    45.194.37.147
103.97.128.109    112.213.120.164   154.201.87.124    154.201.87.75
```

### B.2 C2 域名

```
360sdgg.com           googlevip.icu         oytdwzz.shop
cocdex.cn             xqwmwru.top           gov.incometax.click
```

### B.3 钓鱼域名

```
ksdfuefagfrukayhfka.eu.cc    rdhrse.qpon                 googlehfgj.cyou
fhauifhyileydhfl.com         megamovielord.com           peyvz.com
fzdoor.vip                    amvcoins.vip                sdyteq.shop
ksudeu.nanguanglu.com        domainct.com                host-hunter.com
domainca.top                  fghs.shlowcarbon.com        jinmai.vip
opkllasyy.shop                fdfhddfss.top               primetechstocks.com
juanseguros.com               mohaazon.com                fkfjrvfa.cn
zibenbang.vip                 zptsgryw.cn                 betooo.vip
czxfdz.com                    sgeshex.vip                 eaxwwyr.cn
wwfygid.biz.id                sgegdvip.vip                morecoworking.com
yigushengjin.com              gofjasj.help                xueshirencai.com
wgooglegoogle.com             gfmqvip.vip
```

### B.4 诱饵样本哈希（MD5）

```
18685800b70e3e30f54d1a5fa1c4b080    e28fee7fccbab8e743d953383a52c8e4
245aee2442aab596271add1f62099ec0    6d51a96545f8a3e15308ef8683f64077
cb9e0290c31a6cbc2c04eaa010c49c62    8f785cb7955fb2d7b0914fc8e1e85e1a
365d79c4976c4266e8a928a291ce70bd    803fe35653cb4018b56d27e5e130c2d2
44b2de1f5d9c40947ecf0b5bc6ade7c5    02ff385debe958e1125fae9191579b87
52f76f79bf8bd301bad98d73e52c9bca    dc76c6ff76f78a66e7b242b3a02836c6
04386776404ca093758217563c1777b3    6caeaf0e58bb53ca6493ec177090fe37
```

### B.5 RMM 工具哈希（SHA256，部分）

```
055c3fff8f1f58a41e7571b9bd7ebf4b1b10ba5231f1ffbcb47e0307d7ff6072
06ecf34ecf1f3f56a1760b8757b978d6bd859adcf699af4adfbeb0982e41282a
18cb036bcc7aacf7393575ddf15133e24d3a22cc92a4b14e8595686e4bf80629
249d2d1d6cfcf34d48ac0465ede688759a3c90b7412723373ea5a434d6d64c9c
2a4eab726a878a74dcad41d090681a7fa78d9247b1812e5c3066d7a1aa0413b1
316cbc90ad71a421e571b529af2dee40f901b15b4bc549836c25f1be35597249
36e0368dd4c3c9c70a78050618797705cda87a017e41777968c6b4b9173f553f
3f8e2ef8a5e7b8f8d14e43032ad2b18f0a4fb168609494fd346dcdfe1127a5cd
616be8ba3383909b2b04c87bcb9ca0707f5a19a8eaa6fc1e552181baa4e3e0aa
75bf89f0369b6eef1e2931e6da67a9d4f3095b9a623e6e8fdddf7fee66cc7cc0
80f7f10bcddafaec497a2de78dd3d2a53b72f27bb72e7939443539115f7e2168
8c54e6d91d95885beae125b30ab9096bd341e12be08dec3aeb859e539dc77d47
98be97a6f4663d04cf5382f4ed046b479af1dd300d0ab3fa7a399ab15078d7a0
a6fd51bf2da2c2544ff78ef1824c30d4feef9a77c824f36d9afd2c6093c9b6ae
a8d193e49e6c9c6d7c32ea807d22311bd1b110f2326b8a96c67978ecc6862ee6
a8edb8fb1cf83031a454b5f39ffab0b1d93448cb3b9794246507e35ba0036801
ae243178e201c6ee475e4498cade0d21ef22b8a6923322576115b0888e189013
aed5ce23aa11f28e063c8b1b0836d3dbd059d93867e8e828a8356770ee185d1b
d49bd211364594c671c4e34a31afb75becc69b32b45b140ed0d200f4b05868c6
d91ea2ec158e871408229ec2f7a8fe78a8d30ed0db42f73fe9e31875b30b17c2
eb4a53145734d1ef612897337b1fc3375209598c427590731bb87de3bd8f9bb0
fc43d1640d94ef621c82a4d3a0406df3443b39043c4ddef0a23608c186c307e8
```

### B.6 攻击者基础设施

```
Python 窃取器 C2：xqwmwru.top (154.201.87.124)
钓鱼网站托管：154.201.87.75
钓鱼存储桶：myqcloud 基础设施
ValleyRAT C2：9010.360sdgg.com
C2 回联端口：8880
C2 回联URL模式：http://[域名]:8880/getinstall64
```

---

## 附录 C：已披露企业中招案例汇总

### C.1 整体态势

| 指标 | 数据 | 来源 |
|---|---|---|
| 受影响企业总数 | **1.2万+** | 国家计算机病毒应急处理中心，2026-05-21 |
| 攻击事件同比增长 | **127%** | 同上 |
| 单笔最高诈骗金额 | **500万元** | 同上 |
| 2025年造成企业损失 | **超20亿元** | 公安部网安局，2025-11 |
| 中小企业受害占比 | **73%** | 360银狐木马年度报告，2026-01 |

### C.2 公安部公布的5起典型案件（2024-07-30）

| # | 时间 | 地点 | 受害方 | 损失金额 | 手法摘要 | 结果 |
|---|---|---|---|---|---|---|
| 1 | 2023.09 | 浙江台州 | 科技公司财会人员 | **1000万元+** | 冒充老板拉微信群，指令转账 | 抓获41人，挽回600万+ |
| 2 | 2023.10 | 福建武夷山 | 公司财务 | **1480万元+** | 远程控制，删除真老板微信，添加假老板 | 抓获51人，挽回1200万+ |
| 3 | 2024.05 | 安徽阜阳 | 公司财务 | **100万元** | 工作群下载不明文件 → 木马 → 伪装领导 | 挽回85万；抓获6人 |
| 4 | 2024.03 | 山东烟台 | 公司工作人员 | **险失100万** | 安装激活软件 → 木马 → 自动发消息 | 民警拦截成功 |
| 5 | 2024.05 | 上海松江 | 财务总监 | **险失498.6万** | 木马控制 → 伪装老板要求支付货款 | 财务电话确认，成功识破 |

### C.3 跨境税务巡游攻击

| 时间 | 目标地区 | 伪装机构 | 载荷 | 状态 |
|---|---|---|---|---|
| 2025.01 | 🇹🇼 台湾 | 台湾税务部门 | ValleyRAT | ✅ 已确认 |
| 2025.10 | 🇭🇰 香港 | 香港税务局（IRD） | ValleyRAT | ✅ 已确认 |
| 2025.10 | 🇸🇬 新加坡 | 新加坡税务局（IRAS） | ValleyRAT | ✅ 已确认 |
| 2025.11–12 | 🇮🇳 印度 | 印度所得税部门 | ValleyRAT + ABCDoor | ✅ 1600+恶意邮件 |
| 2026.01 | 🇹🇼 台湾 | 台湾税务部门 | ValleyRAT | ✅ 已确认 |
| 2026.01 | 🇯🇵 日本 | 日本税务部门 | ValleyRAT + ABCDoor | ✅ 已确认 |

---

## 附录 D：参考文献

1. [国家计算机病毒应急处理中心 — 银狐系列木马预警报告（2026-05-21）](https://www.cverc.org.cn/head/zhaiyao/news20260521-yhyj.htm)
2. [SilverFox家族木马远控全解析 — CSDN（2026-06-01）](https://blog.csdn.net/T_RAFFORD/article/details/161583350)
3. [深度拆解银狐木马：七阶段攻击链与防御盲区分析 — FreeBuf（2026-03-28）](https://www.freebuf.com/articles/endpoint/475315.html)
4. [从Silver Fox新变种看2026年网络钓鱼的攻防进化 — CSDN（2026-05-21）](https://blog.csdn.net/fireroothacker/article/details/161219110)
5. [银狐木马Chrome变种分析 — FreeBuf（2026-05-14）](https://www.freebuf.com/articles/481194.html)
6. [银狐（SilverFox）黑产组织详细分析报告 — ZONE.CI（2026-04-30）](https://security.zone.ci/secarticles/wx/527189.html)
7. [双重间谍活动的战术演进：Silver Fox组织攻击模式深度分析 — 腾讯云（2026-03-26）](https://cloud.tencent.com/developer/article/2646665)
8. [揭秘银狐组织利用SEO投毒分发ValleyRAT的攻击活动 — FreeBuf（2025-12-21）](https://www.freebuf.com/articles/web/463288.html)
9. [国家互联网应急中心发布关于"游蛇"黑产攻击活动风险提示（2025-05-23）](https://www.toutiao.com/article/7507571918468317708/)
10. [银狐木马技术原理分析与检测技术 — CSDN（2025-11-05）](https://blog.csdn.net/bdfcfff77fa/article/details/154440643)
11. [Silver Fox 银狐的攻击组织在2025-2026攻击趋势及关键IOC — CN-SEC（2026-03-27）](https://cn-sec.com/archives/5132986.html)
12. [银狐木马分析：从攻击行为到完整应急处置流程 — CN-SEC（2026-03-05）](https://cn-sec.com/archives/5046644.html)
13. [SilverFox组织仿印度税务部门钓鱼攻击机理与防御研究 — 百家号（2026-05-12）](https://baijiahao.baidu.com/s?id=1864943954776601667)
14. [PwC DarkLab — 香港/新加坡税务钓鱼（2026-03-25）](https://blog.darklab.hk/2026/03/25/silver-foxs-dual-pronged-strategy-dissecting-the-valleyrat-distribution-campaign/)
15. [360安全云 — 能源企业案例（2026-05-25）](https://www.jiemian.com/article/14476781.html)
16. [MITRE ATT&CK — Silver Fox Techniques](https://attack.mitre.org/)
17. [国家计算机病毒协同分析平台](https://virus.cverc.org.cn)
18. ["银狐"情报共享站](https://s.threatbook.com/cybercrime/silverfox)

---

> **免责声明**：本报告仅用于安全研究与防御目的。报告中涉及的 IOC、检测规则和攻击手法分析均来源于公开威胁情报，旨在帮助安全团队构建检测能力。请勿将本报告内容用于任何非法用途。
