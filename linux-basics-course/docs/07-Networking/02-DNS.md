# DNS (Domain Name System) | DNS（域名系统）

- Take me to the [Tutorial](https://kodekloud.com/topic/dns/)
- 前往 [视频教程](https://kodekloud.com/topic/dns/)

---

## What is DNS? | 什么是 DNS？

The **Domain Name System (DNS)** is a distributed, hierarchical system for translating human-readable hostnames (like `www.google.com`) into machine-readable IP addresses (like `142.250.185.68`). Instead of requiring every computer to maintain a complete, synchronized copy of all hostname-to-IP mappings, DNS distributes this responsibility across a global network of **name servers**. Each name server is authoritative for its own zone and publishes the IP addresses for the domains it manages. When an IP address changes, only the authoritative name server needs to be updated — all clients will eventually get the new address automatically.

**域名系统（DNS）** 是一个分布式、分层的系统，用于将人类可读的主机名（如 `www.google.com`）转换为机器可读的 IP 地址（如 `142.250.185.68`）。DNS 不需要每台计算机维护一份完整且同步的主机名到 IP 地址映射，而是将这个责任分散到全球的**名称服务器**网络中。每台名称服务器对其管辖的区域具有权威性，并发布它所管理域的 IP 地址。当一个 IP 地址发生变化时，只需要更新权威名称服务器——所有客户端最终都会自动获取新地址。

---

## Name Resolution Methods | 名称解析方法

Linux systems support multiple ways to resolve hostnames to IP addresses. They are queried in an order defined by `/etc/nsswitch.conf`.

Linux 系统支持多种将主机名解析为 IP 地址的方式，查询顺序由 `/etc/nsswitch.conf` 定义。

### 1. Local Hosts File (`/etc/hosts`) | 本地 Hosts 文件

Before DNS existed, every computer kept a local file listing all known hosts. This file still exists today and is checked **before** DNS by default, making it very useful for:
- Development and testing (override a domain locally)
- Accessing internal servers without a DNS record
- Blocking unwanted domains (map them to `127.0.0.1`)

在 DNS 出现之前，每台计算机都保存着一个列出所有已知主机的本地文件。这个文件至今仍然存在，默认情况下在 DNS 之**前**被检查，非常适用于：
- 开发和测试（在本地覆盖某个域名）
- 访问没有 DNS 记录的内部服务器
- 屏蔽不想要的域名（将其映射到 `127.0.0.1`）

**Ping by IP address | 通过 IP 地址 Ping：**

```bash
[~]$ ping 192.168.1.11
Reply from 192.168.1.11: bytes=32 time=4ms TTL=117
Reply from 192.168.1.11: bytes=32 time=4ms TTL=117
```

**Add a hostname entry to `/etc/hosts` | 向 `/etc/hosts` 添加主机名条目：**

```bash
[~]$ sudo bash -c 'echo "192.168.1.11 db" >> /etc/hosts'
```

Or using `cat` with append redirection:

或者使用 `cat` 追加重定向：

```bash
[~]$ cat >> /etc/hosts
192.168.1.11 db
```

**Now ping by hostname | 现在通过主机名 Ping：**

```bash
[~]$ ping db
PING db (192.168.1.11) 56(84) bytes of data.
64 bytes from db (192.168.1.11): icmp_seq=1 ttl=64 time=0.052 ms
64 bytes from db (192.168.1.11): icmp_seq=2 ttl=64 time=0.079 ms
```

**A larger `/etc/hosts` example | 一个更完整的 `/etc/hosts` 示例：**

```bash
[~]$ cat /etc/hosts
# Loopback entries — do not remove
127.0.0.1   localhost
127.0.1.1   my-laptop

# Internal servers — managed manually
192.168.1.10  web
192.168.1.11  db
192.168.1.12  nfs
192.168.1.20  web-2
192.168.1.21  db-1
192.168.1.22  nfs-1
192.168.1.30  web-3
192.168.1.31  db-2
192.168.1.32  nfs-2
192.168.1.40  web-4
192.168.1.41  sql
192.168.1.42  web-5
192.168.1.50  web-test
192.168.1.61  db-prod
192.168.1.52  nfs-4
192.168.1.60  web-3
192.168.1.61  db-test
192.168.1.62  nfs-prod
```

> **Note | 注意：** The `/etc/hosts` file is local only. Changes you make here won't affect other machines on the network. For a shared, centralized solution, use a DNS server.
>
> `/etc/hosts` 文件仅限本地生效。你在这里做的更改不会影响网络上的其他机器。如需共享的集中式解决方案，请使用 DNS 服务器。

---

### 2. DNS Server (`/etc/resolv.conf`) | DNS 服务器

When a hostname isn't found in `/etc/hosts`, the system queries a DNS server. The DNS server address is configured in `/etc/resolv.conf`.

当主机名在 `/etc/hosts` 中找不到时，系统会查询 DNS 服务器。DNS 服务器地址配置在 `/etc/resolv.conf` 中。

```bash
[~]$ cat /etc/resolv.conf
# DNS server provided by the network (e.g., DHCP)
nameserver 192.168.1.100

# Optional: add a search domain so you can use short names
# e.g., "ping web" will try "web.mycompany.local" automatically
search mycompany.local
```

You can specify **multiple nameservers** as fallbacks:

你可以指定**多个名称服务器**作为备用：

```bash
nameserver 192.168.1.100
nameserver 8.8.8.8
nameserver 8.8.4.4
```

---

### 3. Name Resolution Order (`/etc/nsswitch.conf`) | 名称解析顺序

The `/etc/nsswitch.conf` file controls the **order** in which different resolution methods are tried. This is critical — it determines whether your local `/etc/hosts` file takes precedence over DNS or vice versa.

`/etc/nsswitch.conf` 文件控制不同解析方法的**尝试顺序**。这非常关键——它决定了本地 `/etc/hosts` 文件是否优先于 DNS，或者反过来。

```bash
[~]$ cat /etc/nsswitch.conf
# ...
hosts:   files dns myhostname
# ...
```

This line means: first check local `files` (`/etc/hosts`), then query `dns`, then fall back to `myhostname` (the system's own hostname). To prioritize DNS over the local file, you would swap the order to `dns files`.

这行配置的意思是：首先检查本地 `files`（即 `/etc/hosts`），然后查询 `dns`，最后回退到 `myhostname`（系统自身的主机名）。若要让 DNS 优先于本地文件，可以将顺序改为 `dns files`。

---

## Domain Names & Hierarchy | 域名与层级结构

![DNS](../../images//dns.PNG)

DNS names are structured in a **hierarchical, right-to-left** fashion. A fully qualified domain name (FQDN) like `mail.google.com.` consists of:

DNS 名称以**从右到左的分层**结构组织。一个完整限定域名（FQDN）如 `mail.google.com.` 由以下部分组成：

```
mail  .  google  .  com  .
 │           │        │   └── Root (.) — the very top of the hierarchy
 │           │        └────── Top-Level Domain (TLD)
 │           └─────────────── Second-Level Domain (registered by Google)
 └─────────────────────────── Subdomain (e.g., a mail server)
```

### Common Top-Level Domains (TLDs) | 常见顶级域名

| TLD | Purpose | 用途 |
|-----|---------|------|
| `.com` | Commercial / General Purpose | 商业 / 通用 |
| `.net` | Network / General Purpose | 网络 / 通用 |
| `.edu` | Educational institutions | 教育机构 |
| `.org` | Non-profit organizations | 非营利组织 |
| `.gov` | Government entities | 政府机构 |
| `.io` | Tech companies (originally British Indian Ocean Territory) | 科技公司（原属英属印度洋领地） |
| `.cn` | China country code | 中国国家代码 |

![Root](../../images//root.PNG)

---

## DNS Record Types | DNS 记录类型

![Record](../../images//record.PNG)

DNS stores different kinds of data in different **record types**. Understanding them helps when debugging DNS issues.

DNS 以不同的**记录类型**存储不同类型的数据。了解这些类型有助于调试 DNS 问题。

| Record | Description | 说明 |
|--------|-------------|------|
| **A** | Maps a hostname to an **IPv4** address | 将主机名映射到 **IPv4** 地址 |
| **AAAA** | Maps a hostname to an **IPv6** address | 将主机名映射到 **IPv6** 地址 |
| **CNAME** | **Canonical Name** — aliases one name to another | **规范名称** — 将一个名称别名到另一个名称 |
| **MX** | **Mail Exchange** — specifies mail servers for a domain | **邮件交换** — 指定域的邮件服务器 |
| **NS** | **Name Server** — delegates a zone to a DNS server | **名称服务器** — 将区域委托给 DNS 服务器 |
| **TXT** | Arbitrary text — used for SPF, DKIM, domain verification | 任意文本 — 用于 SPF、DKIM、域名验证 |
| **PTR** | **Reverse DNS** — maps an IP address back to a hostname | **反向 DNS** — 将 IP 地址映射回主机名 |

---

## DNS Lookup Tools | DNS 查询工具

### `nslookup` — Quick Hostname Lookup | 快速主机名查询

`nslookup` is the classic tool for querying a DNS server. It sends the query **directly to the DNS server** (bypassing `/etc/hosts` and `nsswitch.conf`), making it reliable for DNS-specific debugging.

`nslookup` 是查询 DNS 服务器的经典工具。它**直接向 DNS 服务器**发送查询（绕过 `/etc/hosts` 和 `nsswitch.conf`），因此在 DNS 专项调试时非常可靠。

```bash
[~]$ nslookup www.google.com
Server:  8.8.8.8
Address: 8.8.8.8#53

Non-authoritative answer:
Name:    www.google.com
Address: 172.217.0.132
```

> **"Non-authoritative answer" | "非权威应答"** means the result came from a caching DNS server, not directly from Google's own authoritative name servers. This is normal.
>
> 表示结果来自缓存 DNS 服务器，而非直接来自 Google 自己的权威名称服务器。这是正常的。

You can also query a specific record type:

你也可以查询特定的记录类型：

```bash
# Look up MX records for a domain | 查询域的 MX 记录
[~]$ nslookup -type=MX google.com

# Query a specific DNS server | 查询特定的 DNS 服务器
[~]$ nslookup www.google.com 1.1.1.1
```

---

### `dig` — Detailed DNS Debugging | 详细 DNS 调试

`dig` (Domain Information Groper) is the preferred tool for advanced DNS troubleshooting. It provides far more detail than `nslookup`, including query time, TTL values, and full DNS response flags.

`dig`（域信息检索工具）是高级 DNS 排查的首选工具。它比 `nslookup` 提供多得多的细节，包括查询时间、TTL 值和完整的 DNS 响应标志。

```bash
[~]$ dig www.google.com

; <<>> DiG 9.10.3-P4-Ubuntu <<>> www.google.com
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 28065
;; flags: qr rd ra; QUERY: 1, ANSWER: 6, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 512

;; QUESTION SECTION:
;www.google.com.                IN A

;; ANSWER SECTION:
www.google.com.  245  IN  A  64.233.177.103
www.google.com.  245  IN  A  64.233.177.105
www.google.com.  245  IN  A  64.233.177.147
www.google.com.  245  IN  A  64.233.177.106
www.google.com.  245  IN  A  64.233.177.104
www.google.com.  245  IN  A  64.233.177.99

;; Query time: 5 msec
;; SERVER: 8.8.8.8#53(8.8.8.8)
;; WHEN: Sun Mar 24 04:34:33 UTC 2019
;; MSG SIZE rcvd: 139
```

**Key fields explained | 关键字段解析：**

| Field | Meaning | 含义 |
|-------|---------|------|
| `status: NOERROR` | Query succeeded | 查询成功 |
| `ANSWER: 6` | 6 IP addresses returned (load balancing) | 返回了 6 个 IP 地址（负载均衡） |
| `245 IN A` | TTL is 245 seconds; IPv4 record | TTL 为 245 秒；IPv4 记录 |
| `Query time: 5 msec` | DNS response took 5 milliseconds | DNS 响应耗时 5 毫秒 |

**Useful `dig` options | 常用 `dig` 选项：**

```bash
# Short output — just the answer | 简短输出——只显示答案
[~]$ dig +short www.google.com

# Query a specific record type | 查询特定记录类型
[~]$ dig MX gmail.com

# Reverse lookup — find hostname for an IP | 反向查找——根据 IP 找主机名
[~]$ dig -x 8.8.8.8

# Use a specific DNS server | 使用特定 DNS 服务器
[~]$ dig @1.1.1.1 www.google.com

# Show the full DNS resolution path (trace) | 显示完整 DNS 解析路径（追踪）
[~]$ dig +trace www.google.com
```

---

## Common DNS Errors | 常见 DNS 错误

| Error | Cause | Resolution | 原因 | 解决方法 |
|-------|-------|------------|------|---------|
| `NXDOMAIN` | Domain does not exist | Check spelling; verify DNS record exists | 域名不存在 | 检查拼写；确认 DNS 记录存在 |
| `SERVFAIL` | DNS server failed to respond | Try a different nameserver | DNS 服务器未响应 | 尝试其他名称服务器 |
| `REFUSED` | Server refused the query | Check firewall rules on port 53 | 服务器拒绝查询 | 检查端口 53 的防火墙规则 |
| `TIMEOUT` | No response from server | Check network connectivity to DNS server | 服务器无响应 | 检查到 DNS 服务器的网络连通性 |

---

## Hands-On Labs | 动手实验

- Let's have some fun with challenging [exercises](https://kodekloud.com/courses/873064/lectures/17074530)
- 来做一些有趣且有挑战性的 [练习](https://kodekloud.com/courses/873064/lectures/17074530)
