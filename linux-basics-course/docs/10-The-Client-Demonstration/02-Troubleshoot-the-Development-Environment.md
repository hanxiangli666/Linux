# Troubleshoot the Development Environment
# 排查开发环境故障

This lab walks you through the exact steps Bob took to diagnose and fix the broken development environment before the client demonstration. Each task builds on the previous one, simulating a real-world incident response scenario.

本实验将引导你完成 Bob 在客户演示前诊断并修复故障开发环境的每一个步骤。每项任务都在前一项的基础上递进，模拟真实的生产事故响应场景。

[Troubleshoot the Development Environment Lab](https://kodekloud.com/topic/lab-troubleshoot-the-development-environment/)

---

1. **Information only | 仅供了解**

1. **Information only | 仅供了解**

1.  <details>
    <summary>Copy the file <code>caleston-code.tar.gz</code> from Bob's laptop to Bob's home directory on the webserver <code>devapp01</code><br>将文件 <code>caleston-code.tar.gz</code> 从 Bob 的笔记本电脑复制到 Web 服务器 <code>devapp01</code> 上 Bob 的家目录</summary>

    **EN:** We use `scp` (Secure Copy Protocol) to transfer files between hosts over SSH. The syntax is `scp <source> <destination>`. Here, `devapp01:~/` means "the home directory on host devapp01".

    **CN:** 我们使用 `scp`（安全复制协议）通过 SSH 在主机之间传输文件。语法为 `scp <源> <目标>`。这里，`devapp01:~/` 表示"devapp01 主机上的家目录"。

    ```bash
    scp caleston-code.tar.gz devapp01:~/
    ```

    </details>

1.  <details>
    <summary>On the <code>devapp01</code> webserver, unzip and extract the copied file in the directory <code>/opt/</code><br>在 <code>devapp01</code> Web 服务器上，将复制的文件解压并提取到 <code>/opt/</code> 目录中</summary>

    **EN:** The `/opt` directory is reserved for optional/third-party software and is owned by `root`. We need `sudo` to write there. The `tar` flags used: `-z` (decompress with gzip), `-x` (extract), `-f` (specify filename), `-C` (change to directory before extracting).

    **CN:** `/opt` 目录用于存放可选/第三方软件，归 `root` 所有，因此写入时需要 `sudo`。所用的 `tar` 参数：`-z`（用 gzip 解压），`-x`（提取），`-f`（指定文件名），`-C`（提取前切换到目标目录）。

    ```bash
    ssh devapp01

    sudo tar -zxf caleston-code.tar.gz -C /opt
    ```

    > **Note | 注意:** Don't exit the SSH session yet — we need to remain on `devapp01` for the next few steps.
    > 暂时不要退出 SSH 会话——接下来几步我们还需要留在 `devapp01` 上。

    </details>

1.  <details>
    <summary>Delete the tar file from <code>devapp01</code> webserver<br>从 <code>devapp01</code> Web 服务器上删除 tar 文件</summary>

    **EN:** Good housekeeping — remove the archive after extraction to free up disk space.

    **CN:** 良好的维护习惯——提取后删除归档文件以释放磁盘空间。

    ```bash
    rm caleston-code.tar.gz
    ```

    </details>

1.  <details>
    <summary>Make sure that the directory is extracted in such a way that the path <code>/opt/caleston-code/mercuryProject/</code> exists on the webserver<br>确保目录已被正确提取，使路径 <code>/opt/caleston-code/mercuryProject/</code> 在 Web 服务器上存在</summary>

    **EN:** Nothing extra to do here — if the previous extraction was done correctly, this path will already exist. You can verify with `ls`.

    **CN:** 这里无需额外操作——如果上一步的提取操作正确，该路径将已经存在。你可以用 `ls` 验证。

    ```bash
    ls -l /opt/caleston-code/mercuryProject/
    ```

    </details>

1.  <details>
    <summary>For the client demo, Bob has installed a <code>postgres</code> database in <code>devdb01</code>. SSH to <code>devdb01</code> and check the status of <code>postgresql.service</code>. What is the state of the DB?<br>为了客户演示，Bob 在 <code>devdb01</code> 上安装了 <code>postgres</code> 数据库。SSH 登录到 <code>devdb01</code> 并检查 <code>postgresql.service</code> 的状态。数据库处于什么状态？</summary>

    **EN:** First return to Bob's laptop (exit devapp01), then SSH into the database node. We use `systemctl status` to inspect a systemd service's current state.

    **CN:** 首先返回 Bob 的笔记本电脑（退出 devapp01），然后 SSH 登录到数据库节点。我们使用 `systemctl status` 来检查 systemd 服务的当前状态。

    ```bash
    exit          # return to Bob's laptop | 返回 Bob 的笔记本

    ssh devdb01

    systemctl status postgresql.service
    ```

    > **Expected output | 预期输出:** `Active: inactive (dead)` — the database is **not running**!
    > 数据库**尚未运行**！
    >
    > Don't exit yet — remain on `devdb01`. | 暂时不要退出，留在 `devdb01` 上。

    </details>

1.  <details>
    <summary>Add an entry <code>host all all 0.0.0.0/0 md5</code> to the end of <code>/etc/postgresql/10/main/pg_hba.conf</code> on the DB server. This allows the web application to connect to the DB.<br>在数据库服务器的 <code>/etc/postgresql/10/main/pg_hba.conf</code> 末尾添加条目 <code>host all all 0.0.0.0/0 md5</code>，以允许 Web 应用连接到数据库</summary>

    **EN:** `pg_hba.conf` is PostgreSQL's host-based authentication configuration file. The line we add permits any host (`0.0.0.0/0`) to connect to any database as any user using MD5 password authentication. This is permissive for a dev/demo environment.

    **CN:** `pg_hba.conf` 是 PostgreSQL 的基于主机的认证配置文件。我们添加的这一行允许任何主机（`0.0.0.0/0`）使用 MD5 密码认证连接到任意数据库的任意用户。这对开发/演示环境来说是合适的宽松策略。

    ```bash
    sudo vi /etc/postgresql/10/main/pg_hba.conf
    ```

    Scroll to the end of the file and add:
    滚动到文件末尾并添加：

    ```
    host all all 0.0.0.0/0 md5
    ```

    Save and exit `vi` with `:wq`. | 用 `:wq` 保存并退出 `vi`。

    </details>

1.  <details>
    <summary>Start the <code>postgres</code> DB on the <code>devdb01</code> server<br>在 <code>devdb01</code> 服务器上启动 <code>postgres</code> 数据库</summary>

    **EN:** Use `systemctl start` to bring the service up. Always check status afterward to confirm it started cleanly.

    **CN:** 使用 `systemctl start` 启动服务。之后始终检查状态，以确认它已顺利启动。

    ```bash
    sudo systemctl start postgresql.service

    # Verify it started | 验证已启动
    systemctl status postgresql.service
    ```

    </details>

1.  <details>
    <summary>What port is <code>postgres</code> running on? Check using the <code>netstat</code> command<br><code>postgres</code> 运行在哪个端口？使用 <code>netstat</code> 命令检查</summary>

    **EN:** `netstat -ptean` shows all active network connections with process names and port numbers. Look for the `postgres` process. You'll see two entries (IPv4 and IPv6) but both use the same port. **Write this port down** — you'll need it in a later step!

    **CN:** `netstat -ptean` 显示所有活动的网络连接及其进程名称和端口号。找到 `postgres` 进程。你会看到两个条目（IPv4 和 IPv6），但两者使用相同的端口。**记下这个端口号**——后面的步骤中会用到！

    ```bash
    sudo netstat -ptean
    ```

    > **Tip | 提示:** The default PostgreSQL port is `5432`, but it may have been configured differently in this lab environment.
    > PostgreSQL 的默认端口是 `5432`，但在此实验环境中可能已被配置为其他端口。

    </details>

1.  <details>
    <summary>Back on <code>devapp01</code>: navigate to <code>/opt/caleston-code/mercuryProject</code> and run <code>python3 manage.py runserver 0.0.0.0:8000</code>. Does it work?<br>回到 <code>devapp01</code>：切换到 <code>/opt/caleston-code/mercuryProject</code> 并运行 <code>python3 manage.py runserver 0.0.0.0:8000</code>。它能正常工作吗？</summary>

    **EN:** Return to Bob's laptop first, then SSH to the app server and attempt to start the Django web application. It is expected to **fail** at this point — this is intentional! Observe the error carefully.

    **CN:** 先返回 Bob 的笔记本电脑，然后 SSH 登录到应用服务器，尝试启动 Django Web 应用程序。此时预计会**失败**——这是故意为之的！仔细观察错误信息。

    ```bash
    exit          # back to Bob's laptop | 返回 Bob 的笔记本

    ssh devapp01

    cd /opt/caleston-code/mercuryProject
    python3 manage.py runserver 0.0.0.0:8000
    ```

    > The app will crash with a stack trace. The answer is **No** — it does not work.
    > 应用程序会崩溃并显示堆栈追踪。答案是**否**——它无法正常工作。
    >
    > Press `CTRL + C` to stop it. | 按 `CTRL + C` 停止它。

    </details>

1.  <details>
    <summary>Why did the command not work?<br>为什么命令无法正常工作？</summary>

    **EN:** Read the stack trace at the bottom carefully. The application is trying to connect to the database using `localhost` (or `127.0.0.1`) and possibly the wrong port. Since the database is running on a *separate server* (`devdb01`), `localhost` is incorrect. This is the classic **environmental parity** problem.

    **CN:** 仔细阅读底部的堆栈追踪。应用程序正试图使用 `localhost`（或 `127.0.0.1`）连接数据库，端口也可能有误。由于数据库运行在*独立服务器*（`devdb01`）上，`localhost` 是不正确的。这就是经典的**环境一致性**问题。

    </details>

1.  <details>
    <summary>Find and fix the configuration file: change the DB host from <code>localhost</code> to <code>devdb01</code> and fix the port number<br>找到并修复配置文件：将数据库主机从 <code>localhost</code> 改为 <code>devdb01</code>，并修正端口号</summary>

    **EN:** We need to locate the Django settings file containing the `DATABASES` configuration. We use `find` combined with `grep -l` to search recursively for the specific string.

    **CN:** 我们需要找到包含 `DATABASES` 配置的 Django 设置文件。使用 `find` 结合 `grep -l` 来递归搜索特定字符串。

    ```bash
    # Find the config file | 找到配置文件
    find . -type f -exec grep -l 'DATABASES = {' "{}" \;
    ```

    Open the returned file for editing:
    打开返回的文件进行编辑：

    ```bash
    vi ./mercury/settings.py
    ```

    Find the `DATABASES = {` section and update:
    找到 `DATABASES = {` 部分并更新：

    | Setting | Before | After |
    |---------|--------|-------|
    | `HOST` | `localhost` | `devdb01` |
    | `PORT` | *(wrong port)* | *(port noted from netstat)* |

    Save and exit with `:wq`. | 用 `:wq` 保存并退出。

    </details>

1.  <details>
    <summary>Change the ownership of ALL files and directories under <code>/opt/caleston-code</code> to user <code>mercury</code><br>将 <code>/opt/caleston-code</code> 下所有文件和目录的所有权更改为用户 <code>mercury</code></summary>

    **EN:** The application will run as the `mercury` service account. For it to read configuration files and write logs, it must own the application directory. The `-R` flag applies the change recursively.

    **CN:** 应用程序将以 `mercury` 服务账户运行。为了让它能够读取配置文件和写入日志，它必须拥有应用程序目录的所有权。`-R` 标志递归地应用此更改。

    ```bash
    sudo chown -R mercury /opt/caleston-code
    ```

    > **Note | 注意:** Sometimes this step may be marked incorrect in the lab even when done correctly — this is a known lab issue. Ignore and move on.
    > 有时即使操作正确，此步骤在实验中也可能被标记为不正确——这是一个已知的实验问题，忽略并继续即可。

    </details>

1.  <details>
    <summary>Restart the application and run the database migration<br>重新启动应用程序并运行数据库迁移</summary>

    **EN:** Before starting the app for the final time, we need to activate the Python virtual environment and run `migrate` to set up the database schema. Django's `migrate` command applies all pending migrations and creates the required tables.

    **CN:** 在最终启动应用程序之前，我们需要激活 Python 虚拟环境并运行 `migrate` 来设置数据库 schema。Django 的 `migrate` 命令会应用所有待处理的迁移并创建所需的数据库表。

    Make sure you are in `/opt/caleston-code/mercuryProject`:
    确保你在 `/opt/caleston-code/mercuryProject` 目录中：

    ```bash
    cd /opt/caleston-code/mercuryProject

    # Activate virtual environment | 激活虚拟环境
    source ../venv/bin/activate

    # Apply database migrations | 应用数据库迁移
    python3 manage.py migrate

    # Start the application | 启动应用程序
    python3 manage.py runserver 0.0.0.0:8000
    ```

    **What is a virtual environment? | 什么是虚拟环境？**

    A Python virtual environment (`venv`) is an isolated Python installation for a specific project. It lets you install packages without affecting the system-wide Python installation — essential when managing multiple projects with different dependency versions.

    Python 虚拟环境（`venv`）是针对特定项目的独立 Python 安装。它允许你安装软件包而不影响系统范围的 Python 安装——在管理具有不同依赖版本的多个项目时至关重要。

    </details>

1.  <details>
    <summary>Create a systemd service <code>mercury.service</code> to run the application persistently<br>创建 systemd 服务 <code>mercury.service</code> 以持久化运行应用程序</summary>

    **EN:** Running a web app manually with `python3 manage.py runserver` is fine for development, but not for production or demo environments. A `systemd` unit file allows the OS to manage the process — automatically restarting it on failure, starting it on boot, and controlling it with `systemctl`.

    **CN:** 用 `python3 manage.py runserver` 手动运行 Web 应用对于开发来说是可以的，但对于生产或演示环境则不够。`systemd` 单元文件允许操作系统管理该进程——在失败时自动重启、开机时自动启动，并通过 `systemctl` 进行控制。

    **Service requirements | 服务要求：**
    - Service name: `mercury.service` | 服务名称：`mercury.service`
    - Working directory: `/opt/caleston-code/mercuryProject/` | 工作目录
    - Command: `/usr/bin/python3 manage.py runserver 0.0.0.0:8000` | 启动命令
    - Restart policy: `on-failure` | 重启策略：失败时重启
    - Target: `multi-user.target` | 目标：多用户模式
    - Run as user: `mercury` | 运行用户：`mercury`
    - Description: `Project Mercury Web Application` | 描述

    First, stop the currently running app: | 首先停止当前运行的应用程序：

    ```bash
    # Press CTRL+C to stop the running server | 按 CTRL+C 停止运行中的服务器
    ```

    Create the unit file: | 创建单元文件：

    ```bash
    sudo vi /etc/systemd/system/mercury.service
    ```

    Enter the following content: | 输入以下内容：

    ```ini
    [Unit]
    Description=Project Mercury Web Application

    [Service]
    ExecStart=/usr/bin/python3 manage.py runserver 0.0.0.0:8000
    Restart=on-failure
    WorkingDirectory=/opt/caleston-code/mercuryProject/
    User=mercury

    [Install]
    WantedBy=multi-user.target
    ```

    Enable and start the service: | 启用并启动服务：

    ```bash
    # Reload systemd to pick up the new unit file | 重新加载 systemd 以识别新的单元文件
    sudo systemctl daemon-reload

    # Enable the service to start on boot | 设置服务开机自启
    sudo systemctl enable mercury

    # Start the service now | 立即启动服务
    sudo systemctl start mercury

    # Verify it is running | 验证服务正在运行
    sudo systemctl status mercury
    ```

    > **Why `daemon-reload`? | 为什么需要 `daemon-reload`？**
    > Whenever you create, edit, or delete a unit file, you must run `systemctl daemon-reload` to tell systemd to re-read its configuration. Without this, systemd will not be aware of your changes.
    >
    > 每次创建、编辑或删除单元文件时，你都必须运行 `systemctl daemon-reload` 来通知 systemd 重新读取其配置。否则，systemd 不会感知到你的更改。

    </details>

1. **Information only — browse the running application and celebrate!**

   **仅供了解——浏览正在运行的应用程序，庆祝成功！**
