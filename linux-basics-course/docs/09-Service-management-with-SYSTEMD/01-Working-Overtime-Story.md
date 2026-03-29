
# Working Overtime

# 加班故事

> This article explores creating a SYSTEMD service for a Django application to run it as a background service and ensure it starts automatically at system boot.

> 本文探讨如何为 Django 应用创建 SYSTEMD 服务，使其作为后台服务运行，并确保在系统启动时自动启动。

---

*July 12th, 7 p.m. – Two days before the demo*

*7月12日，晚上7点 —— 距演示还有两天*

Bob is burning the midnight oil on a Friday evening. Instead of working at his desk, he is unwinding in the lounge area. Next to him, Dave is under immense pressure, handling an urgent production change that required patching hundreds of servers.

Bob 在一个周五的傍晚加班奋战。他没有坐在工位上，而是在休息区放松着。坐在他旁边的 Dave 压力巨大，正在处理一项紧急的生产变更，需要对数百台服务器打补丁。

While Bob is intent on making sure his Django application runs correctly before the switch to the development servers, he encounters a persistent problem: every time he exits the terminal or reboots the system, the application stops running entirely.

Bob 专注于确保在切换到开发服务器之前他的 Django 应用能正确运行，但他遇到了一个令人头疼的持续性问题：每次他退出终端或重启系统，应用就会完全停止运行。

Bob turns to Dave with a common developer challenge:

Bob 带着一个开发者常见的困惑向 Dave 请教：

> "How do I make it run in the background and ensure that it is up and running when the system boots?"

> "我怎样才能让它在后台运行，并且确保系统启动后它也能自动运行起来？"

Dave responds with a straightforward solution:

Dave 给出了一个简洁明了的答案：

> "You need to create a SYSTEMD service for the application."

> "你需要为这个应用创建一个 SYSTEMD 服务。"

Before Dave can continue, Mumshad Mannambeth interjects:

Dave 还没来得及继续说，Mumshad Mannambeth 插话道：

> "Give me about 15 minutes to finish my task, and I'll show you exactly how it's done."

> "给我大概 15 分钟把手头的任务完成，然后我来手把手教你怎么做。"

> "Thanks," Bob replies.

> "太感谢了，"Bob 回答道。

Curious about how Dave managed to patch over 100 servers so quickly, Bob asks:

Bob 对 Dave 能如此快速地给 100 多台服务器打补丁感到好奇，于是问道：

> "But didn't you have to patch more than 100 servers? How are you doing this so quickly?"

> "你不是要给 100 多台服务器打补丁吗？你怎么能做得这么快？"

Dave smiles and explains:

Dave 微笑着解释说：

> "Ah, I am using an automation tool called Ansible. I'll show you how that works someday, but for now, let me demonstrate service management with SYSTEMD."

> "哦，我用的是一个叫 Ansible 的自动化工具。改天我会教你它是怎么运作的，但现在，让我先给你演示一下用 SYSTEMD 进行服务管理。"

---

<Callout icon="lightbulb" color="#1CB2FE">
  Creating a SYSTEMD service is a best practice to ensure your applications run in the background and automatically restart on system boot. It also gives you fine-grained control over process lifecycle, dependencies, logging, and recovery behavior.

  创建 SYSTEMD 服务是确保应用在后台运行并在系统启动时自动重启的最佳实践。它还能让你对进程生命周期、依赖关系、日志记录和恢复行为进行精细化控制。
</Callout>

---

## Why Does This Problem Happen?

## 为什么会出现这个问题？

When you start a process directly from a terminal (e.g., `python manage.py runserver`), it runs as a **foreground child process** tied to your shell session. The moment that session ends — whether you close the terminal, log out via SSH, or the connection drops — the operating system sends a **SIGHUP** (hangup signal) to the process, causing it to terminate.

当你直接从终端启动一个进程时（例如 `python manage.py runserver`），它以**前台子进程**的方式运行，与你的 Shell 会话绑定。一旦该会话结束——无论是关闭终端、通过 SSH 登出，还是连接断开——操作系统就会向该进程发送 **SIGHUP**（挂起信号），导致进程终止。

SYSTEMD solves this by managing the process independently from any user session, running it as a **daemon** (background service) with full lifecycle management.

SYSTEMD 通过独立于任何用户会话来管理进程，以**守护进程**（后台服务）的形式运行，并提供完整的生命周期管理，从而解决了这一问题。

---

## What You Will Learn in This Section

## 本章学习内容

In this section, we will explore how to create a SYSTEMD service for a Django application, enabling it to run as a background service and start automatically at system boot. This approach not only improves reliability but also minimizes downtime during reboots or terminal exits. Topics covered include:

本章将探讨如何为 Django 应用创建 SYSTEMD 服务，使其作为后台服务运行，并在系统启动时自动启动。这种方式不仅提高了可靠性，还最大程度地减少了因重启或退出终端而导致的停机时间。涵盖的主题包括：

- Understanding the structure of a `.service` unit file
- 理解 `.service` 单元文件的结构
- Creating and registering a custom service
- 创建并注册自定义服务
- Using `systemctl` to manage service lifecycle
- 使用 `systemctl` 管理服务生命周期
- Using `journalctl` to view service logs and troubleshoot
- 使用 `journalctl` 查看服务日志并进行故障排查

---

For more insights into setting up reliable services on Linux systems, check out the [systemd documentation](https://www.freedesktop.org/wiki/Software/systemd/) and related [Django deployment guides](https://docs.djangoproject.com/en/stable/howto/deployment/).

如需深入了解在 Linux 系统上设置可靠服务的更多内容，请参阅 [systemd 官方文档](https://www.freedesktop.org/wiki/Software/systemd/) 以及相关的 [Django 部署指南](https://docs.djangoproject.com/en/stable/howto/deployment/)。

<CardGroup>
  <Card title="Watch Video" icon="video" cta="Learn more" href="https://learn.kodekloud.com/user/courses/learning-linux-basics-course-labs/module/35ba7692-62b1-429d-87e3-74ac7cc25061/lesson/da570340-ffb0-4f28-bfba-452b9cd0904f" />
</CardGroup>

Built with [Mintlify](https://mintlify.com).
