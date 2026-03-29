# Lab — Vi Editor | 实验：Vi 编辑器

This lab walks you through the essential Vi operations you'll use every day as a Linux user. Work through each task carefully, and pay attention to the mode you are currently in before executing any command.

本实验将带你练习作为 Linux 用户每天都会用到的基本 Vi 操作。仔细完成每道题，在执行任何命令之前注意当前所处的模式。

> **Before you begin | 开始之前：** Open a file in Vi to follow along:
> ```bash
> $ vi /home/bob/sample.txt
> ```
> Remember: Vi always opens in **Command Mode**.
>
> 记住：Vi 始终以**命令模式**打开。

---

## Task 1 — Enter Insert Mode | 任务 1：进入插入模式

**Requirement | 要求：**
Switch from Command Mode to Insert Mode so you can type text.

从命令模式切换到插入模式，以便可以输入文本。

**Solution | 解答：**
```
Press  i
```

**Explanation | 解析：**
When you press `i`, Vi enters **Insert Mode** and you will see `-- INSERT --` appear at the bottom of the screen. Now every key you press is treated as text input, not a command. There are multiple ways to enter Insert Mode — `i` inserts before the cursor, `a` appends after the cursor, `o` opens a new line below, and `O` opens a new line above. Each is useful in different contexts.

按下 `i` 后，Vi 进入**插入模式**，屏幕底部会出现 `-- INSERT --`。此时你按下的每个键都会被当作文本输入，而不是命令。进入插入模式有多种方式——`i` 在光标前插入，`a` 在光标后追加，`o` 在下方新开一行，`O` 在上方新开一行。每种方式在不同情境下各有用途。

| Key | Insert Location | 说明 |
|-----|----------------|------|
| `i` | Before cursor | 在光标前插入 |
| `I` | Beginning of line | 在行首插入 |
| `a` | After cursor | 在光标后追加 |
| `A` | End of line | 在行尾追加 |
| `o` | New line below | 在下方新开一行 |
| `O` | New line above | 在上方新开一行 |

---

## Task 2 — Return to Command Mode | 任务 2：返回命令模式

**Requirement | 要求：**
Exit from Insert Mode and go back to Command Mode.

退出插入模式，返回命令模式。

**Solution | 解答：**
```
Press  ESC
```

**Explanation | 解析：**
`ESC` is the universal key to exit any mode and return to Command Mode. This is arguably the most important key in Vi. Get into the habit of pressing `ESC` whenever you're done editing and before running any command. The `-- INSERT --` indicator at the bottom of the screen will disappear when you're back in Command Mode.

`ESC` 是退出任何模式并返回命令模式的通用键。这可以说是 Vi 中最重要的按键。养成在完成编辑后、执行任何命令前按 `ESC` 的习惯。返回命令模式后，屏幕底部的 `-- INSERT --` 指示符将消失。

> **Pro tip | 专业技巧：** If you're ever unsure which mode you're in, press `ESC` once or twice — it will always bring you back to Command Mode safely.
>
> 如果你不确定当前处于哪种模式，按一两下 `ESC`——它总能安全地将你带回命令模式。

---

## Task 3 — Delete a Character | 任务 3：删除字符

**Requirement | 要求：**
Remove specific characters from the text.

从文本中删除特定字符。

**Solution | 解答：**
```
# In Command Mode:
# 在命令模式下：
Move the cursor to the character you want to remove, then press  x
将光标移到要删除的字符上，然后按  x
```

**Explanation | 解析：**
The `x` command deletes the single character **under** the cursor (like a forward-delete). To delete multiple characters, you can:

`x` 命令删除光标**下方**的单个字符（类似前向删除）。要删除多个字符，可以：

```
# Delete 3 characters at once | 一次删除 3 个字符
3x

# Delete character to the LEFT of cursor (like Backspace)
# 删除光标左侧的字符（类似退格键）
X (uppercase X)
```

**Cursor navigation reminder | 光标导航提醒：**
```
h ← left   |   l → right
k ↑ up      |   j ↓ down
```

---

## Task 4 — Edit File Content and Save | 任务 4：编辑文件内容并保存

**Requirement | 要求：**
Change the file contents to `Welcome to KodeKloud` and save the file.

将文件内容修改为 `Welcome to KodeKloud` 并保存文件。

**Solution | 解答：**
```
# Step 1: Go to Insert Mode
# 第一步：进入插入模式
Press  i

# Step 2: Delete all existing content and type new content
# 第二步：删除所有现有内容并输入新内容
(Delete old text using Backspace or arrow keys + x, then type new text)
（使用退格键或箭头键 + x 删除旧文本，然后输入新文本）

Welcome to KodeKloud

# Step 3: Return to Command Mode
# 第三步：返回命令模式
Press  ESC

# Step 4: Enter Last Line Mode and save with force write
# 第四步：进入末行模式并强制保存
:w!
```

**More efficient approach with `ggdG` | 使用 `ggdG` 更高效的方式：**
```
# In Command Mode, delete all content first:
# 在命令模式下，先删除所有内容：
gg        # Jump to first line | 跳到第一行
dG        # Delete from current line to end of file | 从当前行删除到文件末尾

# Then enter Insert Mode and type fresh content
# 然后进入插入模式并输入新内容
i
Welcome to KodeKloud
ESC
:w!
```

**Explanation | 解析：**
`:w!` is the "force write" command — the `!` overrides write protection warnings. Use `:wq!` to save and quit in a single command. Without `!`, `:w` will fail if the file is read-only or has permission issues.

`:w!` 是"强制写入"命令——`!` 会覆盖写保护警告。使用 `:wq!` 可以一条命令完成保存并退出。如果没有 `!`，当文件只读或存在权限问题时，`:w` 会失败。

---

## Task 5 — Update a Port Number in a Config File | 任务 5：更新配置文件中的端口号

**Requirement | 要求：**
Update the Apache webserver configuration — change the port it listens on from `80` to `5000`.

更新 Apache Web 服务器配置——将监听端口从 `80` 改为 `5000`。

**Solution | 解答：**

**Method 1: Manual edit in Insert Mode | 方法一：在插入模式中手动编辑**
```
# Search for "80" in Command Mode
# 在命令模式中搜索 "80"
/80
Press Enter

# Navigate to the exact position and enter Insert Mode
# 导航到精确位置并进入插入模式
Press i
(delete "80" and type "5000")
（删除 "80" 并输入 "5000"）
Press ESC
:wq
```

**Method 2: Global search and replace (recommended) | 方法二：全局搜索和替换（推荐）**
```
# In Last Line Mode, use substitution command
# 在末行模式中，使用替换命令
:%s/80/5000/g
```

**Explanation | 解析：**
The substitution command `:%s/old/new/g` is one of Vi's most powerful features:

替换命令 `:%s/old/new/g` 是 Vi 最强大的功能之一：

- `%` means "apply to all lines in the file" (全文范围)
- `s` stands for substitute (替换)
- `/80/` is the pattern to find (要查找的模式)
- `/5000/` is the replacement text (替换文本)
- `g` means "global" — replace all occurrences on each line (全局，替换每行所有匹配)

To confirm each replacement interactively, append `c`: `:%s/80/5000/gc`

要逐个交互确认每次替换，追加 `c`：`:%s/80/5000/gc`

---

## Task 6 — Delete an Entire Line | 任务 6：删除整行

**Requirement | 要求：**
Remove the line that starts with `LogFormat` (line 33).

删除以 `LogFormat` 开头的那一行（第 33 行）。

**Solution | 解答：**
```
# Method 1: Navigate to line 33 using Last Line Mode
# 方法一：通过末行模式导航到第 33 行
:33
Press Enter

# Then delete the line with dd
# 然后用 dd 删除该行
dd

# Method 2: Navigate to line 33 in Command Mode
# 方法二：在命令模式中导航到第 33 行
33G
dd
```

**Explanation | 解析：**
`dd` is the command to **delete (cut) the entire current line**. The deleted line is stored in Vi's buffer, so you can paste it elsewhere with `p` (paste below) or `P` (paste above). To delete multiple lines, prefix `dd` with a number:

`dd` 命令用于**删除（剪切）整个当前行**。被删除的行存储在 Vi 的缓冲区中，因此可以用 `p`（在下方粘贴）或 `P`（在上方粘贴）将其粘贴到其他位置。要删除多行，在 `dd` 前加数字：

```
5dd    # Delete 5 lines starting from current line | 从当前行开始删除 5 行
d$     # Delete from cursor to end of line | 从光标删除到行尾
d0     # Delete from cursor to beginning of line | 从光标删除到行首
dgg    # Delete from current line to beginning of file | 从当前行删除到文件开头
dG     # Delete from current line to end of file | 从当前行删除到文件末尾
```

---

## Task 7 — Undo a Change | 任务 7：撤销更改

**Requirement | 要求：**
Undo the previous change (the deleted line).

撤销上一次更改（被删除的行）。

**Solution | 解答：**
```
# In Command Mode:
# 在命令模式下：
Press  u
```

**Explanation | 解析：**
`u` undoes the most recent change. In Vim (the enhanced version of Vi), you can press `u` multiple times to undo multiple steps. To redo (re-apply an undone change), press `Ctrl+r`.

`u` 撤销最近一次更改。在 Vim（Vi 的增强版本）中，可以多次按 `u` 来撤销多个步骤。要重做（重新应用已撤销的更改），按 `Ctrl+r`。

| Key | Action | 说明 |
|-----|--------|------|
| `u` | Undo last change | 撤销上一次更改 |
| `U` | Undo all changes on current line | 撤销当前行的所有更改 |
| `Ctrl+r` | Redo | 重做 |

---

## Task 8 — Search for a Pattern | 任务 8：搜索特定模式

**Requirement | 要求：**
Find the number `6` hidden somewhere in the file.

找到文件中某处隐藏的数字 `6`。

**Solution | 解答：**
```
# In Command Mode, use forward search:
# 在命令模式下，使用向前搜索：
/6
Press Enter
```

**Explanation | 解析：**
The `/` key in Command Mode activates forward search. After typing the pattern and pressing `Enter`, Vi jumps to the first match and highlights it. Use `n` to jump to the next match, and `N` to jump to the previous match.

命令模式下的 `/` 键激活向前搜索。输入模式并按 `Enter` 后，Vi 会跳到第一个匹配处并高亮显示。使用 `n` 跳到下一个匹配，`N` 跳到上一个匹配。

```
/6         # Search forward for "6" | 向前搜索 "6"
?6         # Search backward for "6" | 向后搜索 "6"
n          # Next match | 下一个匹配
N          # Previous match | 上一个匹配
```

To clear the search highlight after finding the pattern (in Vim):

找到模式后清除搜索高亮（在 Vim 中）：
```
:noh
```

---

## Lab Summary | 实验总结

| Task | Operation | Key(s) | Mode | 模式 |
|------|-----------|--------|------|------|
| 1 | Enter Insert Mode | `i` | Command → Insert | 命令 → 插入 |
| 2 | Return to Command Mode | `ESC` | Any → Command | 任意 → 命令 |
| 3 | Delete a character | `x` | Command | 命令模式 |
| 4 | Edit content and save | `i` + edit + `ESC` + `:w!` | Insert + Last Line | 插入 + 末行 |
| 5 | Replace text globally | `:%s/old/new/g` | Last Line | 末行模式 |
| 6 | Delete entire line | `dd` | Command | 命令模式 |
| 7 | Undo change | `u` | Command | 命令模式 |
| 8 | Search for pattern | `/pattern` | Command | 命令模式 |

> **Final tip | 最后提示：** The three commands every Vi user must memorize first are: `i` (insert), `ESC` (back to command), and `:wq` (save and quit). Everything else builds on top of these three.
>
> 每个 Vi 用户必须首先记住的三个命令是：`i`（插入）、`ESC`（返回命令模式）和 `:wq`（保存并退出）。其他所有操作都建立在这三个命令的基础之上。
