在Linux系统中，可以通过修改本地**Hosts文件**或配置**DNS服务器**（如 `systemd-resolved`、`NetworkManager` 或直接修改 `resolv.conf`）来增加DNS解析。以下是详细方法：

---

## **方法1：修改Hosts文件（临时本地解析）**
**适用场景**：手动指定域名解析到特定IP（优先级高于DNS查询）。

### **步骤**
1. **使用root权限编辑`/etc/hosts`**  
   ```bash
   sudo nano /etc/hosts
   ```
   （或使用`vim`、`vi`等编辑器）

2. **添加DNS解析记录**  
   格式：`IP地址 域名`（每行一条），例如：
   ```plaintext
   192.168.1.100 example.com
   192.168.1.101 api.example.com
   ```

3. **保存并退出**  
   - `nano`：按 `Ctrl + X` → `Y` → `Enter`  
   - `vim`：按 `Esc` → `:wq` → `Enter`

4. **测试是否生效**  
   ```bash
   ping example.com
   ```
   或
   ```bash
   nslookup example.com
   ```

---

## **方法2：修改DNS服务器配置（全局生效）**
**适用场景**：更改系统默认DNS服务器（如改用Google DNS、Cloudflare DNS）。

### **（A）使用`systemd-resolved`（Ubuntu/Debian等新版本推荐）**
1. **编辑`systemd-resolved`配置**  
   ```bash
   sudo nano /etc/systemd/resolved.conf
   ```
2. **取消注释并修改DNS设置**（例如使用Cloudflare DNS）：
   ```ini
   [Resolve]
   DNS=1.1.1.1 8.8.8.8
   Domains=~.
   ```
3. **重启`systemd-resolved`服务**  
   ```bash
   sudo systemctl restart systemd-resolved
   ```
4. **检查当前DNS**  
   ```bash
   systemd-resolve --status
   ```

### **（B）修改`resolv.conf`（传统方法，可能重启失效）**
1. **备份原文件**  
   ```bash
   sudo cp /etc/resolv.conf /etc/resolv.conf.bak
   ```
2. **编辑`resolv.conf`**  
   ```bash
   sudo nano /etc/resolv.conf
   ```
3. **添加DNS服务器**（例如Google DNS）：
   ```plaintext
   nameserver 8.8.8.8
   nameserver 1.1.1.1
   ```
4. **防止被覆盖**（可选）  
   - 如果重启后配置丢失，可运行：
     ```bash
     sudo chattr +i /etc/resolv.conf  # 锁定文件（慎用）
     ```
   - 或使用`NetworkManager`管理（见下）。

### **（C）使用`NetworkManager`（图形界面/命令行）**
1. **查看当前连接**  
   ```bash
   nmcli connection show
   ```
2. **修改DNS**（假设连接名为`eth0`）  
   ```bash
   sudo nmcli connection modify eth0 ipv4.dns "8.8.8.8 1.1.1.1"
   sudo nmcli connection up eth0
   ```
3. **验证**  
   ```bash
   nmcli device show eth0 | grep DNS
   ```

---

## **方法3：使用`dnsmasq`（搭建本地DNS缓存）**
**适用场景**：需要本地DNS缓存或自定义解析规则（如开发测试）。

4. **安装`dnsmasq`**  
   ```bash
   sudo apt install dnsmasq  # Debian/Ubuntu
   sudo yum install dnsmasq  # CentOS/RHEL
   ```
5. **编辑配置文件**  
   ```bash
   sudo nano /etc/dnsmasq.conf
   ```
6. **添加自定义解析**  
   ```ini
   address=/example.com/192.168.1.100
   server=8.8.8.8  # 上游DNS
   ```
7. **重启服务**  
   ```bash
   sudo systemctl restart dnsmasq
   ```
8. **设置系统使用`dnsmasq`**  
   修改`/etc/resolv.conf`，将`nameserver`改为：
   ```plaintext
   nameserver 127.0.0.1
   ```

---

## **验证DNS解析**
```bash
dig example.com
```
或
```bash
nslookup example.com
```
如果修改了`/etc/hosts`，确保`nslookup`未忽略它：
```bash
nslookup example.com 127.0.0.1
```

---

## **总结**
| 方法                | 适用场景                     | 持久性       |
|---------------------|----------------------------|-------------|
| `/etc/hosts`        | 临时本地解析                | 永久        |
| `systemd-resolved`  | 现代Linux系统（Ubuntu等）   | 永久        |
| `/etc/resolv.conf`  | 传统方法（可能被覆盖）       | 临时        |
| `NetworkManager`    | 动态网络管理（如Wi-Fi）      | 永久        |
| `dnsmasq`           | 本地DNS缓存/复杂规则         | 永久        |

选择适合你需求的方式即可！