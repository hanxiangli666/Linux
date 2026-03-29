# Vi Editor | Vi 编辑器

In this section, we will explore the Vi editor — the most universally available and widely used text editor in the Linux world. While modern graphical editors like VS Code are popular for development, Vi (and its enhanced version Vim) remains indispensable for system administrators who frequently work in terminal-only environments without a GUI.

本节将深入探讨 Vi 编辑器——Linux 世界中最广泛使用的文本编辑器。尽管 VS Code 等图形化编辑器在开发中很流行，但 Vi（及其增强版 Vim）对于经常在无图形界面的纯终端环境中工作的系统管理员来说，仍然是不可或缺的工具。

---

## Why Use a Terminal-Based Text Editor? | 为什么使用基于终端的文本编辑器？

The `cat` command can display file contents but is not suitable for editing, especially for large files or code. Terminal-based editors like Vi allow you to:

`cat` 命令可以显示文件内容，但不适合编辑，尤其是大型文件或代码。Vi 等基于终端的编辑器让你能够：

- Edit files directly on remote servers via SSH without transferring files.
- Make quick configuration changes without launching a GUI.
- Work in minimal Linux environments (Docker containers, recovery mode, embedded systems).

---

- 通过 SSH 直接在远程服务器上编辑文件，无需文件传输。
- 无需启动图形界面即可快速修改配置文件。
- 在精简的 Linux 环境中工作（Docker 容器、恢复模式、嵌入式系统）。

---

## Opening Vi | 打开 Vi

```bash
$ vi /home/michael/sample.txt
```

If the file exists, Vi opens it for editing. If it does not exist, Vi creates a new empty file with that name when you save.

如果文件存在，Vi 打开它进行编辑。如果不存在，保存时 Vi 会以该名称创建一个新的空文件。

```bash
# Open a new file
# 打开新文件
$ vi newfile.txt

# Open with cursor at a specific line number
# 从指定行号打开
$ vi +42 config.conf

# Open and search for a pattern
# 打开并搜索特定模式
$ vi +/error logfile.txt
```

---

## The Three Modes of Vi | Vi 的三种模式

Understanding Vi's modal design is the key to mastering it. Unlike most editors where you type and see text immediately, Vi uses distinct **modes** for different operations. This design makes experienced users extremely fast.

理解 Vi 的模式化设计是掌握它的关键。与大多数编辑器不同，Vi 使用不同的**模式**来进行不同的操作。这种设计让有经验的用户操作极为高效。

```
┌─────────────────────────────────────────────────────────┐
│                                                         │
│    ┌──────────────┐   i / I / o / O / a / A             │
│    │              │ ─────────────────────────────▶      │
│    │ COMMAND MODE │          ┌──────────────┐           │
│    │  (default)   │          │  INSERT MODE │           │
│    │              │ ◀─────── │              │           │
│    └──────┬───────┘   ESC    └──────────────┘           │
│           │                                             │
│           │  :                                          │
│           ▼                                             │
│    ┌──────────────┐                                     │
│    │  LAST LINE   │                                     │
│    │     MODE     │ ──── ESC ──▶ back to Command Mode   │
│    └──────────────┘                                     │
│                                                         │
└─────────────────────────────────────────────────────────┘
```

### Mode 1: Command Mode | 模式一：命令模式

Command Mode is the default mode when Vi opens a file. In this mode, every key press is interpreted as a **command** (move cursor, delete, copy, paste, search), not as text input.

命令模式是 Vi 打开文件时的默认模式。在此模式下，每次按键都被解释为**命令**（移动光标、删除、复制、粘贴、搜索），而不是文本输入。

**Navigation | 光标导航：**

| Key | Action | 说明 |
|-----|--------|------|
| `h` | Move left | 向左移动 |
| `l` | Move right | 向右移动 |
| `j` | Move down | 向下移动 |
| `k` | Move up | 向上移动 |
| `w` | Jump to next word | 跳到下一个单词 |
| `b` | Jump to previous word | 跳到上一个单词 |
| `0` | Jump to start of line | 跳到行首 |
| `$` | Jump to end of line | 跳到行尾 |
| `gg` | Go to first line of file | 跳到文件第一行 |
| `G` | Go to last line of file | 跳到文件最后一行 |
| `42G` | Go to line 42 | 跳到第 42 行 |
| `Ctrl+f` | Page down | 向下翻页 |
| `Ctrl+b` | Page up | 向上翻页 |

**Editing in Command Mode | 在命令模式中编辑：**

| Key | Action | 说明 |
|-----|--------|------|
| `x` | Delete character under cursor | 删除光标处的字符 |
| `dd` | Delete (cut) entire line | 删除（剪切）整行 |
| `5dd` | Delete 5 lines | 删除 5 行 |
| `yy` | Yank (copy) current line | 复制当前行 |
| `3yy` | Yank 3 lines | 复制 3 行 |
| `p` | Paste after cursor | 在光标后粘贴 |
| `P` | Paste before cursor | 在光标前粘贴 |
| `u` | Undo last change | 撤销上一次更改 |
| `Ctrl+r` | Redo | 重做 |
| `r` | Replace single character | 替换单个字符 |
| `.` | Repeat last command | 重复上一个命令 |

**Searching in Command Mode | 在命令模式中搜索：**

| Key | Action | 说明 |
|-----|--------|------|
| `/pattern` | Search forward for pattern | 向前搜索模式 |
| `?pattern` | Search backward for pattern | 向后搜索模式 |
| `n` | Jump to next match | 跳到下一个匹配 |
| `N` | Jump to previous match | 跳到上一个匹配 |

```
# Example: Search for "error" in the file
# 示例：在文件中搜索 "error"
/error
# Press n to find the next occurrence
# 按 n 查找下一个出现位置
```

---

### Mode 2: Insert Mode | 模式二：插入模式

Insert Mode is where you actually type text. The status bar at the bottom shows `-- INSERT --` to indicate you are in insert mode.

插入模式是你实际输入文本的模式。底部状态栏显示 `-- INSERT --` 表示当前处于插入模式。

**Entering Insert Mode from Command Mode | 从命令模式进入插入模式：**

| Key | Insert Position | 说明 |
|-----|----------------|------|
| `i` | Before cursor | 在光标前插入 |
| `I` | At the beginning of the line | 在行首插入 |
| `a` | After cursor | 在光标后追加 |
| `A` | At the end of the line | 在行尾追加 |
| `o` | Open a new line below | 在当前行下方新开一行 |
| `O` | Open a new line above | 在当前行上方新开一行 |

**Returning to Command Mode | 返回命令模式：**
```
Press ESC
```

> **Important | 重要提示：** Always press `ESC` after finishing your edits to return to Command Mode before issuing any commands or saving. Forgetting to press `ESC` is the most common Vi beginner mistake.
>
> 完成编辑后，务必按 `ESC` 返回命令模式，然后再执行任何命令或保存操作。忘记按 `ESC` 是 Vi 初学者最常见的错误。

---

### Mode 3: Last Line Mode | 模式三：末行模式

Last Line Mode is accessed by pressing `:` from Command Mode. The cursor jumps to the bottom of the screen where you can type extended commands — most importantly, save and quit operations.

末行模式通过在命令模式下按 `:` 键进入。光标跳到屏幕底部，在那里你可以输入扩展命令——最重要的是保存和退出操作。

**Save and Quit Commands | 保存和退出命令：**

| Command | Action | 说明 |
|---------|--------|------|
| `:w` | Save (write) the file | 保存（写入）文件 |
| `:w filename` | Save as a new filename | 另存为新文件名 |
| `:q` | Quit (only if no unsaved changes) | 退出（仅当无未保存更改时） |
| `:q!` | Force quit without saving | 强制退出，不保存 |
| `:wq` | Save and quit | 保存并退出 |
| `:wq!` | Force save and quit | 强制保存并退出 |
| `:x` | Save (only if changed) and quit | 保存（仅修改时）并退出 |
| `ZZ` | Same as `:wq` (in command mode) | 等同于 `:wq`（在命令模式下） |

**Other Useful Last Line Commands | 其他有用的末行命令：**

| Command | Action | 说明 |
|---------|--------|------|
| `:set nu` | Show line numbers | 显示行号 |
| `:set nonu` | Hide line numbers | 隐藏行号 |
| `:42` | Jump to line 42 | 跳到第 42 行 |
| `:%s/old/new/g` | Replace all occurrences of "old" with "new" | 全文将 "old" 替换为 "new" |
| `:%s/old/new/gc` | Replace with confirmation for each | 逐个确认替换 |
| `:1,10s/old/new/g` | Replace in lines 1 to 10 | 在第 1 到 10 行中替换 |
| `:syntax on` | Enable syntax highlighting | 启用语法高亮 |

---

## Vim — The Improved Vi | Vim——Vi 的增强版

**Vim** (Vi IMproved) is a significantly enhanced version of Vi. In most modern Linux distributions, the `vi` command is actually a symbolic link to `vim`.

**Vim**（Vi IMproved）是 Vi 的重大增强版本。在大多数现代 Linux 发行版中，`vi` 命令实际上是 `vim` 的符号链接。

```bash
$ which vi
/usr/bin/vi
$ ls -l /usr/bin/vi
lrwxrwxrwx 1 root root 20 ... /usr/bin/vi -> /etc/alternatives/vi
```

**Key improvements in Vim over Vi | Vim 相比 Vi 的主要改进：**

| Feature | Vi | Vim |
|---------|----|-----|
| Syntax highlighting | No | Yes |
| Multiple undo levels | Limited | Unlimited |
| Split windows | No | Yes |
| Plugin support | No | Yes |
| Mouse support | No | Yes (in terminal) |
| Visual mode (select text) | No | Yes |

**Visual Mode in Vim | Vim 中的可视模式：**

Vim adds a Visual Mode for selecting blocks of text:

Vim 新增了用于选择文本块的可视模式：

| Key | Action | 说明 |
|-----|--------|------|
| `v` | Character-wise visual mode | 字符级可视模式 |
| `V` | Line-wise visual mode | 行级可视模式 |
| `Ctrl+v` | Block visual mode | 块级可视模式 |

---

## Practical Example: Editing a Config File | 实践示例：编辑配置文件

Here is a step-by-step walkthrough of a typical Vi editing session — editing an Apache web server configuration:

以下是一个典型 Vi 编辑会话的逐步演练——编辑 Apache Web 服务器配置文件：

```bash
# 1. Open the file
#    打开文件
$ sudo vi /etc/apache2/ports.conf

# 2. Vi opens in Command Mode
#    Vi 以命令模式打开

# 3. Search for the Listen port directive
#    搜索 Listen 端口指令
/Listen

# 4. Press n to navigate to the relevant line
#    按 n 跳转到相关行

# 5. Press i to enter Insert Mode
#    按 i 进入插入模式

# 6. Edit the port number (e.g., change 80 to 8080)
#    编辑端口号（例如，将 80 改为 8080）

# 7. Press ESC to return to Command Mode
#    按 ESC 返回命令模式

# 8. Save and quit
#    保存并退出
:wq
```

---

## Common Beginner Mistakes | 常见初学者错误

| Mistake | Fix | 解决方法 |
|---------|-----|---------|
| Typing text in Command Mode | Press `i` first | 先按 `i` 进入插入模式 |
| Can't quit Vi | Press `ESC`, then `:q!` | 按 `ESC`，然后输入 `:q!` |
| Accidentally deleted content | Press `u` to undo | 按 `u` 撤销 |
| File saved as read-only | Use `:w!` or `sudo vi` | 使用 `:w!` 或 `sudo vi` |

> **The famous Vi joke | 著名的 Vi 笑话：** "How do you generate a random string? Put a new user in front of Vi."
>
> "如何生成一个随机字符串？让一个 Vi 新手坐到键盘前。"

This joke highlights that Vi's modal interface can feel confusing at first — but with practice, it becomes second nature. The reward is being able to edit any file on any Linux server, anywhere, instantly.

这个笑话说明 Vi 的模式化界面一开始会让人感到困惑——但经过练习，它会成为第二天性。回报是能够在任何地方立即编辑任何 Linux 服务器上的任何文件。
