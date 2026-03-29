
# Where Is My Storage?
# 我的存储空间去哪了？

> Bob encounters a storage issue on his laptop while preparing for a client demo and seeks help from a storage administrator.
>
> Bob 在为客户演示做准备时，遇到了笔记本电脑存储空间不足的问题，并向存储管理员寻求帮助。

---

**July 9th, 9 a.m. – Five Days Before the Demo**
**7月9日，上午9点——距演示还有五天**

Bob is hard at work preparing for an upcoming client demonstration. His final task is to migrate a Django project from his laptop to the development servers. However, while attempting to download a large file into his home directory via the shell, he encounters the error **"no space left on device."** Running the `df` command reveals that his home directory is 95% full, despite the total mounted filesystems amounting to only about 50 gigabytes. This is particularly puzzling given that his laptop is advertised to have a 128-gigabyte SSD.

Bob 正在紧张地为即将到来的客户演示做准备。他的最后一项任务是将一个 Django 项目从笔记本电脑迁移到开发服务器上。然而，当他尝试通过 shell 将一个大文件下载到主目录时，却遭遇了错误提示：**"no space left on device"（设备上没有可用空间）**。运行 `df` 命令后发现，他的主目录已经使用了 95%，而所有挂载的文件系统加起来只有约 50 GB。考虑到他的笔记本配备的是 128 GB 的 SSD，这个数字实在令人困惑。

```bash
# 查看磁盘使用情况 / Check disk usage
[bob@laptop ~]$ df -hP
Filesystem      Size  Used Avail Use% Mounted on
/dev/sda1        50G   47G  500M  95% /home
tmpfs           3.9G     0  3.9G   0% /dev/shm
```

<Callout icon="lightbulb" color="#1CB2FE">
  When disk space discrepancies occur, consider checking for reserved system space, hidden files, and unmounted partitions that might be consuming storage. Use `lsblk` to view the complete picture of all block devices.

  当磁盘空间出现异常时，应检查系统保留空间、隐藏文件以及未挂载的分区。使用 `lsblk` 命令可以全面查看所有块设备的状态。
</Callout>

"Where is the rest of the storage?" Bob wonders. He knows his laptop has 128 GB of SSD, but `df` only shows about 50 GB total. Could there be partitions that are not mounted? Could some space be reserved by the system? To get to the bottom of the issue, he decides to consult with Dave. After grabbing a much-needed coffee, Bob heads to the cafeteria.

"剩下的存储空间去哪了？"Bob 心里嘀咕着。他清楚地知道自己的笔记本有 128 GB 的 SSD，但 `df` 命令显示的总量却只有约 50 GB。难道有些分区没有被挂载？还是说有部分空间被系统保留了？为了弄清原委，他决定去找 Dave 请教。喝了一杯急需的咖啡后，Bob 向餐厅走去。

---

## A Fortuitous Meeting
## 一次偶遇

In the cafeteria, Bob notices Dave engaged in a deep conversation with another colleague. Dave spots Bob and calls him over.

在餐厅里，Bob 看到 Dave 正在和另一位同事深入交谈。Dave 发现了 Bob，立刻招手叫他过来。

"Morning, Bob! Come on over!" Dave greets warmly.

"早上好，Bob！过来吧！"Dave 热情地打招呼。

Dave introduces Bob to **Mohanarajan Menavanan**, the resident storage administrator. "You can call me Mohan," he says with a friendly smile.

Dave 将 Bob 介绍给公司的存储管理员 **Mohanarajan Menavanan**。"叫我 Mohan 就好，"他友善地微笑着说道。

Dave explains, "Mohan was just telling me that we are running out of space on our production SAN appliance." He continues, "This is Bob. He is our new front-end developer who joined the Project Mercury team two weeks ago."

Dave 解释道："Mohan 刚刚告诉我，我们的生产 SAN 设备快要没有空间了。"他接着说，"这位是 Bob，他两周前加入了 Mercury 项目团队，是我们的新前端开发工程师。"

After a brief exchange of pleasantries, Bob remarks, "Dave has been terrific. He helped me break into the world of Linux, and after overcoming some initial hurdles, I'm now fairly comfortable with this environment."

简短寒暄后，Bob 说道："Dave 真的非常棒，帮助我踏入了 Linux 的世界。克服了最初的一些障碍之后，我现在对这个环境已经比较得心应手了。"

Mohan smiles, adding, "That's fantastic. I had a similar experience when I joined the firm eight years ago. Dave was a great help back then when we were working on Fedora 16 and RHEL 5. Things have changed so much since then — we've gone from spinning rust to NVMe SSDs, from physical servers to containerized workloads."

Mohan 微笑着回应："太棒了。我八年前刚加入公司时也有类似的经历。那时候我们还在用 Fedora 16 和 RHEL 5，Dave 帮了我很大的忙。自那以后，变化太大了——从机械硬盘到 NVMe 固态硬盘，从物理服务器到容器化工作负载，一切都大不相同了。"

Bob then says, "I think either of you can help me understand a storage problem I've been encountering on my laptop. My `df` output shows only 50 GB, but I have a 128 GB SSD."

Bob 说道："我想你们两位都能帮我解答一个在笔记本上遇到的存储问题。我的 `df` 输出只显示了 50 GB，但我有一块 128 GB 的 SSD。"

Just then, Dave glances at his phone and says, "Sorry, Bob, I have to join an escalation call in five minutes. In fact, I need to leave right now." He adds, "Mohan, do you mind taking a look at Bob's question? He's a learning machine!" With a laugh, Dave continues, "And if you have some time, perhaps you could arrange a storage session with him."

就在这时，Dave 瞥了一眼手机，说道："抱歉，Bob，我五分钟后要参加一个紧急电话会议，现在就得走了。"他补充说，"Mohan，你介意帮 Bob 看看这个问题吗？他是个学习机器！"Dave 大笑着继续说，"如果你有时间，也许可以为他安排一个存储专题讲解。"

<Callout icon="lightbulb" color="#1CB2FE">
  Mohan reassures Dave, "No problem. I'm free pretty much every day this week. Just find a suitable slot and send me a meeting invite."

  Mohan 安慰 Dave 说："没问题，这周我几乎每天都有空。找个合适的时间把会议邀请发给我就行。"
</Callout>

Bob beams, "That would be great. Thank you both!"

Bob 高兴地说："那太好了，谢谢你们两位！"

---

## Delving Into the Issue
## 深入排查问题

Later that afternoon, Bob sets up a session with Mohan. On his way to the meeting room, he passes a crowded conference room where Dave, Donald, and several unfamiliar colleagues are engaged in an animated discussion about production incidents.

当天下午晚些时候，Bob 与 Mohan 约好了一个讨论时间。前往会议室的路上，他路过一间拥挤的会议室，里面 Dave、Donald 以及几位不认识的同事正在热烈地讨论生产环境中的紧急事故。

Below is a sample of Bob's investigation on his laptop. He uses `lsblk` to reveal the true layout of the disk — and discovers that a large portion of the SSD was never partitioned or mounted:

以下是 Bob 在笔记本上进行排查的一些操作记录。他使用 `lsblk` 命令揭开了磁盘的真实布局，发现 SSD 的一大部分从未被分区或挂载：

```bash
# 查看所有块设备 / List all block devices
[bob@laptop ~]$ lsblk
NAME   MAJ:MIN RM   SIZE RO TYPE MOUNTPOINT
sda      8:0    0 128.0G  0 disk
├─sda1   8:1    0  50.0G  0 part /
├─sda2   8:2    0   8.0G  0 part [SWAP]
└─sda3   8:3    0  70.0G  0 part          # <-- 未挂载！/ Not mounted!
```

The mystery is solved: `sda3` — a 70 GB partition — exists on the drive but has never been mounted into the filesystem. It is simply invisible to `df` because `df` only reports on **mounted** filesystems.

谜底揭开了：磁盘上存在一个 70 GB 的分区 `sda3`，但它从未被挂载到文件系统中。由于 `df` 命令只显示**已挂载**的文件系统，这个分区就这样"隐身"了。

```bash
# 尝试下载时遇到的原始错误 / The original error Bob encountered
[bob@laptop ~]$ curl -s -o /dev/null -w "%{time_starttransfer} %{time_total} %{size_download} %{speed_download}" \
  https://download.example.com/python/python-3.9.12-linux-64bit.tar.xz
2.257 2.304 78854 34230

[bob@laptop ~]$ # 下载后写入 /home 时空间不足 / Ran out of space writing to /home
curl: (23) Failed writing body (0 != 16384)
```

A note written in permanent marker below the terminal in the shared lab reads: **"Work at Risk Meeting. Booked 9–5 until July 10th."** Bob muses, "Ah, so that's the meaning of a war room," and feels relieved that he is not part of that session.

共用实验室的终端屏幕下方用记号笔写着：**"高风险工作会议，7月10日前每天9点到5点占用。"** Bob 若有所思："啊，原来这就是'作战室'的含义，"他庆幸自己不需要参与其中。

With determination, Bob proceeds to meet Mohan in the adjacent room, notebook in hand, ready to dive deeper into solving his storage mystery — and perhaps, to finally understand how to make the most of every gigabyte on his drive.

带着解决问题的决心，Bob 拿着笔记本走进了隔壁会议室，准备与 Mohan 深入探讨他的存储谜题——也许，他终将学会如何充分利用硬盘上的每一个 GB。

---

<CardGroup>
  <Card title="Watch Video" icon="video" cta="Learn more" href="https://learn.kodekloud.com/user/courses/learning-linux-basics-course-labs/module/e0021af2-9983-4bde-97a2-29255d3ea1da/lesson/afb817a0-c2a7-4c4c-847a-8d6915737d04" />
</CardGroup>
