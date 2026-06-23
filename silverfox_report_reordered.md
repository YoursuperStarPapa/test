# 🦊 Silver Fox（银狐）APT 组织深度分析报告 — 整合版

> **报告生成时间**：2026-06-22
> **情报时效**：基于 2025–2026 年公开威胁情报
> **建议更新频率**：每月
> **数据来源**：ThreatBook、国家计算机病毒应急处理中心（CVERC）、微步在线、奇安信、安天、360、PwC DarkLab、Sekoia、卡巴斯基、CN-SEC、CSDN 等

---

## 第一部分：概览与影响

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

### 1.2 "双重间谍"模式（2025-2026 新趋势）

| 维度 | 战略情报线 | 机会主义犯罪线 |
|---|---|---|
| **目标** | 特定目标（如台湾机构、印度政府部门） | 广泛的金融、税务相关行业 |
| **时机** | 特定时间窗口（如税务审计期） | 全年持续攻击 |
| **目的** | 获取敏感政治或经济情报 | 窃取资金或凭证变现 |
| **工具** | 定制化ValleyRAT + 高级持久化 | Python Stealer + 合法远控 |
| **投递** | 精准鱼叉式钓鱼 | 批量IM投递 + SEO投毒 |

### 1.3 攻击目标

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

---

## 第二部分：攻击链与攻击演进

### 2.1 攻击链全貌

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

### 2.2 2025–2026 三波攻击演进

| 阶段 | 时间 | 投递方式 | 载荷类型 | 目标扩展 |
|------|------|----------|----------|----------|
| **Wave 1** | 2025.01 | 税务审计主题 PDF 附件（台湾财政部公告仿冒） | ValleyRAT（DLL 侧加载） | 台湾 → 日本 |
| **Wave 2** | 2025.12 | 钓鱼邮件 → 虚假税务网站 → ZIP/RAR 下载 | 合法 RMM 工具（SyncFutureTec 签名） | 马来西亚、菲律宾、泰国、印尼、新加坡、印度 |
| **Wave 3** | 2026.02 | 钓鱼网站（马来语）→ 下载页 | Python 窃取器伪装 WhatsApp | 马来西亚及更广泛南亚 |

---

## 第三部分：技术剖析

### 3.1 变种演化时间线（2020–2026）

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

### 3.2 恶意软件家族矩阵

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

### 3.3 免杀技术演进五代

| 时代 | 时间段 | 核心手段 | 绕过能力 |
|---|---|---|---|
| **1.0** | 2020-2021H1 | 简单加壳、文件属性伪造、宏代码混淆 | 可被主流AV静态+动态检测 |
| **2.0** | 2021H2-2022H1 | 白加黑 + DLL侧载成为标配 | 大幅绕过静态检测，利用白程序信誉 |
| **3.0** | 2022H2-2023H1 | Direct Syscall + Shellcode多层加密 | 绕过EDR用户态Hook |
| **4.0** | 2023H2-2024 | Indirect Syscall + ETW/AMSI Patch + Unhooking | APT级EDR绕过 |
| **5.0** | 2024-2026 | 全栈无文件 + Callback执行 + AI辅助免杀 | 传统安全产品检测面临极大挑战 |

### 3.4 关键技术能力详解

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

### 3.5 MITRE ATT&CK 映射

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

## 第四部分：威胁狩猎与检测

### 4.1 爆炸半径评估假设

> **核心假设**：如果在环境中发现一台 Silver Fox 感染主机，攻击者很可能已通过聊天工具完成二次传播，感染范围远超初始受害者。

### 4.2 狩猎假设（Hunt Hypotheses）

| # | Hypothesis | Confidence | Rationale |
|---|---|---|---|
| H1 | 组织内存在通过IM工具接收并执行伪装成文档的可执行文件的行为 | High | 银狐90%以上通过微信/企微/QQ投递 |
| H2 | 存在`C:\Program Files\Internet Explorer\`目录下异常DLL文件（如log.dll） | High | 本次预警明确的白利用载荷路径 |
| H3 | 系统关键进程（explorer.exe/svchost.exe）存在异常出站HTTPS连接至端口8880 | High | C2回联特征端口 |
| H4 | 存在伪装成"Adobe Updater"/"Windows Update"的高频计划任务 | Medium | 银狐偏好持久化方式 |
| H5 | 终端静默安装了AnyDesk/TeamViewer等合法远控软件 | Medium | AnyDesk联合诈骗变种特征 |
| H6 | PowerShell进程存在内存加载.NET CLR的异常行为 | Medium | 无文件攻击特征 |
| H7 | 存在高熵.text节且导入函数极少的DLL | Medium | 银狐Chrome变种特征 |

### 4.3 遥测数据源（Telemetry Sources）

- [x] **EDR 进程树 & 文件事件** — 进程父子关系、DLL加载、文件创建
- [x] **防火墙/代理日志** — 出站连接目标IP/域名/端口
- [x] **DNS 查询日志** — DGA域名识别、DNS隧道检测
- [x] **Windows 计划任务日志** — Event ID 106/140/200/201
- [x] **Windows PowerShell 日志** — Event ID 4103/4104（脚本块日志）
- [x] **Windows Sysmon** — Event ID 1/3/7/11/12-13/19-21/22
- [x] **Active Directory 认证日志** — 横向移动检测
- [x] **即时通讯工具审计日志** — 文件传输记录

### 4.4 分层检测查询

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
  COUNT(*) AS hit_count,
  MIN(starttime) AS first_seen,
  MAX(starttime) AS last_seen
FROM events
WHERE
  parentprocessname IN ('explorer.exe','wechat.exe','wxwork.exe','qq.exe')
  AND (commandline ILIKE '%名单%' OR commandline ILIKE '%违纪%' OR commandline ILIKE '%裁员%'
       OR commandline ILIKE '%补偿%' OR commandline ILIKE '%稽查%' OR commandline ILIKE '%通报%'
       OR commandline ILIKE '%调查%')
GROUP BY hostname, username, parentprocessname, processname, commandline
ORDER BY hit_count DESC
```

**Cortex XDR XQL**
```xql
dataset = xdr_data
| filter event_type = "process"
| filter actor_process_image_name in ("explorer.exe","wechat.exe","wxwork.exe","qq.exe")
| filter action_process_command_line contains "名单"
     or action_process_command_line contains "违纪"
     or action_process_command_line contains "裁员"
     or action_process_command_line contains "补偿"
     or action_process_command_line contains "稽查"
     or action_process_command_line contains "通报"
     or action_process_command_line contains "调查"
| fields _time, agent_hostname, actor_effective_username, actor_process_image_name,
         action_process_image_name, action_process_command_line, action_process_sha256
| sort desc _time
```

---

## 附录：IOC 快速参考

### 已知 C2 IP 地址

```
115.187.17.212
112.121.183.102
156.251.18.45
206.238.178.116
45.119.55.66
69.30.250.99
103.228.12.151
154.91.84.3
93.127.142.77
150.109.79.82
170.205.54.88
222.186.190.138
103.203.48.174
130.250.191.46
220.167.103.145
220.167.103.160
154.201.87.124
154.201.87.75
```

### 已知 C2 域名

```
360sdgg.com
googlevip.icu
oytdwzz.shop
cocdex.cn
xqwmwru.top
gov.incometax.click
```

### 特征端口

```
8880（C2 回联）
```
