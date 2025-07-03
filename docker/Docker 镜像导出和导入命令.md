# Docker 镜像导出和导入命令

## 导出镜像

### 1. 保存镜像为 tar 文件 (docker save)
将镜像保存为一个 tar 归档文件，适用于后续重新导入到 Docker 中。

```bash
docker save -o <输出文件名>.tar <镜像名>:<标签>
# 或
docker save <镜像名>:<标签> > <输出文件名>.tar
```

示例：
```bash
docker save -o my_ubuntu_image.tar ubuntu:20.04
```

### 2. 导出容器文件系统 (docker export)
将容器的文件系统导出为 tar 归档文件（不包含镜像历史、元数据等信息）。

```bash
docker export -o <输出文件名>.tar <容器ID或名称>
# 或
docker export <容器ID或名称> > <输出文件名>.tar
```

示例：
```bash
docker export -o my_container.tar my_running_container
```

## 导入镜像

### 1. 从 tar 文件加载镜像 (docker load)
导入由 `docker save` 命令导出的镜像。

```bash
docker load -i <文件名>.tar
# 或
docker load < <文件名>.tar
```

示例：
```bash
docker load -i my_ubuntu_image.tar
```

### 2. 从容器文件系统创建镜像 (docker import)
从由 `docker export` 导出的文件系统创建新镜像。

```bash
docker import <文件名>.tar [新镜像名]:[标签]
```

示例：
```bash
docker import my_container.tar my_ubuntu:imported
```

## 主要区别

| 命令组合 | 包含内容 | 适用场景 |
|---------|---------|---------|
| `docker save` + `docker load` | 完整镜像（包括所有层、历史、元数据） | 镜像备份和迁移 |
| `docker export` + `docker import` | 仅容器文件系统快照（无历史、元数据） | 容器状态快照 |

## 其他相关命令

- 查看镜像列表：`docker images`
- 查看容器列表：`docker ps -a`
- 查看导入/导出进度：添加 `-v` 或 `--progress` 参数（某些 Docker 版本支持）