## 一、基本数据持久化方式

### 1. 使用绑定挂载（Bind Mount）
```bash
# 将主机目录挂载到容器
docker run -d -v /宿主机/路径:/容器内路径 镜像名:标签

# 示例：MySQL数据持久化
docker run -d --name mysql \
  -v /data/mysql:/var/lib/mysql \
  -e MYSQL_ROOT_PASSWORD=123456 \
  mysql:8.0
```

### 2. 使用 Docker 卷（Volume）
```bash
# 创建卷
docker volume create 卷名

# 查看所有卷
docker volume ls

# 使用卷启动容器
docker run -d -v 卷名:/容器内路径 镜像名:标签

# 示例：PostgreSQL数据持久化
docker volume create pgdata
docker run -d --name postgres \
  -v pgdata:/var/lib/postgresql/data \
  -e POSTGRES_PASSWORD=123456 \
  postgres:13
```

## 二、常用数据库持久化命令

### 1. MySQL/MariaDB
```bash
# 使用自定义配置和数据存储
docker run -d --name mysql \
  -v /data/mysql:/var/lib/mysql \
  -v /data/mysql-conf:/etc/mysql/conf.d \
  -e MYSQL_ROOT_PASSWORD=123456 \
  mysql:8.0
```

### 2. PostgreSQL
```bash
# 带初始化脚本的持久化
docker run -d --name postgres \
  -v pgdata:/var/lib/postgresql/data \
  -v /data/init-scripts:/docker-entrypoint-initdb.d \
  -e POSTGRES_PASSWORD=123456 \
  postgres:14
```

### 3. MongoDB
```bash
# 带认证的持久化部署
docker run -d --name mongo \
  -v mongo_data:/data/db \
  -e MONGO_INITDB_ROOT_USERNAME=admin \
  -e MONGO_INITDB_ROOT_PASSWORD=123456 \
  mongo:5.0
```

### 4. Redis
```bash
# 持久化Redis数据
docker run -d --name redis \
  -v redis_data:/data \
  redis:6.2 \
  redis-server --appendonly yes
```

### 5. Elasticsearch
```bash
# 生产环境ES持久化配置
docker run -d --name elasticsearch \
  -v es_data:/usr/share/elasticsearch/data \
  -e "discovery.type=single-node" \
  -e "ES_JAVA_OPTS=-Xms4g -Xmx4g" \
  elasticsearch:9.0.1
```

## 三、日常维护命令

### 1. 备份数据卷
```bash
# 备份MySQL卷到tar文件
docker run --rm -v mysql_data:/volume -v /backup:/backup alpine \
  tar -czf /backup/mysql-backup-$(date +%Y%m%d).tar.gz -C /volume ./
```

### 2. 恢复数据卷
```bash
# 从备份恢复MySQL数据
docker run --rm -v mysql_data:/volume -v /backup:/backup alpine \
  sh -c "rm -rf /volume/* && tar -xzf /backup/mysql-backup-20230101.tar.gz -C /volume"
```

### 3. 迁移数据卷
```bash
# 从旧卷迁移到新卷
docker run --rm -v old_volume:/from -v new_volume:/to alpine \
  sh -c "cp -a /from/. /to/"
```

### 4. 清理无用卷
```bash
# 删除未使用的卷
docker volume prune
```

## 四、高级持久化技巧

### 1. 只读挂载
```bash
# 配置文件以只读方式挂载
docker run -d -v /app/config:/etc/config:ro 镜像名
```

### 2. 多个挂载点
```bash
# 同时挂载数据和配置
docker run -d \
  -v app_data:/var/lib/app \
  -v /host/config:/etc/app \
  镜像名
```

### 3. 使用临时文件系统
```bash
# 对临时数据使用tmpfs
docker run -d --tmpfs /tmp:rw,size=512m 镜像名
```

### 4. 共享数据卷
```bash
# 多个容器共享同一个卷
docker run -d --name app1 -v shared_data:/data 镜像名
docker run -d --name app2 -v shared_data:/data 镜像名
```

## 五、容器数据操作

### 1. 从容器复制数据到主机
```bash
docker cp 容器名:/容器内路径 /宿主机路径
```

### 2. 从主机复制数据到容器
```bash
docker cp /宿主机路径 容器名:/容器内路径
```

### 3. 进入容器检查数据
```bash
docker exec -it 容器名 bash
ls -l /容器内路径
```

## 六、实用场景示例

### 1. 开发环境数据库
```bash
docker run -d --name dev-db \
  -v ~/dev/data:/var/lib/mysql \
  -p 3306:3306 \
  -e MYSQL_ROOT_PASSWORD=dev123 \
  mysql:8.0
```

### 2. 生产环境备份方案
```bash
# 每天凌晨备份
0 2 * * * docker run --rm -v prod_db_data:/volume -v /backups:/backup alpine tar -czf /backup/db-$(date +\%Y\%m\%d).tar.gz -C /volume ./
```

### 3. 测试环境快速重置
```bash
# 重置测试数据库
docker stop test-db && docker rm test-db
docker volume rm test_db_data
docker run -d --name test-db -v test_db_data:/var/lib/mysql mysql:8.0
```

掌握这些命令可以满足日常开发、测试和生产环境中大多数数据持久化需求，建议根据实际场景调整参数和路径。