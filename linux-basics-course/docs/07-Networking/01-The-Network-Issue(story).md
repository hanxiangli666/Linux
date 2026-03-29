# The Network Issue | 网络故障

> Bob encounters a network issue while trying to access a company repository, leading to a troubleshooting session with a network team member.
>
> Bob 在尝试访问公司代码仓库时遇到了网络问题，随后与网络团队成员展开了一次排查会话。

---

*Dated: July 8th, 1 p.m. — Six days before the demo*

*时间：7月8日下午1点 —— 演示前六天*

---

Bob is ecstatic—after finally getting his sample project to run on his laptop, he decides to explore the company's internal software repository. Remembering that Dave mentioned the URL is accessible from the office network, Bob eagerly opens his web browser and types:

Bob 兴奋不已——在终于让他的示例项目在笔记本上跑起来之后，他决定探索一下公司的内部软件仓库。想起 Dave 曾提到这个 URL 可以在公司网络中访问，Bob 迫不及待地打开浏览器，输入：

```text
http://caleston-repos
```

Instead of the expected page, Bob is confronted with an error message. The browser displays a stark **"DNS_PROBE_FINISHED_NXDOMAIN"** error — the site simply cannot be found.

然而，预期的页面没有出现，取而代之的是一条错误信息。浏览器显示了一个刺眼的 **"DNS_PROBE_FINISHED_NXDOMAIN"** 错误——该站点根本无法被找到。

Unsure of the cause, he reaches out to Dave via communicator, only to remember that Dave is currently on vacation. Suspecting a network issue, Bob promptly opens a ticket with the network team via ServiceNow.

不确定原因所在，他尝试通过即时通讯联系 Dave，这才想起 Dave 正在休假。怀疑是网络问题，Bob 立即通过 ServiceNow 向网络团队提交了一张工单。

---

After a couple of hours, Bob receives a Skype message from Jackie on the network team.

几个小时后，Bob 收到了来自网络团队 Jackie 的 Skype 消息。

**Jackie:**
"Hey Bob, my name is Jackie and I'm from the network team. I got your ticket and was wondering if you have some time to troubleshoot?"

**Jackie：**
"嗨 Bob，我是 Jackie，来自网络团队。我看到了你的工单，想问问你现在有时间来排查一下这个问题吗？"

**Bob:**
"Hey Jackie, thank you. Yes, I do have some time now."

**Bob：**
"嗨 Jackie，谢谢你。我现在有时间。"

**Jackie:**
"Great, can you come find me at the IT support floor, desk 8A-052? I think it's better to handle this in person."

**Jackie：**
"太好了，你能来 IT 支持楼层找我吗？我在 8A-052 号工位。我觉得面对面处理会更方便。"

---

Bob agrees and soon meets Jackie at her desk. He explains:

Bob 同意了，很快来到 Jackie 的工位旁，向她说明了情况：

**Bob:**
"I'm trying to connect to the repo server using my browser, but I keep getting an error. It was working on Dave's laptop, so I'm not sure why it's failing on mine."

**Bob：**
"我尝试用浏览器连接代码仓库服务器，但一直报错。在 Dave 的笔记本上是能用的，所以我不明白为什么在我这边会失败。"

Jackie examines the error message carefully and asks:

Jackie 仔细查看了错误信息，然后问道：

"Are you sure that's the correct URL? Judging by the error `DNS_PROBE_FINISHED_NXDOMAIN`, it doesn't appear to be a valid DNS name. This means the DNS server has no record for the hostname you entered. Was Dave using file-based name resolution for this site — like a custom entry in `/etc/hosts`?"

"你确定这个 URL 是正确的吗？从 `DNS_PROBE_FINISHED_NXDOMAIN` 这个错误来看，它似乎不是一个有效的 DNS 名称。这意味着 DNS 服务器找不到你输入的主机名记录。Dave 有没有可能是用基于文件的名称解析来访问这个站点的——比如在 `/etc/hosts` 里加了自定义条目？"

Bob admits he isn't sure what that means. Jackie suggests they delve deeper into the issue.

Bob 承认自己不太明白那是什么意思。Jackie 建议他们深入排查这个问题。

---

Jackie decides to replicate the scenario on her own laptop by accessing the same URL. She encounters the same error and remarks:

Jackie 决定在自己的笔记本上复现这个场景，访问同一个 URL。她遇到了同样的错误，并指出：

"See, the problem is with the URL. I don't think it's a valid name. If I remember correctly, the correct address should be:"

"你看，问题出在 URL 上。我认为它不是一个有效的主机名。如果我没记错的话，正确的地址应该是："

```text
http://caleston-repos-01
```

Jackie enters the new URL. The DNS error disappears, but a different error surfaces — a connection timeout.

Jackie 输入了新的 URL。DNS 错误消失了，但出现了一个新的错误——连接超时。

"Hmm, that appears to be the correct URL because the DNS error is gone. The system can now find the server's IP address — but it still can't connect to it. This means the problem is no longer about name resolution; it's about routing or the service itself."

"嗯，这应该是正确的 URL，因为 DNS 错误已经消失了。系统现在可以解析出服务器的 IP 地址了——但仍然无法连接。这说明问题已经不再是名称解析的问题了，而是路由或服务本身的问题。"

---

Eager to learn, Bob asks:

Bob 求知欲大增，问道：

"If you don't mind, can you explain how to approach troubleshooting these kinds of issues? I'm relatively new to Linux and networking, and I have a client demonstration next week. I want to ensure everything goes smoothly."

"如果你不介意的话，能给我讲讲如何排查这类网络问题吗？我对 Linux 和网络还比较陌生，下周有一个客户演示，我想确保一切顺利。"

Jackie smiles and replies:

Jackie 微笑着回答：

"Sure thing. Do you have a few minutes? I can walk you through some networking basics, and then we can troubleshoot this problem step by step. Understanding how name resolution and routing work will save you a lot of headaches in the future."

"当然。你有几分钟时间吗？我可以给你讲讲一些网络基础知识，然后我们再一步一步地排查这个问题。了解名称解析和路由的工作原理，以后会省去你很多麻烦。"

**Bob:**
"Sounds perfect. Thank you very much."

**Bob：**
"太好了。非常感谢你。"

---

> **Key Insight | 关键概念**
>
> Jackie begins the troubleshooting session by summarizing the situation:
> "You were trying to access the site using the URL `http://caleston-repos` and encountered the error: **'DNS_PROBE_FINISHED_NXDOMAIN'**. This error indicates that your laptop, acting as a DNS client, was unable to resolve the IP address for `caleston-repos`. The domain simply doesn't exist in any DNS server it queried."
>
> Jackie 开始排查会话，先做了情况总结：
> "你尝试用 URL `http://caleston-repos` 访问站点，遇到了错误：**'DNS_PROBE_FINISHED_NXDOMAIN'**。这个错误表明你的笔记本作为 DNS 客户端，无法解析 `caleston-repos` 的 IP 地址。这个域名在它查询的任何 DNS 服务器中都不存在。"
>
> She explains that every hostname must ultimately be mapped to an IP address before a connection can be made. There are two common ways to achieve this:
> - **DNS (Domain Name System):** A distributed, hierarchical naming system. Your system queries a DNS server (configured in `/etc/resolv.conf`) to look up the IP for a hostname.
> - **Local hosts file (`/etc/hosts`):** A plain-text file on your machine that maps hostnames to IP addresses directly, bypassing DNS entirely. Entries here take priority over DNS by default.
>
> 她解释道，每个主机名在建立连接之前都必须最终被映射到一个 IP 地址。实现这一目的有两种常见方式：
> - **DNS（域名系统）：** 一个分布式的分层命名系统。你的系统向 DNS 服务器（配置在 `/etc/resolv.conf` 中）查询主机名对应的 IP 地址。
> - **本地 hosts 文件（`/etc/hosts`）：** 你机器上的一个纯文本文件，直接将主机名映射到 IP 地址，完全绕过 DNS。默认情况下，这里的条目优先于 DNS。

---

## Troubleshooting Mindset | 排查思路

Jackie emphasizes that good network troubleshooting follows a **layered approach** — you start from the lowest level (physical connectivity) and work your way up:

Jackie 强调，良好的网络排查遵循**分层方法**——从最底层（物理连通性）开始，逐层向上：

| Step | Check | Command | 步骤 | 检查项 |
|------|-------|---------|------|--------|
| 1 | Interface is up | `ip link` | 1 | 网卡接口是否启动 |
| 2 | IP address assigned | `ip addr` | 2 | 是否已分配 IP 地址 |
| 3 | Default route exists | `ip route` | 3 | 默认路由是否存在 |
| 4 | DNS resolution works | `nslookup` / `dig` | 4 | DNS 解析是否正常 |
| 5 | Host is reachable | `ping` | 5 | 目标主机是否可达 |
| 6 | Route is correct | `traceroute` | 6 | 路由路径是否正确 |
| 7 | Port/service is open | `netstat` / `curl` | 7 | 端口/服务是否开放 |

---

## Further Reading | 延伸阅读

For those interested in learning more about DNS and network troubleshooting:

对于有兴趣深入了解 DNS 和网络排查的读者：

* [Understanding DNS | 了解 DNS](https://en.wikipedia.org/wiki/Domain_Name_System)
* [Linux Networking Basics | Linux 网络基础](https://www.linux.com/training-tutorials/introduction-linux-networking/)
* [Troubleshooting DNS Issues | DNS 问题排查](https://support.dnsimple.com/articles/dns-troubleshooting/)
