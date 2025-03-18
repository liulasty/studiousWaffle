管理服务器上的 SSH 公钥是确保安全访问的重要环节。以下是管理服务器公钥的常见操作：

---

### 1. **添加新的公钥**
将新的公钥添加到服务器的 `~/.ssh/authorized_keys` 文件中。

#### 方法一：使用 `ssh-copy-id`
```bash
ssh-copy-id -i ~/.ssh/id_rsa.pub username@server_ip
```
- `-i ~/.ssh/id_rsa.pub`：指定要添加的公钥文件。
- `username@server_ip`：服务器的用户名和 IP 地址。

#### 方法二：手动添加
1. 复制公钥内容：
   ```bash
   cat ~/.ssh/id_rsa.pub
   ```
2. 登录服务器：
   ```bash
   ssh username@server_ip
   ```
3. 将公钥追加到 `~/.ssh/authorized_keys` 文件中：
   ```bash
   echo "粘贴的公钥内容" >> ~/.ssh/authorized_keys
   chmod 600 ~/.ssh/authorized_keys
   ```

---

### 2. **删除无效的公钥**
如果某个公钥不再需要，可以从 `~/.ssh/authorized_keys` 文件中删除。

1. 登录服务器：
   ```bash
   ssh username@server_ip
   ```
2. 编辑 `~/.ssh/authorized_keys` 文件：
   ```bash
   nano ~/.ssh/authorized_keys
   ```
3. 删除对应的公钥行，保存并退出。

---

### 3. **禁用某个公钥**
如果需要临时禁用某个公钥，可以在 `~/.ssh/authorized_keys` 文件中注释掉对应的行。

1. 登录服务器：
   ```bash
   ssh username@server_ip
   ```
2. 编辑 `~/.ssh/authorized_keys` 文件：
   ```bash
   nano ~/.ssh/authorized_keys
   ```
3. 在对应的公钥行前添加 `#` 注释：
   ```bash
   # ssh-rsa AAAAB3NzaC1yc2E... user@example.com
   ```

---

### 4. **查看当前公钥**
查看服务器上已授权的公钥列表。

1. 登录服务器：
   ```bash
   ssh username@server_ip
   ```
2. 查看 `~/.ssh/authorized_keys` 文件：
   ```bash
   cat ~/.ssh/authorized_keys
   ```

---

### 5. **限制公钥的使用**
可以通过在公钥前添加选项来限制其使用。例如，限制公钥只能用于特定命令或从特定 IP 地址访问。

1. 编辑 `~/.ssh/authorized_keys` 文件：
   ```bash
   nano ~/.ssh/authorized_keys
   ```
2. 在公钥前添加选项。例如：
   - 限制 IP 地址：
     ```bash
     from="192.168.1.100" ssh-rsa AAAAB3NzaC1yc2E... user@example.com
     ```
   - 限制命令：
     ```bash
     command="/usr/bin/backup-script" ssh-rsa AAAAB3NzaC1yc2E... user@example.com
     ```

---

### 6. **备份和恢复公钥**
定期备份 `~/.ssh/authorized_keys` 文件，以防止意外丢失。

#### 备份：
```bash
cp ~/.ssh/authorized_keys ~/.ssh/authorized_keys.backup
```

#### 恢复：
```bash
cp ~/.ssh/authorized_keys.backup ~/.ssh/authorized_keys
```

---

### 7. **批量管理公钥**
如果需要管理多个用户的公钥，可以将公钥集中存储在一个目录中，然后使用脚本动态更新 `authorized_keys` 文件。

1. 创建一个目录存储公钥：
   ```bash
   mkdir -p ~/.ssh/keys
   ```
2. 将公钥文件放入该目录，例如 `user1.pub`、`user2.pub`。
3. 使用脚本更新 `authorized_keys`：
   ```bash
   cat ~/.ssh/keys/*.pub > ~/.ssh/authorized_keys
   chmod 600 ~/.ssh/authorized_keys
   ```

---

### 8. **使用工具管理公钥**
可以使用一些工具来简化公钥管理，例如：
- **Ansible**：批量管理多台服务器的公钥。
- **GitHub/GitLab**：通过 API 动态同步团队成员的公钥。

---

### 9. **检查公钥权限**
确保 `~/.ssh` 目录和 `authorized_keys` 文件的权限正确，否则 SSH 会拒绝使用密钥认证。

1. 检查目录权限：
   ```bash
   chmod 700 ~/.ssh
   ```
2. 检查文件权限：
   ```bash
   chmod 600 ~/.ssh/authorized_keys
   ```

---

### 10. **定期清理公钥**
定期检查并清理不再使用的公钥，以减少安全风险。

---

通过以上方法，你可以有效地管理服务器上的 SSH 公钥，确保访问的安全性和可控性。