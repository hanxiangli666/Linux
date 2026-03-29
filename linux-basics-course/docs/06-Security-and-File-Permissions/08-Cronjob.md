# Cronjob in Linux
# Linux 中的定时任务（Cron）

> In this section we will learn about **Cron jobs** in Linux — the standard mechanism for scheduling commands to run automatically at specified times or intervals. From system backups to log rotation, cron is the backbone of Linux automation.
>
> 本节我们将学习 Linux 中的 **Cron 定时任务**——在指定时间或间隔自动运行命令的标准机制。从系统备份到日志轮转，cron 是 Linux 自动化的骨干。

---

## What is Cron?
## 什么是 Cron？

**Cron** is a time-based job scheduler in Unix-like operating systems. The **cron daemon** (`crond`) runs continuously in the background, waking up every minute to check whether any scheduled jobs need to be executed.

**Cron** 是 Unix 类操作系统中基于时间的作业调度器。**cron 守护进程**（`crond`）在后台持续运行，每分钟唤醒一次检查是否有需要执行的定时任务。

**Crontab** (cron table) is:
**Crontab**（cron 表）是：

1. The **file** where cron jobs are defined for each user — 定义每个用户 cron 任务的**文件**
2. The **command** used to create, edit, list, and remove those job definitions — 用于创建、编辑、列出和删除这些任务定义的**命令**

---

## Crontab File Format
## Crontab 文件格式

Each line in a crontab file represents one scheduled job. The format is:

crontab 文件中的每一行代表一个定时任务。格式为：

```
MIN  HOUR  DOM  MON  DOW  COMMAND
 *    *     *    *    *   /path/to/command arg1 arg2
```

| Field 字段 | Name 名称 | Allowed Values 允许值 |
| --- | --- | --- |
| `MIN` | Minute 分钟 | 0–59 |
| `HOUR` | Hour 小时 | 0–23 |
| `DOM` | Day of Month 月份中的第几天 | 1–31 |
| `MON` | Month 月份 | 1–12 (or jan, feb, …) |
| `DOW` | Day of Week 星期几 | 0–7 (0 and 7 = Sunday; or sun, mon, …) |
| `COMMAND` | Command to execute 要执行的命令 | Any shell command 任何 Shell 命令 |

---

## Special Characters in Crontab
## Crontab 中的特殊字符

| Character 字符 | Meaning 含义 | Example 示例 |
| --- | --- | --- |
| `*` | Any value (wildcard) 任意值（通配符） | `* * * * *` = every minute 每分钟 |
| `,` | Value list separator 值列表分隔符 | `1,15,30` = at 1st, 15th, 30th 在第1、15、30时 |
| `-` | Range of values 值范围 | `9-17` = from 9 to 17 从9到17 |
| `/` | Step values 步进值 | `*/15` = every 15 units 每15单位 |
| `@reboot` | Special: run once at startup 特殊：启动时运行一次 | — |
| `@daily` | Special: run once per day at midnight 特殊：每天午夜运行一次 | = `0 0 * * *` |
| `@weekly` | Special: run once per week 特殊：每周运行一次 | = `0 0 * * 0` |
| `@monthly` | Special: run once per month 特殊：每月运行一次 | = `0 0 1 * *` |
| `@yearly` | Special: run once per year 特殊：每年运行一次 | = `0 0 1 1 *` |

---

## Common Crontab Examples
## 常用 Crontab 示例

```
# Every 30 minutes 每30分钟
*/30 * * * *    /usr/local/bin/sync-data.sh

# Every hour (at minute 0) 每小时（第0分钟）
0 * * * *       /usr/local/bin/check-health.sh

# Every day at 2:30 AM 每天凌晨2:30
30 2 * * *      /usr/local/bin/backup.sh

# Every Sunday at midnight 每周日午夜
0 0 * * 0       /usr/local/bin/weekly-report.sh
# or: 0 0 * * sun

# Every Monday to Friday at 9:00 AM 每周一至周五上午9:00
0 9 * * 1-5     /usr/local/bin/workday-start.sh

# Every 15th of the month at 6:00 AM 每月15日上午6:00
0 6 15 * *      /usr/local/bin/monthly-cleanup.sh

# Every January 1st at midnight 每年1月1日午夜
0 0 1 1 *       /usr/local/bin/new-year-report.sh

# At multiple specific hours (8am, 12pm, 6pm) 在多个特定时间（早8点、中午12点、下午6点）
0 8,12,18 * * * /usr/local/bin/check-service.sh

# Every 5 minutes during business hours (9am-5pm, weekdays)
# 工作日工作时间（9am-5pm）每5分钟
*/5 9-17 * * 1-5 /usr/local/bin/monitor.sh

# At system reboot 系统重启时
@reboot         /usr/local/bin/startup-tasks.sh
```

---

## Crontab Commands
## Crontab 命令

```bash
# Edit the current user's crontab (opens in default editor)
# 编辑当前用户的 crontab（在默认编辑器中打开）
crontab -e

# List the current user's crontab entries
# 列出当前用户的 crontab 条目
crontab -l

# Remove the current user's crontab entirely
# 完全删除当前用户的 crontab
crontab -r

# Edit another user's crontab (requires root)
# 编辑另一个用户的 crontab（需要 root 权限）
sudo crontab -u bob -e

# List another user's crontab 列出另一个用户的 crontab
sudo crontab -u bob -l
```

> **Warning 警告:** `crontab -r` removes ALL your cron jobs without confirmation. If you meant to edit, use `crontab -e` instead. Some admins alias `crontab` to add a confirmation prompt.
>
> **警告：** `crontab -r` 会不经确认地删除你的所有 cron 任务。如果你想要编辑，请改用 `crontab -e`。一些管理员会为 `crontab` 设置别名以添加确认提示。

---

## System-Wide Crontabs
## 系统级 Crontab

In addition to per-user crontabs, the system has its own cron directories:

除了每个用户的 crontab 外，系统还有自己的 cron 目录：

```bash
/etc/cron.d/          # Custom cron files placed here by packages 包放置的自定义 cron 文件
/etc/cron.daily/      # Scripts run daily 每天运行的脚本
/etc/cron.hourly/     # Scripts run hourly 每小时运行的脚本
/etc/cron.weekly/     # Scripts run weekly 每周运行的脚本
/etc/cron.monthly/    # Scripts run monthly 每月运行的脚本
/etc/crontab          # System-wide crontab (includes username field) 系统级 crontab（包含用户名字段）
```

The system-wide `/etc/crontab` has an extra field for the username:
系统级 `/etc/crontab` 有一个额外的用户名字段：

```
# MIN  HOUR  DOM  MON  DOW  USER    COMMAND
  17   *     *    *    *    root    cd / && run-parts --report /etc/cron.hourly
  25   6     *    *    *    root    test -x /usr/sbin/anacron || run-parts --report /etc/cron.daily
  47   6     *    *    7    root    test -x /usr/sbin/anacron || run-parts --report /etc/cron.weekly
```

---

## Handling Cron Output
## 处理 Cron 输出

By default, cron emails the output of jobs to the user. To suppress this or redirect output:

默认情况下，cron 会将任务输出发送邮件给用户。要抑制此功能或重定向输出：

```bash
# Suppress all output (both stdout and stderr)
# 抑制所有输出（stdout 和 stderr）
*/30 * * * *  /usr/local/bin/sync.sh > /dev/null 2>&1

# Log output to a file (overwrite each time)
# 将输出记录到文件（每次覆盖）
0 2 * * *     /usr/local/bin/backup.sh > /var/log/backup.log 2>&1

# Log output to a file (append each time)
# 将输出记录到文件（每次追加）
0 2 * * *     /usr/local/bin/backup.sh >> /var/log/backup.log 2>&1

# Log with timestamp 带时间戳记录
0 2 * * *     echo "$(date): Starting backup" >> /var/log/backup.log && /usr/local/bin/backup.sh >> /var/log/backup.log 2>&1
```

---

## Environment Variables in Cron
## Cron 中的环境变量

Cron runs in a **minimal environment** — it does NOT load your `~/.bashrc` or `~/.bash_profile`. This is a common source of "works in terminal, fails in cron" bugs.

Cron 在**最小化环境**中运行——它不加载你的 `~/.bashrc` 或 `~/.bash_profile`。这是"在终端中有效，在 cron 中失败"这类问题的常见原因。

```bash
# Set environment variables in your crontab
# 在 crontab 中设置环境变量
SHELL=/bin/bash
PATH=/usr/local/sbin:/usr/local/bin:/sbin:/bin:/usr/sbin:/usr/bin
MAILTO=admin@example.com

# Your jobs below 以下是你的任务
0 2 * * * /usr/local/bin/backup.sh
```

> **Best Practice 最佳实践:** Always use **absolute paths** for both commands and files in cron jobs, since the PATH may differ from your interactive shell.
>
> **最佳实践：** 在 cron 任务中始终使用命令和文件的**绝对路径**，因为 PATH 可能与你的交互式 Shell 不同。

---

## Cron Access Control
## Cron 访问控制

You can control which users are allowed to use cron:

你可以控制允许哪些用户使用 cron：

```bash
/etc/cron.allow   # If this file exists, only users listed here can use crontab
                  # 如果此文件存在，只有此处列出的用户才能使用 crontab

/etc/cron.deny    # Users listed here are denied crontab access
                  # 此处列出的用户被拒绝使用 crontab
```

If neither file exists, only root can use cron (on some distros, all users can).
如果两个文件都不存在，则只有 root 可以使用 cron（在某些发行版上，所有用户都可以）。

---

## Troubleshooting Cron Jobs
## 排查 Cron 任务问题

```bash
# Check cron daemon status 检查 cron 守护进程状态
sudo systemctl status cron      # Ubuntu/Debian
sudo systemctl status crond     # RHEL/CentOS

# Check cron logs 检查 cron 日志
sudo tail -f /var/log/syslog | grep CRON      # Ubuntu/Debian
sudo tail -f /var/log/cron                    # RHEL/CentOS

# Test your command manually first! 首先手动测试你的命令！
/usr/local/bin/backup.sh

# Common issues checklist 常见问题检查清单:
# 1. Script not executable? 脚本不可执行？
chmod +x /usr/local/bin/backup.sh

# 2. Wrong PATH? 错误的 PATH？
which my-command  # Find the full path 找到完整路径

# 3. Environment differences? 环境差异？
# Run: env > /tmp/cron-env.txt in cron to see its environment
# 在 cron 中运行：env > /tmp/cron-env.txt 查看其环境

# 4. Check cron syntax with an online validator
# 使用在线验证器检查 cron 语法
# e.g., crontab.guru
```

---

## Quick Reference: Common Patterns
## 快速参考：常用模式

| Schedule 计划 | Cron Expression | Meaning 含义 |
| --- | --- | --- |
| Every minute 每分钟 | `* * * * *` | Runs every 60 seconds 每60秒运行 |
| Every 5 minutes 每5分钟 | `*/5 * * * *` | |
| Every 30 minutes 每30分钟 | `*/30 * * * *` | |
| Every hour 每小时 | `0 * * * *` | At minute 0 of every hour 每小时第0分钟 |
| Every 6 hours 每6小时 | `0 */6 * * *` | At 00:00, 06:00, 12:00, 18:00 |
| Every day at midnight 每天午夜 | `0 0 * * *` or `@daily` | |
| Every day at 2:30 AM 每天凌晨2:30 | `30 2 * * *` | |
| Every Sunday 每周日 | `0 0 * * 0` or `@weekly` | |
| Every weekday 每个工作日 | `0 9 * * 1-5` | Monday–Friday at 9 AM |
| First of month 每月第一天 | `0 0 1 * *` or `@monthly` | |
| Once a year 每年一次 | `0 0 1 1 *` or `@yearly` | January 1st |
| On reboot 重启时 | `@reboot` | Runs once when system starts |

> **Tip 提示:** Use the online tool **[crontab.guru](https://crontab.guru)** to visually verify your cron expressions before deploying them.
>
> **提示：** 在部署之前，使用在线工具 **[crontab.guru](https://crontab.guru)** 直观地验证你的 cron 表达式。
