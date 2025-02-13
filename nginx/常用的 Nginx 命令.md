以下是一些常用的 Nginx 命令，可以用来管理和操作 Nginx 服务。

### 启动 Nginx

```bash
sudo nginx
```

### 停止 Nginx

```bash
sudo nginx -s stop
```

### 重启 Nginx

```bash
sudo nginx -s reload
```

### 测试 Nginx 配置

在修改 Nginx 配置文件后，可以使用以下命令测试配置文件的正确性：

```bash
sudo nginx -t
```

### 检查 Nginx 状态

查看 Nginx 服务的状态：

```bash
sudo systemctl status nginx
```

### 启动 Nginx 服务（使用 systemd）

```bash
sudo systemctl start nginx
```

### 停止 Nginx 服务（使用 systemd）

```bash
sudo systemctl stop nginx
```

### 重启 Nginx 服务（使用 systemd）

```bash
sudo systemctl restart nginx
```

### 重新加载 Nginx 配置（使用 systemd）

```bash
sudo systemctl reload nginx
```

这些命令将帮助你管理 Nginx 服务的启动、停止、重启和配置检查。确保每次修改配置文件后都使用 `nginx -t` 测试配置文件，以避免因配置错误导致的服务启动失败。