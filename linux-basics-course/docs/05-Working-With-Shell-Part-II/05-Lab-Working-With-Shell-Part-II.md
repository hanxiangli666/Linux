# Lab — Working With Shell Part II | 实验：Shell 进阶操作

This lab reinforces the concepts covered in File Compression & Archival, Searching for Files & Patterns, and IO Redirection. Work through each task and understand not just the commands, but *why* each step is performed.

本实验巩固文件压缩与归档、搜索文件与模式、IO 重定向等章节所学的内容。完成每道题时，不仅要知道命令怎么写，更要理解**为什么**这样做。

---

## Task 1 — Create a Compressed Tarball | 任务 1：创建压缩归档包

**Requirement | 要求：**
Create a tarball of the directory `/home/bob/reptile/snake/python` and compress it using gzip. The final compressed file should be saved at `/home/bob/python.tar.gz`.

将目录 `/home/bob/reptile/snake/python` 打包，并使用 gzip 压缩。最终的压缩文件应保存在 `/home/bob/python.tar.gz`。

**Solution | 解答：**
```bash
# Method 1: Two separate steps (create tarball, then compress)
# 方法一：分两步（先打包，再压缩）
$ tar -cf /home/bob/python.tar /home/bob/reptile/snake/python
$ gzip /home/bob/python.tar
# gzip will produce python.tar.gz automatically
# gzip 会自动生成 python.tar.gz

# Method 2: One step using tar's built-in gzip support (recommended)
# 方法二：一步完成，使用 tar 内置的 gzip 支持（推荐）
$ tar -zcf /home/bob/python.tar.gz /home/bob/reptile/snake/python
```

**Explanation | 解析：**
- `tar -cf` creates (`-c`) an archive with the specified filename (`-f`).
- `gzip` compresses the `.tar` file and renames it to `.tar.gz`.
- The `-z` flag in `tar` invokes gzip compression directly, which is more efficient and requires only a single command.

- `tar -cf` 使用指定文件名（`-f`）创建（`-c`）归档文件。
- `gzip` 压缩 `.tar` 文件并自动重命名为 `.tar.gz`。
- `tar` 的 `-z` 标志直接调用 gzip 压缩，更高效，只需一条命令。

---

## Task 2 — Extract a Compressed File | 任务 2：解压压缩文件

**Requirement | 要求：**
There is a compressed file `eaglet.dat.gz` located at `/home/bob/birds/eagle/`. Extract it in the same directory.

`/home/bob/birds/eagle/` 目录下有一个压缩文件 `eaglet.dat.gz`，请在原目录将其解压。

**Solution | 解答：**
```bash
$ gunzip /home/bob/birds/eagle/eaglet.dat.gz
# Extracts eaglet.dat.gz → eaglet.dat in the same directory
# 解压 eaglet.dat.gz → 在同目录生成 eaglet.dat
```

**Alternative using `gzip -d` | 使用 `gzip -d` 的替代方式：**
```bash
$ gzip -d /home/bob/birds/eagle/eaglet.dat.gz
```

**Explanation | 解析：**
`gunzip` is the decompression counterpart of `gzip`. By default, it removes the `.gz` file and leaves the decompressed file in its place. The original `.gz` file is deleted after extraction.

`gunzip` 是 `gzip` 的解压对应命令。默认情况下，它删除 `.gz` 文件，并在原位置留下解压后的文件。提取后，原 `.gz` 文件会被删除。

---

## Task 3 — Find a File by Name | 任务 3：按文件名查找文件

**Requirement | 要求：**
A file named `caleston-code` was copied to the laptop, but the exact directory is unknown. Find it.

一个名为 `caleston-code` 的文件已被复制到笔记本上，但具体目录不详。请找到它。

**Solution | 解答：**
```bash
$ sudo find / -name caleston-code
```

**Explanation | 解析：**
- `find /` searches the entire filesystem starting from root (`/`).
- `-name caleston-code` filters results to only files with that exact name.
- `sudo` is required because searching system directories (like `/etc`, `/var`, `/usr`) requires elevated privileges.

- `find /` 从根目录（`/`）开始搜索整个文件系统。
- `-name caleston-code` 过滤结果，只显示精确匹配该名称的文件。
- 需要 `sudo`，因为搜索系统目录（如 `/etc`、`/var`、`/usr`）需要提升的权限。

**Pro tip | 专业技巧：** To suppress "Permission denied" errors and only see results:

要屏蔽"Permission denied"错误，只显示搜索结果：
```bash
$ sudo find / -name caleston-code 2>/dev/null
```

---

## Task 4 — Find a File and Save its Path | 任务 4：查找文件并保存其路径

**Requirement | 要求：**
Find the location of `dummy.service` and save its absolute path to the file `/home/bob/dummy-service`.

找到 `dummy.service` 的位置，并将其绝对路径保存到文件 `/home/bob/dummy-service`。

**Solution | 解答：**
```bash
# Step 1: Locate the file
# 第一步：定位文件
$ sudo find / -name dummy.service
# Output: /etc/systemd/system/dummy.service
# 输出：/etc/systemd/system/dummy.service

# Step 2: Save the path to the target file using echo + redirect
# 第二步：使用 echo 加重定向将路径保存到目标文件
$ echo /etc/systemd/system/dummy.service > /home/bob/dummy-service

# Verify the result
# 验证结果
$ cat /home/bob/dummy-service
```

**Explanation | 解析：**
The `echo` command combined with `>` (overwrite redirect) writes a string directly to a file. This is a clean, reliable way to save a value to a file without opening an editor.

`echo` 命令结合 `>`（覆盖重定向）将字符串直接写入文件。这是一种无需打开编辑器即可将值保存到文件的简洁可靠方式。

---

## Task 5 — Find File Containing an IP Address | 任务 5：查找包含 IP 地址的文件

**Requirement | 要求：**
Find the file under `/etc` that contains the string `172.16.238.197` and save the result (with file path) to `/home/bob/ip`.

在 `/etc` 目录下查找包含字符串 `172.16.238.197` 的文件，并将结果（含文件路径）保存到 `/home/bob/ip`。

**Solution | 解答：**
```bash
$ sudo grep -ir 172.16.238.197 /etc/ > /home/bob/ip
```

**Explanation | 解析：**
- `-i` makes the search case-insensitive (useful for hex-based strings).
- `-r` enables recursive search through all files and subdirectories under `/etc/`.
- `>` redirects the matching output (file path + matching line) to `/home/bob/ip`.

- `-i` 使搜索不区分大小写（对十六进制字符串很有用）。
- `-r` 启用递归搜索，遍历 `/etc/` 下的所有文件和子目录。
- `>` 将匹配输出（文件路径 + 匹配行）重定向到 `/home/bob/ip`。

**To see only filenames, use `-l` | 只显示文件名，使用 `-l`：**
```bash
$ sudo grep -irl 172.16.238.197 /etc/
```

---

## Task 6 — Create a File with Text Using Redirection | 任务 6：使用重定向创建带内容的文件

**Requirement | 要求：**
Create `/home/bob/file_wth_data.txt` containing exactly one line: `a file in my home directory`.

创建文件 `/home/bob/file_wth_data.txt`，其中恰好包含一行内容：`a file in my home directory`。

**Solution | 解答：**
```bash
$ echo "a file in my home directory" > /home/bob/file_wth_data.txt

# Verify the content
# 验证文件内容
$ cat /home/bob/file_wth_data.txt
# Output: a file in my home directory
# 输出：a file in my home directory
```

**Explanation | 解析：**
`echo "text" > file` is the simplest way to create a file with specific text content. The `"` quotes preserve spaces. Using `>` ensures only one line exists; `>>` would append to existing content.

`echo "text" > file` 是创建含特定文本内容文件的最简单方式。引号保留空格。使用 `>` 确保只有一行内容；`>>` 则会追加到现有内容后面。

---

## Task 7 — Redirect STDERR to a File | 任务 7：将 STDERR 重定向到文件

**Requirement | 要求：**
Run the script `/home/bob/my_python_test.py` using Python 3, and redirect any standard error output to `/home/bob/py_error.txt`.

使用 Python 3 运行脚本 `/home/bob/my_python_test.py`，并将标准错误输出重定向到 `/home/bob/py_error.txt`。

**Solution | 解答：**
```bash
$ python3 /home/bob/my_python_test.py 2> /home/bob/py_error.txt

# Check if there were any errors
# 检查是否有错误
$ cat /home/bob/py_error.txt
```

**Explanation | 解析：**
`2>` redirects file descriptor 2 (STDERR) to the specified file. Normal output (STDOUT) still appears on the terminal. This is the standard way to capture error logs from scripts or programs without mixing them with regular output.

`2>` 将文件描述符 2（STDERR）重定向到指定文件。正常输出（STDOUT）仍然显示在终端上。这是从脚本或程序捕获错误日志而不与常规输出混合的标准方式。

---

## Task 8 — Read Compressed File and Pipe Output | 任务 8：读取压缩文件并通过管道处理输出

**Requirement | 要求：**
Read the compressed file `/usr/share/man/man1/tail.1.gz` without extracting it, and copy only the **first line** to `/home/bob/pipes`.

在不解压的情况下读取压缩文件 `/usr/share/man/man1/tail.1.gz`，并将**第一行**内容复制到 `/home/bob/pipes`。

**Solution | 解答：**
```bash
$ zcat /usr/share/man/man1/tail.1.gz | head -1 > /home/bob/pipes

# Verify the result
# 验证结果
$ cat /home/bob/pipes
```

**Explanation | 解析：**
This task beautifully combines three concepts from this section:

这道题完美地结合了本节的三个核心概念：

1. **`zcat`** reads a gzip-compressed file and prints its contents to STDOUT without creating a decompressed file on disk.
2. **`|` (pipe)** connects `zcat`'s output directly to `head`'s input.
3. **`head -1`** extracts only the first line from the stream.
4. **`>`** writes that single line to the destination file.

---

1. **`zcat`** 读取 gzip 压缩文件并将内容输出到 STDOUT，不在磁盘上创建解压文件。
2. **`|`（管道）** 将 `zcat` 的输出直接连接到 `head` 的输入。
3. **`head -1`** 从数据流中只提取第一行。
4. **`>`** 将这一行内容写入目标文件。

---

## Lab Summary | 实验总结

| Task | Key Command(s) | Concept | 核心概念 |
|------|---------------|---------|---------|
| 1 | `tar -zcf` | Compression & Archival | 压缩归档 |
| 2 | `gunzip` | Decompression | 解压 |
| 3 | `find / -name` | File Search | 文件查找 |
| 4 | `find` + `echo >` | Search + Redirection | 搜索 + 重定向 |
| 5 | `grep -ir` + `>` | Pattern Search + Redirection | 模式搜索 + 重定向 |
| 6 | `echo "..." >` | STDOUT Redirection | STDOUT 重定向 |
| 7 | `python3 ... 2>` | STDERR Redirection | STDERR 重定向 |
| 8 | `zcat \| head \| >` | Pipes + Redirection | 管道 + 重定向 |
