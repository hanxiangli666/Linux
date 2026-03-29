# Behind Schedule | 进度落后

> **Story Overview | 故事概览**
> Team members discuss project updates, focusing on issues with Project Sapphire and progress on Project Mercury ahead of an upcoming demo.
>
> 团队成员召开会议讨论项目进展，重点关注 Sapphire 项目的持续问题，以及 Mercury 项目在 Demo 前的推进情况。

---

## July 2nd, 10 a.m. — 12 Days Before the Demo
## 7 月 2 日，上午 10 点 —— 距 Demo 还有 12 天

During their weekly conference call, team members Bob, Andrew, and Amira converge to discuss project updates. Early on, Andrew expresses his frustration regarding the persistent escalations in Project Sapphire:

团队成员 Bob、Andrew 和 Amira 在每周例行电话会议上汇聚一堂，讨论各项目的最新进展。会议一开始，Andrew 就对 Sapphire 项目接连不断的升级问题表达了强烈不满：

---

**Andrew:**
> "Three back-to-back escalations. There are too many issues with Project Sapphire. I'm tired of it. Luckily, Donald is managing the situation for me — he's doing a fantastic job."

**Andrew（安德鲁）：**
> "接连三次问题升级，Sapphire 项目的麻烦实在太多了，我已经精疲力竭。幸好 Donald 在替我处理——他做得非常出色。"

---

After a brief pause, Andrew shifts focus to Project Mercury:

短暂停顿后，Andrew 将话题转向 Mercury 项目：

---

**Andrew:**
> "Tell me some good news, Bob. How is the progress on Project Mercury? I hope you've been able to make headway on your laptop."

**Andrew：**
> "Bob，给我说点好消息吧。Mercury 项目进展如何？希望你在自己的电脑上已经有所推进了。"

---

Bob responds somewhat hesitantly:

Bob 有些犹豫地回应道：

**Bob:**
> "I just started reviewing the design documents. I'm also working on deploying it locally and assisting with the issues related to Project Sapphire."

**Bob（鲍勃）：**
> "我刚开始审阅设计文档，同时也在本地尝试部署，并协助处理 Sapphire 项目的相关问题。"

---

Andrew's tone sharpens as he addresses the disconnect in communication:

Andrew 语气明显加重，指出沟通上的严重脱节：

**Andrew:**
> "That's not what I wanted to hear. Donald mentioned that he handed you the design documents last Wednesday and expected you to migrate the template code to your laptop this week."

**Andrew：**
> "这不是我想听到的答案。Donald 告诉我，他上周三就把设计文档交给你了，并且期望你本周完成模板代码迁移到本地的工作。"

---

Bob defends himself, clearly caught off guard:

Bob 显然措手不及，为自己辩解道：

**Bob:**
> "Well, he did not tell me. In fact, he hasn't mentioned anything at all regarding that."

**Bob：**
> "他根本没有告诉我任何这方面的事情，我完全不知情。"

---

Andrew, visibly frustrated, continues:

Andrew 面露不悦，继续追问：

**Andrew:**
> "And what template code is he talking about?"

**Andrew：**
> "他说的模板代码到底是什么？"

---

Sensing the tension, Bob stays quiet. Andrew then turns to Amira with a new directive:

感受到会议室里紧张的气氛，Bob 沉默不语。Andrew 随即转向 Amira，发出新的指令：

**Andrew:**
> "Amira, please set up bi-weekly meetings to sort this out."

**Andrew：**
> "Amira，请安排每两周一次的同步会议，确保大家步调一致。"

---

Amira, always thorough, asks for clarification:

一向细心的 Amira 进一步确认细节：

**Amira:**
> "Do you also want me to send a progress report daily?"

**Amira（阿米拉）：**
> "您是否希望我每天发送一份进度报告？"

---

![An office worker participates in a video call with colleagues, wearing headsets in a comic-style illustration.](https://kodekloud.com/kk-media/image/upload/v1752881159/notes-assets/images/Learning-Linux-Basics-Course-Labs-Behind-Schedule/frame_100.jpg)

---

Andrew confirms the reporting requirement and emphasizes what's at stake:

Andrew 确认了汇报要求，并再次强调当前任务的重要性：

**Andrew:**
> "Yes, please. And keep me updated on any showstoppers. We must have everything ready and functioning for Phase 1."

**Andrew：**
> "是的，请这样做。任何阻塞性问题都要第一时间告诉我。第一阶段的所有功能必须准备就绪，不容有失。"

---

Andrew turns back to Bob, making the priority crystal clear:

Andrew 重新看向 Bob，语气严肃地明确优先级：

**Andrew:**
> "Bob, it's great that you're addressing the escalations, but your primary focus must be on Project Mercury. Follow Mumshad Mannambeth's guidance and ensure the application is running on your laptop by Monday. This gives us one week to test it before pushing to the development servers."

**Andrew：**
> "Bob，你协助处理升级问题固然值得肯定，但你当前的首要任务必须是 Mercury 项目。请参照 Mumshad Mannambeth 的指导，务必在周一之前让应用程序在你本地正常运行。这样我们还有一周时间进行测试，然后才能推送到开发服务器。"

---

Bob, resolved and with a clear sense of urgency:

Bob 神情坚定，语气中透着紧迫感：

**Bob:**
> "Sure, Andrew. I'll make sure it's completed on time."

**Bob：**
> "明白，Andrew。我一定会按时完成的。"

---

## July 3rd, 11 a.m. — 11 Days Before the Demo
## 7 月 3 日，上午 11 点 —— 距 Demo 还有 11 天

After reviewing numerous emails from Donald regarding Project Mercury, Bob discovers instructions for migrating template Python scripts from a Windows environment to Linux.

仔细阅读了 Donald 关于 Mercury 项目的一系列邮件之后，Bob 终于找到了将模板 Python 脚本从 Windows 环境迁移到 Linux 的详细说明。

---

Bob now faces the concrete task of setting up a Django project on Linux. This involves several technical steps:

Bob 现在面临的具体任务是在 Linux 上搭建 Django 项目，这涉及以下几个技术步骤：

- **Remotely copying files** using `scp` or `rsync`
- **Extracting compressed archives** using `tar`, `gzip`, or `bzip2`
- **Making configuration adjustments** to Django settings
- **Editing source code** using command-line text editors like `vi` or `vim`

---

- **远程复制文件**：使用 `scp` 或 `rsync` 命令
- **解压归档文件**：使用 `tar`、`gzip` 或 `bzip2` 工具
- **修改配置文件**：调整 Django 的 `settings.py`
- **编辑源代码**：使用命令行文本编辑器 `vi` 或 `vim`

---

To support his efforts, Bob reviews the following key areas — which form the core of this section:

为了顺利完成任务，Bob 系统性地回顾了以下关键技术领域，这些内容也是本章的学习核心：

| Topic | 主题 | Description | 说明 |
|-------|------|-------------|------|
| File Compression & Archival | 文件压缩与归档 | Package and compress files for transfer | 打包并压缩文件以便传输 |
| Searching for Files & Patterns | 搜索文件与内容 | Locate files and search text efficiently | 高效定位文件并搜索文本内容 |
| IO Redirection | 输入输出重定向 | Control command input, output, and errors | 控制命令的输入、输出与错误流 |
| Vi Editor | Vi 编辑器 | Edit files directly from the terminal | 直接在终端中编辑文件 |

---

> **Tip | 提示**
> For more information on setting up a Django project on Linux, refer to the official [Django Documentation](https://docs.djangoproject.com/).
>
> 有关在 Linux 上搭建 Django 项目的更多信息，请参阅官方 [Django 文档](https://docs.djangoproject.com/)。
