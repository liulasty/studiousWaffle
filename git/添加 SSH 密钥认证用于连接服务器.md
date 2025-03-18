添加 SSH 密钥认证用于连接服务器的全部过程如下：

### 1. 生成 SSH 密钥对
在本地计算机上生成 SSH 密钥对（公钥和私钥）。

```bash
ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
```
- `-t rsa`：指定密钥类型为 RSA。
- `-b 4096`：指定密钥长度为 4096 位。
- `-C "your_email@example.com"`：添加注释，通常使用邮箱。

按提示操作：
- 选择保存密钥的文件路径（默认 `~/.ssh/id_rsa`）。
- 设置密钥的密码（可选）。

### 2. 将公钥上传到服务器
将生成的公钥上传到服务器的 `~/.ssh/authorized_keys` 文件中。

#### 方法一：使用 `ssh-copy-id` 命令
```bash
ssh-copy-id -i ~/.ssh/id_rsa.pub username@server_ip
```
- `-i ~/.ssh/id_rsa.pub`：指定公钥文件。
- `username@server_ip`：服务器的用户名和 IP 地址。

#### 方法二：手动复制
1. 复制公钥内容：
   ```bash
   cat ~/.ssh/id_rsa.pub
   ```
2. 登录服务器：
   ```bash
   ssh username@server_ip
   ```
3. 将公钥粘贴到 `~/.ssh/authorized_keys` 文件中：
   ```bash
   mkdir -p ~/.ssh
   echo "粘贴的公钥内容" >> ~/.ssh/authorized_keys
   chmod 600 ~/.ssh/authorized_keys
   ```

### 3. 配置 SSH 客户端（可选）
编辑本地 `~/.ssh/config` 文件，简化连接命令。

```bash
Host myserver
    HostName server_ip
    User username
    IdentityFile ~/.ssh/id_rsa
```
- `Host myserver`：自定义服务器别名。
- `HostName server_ip`：服务器 IP 地址。
- `User username`：登录用户名。
- `IdentityFile ~/.ssh/id_rsa`：指定私钥文件。

### 4. 测试 SSH 连接
使用密钥认证连接服务器：

```bash
ssh username@server_ip
```
或使用配置的别名：
```bash
ssh myserver
```

### 5. 禁用密码认证（可选）
为增强安全性，可禁用服务器的密码认证。

4. 编辑 SSH 配置文件：
   ```bash
   sudo nano /etc/ssh/sshd_config
   ```
5. 修改以下配置：
   ```bash
   PasswordAuthentication no
   ```
6. 重启 SSH 服务：
   ```bash
   sudo systemctl restart sshd
   ```

### 总结
通过以上步骤，你已成功配置 SSH 密钥认证，并可通过密钥连接服务器。