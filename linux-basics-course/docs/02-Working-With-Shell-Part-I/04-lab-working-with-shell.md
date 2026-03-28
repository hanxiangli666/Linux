# Lab - Working with the Shell
# 实验 - Shell 基础操作

- Access Hands-On Labs here [Hands-On Labs](https://kodekloud.com/topic/lab-working-with-the-shell/)

This lab walks through common shell tasks using the concepts covered in the previous sections. Each exercise includes the command, an explanation of what it does, and notes on key concepts.

本实验通过前几节涉及的概念来完成常见的 Shell 操作。每道题都包含命令、操作说明以及关键概念注释。

---

### Exercise 1 — Check the Home Directory of a Specific User
### 练习 1 — 查看指定用户的家目录

**Method 1: Parse `/etc/passwd` / 方法一：解析 `/etc/passwd`**
```bash
$ grep bob /etc/passwd | cut -d ":" -f6
/home/bob
```

**Explanation / 说明:**
- `grep bob /etc/passwd` — finds the line for user `bob` in the passwd file / 在 passwd 文件中找到 `bob` 用户的行
- `cut -d ":" -f6` — splits the line by `:` delimiter and extracts field 6 (home directory) / 以 `:` 为分隔符，取第 6 个字段（家目录）

The format of `/etc/passwd` is: `username:password:UID:GID:comment:home_dir:shell`

`/etc/passwd` 的格式为：`用户名:密码:UID:GID:注释:家目录:Shell`

**Method 2: Use the built-in `$HOME` variable (for the current user) / 方法二：使用内置变量 `$HOME`（适用于当前用户）**
```bash
$ echo $HOME
/home/bob
```

> **Key concept / 关键概念**: `$HOME` is a shell environment variable that always points to the current user's home directory. It is equivalent to `~`.
>
> `$HOME` 是 Shell 环境变量，始终指向当前用户的家目录，等同于 `~`。

---

### Exercise 2 — Understand Command Arguments
### 练习 2 — 理解命令参数

**Question / 问题**: In the command `echo Welcome`, what does `Welcome` represent?

**Answer / 答案**: `Welcome` is an **argument** to the `echo` command.

```bash
$ echo Welcome
Welcome
```

**Explanation / 说明:**
- `echo` is the **command** / `echo` 是**命令**
- `Welcome` is the **argument** (the input passed to the command) / `Welcome` 是**参数**（传递给命令的输入）
- The command prints the argument to standard output / 命令将参数打印到标准输出

> **Reminder / 提示**: Arguments provide data for commands to work on. Options/flags (starting with `-`) modify *how* the command works, while arguments specify *what* it works on.
>
> 参数为命令提供操作数据。选项/标志（以 `-` 开头）修改命令*如何*工作，而参数指定命令*操作什么*。

---

### Exercise 3 — Check the Type of a Command
### 练习 3 — 检查命令类型

```bash
$ type git
git is /usr/bin/git
```

This tells us that `git` is an **external command** located at `/usr/bin/git`.

这表明 `git` 是一个**外部命令**，位于 `/usr/bin/git`。

**Compare with a built-in command / 与内置命令对比:**
```bash
$ type echo
echo is a shell builtin

$ type cd
cd is a shell builtin
```

> **Key concept / 关键概念**:
> - **Built-in commands** are part of the shell — faster, no file on disk / **内置命令**是 Shell 的一部分，速度更快，磁盘上没有对应文件
> - **External commands** are programs on disk, found via the `$PATH` variable / **外部命令**是磁盘上的程序，通过 `$PATH` 变量查找

---

### Exercise 4 — Create Directories
### 练习 4 — 创建目录

**Create a single directory / 创建单个目录:**
```bash
$ mkdir /home/bob/birds
```

**Create directories recursively / 递归创建目录:**
```bash
$ mkdir -p /home/bob/fish/salmon
```

**Why `-p` is needed / 为什么需要 `-p`**: If `/home/bob/fish` doesn't exist yet, `mkdir /home/bob/fish/salmon` would fail without `-p`. The `-p` flag creates all necessary parent directories.

如果 `/home/bob/fish` 还不存在，不加 `-p` 的话 `mkdir /home/bob/fish/salmon` 会失败。`-p` 标志会创建所有必要的父目录。

**Create multiple nested directories / 创建多个嵌套目录:**
```bash
$ mkdir -p /home/bob/mammals/elephant
$ mkdir -p /home/bob/mammals/monkey
$ mkdir /home/bob/birds/eagle
$ mkdir -p /home/bob/reptile/snake
$ mkdir -p /home/bob/reptile/frog
$ mkdir -p /home/bob/amphibian/salamander
```

**Verify the structure / 验证目录结构:**
```bash
$ ls -R /home/bob/
```

> **Pro tip / 专业技巧**: You can also create multiple sibling directories with brace expansion:
> ```bash
> $ mkdir -p /home/bob/mammals/{elephant,monkey}
> $ mkdir -p /home/bob/reptile/{snake,frog}
> ```
> 也可以使用花括号展开一次创建多个同级目录。

---

### Exercise 5 — Move a Directory
### 练习 5 — 移动目录

```bash
$ mv /home/bob/reptile/frog /home/bob/amphibian
```

**Explanation / 说明**: This moves the `frog` directory from `reptile/` into `amphibian/`. Frogs are amphibians, not reptiles — this corrects the classification.

这将 `frog` 目录从 `reptile/` 移动到 `amphibian/`。青蛙是两栖动物，不是爬行动物——此操作修正了分类错误。

**Verify / 验证:**
```bash
$ ls /home/bob/reptile/
snake

$ ls /home/bob/amphibian/
salamander  frog
```

---

### Exercise 6 — Rename a Directory
### 练习 6 — 重命名目录

```bash
$ mv /home/bob/reptile/snake /home/bob/reptile/crocodile
```

**Explanation / 说明**: `mv` with a source and destination **in the same parent directory** performs a rename. The directory `snake` is renamed to `crocodile`.

当 `mv` 的源和目标在**同一父目录**中时，执行重命名操作。目录 `snake` 被重命名为 `crocodile`。

---

### Exercise 7 — Delete a Directory
### 练习 7 — 删除目录

```bash
$ rm -r /home/bob/reptile
```

**Explanation / 说明:**
- `rm` alone cannot delete directories — it only works on files / 单独的 `rm` 无法删除目录，只对文件有效
- `-r` (recursive) flag is required to delete a directory and **all its contents** / 需要 `-r`（递归）标志来删除目录及其**所有内容**

**Verify deletion / 验证删除:**
```bash
$ ls /home/bob/
amphibian  birds  fish  mammals
```

> **Safety tip / 安全提示**: Before deleting, you can preview what will be deleted with `ls -R`:
> ```bash
> $ ls -R /home/bob/reptile
> /home/bob/reptile:
> crocodile
> ```
> Always confirm the path before running `rm -r`.
>
> 删除前，可以用 `ls -R` 预览将要删除的内容，始终在运行 `rm -r` 前确认路径。

---

## Final Directory Structure
## 最终目录结构

After all exercises, the directory structure under `/home/bob/` should look like:

完成所有练习后，`/home/bob/` 下的目录结构应如下所示：

```
/home/bob/
├── amphibian/
│   ├── frog/
│   └── salamander/
├── birds/
│   └── eagle/
├── fish/
│   └── salmon/
└── mammals/
    ├── elephant/
    └── monkey/
```

**Verify with a tree-like view / 用树形视图验证:**
```bash
# If 'tree' is installed / 如果安装了 tree 命令
$ tree /home/bob/

# Alternative using ls / 使用 ls 的替代方法
$ ls -R /home/bob/
```

---

## Key Takeaways
## 关键要点

| Task / 任务 | Command / 命令 |
|---|---|
| Check home directory of user `bob` | `grep bob /etc/passwd \| cut -d ":" -f6` |
| Check current user's home dir | `echo $HOME` |
| Identify command type | `type git` |
| Create single directory | `mkdir /path/to/dir` |
| Create nested directories | `mkdir -p /path/to/deep/dir` |
| Move a directory | `mv /source/dir /destination/` |
| Rename a directory | `mv oldname newname` |
| Delete a directory and contents | `rm -r /path/to/dir` |
