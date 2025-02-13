要查看 Nginx 服务的状态，有几种不同的方法，具体取决于你的操作系统和 Nginx 的配置。以下是一些常见的方法：

1. **使用 `systemctl` (适用于使用 systemd 的系统，如 Linux 的大多数现代发行版)**
    

```bash
sudo systemctl status nginx
```

这将显示 Nginx 服务的状态，包括是否正在运行、最近的日志条目等。  
2. **使用 `service` 命令 (在某些 Linux 发行版中)**

```bash
sudo service nginx status
```

这将显示与 `systemctl` 类似的输出。  
3. **使用 `nginx` 命令的 `-s` 选项和 `status` 参数 (需要 Nginx 的 `ngx_http_stub_status_module` 模块)**

首先，确保你的 Nginx 配置启用了 `ngx_http_stub_status_module`。这通常在 Nginx 的主配置文件 (`nginx.conf`) 中的 `http` 块中通过 `load_module` 指令或简单地包含模块来完成。

然后，你可以使用以下命令来检查状态：

```bash
sudo nginx -s status
```

但请注意，这通常不会直接在命令行上显示输出。相反，它会在 Nginx 的错误日志中生成一条状态消息。要查看此消息，你可以使用 `tail` 命令查看 Nginx 的错误日志文件（通常位于 `/var/log/nginx/error.log`，但可能因系统而异）：

```bash
tail -f /var/log/nginx/error.log
```

然后，在另一个终端窗口中运行 `sudo nginx -s status`，并查看错误日志文件中的输出。

如果你已经配置了 `location = /nginx_status` 来使用 `ngx_http_stub_status_module`，你也可以通过 HTTP 请求来检查状态：

```bash
curl http://localhost/nginx_status
```

这将返回一个简单的文本响应，显示连接、请求和其他状态信息。  
4. **检查进程**

你还可以使用 `ps` 命令来检查 Nginx 进程是否正在运行：

```bash
ps -ef | grep nginx
```

如果 Nginx 正在运行，你应该能看到相关的进程条目。