# Bob's First Team Meeting
# Bob 的第一次团队会议

> A new developer named Bob joins a team working on Project Mercury, facing challenges with project confusion and Linux environment setup.
>
> 新开发者 Bob 加入了负责 Mercury 项目的团队，面临项目混乱和 Linux 环境搭建的双重挑战。

---

## Welcome to the Team
## 欢迎加入团队

"Good morning, team! Join me in welcoming Bob, our new developer, who will be contributing to the delivery of Project Mercury."

"早上好，大家！请和我一起欢迎 Bob，我们的新开发者，他将参与 Mercury 项目的交付工作。"

Bob receives a warm welcome from his new teammates. Andrew enthusiastically praises him: "I must say, Bob, good job on completing the mandatory training and tests on such short notice."

Bob 受到了新同事们的热情欢迎。Andrew 热情地称赞道："我必须说，Bob，你能在这么短的时间内完成强制培训和测试，做得非常好。"

Andrew continues: "Now, Bob, if you don't mind, please introduce yourself to the team." With a smile, Bob introduces himself.

Andrew 继续说："Bob，如果你不介意的话，请向团队介绍一下自己。" Bob 微笑着做了自我介绍。

"Excellent. Let me introduce the team working on Project Mercury:
- **Donald** — our senior developer who joined last year, has already contributed to several projects and will support you during the first phase of Project Mercury. Feel free to approach him for any technical advice on application development.
- **Amira** — our project manager assigned to this engagement, will be sending you frequent emails regarding project updates.
- **Aditi** — our test engineer who joined last month from our offshore office in Bangalore, will assist with software testing during the later stages of the project."

"很好。让我来介绍一下 Mercury 项目团队：
- **Donald** — 去年加入的高级开发者，已参与多个项目，将在 Mercury 项目第一阶段为你提供支持。如有应用开发方面的技术问题，随时可以找他。
- **Amira** — 本项目的项目经理，会定期发送项目进展邮件。
- **Aditi** — 上个月从班加罗尔离岸办公室加入的测试工程师，将在项目后期协助软件测试工作。"

---

## Project Mercury Overview
## Mercury 项目概述

Andrew outlines the task at hand:

Andrew 概述了当前任务：

> Project Mercury will progress along several parallel streams. Our immediate goal is to deliver a working sample of the application for the client demo website. This sample will run on our on-premise development servers, with the **client demo scheduled for July 15th** — less than three weeks away.

> Mercury 项目将沿多条并行线推进。我们的当前目标是为客户演示网站交付一个可运行的应用样本，该样本将运行在我们的本地开发服务器上，**客户演示定于 7 月 15 日**——距今不到三周时间。

After an in-depth discussion (or, as some call it, a **distro war** among the IT team), the decision was made to use **Ubuntu** for the development servers. Looking further ahead, there is potential to:
- Deploy the application in the cloud on a VM running **Debian**, **CentOS**, or **Red Hat**
- Containerize the entire application to run as **microservices** on a managed cloud **Kubernetes** environment

经过深入讨论（IT 团队内部称之为"发行版之战"），最终决定在开发服务器上使用 **Ubuntu**。展望未来，还有可能：
- 将应用部署到运行 **Debian**、**CentOS** 或 **Red Hat** 的云端虚拟机
- 将整个应用容器化，以**微服务**形式运行在托管云 **Kubernetes** 环境中

> "That's why you've received an Ubuntu laptop. Donald will brief you on the progress so far, and Amira will share the comprehensive project plan. I also suggest you connect with **Dave** when you have a spare moment — he will be your go-to person for any issues related to the new OS."

> "这就是为什么你拿到了一台 Ubuntu 笔记本。Donald 会向你介绍目前的进展，Amira 会分享完整的项目计划。我建议你有空的时候联系一下 **Dave**——他是你解决新操作系统相关问题的最佳人选。"

> **Learning Point / 学习要点**: This scenario illustrates why Linux knowledge is essential for modern developers. Whether deploying to bare-metal servers, VMs, containers, or Kubernetes, the underlying OS is almost always Linux.
>
> 这个场景说明了为什么 Linux 知识对现代开发者至关重要。无论是部署到物理服务器、虚拟机、容器还是 Kubernetes，底层操作系统几乎都是 Linux。

---

## Initial Emails and Project Confusion
## 初始邮件与项目混乱

On **June 26 at 11 a.m.**, just 18 days before the demo, Bob settles at his desk and notices several new emails:
- A welcome message from Amira with high-level timelines for Project Mercury
- Multiple urgent emails from Donald marked "ASAP"

**6 月 26 日上午 11 点**，距演示还有 18 天，Bob 坐在工位上发现了几封新邮件：
- Amira 发来的欢迎邮件，附带 Mercury 项目的高层时间线
- Donald 发来的多封标注"尽快处理"的紧急邮件

As Bob scrolls through Donald's emails, he discovers a long email thread referencing **"Project Sapphire"** — a completely different project dating back three months. One email even contains a JavaScript code snippet:

在翻阅 Donald 的邮件时，Bob 发现了一个关于 **"Sapphire 项目"** 的长邮件串——这是一个完全不同的项目，邮件可以追溯到三个月前。其中一封邮件甚至包含一段 JavaScript 代码：

```javascript
document.M = new Array();
document.M[0] = new Image();
document.M[1] = new Image();
document.M[2] = new Image();
```

Bob wonders: *"Perhaps Donald wants me to help with Project Sapphire before diving into Project Mercury?"* He tries to reach Donald on Skype, only to find his status is set to **Do Not Disturb**.

Bob 心想：*"也许 Donald 希望我在处理 Mercury 项目之前先帮忙解决 Sapphire 项目的问题？"* 他尝试通过 Skype 联系 Donald，却发现对方状态设置为**请勿打扰**。

Another confusing code snippet appears in subsequent emails:

后续邮件中又出现了另一段令人困惑的代码：

```javascript
document.MY = new Array();
for (i = 0; i < document.MY.length; i++) {
    document.MY[i].id = new Array();
    document.MY[i].id[0] = i;
}
```

Despite repeated attempts to contact Donald, Bob has no luck. He spends the remainder of the day trying to manage on his own.

尽管多次尝试联系 Donald，Bob 都没有得到回应。他只能独自一人度过了这一天，艰难地处理各种问题。

> **Workplace Lesson / 职场启示**: Clear communication and proper task assignment are critical in software teams. Unclear ownership and forwarded emails without context create confusion and waste time.
>
> 清晰的沟通和明确的任务分配在软件团队中至关重要。不明确的所有权和缺乏背景的转发邮件会造成混乱、浪费时间。

---

## Linux Environment and Directory Structure
## Linux 环境与目录结构

On **June 28 at 2 p.m.**, 16 days before the demo, Bob is busy addressing client escalations related to Project Sapphire. Amid the daily chaos, Bob finds himself comfortable with basic Linux commands but still unclear about Linux internals.

**6 月 28 日下午 2 点**，距演示还有 16 天，Bob 忙于处理 Sapphire 项目的客户升级问题。在日常混乱之中，Bob 发现自己对基本 Linux 命令还算熟悉，但对 Linux 内部机制仍一知半解。

He knows his home directory is at `/home`, yet these directories remain puzzling:

他知道自己的家目录在 `/home`，但以下这些目录仍令他困惑：

| Directory / 目录 | Bob's Question / Bob 的疑问 |
|---|---|
| `/etc` | What kind of files live here? / 这里存放什么文件？ |
| `/usr` | Is this for users? Why is it so large? / 这是给用户用的吗？为什么这么大？ |
| `/var` | What "varies" here? / 什么东西在这里"变化"？ |
| `/tmp` | Is this safe to use? / 这个可以安全使用吗？ |
| `/dev` | Are these real devices? / 这些是真实设备吗？ |

Bob wonders: *"Were these directories generated automatically during installation? What exactly is the Linux Kernel?"* Growing increasingly confused, Bob reaches out to **Dave** on Skype.

Bob 心想：*"这些目录是安装时自动生成的吗？Linux 内核到底是什么？"* 越来越困惑的他通过 Skype 联系了 **Dave**。

Dave warmly welcomes Bob and says: *"I'd be happy to help clear up your Linux questions. Let's examine the file system structure on your machine."*

Dave 热情地欢迎 Bob，说道：*"我很乐意帮你解答 Linux 问题。我们来看看你机器上的文件系统结构。"*

Bob runs `ls /` on his Ubuntu system:

Bob 在 Ubuntu 系统上运行 `ls /`：

```bash
bob@caleston-lp10:~$ ls /
bin   boot   cdrom   dev   etc   home   initrd.img   lib   lib64
lost+found   media   mnt   opt   proc   root   run   sbin   snap
srv   swapfile   sys   tmp   timeshift   usr   var   vmlinuz
```

> **Key insight / 关键认识**: Every path in Linux starts from `/` (root). Unlike Windows which has drive letters (`C:\`, `D:\`), Linux has a single unified tree starting at `/`. All storage devices, network shares, and virtual filesystems are **mounted** somewhere within this tree.
>
> Linux 中的每个路径都从 `/`（根目录）开始。与 Windows 使用盘符（`C:\`、`D:\`）不同，Linux 有一棵从 `/` 开始的统一目录树。所有存储设备、网络共享和虚拟文件系统都**挂载**在这棵树的某处。

---

## Setting Up the Lab Session
## 准备实验环境

Dave highlights the benefits of the upcoming training session:

Dave 介绍了即将到来的培训课程的好处：

> "This isn't your typical meeting room. See those computers over there? They give you access to an environment similar to what's in the quick start guide. You can experiment with **real operating system commands** and test different scenarios during our session. Don't worry about permanent changes — **everything will be reset after an hour**."

> "这不是普通的会议室。看到那边的电脑了吗？它们可以让你访问与快速入门指南中类似的环境。你可以在我们的课程中练习**真实的操作系统命令**并测试不同场景。不用担心永久修改——**环境会在一小时后自动重置**。"

Bob feels a surge of relief and excitement at the prospect of hands-on learning.

Bob 对即将到来的动手实践感到既轻松又兴奋。

During their conversation, Dave clarifies an important workplace boundary:

在交谈中，Dave 澄清了一个重要的职场边界：

> "Donald from your team assigned you some tasks related to Project Sapphire, but remember — you're also working on **Project Mercury**. Prioritize the Mercury tasks, as Andrew expects that focus from you. And remember — technically, **Donald isn't your manager**, so you can choose which tasks to accept."

> "你们团队的 Donald 给你分配了一些 Sapphire 项目的任务，但请记住——你的主要工作是 **Mercury 项目**。要优先处理 Mercury 的任务，这是 Andrew 对你的期望。另外，从技术上讲，**Donald 不是你的上司**，所以你可以选择接受哪些任务。"

Although Bob finds this advice unusual, he is ready to learn.

虽然这个建议让 Bob 感到有些意外，但他已经准备好开始学习了。

Dave concludes: *"Let's get started with the Linux basics. Today's session in the lab will not be purely theoretical — you'll get significant **hands-on experience**. Ready?"*

Dave 总结道：*"让我们开始 Linux 基础学习吧。今天的实验课不是纯理论的——你会有大量的**动手实践**机会。准备好了吗？"*

Bob nods enthusiastically: *"Yes, absolutely."*

Bob 热情地点头：*"是的，完全准备好了。"*

**And so, the training begins. / 培训就此开始。**

---

> **What's Coming Next / 接下来的内容**:
> In the following sections, Bob (and you!) will learn:
> - The Linux Kernel — what it is and what it does
> - How Linux manages hardware
> - The Linux boot sequence
> - Run levels and systemd targets
> - Linux file types
> - The Filesystem Hierarchy Standard (FHS) — explaining all those mysterious directories
>
> 在接下来的章节中，Bob（以及你！）将学习：
> - Linux 内核——它是什么、做什么
> - Linux 如何管理硬件
> - Linux 启动流程
> - 运行级别与 systemd 目标
> - Linux 文件类型
> - 文件系统层级标准（FHS）——解释所有那些神秘的目录
