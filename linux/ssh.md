# ssh远程登录

## 文档

以下是一些常见的SSH命令和选项，你可以在终端中使用 `man ssh` 命令来查看SSH的帮助文档。

```shell
man ssh
```

### 常用参数

当使用SSH命令时，可以使用许多不同的参数来控制连接和执行操作。以下是一些常见的SSH参数的中文列举：

- `-p`：指定远程服务器的端口号。
- `-i`：指定用于身份验证的私钥文件。
- `-l`：指定登录远程服务器的用户名。
- `-o`：设置SSH连接的选项，如禁用主机密钥检查、设置连接超时等。
- `-X`：启用X11转发，允许在SSH会话中打开图形化应用程序。
- `-C`：启用压缩，以减少数据传输的带宽占用。
- `-L`：设置本地端口转发，将远程主机的端口映射到本地机器上。
- `-R`：设置远程端口转发，将本地机器的端口映射到远程主机上。
- `-D`：启用动态端口转发，将SSH客户端配置为SOCKS代理。

这只是一些常见的SSH参数示例，还有其他更多的参数和选项可供使用。你可以在终端中使用 `man ssh` 命令来查看完整的SSH帮助文档，其中包含了更详细的参数说明和用法示例。

### 示例

当使用SSH命令时，以下是几种常用的参数组合示例：

1. 使用用户名和密码登录远程服务器：

```shell
ssh username@hostname
```

2. 指定端口号登录远程服务器：

```shell
ssh -p 2222 username@hostname
```

3. 使用私钥文件进行身份验证登录：

```shell
ssh -i /path/to/private_key username@hostname
```

4. 禁用主机密钥检查，并设置连接超时：

```shell
ssh -o StrictHostKeyChecking=no -o ConnectTimeout=10 username@hostname
```

5. 启用X11转发，并登录远程服务器：

```shell
ssh -X username@hostname
```

这些示例展示了一些常见的SSH参数组合，你可以根据实际情况选择适合你的用法。

希望这些示例对你有帮助。如果你有其他问题，请随时提问。

## 密码登录

### 明文密码登录

SSH协议默认情况下是使用加密的方式进行身份验证和数据传输，以提供更高的安全性。然而，SSH也支持使用明文密码登录，但不推荐这种做法，因为明文密码的传输容易受到中间人攻击的威胁。

在SSH中，使用明文密码登录可以通过以下方式实现：

```shell
sshpass -p "your_password" ssh username@hostname
```

这里使用了 `sshpass` 工具，它可以在命令行中直接提供密码。但请注意，`sshpass` 工具的使用并不推荐，因为密码可能会被其他人在系统日志或命令历史中看到。

### 脚本编写

```bash
#!/bin/bash
while getopts ":h:p:u:" opt; do
  case $opt in
    h)
      hostname=$OPTARG
      ;;
    p)
      password=$OPTARG
      ;;
    u)
      username=$OPTARG
      ;;
    \?)
      echo "无效的选项： -$OPTARG" >&2
      exit 1
      ;;
    :)
      echo "选项 -$OPTARG 需要参数." >&2
      exit 1
      ;;
  esac
done

if [[ -z $hostname ]] || [[ -z $username ]] || [[ -z $password ]]; then
    echo "请提供正确的参数：-h 主机名 -u 用户名 -p 密码"
else
    # 使用 sshpass 命令登录远程服务器
    sshpass -p "$password" ssh -o StrictHostKeyChecking=no -t $username@$hostname
    echo "连接已关闭。"
fi
```

授予脚本文件可执行权限：

```bash
chmod +x script.sh
```

执行脚本:

```bash
./script.sh -h 192.168.0.1 -u myuser -p mypassword
```

### Sshpass安装

下面是在不同操作系统上下载和安装 `sshpass` 工具的方法：

**在Mac上安装 `sshpass`**：

1. 使用Homebrew包管理器安装 `sshpass`：

   ```shell
   brew install hudochenkov/sshpass/sshpass
   ```

**在Linux上安装 `sshpass`**：

1. Debian/Ubuntu：

   ```shell
   sudo apt-get install sshpass
   ```

2. CentOS/Fedora/RHEL：

   ```shell
   sudo yum install sshpass
   ```

**在Windows上安装 `sshpass`**：

1. 安装Cygwin，它是一个为Windows提供类Unix环境的开源项目。在Cygwin安装程序中选择 `openssh`、`make` 和 `gcc-core` 这些软件包，并完成安装。
   下载链接：[https://cygwin.com/install.html](https://cygwin.com/install.html)

2. 下载 `sshpass` 源代码：

   ```shell
   wget https://sourceforge.net/projects/sshpass/files/latest/download/sshpass-1.09.tar.gz
   ```

3. 解压并编译 `sshpass`：

   ```shell
   tar -xvf sshpass-1.09.tar.gz
   cd sshpass-1.09
   ./configure
   make
   make install
   ```

## 秘钥登录

### 秘钥生成配置

要使用密钥对进行SSH登录，你需要遵循以下步骤：

当将公钥复制到目标服务器时，可以按照以下步骤操作：

1. 在本地计算机上生成SSH密钥对：
   - 打开终端或命令行窗口。
   - 运行以下命令生成RSA密钥对：

     ```shell
     ssh-keygen -t rsa
     ```

   - 根据提示，选择密钥文件的保存位置和可选的密码（可留空，留空表示无密码）。
   - 这将生成两个文件：`id_rsa`（私钥）和 `id_rsa.pub`（公钥）。

2. 登录到目标服务器：
   - 打开终端或命令行窗口。
   - 运行以下命令以SSH方式登录到目标服务器：

     ```shell
     ssh username@hostname
     ```

   - 替换 `username` 和 `hostname` 为目标服务器的用户名和主机名或IP地址。
   - 输入目标服务器的密码以完成登录。

3. 将本地计算机上的公钥复制到目标服务器：
   - 在目标服务器的终端会话中，运行以下命令将本地计算机上的公钥追加到目标服务器的 `~/.ssh/authorized_keys` 文件中：

     ```shell
     cat >> ~/.ssh/authorized_keys
     ```

   - 此命令将进入追加模式并等待输入。
   - 打开本地计算机上的 `id_rsa.pub` 文件，将其中的内容复制。
   - 在目标服务器的终端会话中，粘贴公钥内容，并按下 `Ctrl + D` 键保存并退出。

4. 验证公钥是否成功添加到目标服务器：
   - 在本地计算机上，尝试使用密钥进行SSH登录到目标服务器：

     ```shell
     ssh username@hostname
     ```

   - 如果公钥配置正确，将无需输入密码，并且你将通过密钥进行身份验证并成功登录到目标服务器。

以上步骤中，第3步是将本地计算机上的公钥内容追加到目标服务器的 `~/.ssh/authorized_keys` 文件中。这样，当你使用SSH登录时，服务器将使用密钥进行身份验证。

### ssh-agent管理秘钥

如果你设置了SSH密钥密码，但不想每次使用SSH时手动输入密码，可以使用 `ssh-agent` 和 `ssh-add` 来管理密钥密码。以下是具体的操作步骤：

1. 启动 `ssh-agent`：
   - 打开终端或命令行窗口。
   - 运行以下命令来启动 `ssh-agent`：

     ```shell
     eval "$(ssh-agent -s)"
     ```

2. 添加私钥到 `ssh-agent`：
   - 运行以下命令来将私钥添加到 `ssh-agent`：

     ```shell
     ssh-add /path/to/private_key
     ```

   - 替换 `/path/to/private_key` 为你的私钥文件的路径。
   - 输入你设置的密钥密码进行确认。

3. 输入密钥密码一次：
   - 当你添加私钥到 `ssh-agent` 时，它会要求输入密钥密码。
   - 输入你设置的密钥密码进行确认。此时，你只需要输入一次密码。

4. 使用SSH无需手动输入密码：
   - 现在，当你使用SSH连接到远程服务器时，`ssh-agent` 会自动提供私钥密码，无需手动输入密码。
   - 例如，运行以下命令进行SSH登录：

     ```shell
     ssh username@hostname
     ```

### SSH配置文件

> 此配置配合，秘钥和ssh-agent管理秘钥，可以进行快速登录

1. 打开终端或命令行窗口。

2. 编辑SSH配置文件（如果不存在，则创建它）：

   ```shell
   vi ~/.ssh/config
   ```

3. 在配置文件中，添加主机配置信息。以下是一个示例：

   ```s
      Host myhost
       HostName 192.168.0.1
       User username
       Port 22
       IdentityFile /path/to/private_key
   ```

   - 在上面的示例中，`myhost` 是你为主机设置的别名，可以自定义。
   - `HostName` 指定主机的 IP 地址或主机名。
   - `User` 指定登录主机的用户名。
   - `Port` 指定主机的 SSH 端口号。
   - `IdentityFile` 指定私钥文件的路径。

4. 保存并关闭文件。

5. 现在，你可以直接使用 `ssh hostname` 登录目标主机，其中 `hostname` 是你在配置文件中设置的别名。例如：

   ```shell
   ssh myhost
   ```

   SSH 将自动根据配置文件中的信息进行连接，并登录到目标主机。

通过配置 SSH 配置文件，你可以在登录时省去手动输入目标主机的详细信息，提高登录效率。
