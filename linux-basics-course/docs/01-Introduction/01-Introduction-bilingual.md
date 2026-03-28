> ## Documentation Index
>
> Fetch the complete documentation index at: https://notes.kodekloud.com/llms.txt
> Use this file to discover all available pages before exploring further.

> ## 文档索引
>
> 完整文档索引获取地址：https://notes.kodekloud.com/llms.txt
> 请先获取该文件以发现所有可用页面，再进一步浏览。

# Course Introduction

# 课程介绍

> This article introduces a Linux Basics course focusing on essential skills for DevOps and cloud professionals.

> 本文介绍了一门 Linux 基础课程，专注于 DevOps 和云从业者的必备技能。

Hello and welcome to the Linux Basics course. My name is Michael Forrester, and I'm excited to have you join us at KodeKloud.

你好，欢迎来到 Linux 基础课程。我叫 Michael Forrester，很高兴你加入 KodeKloud。

## Why Learn Linux?

## 为什么学习 Linux？

Linux is the predominant server operating system worldwide. According to insights from Stack Overflow and other reputable sources:

Linux 是全球最主流的服务器操作系统。根据 Stack Overflow 和其他权威来源的数据：

* Every flavor of Linux is widely utilized.
* All 500 of the fastest supercomputers run on Linux.
* 96.3% of the top 1 million websites operate on Linux.
* Approximately 86% of all smartphones are powered by Linux.
* 每种 Linux 发行版都被广泛使用。
* 全球最快的 500 台超级计算机均运行 Linux。
* 全球前 100 万网站中有 96.3% 运行在 Linux 上。
* 约 86% 的智能手机由 Linux 驱动。

In the cloud and DevOps landscape, many breakthrough tools were initially designed and deployed on Linux before expanding support to other operating systems. For example, containerization tools like Docker were Linux-exclusive for many years before Windows support was introduced. Similarly, automation and configuration management tools such as Red Hat Ansible require a Linux environment for installation and centralized control, even though they can manage Windows systems as remote targets.

在云计算和 DevOps 领域，许多突破性工具最初都是为 Linux 设计和部署的，随后才扩展支持其他操作系统。例如，Docker 等容器化工具在引入 Windows 支持之前，多年来一直是 Linux 专属工具。同样，Red Hat Ansible 等自动化和配置管理工具也需要 Linux 环境进行安装和集中控制，尽管它们可以将 Windows 系统作为远程目标进行管理。

When it comes to container orchestration with tools like Kubernetes, the control plane (or master nodes) remains exclusively Linux-based. Current documentation confirms that there are no plans to develop a Windows-only Kubernetes control plane, even if Windows may be used as a data plane.

在使用 Kubernetes 等工具进行容器编排时，控制平面（或主节点）仍然是专属于 Linux 的。当前文档确认，即使 Windows 可用作数据平面，目前也没有开发纯 Windows Kubernetes 控制平面的计划。

For certification exams in Kubernetes, Red Hat Ansible, and other related technologies, having practical experience with Linux systems is crucial. Mastering Linux is a foundational step for anyone pursuing a career in DevOps, cloud, or infrastructure roles.

对于 Kubernetes、Red Hat Ansible 及其他相关技术的认证考试，拥有 Linux 系统的实际操作经验至关重要。掌握 Linux 是任何希望从事 DevOps、云计算或基础架构领域职业的人的基础一步。

## The Growing Demand for Linux Skills

## 对 Linux 技能日益增长的需求

The rapid rise of DevOps has created a significant need for professionals with strong Linux, cloud, and DevOps fundamentals. Almost every new job posting now demands a basic understanding of these areas. Engineers in organizations that leverage DevOps and cloud technologies need to be comfortable with:

DevOps 的迅速崛起催生了对具备扎实 Linux、云计算和 DevOps 基础技能的专业人员的大量需求。如今，几乎每一份新招聘启事都要求具备这些领域的基本知识。在使用 DevOps 和云技术的组织中，工程师需要熟练掌握：

* Using the Linux command line
* Managing system configurations
* Maintaining security and networking practices in a Linux environment
* 使用 Linux 命令行
* 管理系统配置
* 在 Linux 环境中维护安全和网络实践

<Callout icon="lightbulb" color="#1CB2FE">
  This course features hands-on labs designed to help you practice and apply Linux fundamentals in real-world scenarios.
</Callout>

<Callout icon="lightbulb" color="#1CB2FE">
  本课程包含动手实验，旨在帮助你在真实场景中练习和应用 Linux 基础知识。
</Callout>

## Course Structure and Learning Approach

## 课程结构与学习方法

Our course uses a story-driven format to contextualize important Linux concepts. Inspired by narrative styles in books like [The Phoenix Project](https://en.wikipedia.org/wiki/The_Phoenix_Project) and [The Unicorn Project](https://en.wikipedia.org/wiki/The_Unicorn_Project), you'll follow Bob, a new intern at Caleston Technologies. Bob faces challenges while building and deploying an application for a client demo, all while navigating a Linux-based environment across both his laptop and servers.

我们的课程采用故事驱动的方式来诠释重要的 Linux 概念。受[《凤凰项目》](https://en.wikipedia.org/wiki/The_Phoenix_Project)和[《独角兽项目》](https://en.wikipedia.org/wiki/The_Unicorn_Project)等书籍叙事风格的启发，你将跟随 Bob——Caleston Technologies 的一名新实习生——的旅程。Bob 在为客户演示构建和部署应用程序时面临各种挑战，同时需要在笔记本电脑和服务器上驾驭 Linux 环境。

Throughout the course, you will:

在整个课程中，你将：

* Follow Bob's journey as he explores the Linux operating system.
* Learn how to interact with the terminal and understand the shell, which serves as the gateway to the Linux kernel.
* Study core Linux concepts that help you understand how systems operate.
* Explore various package management methods used by different Linux distributions.
* Gain confidence in navigating the command line, manipulating files, and using a variety of text editors.
* Understand file-level security, configure permissions, and implement essential Linux security practices.
* Troubleshoot networking connectivity issues effectively.
* Configure storage options including disk formatting, mounting, and creating logical volume groups.
* Set up custom applications to start on boot using SYSTEMD, the primary initializer for Linux services.
* 跟随 Bob 探索 Linux 操作系统的旅程。
* 学习如何与终端交互，理解 shell——Linux 内核的入口。
* 学习帮助你理解系统运作方式的核心 Linux 概念。
* 探索不同 Linux 发行版使用的各种软件包管理方法。
* 熟练掌握命令行导航、文件操作及多种文本编辑器的使用。
* 理解文件级安全，配置权限，并实践必要的 Linux 安全措施。
* 有效排查网络连接问题。
* 配置存储选项，包括磁盘格式化、挂载和创建逻辑卷组。
* 使用 SYSTEMD——Linux 服务的主要初始化程序——设置自定义应用程序开机自启。

<Frame>
  ![A person is sitting in front of a list of Linux topics, including shell, core concepts, package management, security, networking, and storage, with plants in the background.](https://kodekloud.com/kk-media/image/upload/v1752881115/notes-assets/images/Learning-Linux-Basics-Course-Labs-Course-Introduction/frame_550.jpg)
</Frame>

Our lectures incorporate visualization, storytelling, and analogies to demystify complex topics. After each lecture, you will work on hands-on labs that challenge you with practical exercises, guided hints, and personal feedback.

我们的课程融合了可视化、故事叙述和类比来揭开复杂主题的神秘面纱。每节课结束后，你将参与动手实验，通过实践练习、引导提示和个人反馈来接受挑战。

<Callout icon="lightbulb" color="#1CB2FE">
  All labs are executed directly in your browser, eliminating the need for local environment setup so you can start practicing immediately.
</Callout>

<Callout icon="lightbulb" color="#1CB2FE">
  所有实验均直接在浏览器中执行，无需配置本地环境，让你能够立即开始练习。
</Callout>

## Let's Get Started

## 让我们开始吧

If you're ready, dive into the fascinating world of Linux and lay a solid foundation for your DevOps journey. Welcome aboard and enjoy the course!

如果你准备好了，就投身 Linux 的精彩世界，为你的 DevOps 之旅奠定坚实基础吧。欢迎加入，祝你学习愉快！

<CardGroup>
  <Card title="Watch Video" icon="video" cta="Learn more" href="https://learn.kodekloud.com/user/courses/learning-linux-basics-course-labs/module/571673f9-96f0-4212-bd13-a61a069c9e1f/lesson/09b34031-7fd7-4e26-ab3b-6df0acad154b" />
</CardGroup>


# WAR story

# 战时故事

> A high-pressure scenario where Bob faces a career-defining crisis while preparing for a crucial client demonstration.

> 一个高压场景，Bob 在准备一场关键客户演示时面临职业生涯中的重大危机。

In a high-pressure war room scenario, Bob found himself facing a career-defining crisis. The atmosphere was tense, and every second counted. With a client demonstration scheduled for 10:00 AM and his watch already reading 8:50 AM, Bob realized he had less than an hour to deliver a flawless presentation.

在一个高压的战时会议室场景中，Bob 发现自己正面临一场职业生涯中的重大危机。气氛紧张，分秒必争。客户演示定于上午 10:00，而他的表已经指向 8:50，Bob 意识到自己只剩不到一小时来完成一场完美的演示。

He glanced over at Andrew, who was pacing nervously, adding to the urgency of the moment. Bob thought, "I have to fix this ASAP—or I might get fired." Although he was confident in his approach using KodeKloud, his web application stubbornly refused to work.

他瞥了一眼正在焦急踱步的 Andrew，这更增添了紧迫感。Bob 心想："我必须尽快解决这个问题——否则我可能会被炒鱿鱼。"尽管他对使用 KodeKloud 的方法很有信心，但他的 Web 应用程序就是不肯正常运行。

<Callout icon="lightbulb" color="#1CB2FE">
  Bob's mounting anxiety stemmed from a flood of questions: Were there network issues? Could a permissions error be the culprit? Or was an entirely different problem at work? In that moment of panic, every possibility raced through his mind.
</Callout>

<Callout icon="lightbulb" color="#1CB2FE">
  Bob 不断加剧的焦虑源于一连串问题：是网络问题？还是权限错误在作怪？亦或是完全不同的问题？在那一刻的慌乱中，各种可能性在他脑海中飞速闪过。
</Callout>

Determined not to let his team down, Bob scrambled for answers and prepared to troubleshoot the issue swiftly, hoping to turn the crisis around before it was too late.

下定决心不让团队失望，Bob 拼命寻找答案，准备迅速排查问题，希望能在为时已晚之前化解这场危机。

<CardGroup>
  <Card title="Watch Video" icon="video" cta="Learn more" href="https://learn.kodekloud.com/user/courses/learning-linux-basics-course-labs/module/571673f9-96f0-4212-bd13-a61a069c9e1f/lesson/cc02915f-50d4-45fe-9e11-916032187579" />
</CardGroup>


# Bobs first day at work

# Bob 的入职第一天

> Bobs first day at work includes an induction, meeting colleagues, and starting his journey into Linux for a new project.

> Bob 的入职第一天包括入职培训、结识同事，以及为新项目开启他的 Linux 学习之旅。

June 24th, 6:55 AM

6 月 24 日，早上 6:55

Bob boards the early morning train bound for the city, ready for his first day at Caleston Technologies. As sunlight streams through the window, he contemplates the day ahead. The train is crowded, yet his calm confidence remains—his induction is at 9 AM in Building A, leaving plenty of time for a quick coffee stop and final preparations.

Bob 登上了开往市区的早班列车，准备迎接在 Caleston Technologies 的第一天。阳光透过车窗倾洒进来，他思索着眼前这一天。列车上人声嘈杂，但他依然保持着从容的自信——入职培训在上午 9 点于 A 楼举行，留有足够的时间喝杯咖啡、做最后的准备。

## A Fortuitous Encounter on the Train

## 列车上的邂逅

Lost in thought, Bob doesn't notice the man seated across from him scrutinizing his every move.

陷入沉思的 Bob 没有注意到对面坐着一个人正在打量他的一举一动。

"Hi there. Are you new to the city?" asks the man in a neat blue suit.

"你好。你是刚来这座城市的吧？"一位穿着整洁蓝色西装的男士问道。

Surprised, Bob looks over.
"Hello, good morning. My name is Bob, and yes, I am new here. But how did you know?"

Bob 惊讶地抬起头。
"你好，早上好。我叫 Bob，是的，我刚来这里。但你怎么知道的？"

"Look around. You're the only one gazing out the window. Most commuters are absorbed in their mobile phones at this time—they miss out on the beauty of the real world," the stranger chuckles.

"看看四周。你是唯一一个凝望窗外的人。这个时间大多数通勤者都沉浸在手机里——他们错过了现实世界的美好，"陌生人笑着说。

Bob laughs. "I hope to be one of them soon."

Bob 笑了起来。"我希望自己很快也能成为他们中的一员。"

"My name is Dave. Nice to meet you," the stranger offers with a handshake.

"我叫 Dave。很高兴认识你，"陌生人伸出手来。

Bob explains that he's starting as a junior front-end developer at Caleston Technologies. Dave smiles and exclaims, "You must be joking! I've been with Caleston for nearly fifteen years. I recall an email mentioning that many new hires would join by the end of the month."

Bob 解释说他将以初级前端开发工程师的身份加入 Caleston Technologies。Dave 微笑着惊呼："你一定是在开玩笑！我在 Caleston 已经待了将近十五年了。我记得有封邮件提到月底会有很多新员工加入。"

As the train halts, Dave asks, "Here's where we get off. The office building is right across the station. You're heading to Building A, correct?"

列车停靠时，Dave 问道："我们在这里下车。办公楼就在车站对面。你去 A 楼，对吧？"

"Yes, Building A," Bob confirms, adding, "though I have a team meeting in Building C in fifteen minutes."

"是的，A 楼，"Bob 确认道，并补充说，"不过我十五分钟后在 C 楼有个团队会议。"

Dave sighs, "It was great meeting you. I'm sure we'll meet again. Good luck!" He then heads toward Building C.

Dave 叹了口气："很高兴认识你。我相信我们还会再见面的。祝你好运！"随后他朝 C 楼走去。

Bob, both amused and hopeful, muses, "What a stroke of luck—meeting a colleague before even stepping into the office. I wonder what the team meeting is like," as he walks toward Building A.

Bob 既觉得有趣又充满期待，心想："真是好运气——还没进办公室就遇到了同事。不知道那个团队会议是什么样的，"他一边想着，一边向 A 楼走去。

---

## 11:20 AM – The Induction Event

## 上午 11:20——入职培训活动

The induction event is both informative and energizing. Bob joins fifteen other software professionals in a session where he learns about Caleston Technologies' global projects, particularly in DevOps and cloud technologies. The forty-five-minute presentation details the company's vision, workplace safety guidelines, and the Anti-Corruption Act.

入职培训既充实又振奋人心。Bob 与另外十五名软件专业人员一同参加了这次活动，了解了 Caleston Technologies 的全球项目，特别是在 DevOps 和云技术方面的布局。这场四十五分钟的演示详细介绍了公司愿景、工作场所安全准则和《反腐败法》。

After the presentation, Bob heads down to the IT floor to collect his key card and laptop. He then takes the elevator to the eighth floor and proceeds to desk 8A.001, where his new manager, Mumshad Mannambeth, awaits him.

演示结束后，Bob 前往 IT 层领取门禁卡和笔记本电脑。随后他乘电梯来到八楼，走向 8A.001 号工位，新经理 Mumshad Mannambeth 在那里等候着他。

<Callout icon="lightbulb" color="#1CB2FE">
  Settling down at his desk, Bob thinks, "It's unboxing time!" The laptop turns out to be an eighth-generation i5 with a 128GB SSD and 8GB of RAM—adequate for now despite the limited disk space. As Bob powers on the laptop, he anticipates the familiar Windows boot screen.
</Callout>

<Callout icon="lightbulb" color="#1CB2FE">
  在工位安顿下来后，Bob 心想："开箱时间到！"这台笔记本电脑搭载第八代 i5 处理器、128GB 固态硬盘和 8GB 内存——尽管磁盘空间有限，目前还算够用。Bob 开机时，期待着熟悉的 Windows 启动画面出现。
</Callout>

<Frame>
  ![A man named Bob attends a meeting, retrieves a laptop from a case labeled "8A-001," and begins working on it, displaying code on the screen.](https://kodekloud.com/kk-media/image/upload/v1752881109/notes-assets/images/Learning-Linux-Basics-Course-Labs-Bobs-first-day-at-work/frame_310.jpg)
</Frame>

After a few moments of confusing boot messages, Bob exclaims, "This is not Windows... Ah, it's Ubuntu Linux! Sweet!" Although his past experience was mostly with Windows 10 and occasionally macOS, Bob had dabbled in Ubuntu on a spare personal computer and enjoyed exploring open-source software.

经过几秒钟令人困惑的启动信息后，Bob 脱口而出："这不是 Windows……啊，是 Ubuntu Linux！太棒了！"虽然他以往主要使用 Windows 10，偶尔用 macOS，但 Bob 曾在一台闲置的个人电脑上玩过 Ubuntu，并且很享受探索开源软件的过程。

Logging in with the initial credentials provided by IT support, he quickly notices that his desktop interface differs significantly from Windows—the taskbar is now located on the left-hand side. Pre-installed apps include Thunderbird for email, Mozilla Firefox, and LibreOffice Writer. An icon labeled Ubuntu Software provides quick access to applications like Skype and Slack, which feels curiously familiar.

使用 IT 支持提供的初始凭证登录后，他很快注意到桌面界面与 Windows 有很大不同——任务栏现在位于左侧。预装应用包括用于邮件的 Thunderbird、Mozilla Firefox 和 LibreOffice Writer。一个标有"Ubuntu 软件"的图标可以快速访问 Skype 和 Slack 等应用，这种熟悉感让他倍感亲切。

Just as Bob is about to launch Thunderbird, a tall figure approaches his desk.

就在 Bob 准备打开 Thunderbird 时，一个高挑的身影走近了他的工位。

"You must be Bob. I am Andrew—we spoke over the phone the other day," the newcomer greets him.

"你一定是 Bob。我是 Andrew——我们前几天通过电话，"来者打招呼道。

"Hey, Andrew. How are you?" Bob replies, shaking his hand.

"嘿，Andrew，你好吗？"Bob 握手回应道。

Andrew inquires, "How was your induction?"

Andrew 问道："入职培训怎么样？"

"It went great! I learned a lot about the company and the upcoming projects—including Project Mercury and Project Kanban," Bob excitedly responds.

"非常棒！我了解了很多关于公司和即将启动的项目——包括水星项目和看板项目，"Bob 兴奋地回答道。

Andrew nods, "Excellent. Project Mercury is a major win for us. Incidentally, that's the project you'll be working on. Get ready to dive into a world of cloud computing and DevOps."

Andrew 点头道："很好。水星项目对我们来说是重大突破。顺便说一下，那正是你将要参与的项目。准备好投身云计算和 DevOps 的世界吧。"

Bob beams, "I sure am—I'm eager to learn."

Bob 笑容满面："当然，我迫不及待想要学习。"

Andrew notices Bob is already logged into his laptop, commenting, "I see you've discovered one of the surprises I had in store for you. I know you've primarily used Windows, but with Project Mercury, we're embracing DevOps and cloud-native tools. A basic knowledge of Linux is essential for success in this role."

Andrew 注意到 Bob 已经登录了笔记本电脑，说道："我看你已经发现了我为你准备的惊喜之一。我知道你主要使用 Windows，但在水星项目中，我们将全面拥抱 DevOps 和云原生工具。基本的 Linux 知识对于在这个职位上取得成功至关重要。"

"Absolutely. This is a fantastic opportunity, and I'm going to make the most of it," Bob assures him.

"当然。这是一个绝佳的机会，我会好好把握的，"Bob 保证道。

Andrew continues, "Your first task is to learn Linux. Don't worry—you won't be starting from scratch. The IT team has provided an online self-start guide for mastering the command line interface, complete with hands-on lab exercises and a mandatory test at the end of each chapter. Once you complete the first chapter, I'll introduce you to Dave, our lead systems engineer, who will provide more advanced lessons."

Andrew 继续说："你的第一项任务是学习 Linux。别担心——你不会从零开始。IT 团队提供了一份在线自学指南，帮助你掌握命令行界面，内含动手实验练习，每章结束后还有必修测试。完成第一章后，我会把你介绍给 Dave——我们的首席系统工程师，他会提供更高级的课程。"

Bob gasps, "Dave? I think I already met him this morning on the train!"

Bob 惊呼："Dave？我想我今天早上在火车上已经见过他了！"

"Why am I not surprised?" Andrew laughs. "Remember, he knows everyone. I won't take up more of your time, but I want you to take this training seriously—your first project depends on it. Please complete the first chapter by Wednesday; in fact, I'm asking everyone on my team to finish the online test by tomorrow evening."

"这有什么好奇怪的？"Andrew 笑道。"记住，他认识所有人。我就不多占用你的时间了，但我希望你认真对待这次培训——你的第一个项目就取决于此。请在周三之前完成第一章；实际上，我要求我团队里的所有人明晚之前完成在线测试。"

---

## Diving into Linux

## 深入 Linux

After the conversation with Andrew, Bob opens Thunderbird and notices his mailbox is pre-configured. One email from IT support includes a quick start guide for the new Linux OS—a full crash course on navigating the Linux shell with two chapters and hands-on lab tests. The first chapter is due on June 26th.

与 Andrew 交谈结束后，Bob 打开 Thunderbird，发现邮箱已预先配置好。IT 支持发来的一封邮件中包含新 Linux 系统的快速入门指南——一套完整的 Linux shell 速成课程，共两章，附带动手实验测试。第一章的截止日期为 6 月 26 日。

Reviewing the guide, Bob sees that the terminal application isn't pinned to the taskbar. Following one tip, he clicks on the "Show Applications" launcher and types "terminal" into the search bar. "There you go. I'm in. This is the part I really want to dig into," he muses.

查看指南时，Bob 发现终端应用没有固定在任务栏上。按照一条提示，他点击"显示应用程序"启动器，在搜索栏中输入"terminal"。"找到了。进去了。这正是我真正想深入探索的部分，"他自言自语道。

<Frame>
  ![The image shows a Linux quick start guide, email notifications, a quiz about home directories, a file search task, and a comic-style illustration of a person thinking.](https://kodekloud.com/kk-media/image/upload/v1752881110/notes-assets/images/Learning-Linux-Basics-Course-Labs-Bobs-first-day-at-work/frame_720.jpg)
</Frame>

With determination, Bob launches the first chapter of the quick start guide and embarks on his journey into the world of Linux.

满怀决心，Bob 打开了快速入门指南的第一章，踏上了他探索 Linux 世界的旅程。

<Callout icon="lightbulb" color="#1CB2FE">
  When working with Linux, keep your terminal commands handy. Save essential scripts and commands in text files for quick reference.
</Callout>

<Callout icon="lightbulb" color="#1CB2FE">
  在使用 Linux 时，请随时备好终端命令。将常用脚本和命令保存在文本文件中以便快速查阅。
</Callout>

```bash
bash
/home/bob/answer-2.txt
```
