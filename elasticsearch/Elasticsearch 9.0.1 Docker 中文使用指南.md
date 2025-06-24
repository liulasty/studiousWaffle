
## 一、镜像获取

```bash
docker pull elasticsearch:9.0.1
```

## 二、快速启动（开发模式）

```bash
docker run -d --name elasticsearch \
  -p 9200:9200 -p 9300:9300 \
  -e "discovery.type=single-node" \
  -e "ES_JAVA_OPTS=-Xms1g -Xmx1g" \
  elasticsearch:9.0.1
```

## 三、生产环境推荐配置

### 1. 带持久化存储的启动方式

```bash
docker run -d --name elasticsearch \
  -p 9200:9200 -p 9300:9300 \
  -v es_data:/usr/share/elasticsearch/data \
  -e "discovery.type=single-node" \
  -e "ES_JAVA_OPTS=-Xms4g -Xmx4g" \
  -e "xpack.security.enabled=true" \
  --ulimit nofile=65535:65535 \
  --ulimit memlock=-1:-1 \
  elasticsearch:9.0.1
```

### 2. 多节点集群配置示例

```bash
# 节点1
docker run -d --name es01 \
  --net elastic \
  -p 9200:9200 \
  -e "node.name=es01" \
  -e "cluster.name=es-docker-cluster" \
  -e "discovery.seed_hosts=es02,es03" \
  -e "cluster.initial_master_nodes=es01,es02,es03" \
  -e "bootstrap.memory_lock=true" \
  elasticsearch:9.0.1

# 节点2
docker run -d --name es02 \
  --net elastic \
  -e "node.name=es02" \
  -e "cluster.name=es-docker-cluster" \
  -e "discovery.seed_hosts=es01,es03" \
  -e "cluster.initial_master_nodes=es01,es02,es03" \
  -e "bootstrap.memory_lock=true" \
  elasticsearch:9.0.1
```

## 四、基本验证

1. 检查服务状态：
```bash
curl -X GET "localhost:9200/_cat/health?v"
```

2. 查看节点信息：
```bash
curl -X GET "localhost:9200/_cat/nodes?v"
```

## 五、重要配置参数

| 参数 | 说明 | 示例值 |
|------|------|--------|
| discovery. Type | 节点发现类型 | single-node (单节点)/multi-node (集群) |
| ES_JAVA_OPTS | JVM 内存设置 | -Xms 4 g -Xmx 4 g |
| cluster. Name | 集群名称 | my-cluster |
| node. Name | 节点名称 | node-1 |
| network. Host | 绑定地址 | 0.0.0.0 |
| xpack. Security. Enabled | 安全功能 | true/false |

## 六、数据持久化

建议使用 Docker 卷实现数据持久化：

```bash
# 创建数据卷
docker volume create es_data

# 使用数据卷启动
docker run -d -v es_data:/usr/share/elasticsearch/data elasticsearch:9.0.1
```

## 七、安全配置

1. 启用基础安全功能：
```bash
docker run -d -e "xpack.security.enabled=true" elasticsearch:9.0.1
```

2. 设置内置用户密码：
```bash
docker exec -it elasticsearch /bin/bash
bin/elasticsearch-setup-passwords auto
```

## 八、性能调优建议

1. 设置合理的 JVM 内存（不超过物理内存的 50%）
2. 禁用交换分区：
```bash
-e "bootstrap.memory_lock=true"
```
1. 调整文件描述符限制：
```bash
--ulimit nofile=65535:65535
```

## 九、常见问题解决

2. **启动失败**：检查日志 `docker logs elasticsearch`
3. **内存不足**：调整 ES_JAVA_OPTS 参数
4. **无法连接**：检查防火墙和网络配置
5. **认证问题**：确认 xpack. Security. Enabled 设置

## 十、与 Kibana 集成

```bash
docker run -d --name kibana --net elastic -p 5601:5601 kibana:9.0.1
```

注意：确保 Elasticsearch 和 Kibana 版本一致。