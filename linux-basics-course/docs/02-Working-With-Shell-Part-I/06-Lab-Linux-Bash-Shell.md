# Lab - Linux Bash Shell
# 实验 - Linux Bash Shell

- Access Hands-On Labs here [Hands-On Labs](https://kodekloud.com/topic/lab-linux-bash-prompt/)

This lab reinforces the Bash shell concepts covered in the previous section. Each exercise includes the answer command, a detailed explanation, and additional context to deepen your understanding.

本实验巩固上一节介绍的 Bash Shell 概念。每道题都包含答案命令、详细解释以及帮助你深入理解的扩展内容。

---

### Exercise 1 — Check the Default Shell
### 练习 1 — 检查默认 Shell

Display the shell configured for the current user. This shows the **default login shell**, not necessarily the shell currently running.

显示当前用户配置的 Shell。这显示的是**默认登录 Shell**，不一定是当前正在运行的 Shell。

```bash
$ echo $SHELL
/bin/bash
```

**Explanation / 说明**: `$SHELL` is an environment variable that stores the path to the user's default shell. It is set when the user account is created and can be changed with `chsh`.

`$SHELL` 是存储用户默认 Shell 路径的环境变量。它在创建用户账号时设置，可通过 `chsh` 更改。

> **To see the currently running shell / 查看当前正在运行的 Shell:**
> ```bash
> $ echo $0
> bash
> # or / 或者
> $ ps -p $$
>   PID TTY          TIME CMD
>  1234 pts/0    00:00:00 bash
> ```

---

### Exercise 2 — Change a User's Shell
### 练习 2 — 更改用户的 Shell

Change the shell for user `bob` from **Bash** to **Bourne Shell (`sh`)**.

将用户 `bob` 的 Shell 从 **Bash** 更改为 **Bourne Shell（`sh`）**。

```bash
$ chsh -s /bin/sh bob
```

**Explanation / 说明:**
- `chsh` — "change shell" command / 更改 Shell 的命令
- `-s /bin/sh` — specifies the new shell path / 指定新 Shell 的路径
- `bob` — the target username (requires root/sudo if changing another user's shell) / 目标用户名（更改他人 Shell 时需要 root/sudo 权限）

**Verify the change / 验证更改:**
```bash
$ grep bob /etc/passwd | cut -d ":" -f7
/bin/sh
```

> **Note / 注意**: The change takes effect on the **next login**, not immediately. The user must log out and log back in.
>
> 更改在**下次登录时**生效，不会立即应用。用户必须注销后重新登录。

> **List available shells / 列出可用的 Shell:**
> ```bash
> $ cat /etc/shells
> /bin/sh
> /bin/bash
> /usr/bin/bash
> /bin/zsh
> /usr/bin/zsh
> ```

---

### Exercise 3 — Check an Environment Variable
### 练习 3 — 检查环境变量

Check the value of the `TERM` environment variable.

检查 `TERM` 环境变量的值。

```bash
$ echo $TERM
xterm-256color
```

**Explanation / 说明**: The `$TERM` variable tells programs what type of terminal is being used, so they can correctly render output (colors, cursor movement, etc.). Common values include:

`$TERM` 变量告诉程序正在使用什么类型的终端，以便正确渲染输出（颜色、光标移动等）。常见值包括：

| Value / 值 | Meaning / 含义 |
|---|---|
| `xterm` | Basic X terminal emulator / 基本 X 终端模拟器 |
| `xterm-256color` | X terminal with 256-color support / 支持 256 色的 X 终端 |
| `vt100` | VT100 terminal (basic) / VT100 终端（基础） |
| `screen` | GNU Screen session / GNU Screen 会话 |
| `tmux-256color` | tmux with 256-color support / 支持 256 色的 tmux |

---

### Exercise 4 — Create a Persistent Environment Variable
### 练习 4 — 创建持久化环境变量

Create a new environment variable `PROJECT=MERCURY` and make it **persistent** by adding it to `~/.profile`.

创建新环境变量 `PROJECT=MERCURY`，并通过将其添加到 `~/.profile` 使其**持久化**。

```bash
$ echo export PROJECT=MERCURY >> ~/.profile
```

**Explanation / 说明:**
- `echo export PROJECT=MERCURY` — generates the text to be written / 生成要写入的文本
- `>>` — **appends** to the file (does NOT overwrite) / **追加**到文件（不覆盖）
- `~/.profile` — startup file loaded for login shells / 登录 Shell 的启动文件

**Apply immediately in the current session / 立即在当前会话中应用:**
```bash
$ source ~/.profile
# or / 或者
$ . ~/.profile
```

**Verify / 验证:**
```bash
$ echo $PROJECT
MERCURY
$ env | grep PROJECT
PROJECT=MERCURY
```

> **WARNING — `>` vs `>>` / 警告 — `>` vs `>>`**:
> - `>` **overwrites** the entire file — this would destroy your `~/.profile`! / `>` **覆盖**整个文件——这会销毁你的 `~/.profile`！
> - `>>` **appends** to the file — always use this when adding to config files / `>>` **追加**到文件——向配置文件添加内容时始终使用此符号
>
> 永远不要用 `>` 向重要的配置文件写入，否则会清空文件内容！

---

### Exercise 5 — Identify a Directory Not in PATH
### 练习 5 — 找出不在 PATH 中的目录

Which of the following directories is NOT part of the `PATH` variable?

以下哪个目录不在 `PATH` 变量中？

**Answer / 答案**: `/opt/caleston-code`

**Check / 验证:**
```bash
$ echo $PATH
/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin

# Confirm /opt/caleston-code is absent / 确认 /opt/caleston-code 不在其中
$ echo $PATH | grep -o 'caleston'
# (no output) / （无输出）
```

> **How the shell uses PATH / Shell 如何使用 PATH**: When you type a command like `python3`, the shell searches each directory in `$PATH` from left to right until it finds an executable with that name. If none is found, you get "command not found".
>
> 当你输入 `python3` 这样的命令时，Shell 从左到右搜索 `$PATH` 中的每个目录，直到找到同名的可执行文件。如果找不到，则显示"command not found"。

---

### Exercise 6 — Create a Persistent Alias
### 练习 6 — 创建持久化别名

Set an alias `up` for the command `uptime` and make it **persistent** by adding it to `~/.profile`.

为 `uptime` 命令设置别名 `up`，并通过添加到 `~/.profile` 使其**持久化**。

```bash
$ echo alias up=uptime >> ~/.profile
```

**Apply and test / 应用并测试:**
```bash
$ source ~/.profile
$ up
 10:30:15 up 2 days,  3:15,  1 user,  load average: 0.00, 0.01, 0.05
```

> **Why single quotes matter for aliases / 别名中单引号的重要性**: For simple aliases without spaces in the value (like `up=uptime`), quotes are optional. But for complex aliases, always use single quotes:
> ```bash
> $ echo "alias ll='ls -la'" >> ~/.profile
> ```
> 对于简单的别名（值中没有空格，如 `up=uptime`），引号是可选的。但对于复杂的别名，始终使用单引号。

> **Aliases vs Functions / 别名 vs 函数**: Aliases are simple command substitutions. For more complex reusable commands (with arguments, conditionals), use shell **functions** instead:
> ```bash
> # Function that accepts arguments / 接受参数的函数
> mkcd() {
>     mkdir -p "$1" && cd "$1"
> }
> ```

---

### Exercise 7 — Customize the Bash Prompt
### 练习 7 — 自定义 Bash 提示符

Update Bob's prompt to display the date, username, hostname, and working directory in the format:

将 Bob 的提示符更新为按以下格式显示日期、用户名、主机名和工作目录：

**Target format / 目标格式**: `[Wed Apr 22]bob@caleston-lp10:~$`

```bash
# Set the prompt for the current session / 为当前会话设置提示符
$ PS1='[\d]\u@\h:\w\$'

# Make it persistent / 使其持久化
$ echo 'PS1=[\d]\u@\h:\w\$' >> ~/.profile
```

**Explanation of each escape code / 每个转义代码的说明:**

| Code / 代码 | Output / 输出 | Meaning / 含义 |
|---|---|---|
| `\d` | `Wed Apr 22` | Day Month Date / 星期 月 日 |
| `\u` | `bob` | Current username / 当前用户名 |
| `\h` | `caleston-lp10` | Hostname (short) / 主机名（短格式） |
| `\w` | `~` or `/home/bob/docs` | Current directory (full path) / 当前目录（完整路径） |
| `\$` | `$` (or `#` for root) | Prompt character / 提示符字符 |

> **Why use single quotes in `echo '...' >> ~/.profile` / 为什么在 `echo '...' >> ~/.profile` 中使用单引号**: Single quotes prevent the shell from interpreting `\d`, `\u`, etc. as escape sequences *at the time of writing*. If you used double quotes, the shell would try to expand them immediately and they wouldn't work correctly in the prompt later.
>
> 单引号防止 Shell 在**写入时**将 `\d`、`\u` 等解释为转义序列。如果使用双引号，Shell 会立即尝试展开它们，导致提示符之后无法正常工作。

**Apply and verify / 应用并验证:**
```bash
$ source ~/.profile
[Wed Mar 28]bob@caleston-lp10:~$
```

---

## Summary Table
## 汇总表

| Exercise / 练习 | Task / 任务 | Key Command / 关键命令 |
|---|---|---|
| 1 | Check default shell / 检查默认 Shell | `echo $SHELL` |
| 2 | Change user's shell / 更改用户 Shell | `chsh -s /bin/sh bob` |
| 3 | Check TERM variable / 检查 TERM 变量 | `echo $TERM` |
| 4 | Create persistent env var / 创建持久化环境变量 | `echo export PROJECT=MERCURY >> ~/.profile` |
| 5 | Find directory not in PATH / 找出不在 PATH 中的目录 | `echo $PATH` |
| 6 | Create persistent alias / 创建持久化别名 | `echo alias up=uptime >> ~/.profile` |
| 7 | Customize Bash prompt / 自定义 Bash 提示符 | `echo 'PS1=[\d]\u@\h:\w\$' >> ~/.profile` |

**Common pattern for persistence / 持久化的通用模式:**
```bash
# 1. Test the change in the current session / 在当前会话中测试更改
$ export MY_VAR=value
$ alias myalias='command'
$ PS1="my prompt> "

# 2. Once satisfied, make it permanent / 满意后，使其永久生效
$ echo 'export MY_VAR=value' >> ~/.bashrc
$ echo "alias myalias='command'" >> ~/.bashrc
$ echo 'PS1="my prompt> "' >> ~/.bashrc

# 3. Reload / 重新加载
$ source ~/.bashrc
```
