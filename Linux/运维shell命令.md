---
excalidraw-css:
---


### **1. 应用部署相关**
#### **1.1 拉取代码并构建**
```bash
# 拉取最新代码
git pull origin main

# 使用 Maven 打包，跳过测试
mvn clean package -DskipTests
```

#### **1.2 停止当前运行的应用**
```bash
# 查找并杀死正在运行的 Java 进程
APP_NAME="your-app-name.jar"
PID=$(ps -ef | grep java | grep $APP_NAME | awk '{print $2}')
if [ -n "$PID" ]; then
    echo "Killing process $PID"
    kill -9 $PID
else
    echo "No running process found for $APP_NAME"
fi
```

#### **1.3 启动应用**
```bash
# 启动 Java 应用，指定 JVM 参数和 Spring 配置文件
nohup java -jar \
-XX:MetaspaceSize=256m -XX:MaxMetaspaceSize=256m \
-Xms512m -Xmx1024m \
-Dspring.profiles.active=prod \
target/your-app-name.jar > app.log 2>&1 &
```

#### **1.4 检查应用是否启动成功**
```bash
# 检查日志中是否包含启动成功的标志
tail -f app.log | grep "Started Application"
```

---

### **2. 日志管理**
#### **2.1 查看实时日志**
```bash
tail -f app.log
```

#### **2.2 按时间过滤日志**
```bash
# 查看最近 10 分钟的日志
grep "$(date -d '10 minutes ago' '+%Y-%m-%d %H:%M')" app.log
```

#### **2.3 归档日志**
```bash
# 将日志按日期归档
LOG_DIR="/path/to/logs"
ARCHIVE_DIR="/path/to/archive"
TIMESTAMP=$(date '+%Y%m%d%H%M%S')
mv $LOG_DIR/app.log $ARCHIVE_DIR/app_$TIMESTAMP.log
```

#### **2.4 清理过期日志**
```bash
# 删除 7 天前的日志
find /path/to/logs -name "*.log" -mtime +7 -exec rm {} \;
```

---

### **3. 性能监控与调优**
#### **3.1 查看 Java 进程的 CPU 和内存使用情况**
```bash
top -p $(pgrep -f your-app-name.jar)
```

#### **3.2 查看 JVM 内存状态**
```bash
# 使用 jstat 查看堆内存和 GC 情况
jstat -gc $(pgrep -f your-app-name.jar) 1000 10
```

#### **3.3 生成堆转储文件（Heap Dump）**
```bash
# 使用 jmap 生成 Heap Dump
jmap -dump:live,format=b,file=heapdump.hprof $(pgrep -f your-app-name.jar)
```

#### **3.4 查看线程状态**
```bash
# 使用 jstack 查看线程栈
jstack $(pgrep -f your-app-name.jar) > thread_dump.txt
```

---

### **4. 网络与端口管理**
#### **4.1 检查端口占用**
```bash
# 查看某个端口是否被占用
netstat -tuln | grep 8080
```

#### **4.2 查看应用的网络连接**
```bash
# 查看 Java 应用的网络连接
lsof -i -P -n | grep $(pgrep -f your-app-name.jar)
```

---

### **5. 定时任务与自动化**
#### **5.1 使用 Cron 定时重启应用**
```bash
# 编辑 Crontab
crontab -e

# 每天凌晨 3 点重启应用
0 3 * * * /path/to/restart_script.sh
```

#### **5.2 自动化部署脚本**
```bash
#!/bin/bash

# 拉取代码
git pull origin main

# 构建应用
mvn clean package -DskipTests

# 停止当前应用
APP_NAME="your-app-name.jar"
PID=$(ps -ef | grep java | grep $APP_NAME | awk '{print $2}')
if [ -n "$PID" ]; then
    echo "Killing process $PID"
    kill -9 $PID
fi

# 启动应用
nohup java -jar \
-XX:MetaspaceSize=256m -XX:MaxMetaspaceSize=256m \
-Xms512m -Xmx1024m \
-Dspring.profiles.active=prod \
target/$APP_NAME > app.log 2>&1 &

echo "Application restarted successfully!"
```

---

### **6. 其他常用命令**
#### **6.1 查看系统资源**
```bash
# 查看 CPU 和内存使用情况
htop

# 查看磁盘使用情况
df -h
```

#### **6.2 查找文件**
```bash
# 查找某个文件
find /path/to/search -name "*.log"
```

#### **6.3 压缩和解压文件**
```bash
# 压缩文件
tar -czvf archive.tar.gz /path/to/files

# 解压文件
tar -xzvf archive.tar.gz
```

#### **6.4 查看系统日志**
```bash
# 查看系统日志
tail -f /var/log/syslog
```

---

### **7. 安全相关**
#### **7.1 检查文件权限**
```bash
# 检查文件权限
ls -l /path/to/file
```

#### **7.2 修改文件权限**
```bash
# 修改文件权限为 755
chmod 755 /path/to/file
```

#### **7.3 检查开放端口**
```bash
# 查看所有开放端口
nmap localhost
```

---

