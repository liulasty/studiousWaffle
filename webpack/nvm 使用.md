# Nvm (Node Version Manager) 中文使用指南

## 什么是 nvm？

Nvm 是一个 Node. Js 版本管理工具，允许你在同一台机器上安装和切换不同版本的 Node. Js。这对于开发者测试不同 Node 版本下的应用兼容性非常有用。

## 安装 nvm

### Linux/macOS 安装

1. 使用安装脚本：

```bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.7/install.sh | bash
```

或

```bash
wget -qO- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.7/install.sh | bash
```

2. 安装完成后，重新打开终端或运行：

```bash
source ~/.bashrc  # 或 ~/.zshrc, ~/.profile, 取决于你的shell
```

### Windows 安装

Windows 用户可以使用 [nvm-windows](https://github.com/coreybutler/nvm-windows)：

1. 下载最新安装包
2. 运行安装程序
3. 按照向导完成安装

## 基本使用方法

### 安装 Node. Js 版本

```bash
nvm install <version>  # 安装特定版本，如14.17.0
nvm install node       # 安装最新稳定版
nvm install --lts      # 安装最新的LTS版本
```

### 列出可用版本

```bash
nvm ls-remote         # 查看所有远程可用版本
nvm ls                # 查看本地已安装版本
```

### 切换 Node. Js 版本

```bash
nvm use <version>      # 切换到指定版本
nvm use node           # 使用最新安装的版本
nvm use --lts          # 使用最新的LTS版本
```

### 设置默认版本

```bash
nvm alias default <version>  # 设置默认版本
```

### 运行特定版本的 Node

```bash
nvm run <version> <script>  # 使用指定版本运行脚本
```

### 卸载 Node. Js 版本

```bash
nvm uninstall <version>     # 卸载特定版本
```

## 常用命令速查

| 命令 | 描述 |
|------|------|
| `nvm --version` | 查看 nvm 版本 |
| `nvm install` | 安装 Node. Js |
| `nvm uninstall` | 卸载 Node. Js |
| `nvm use` | 切换 Node 版本 |
| `nvm current` | 显示当前使用的版本 |
| `nvm ls` | 列出已安装版本 |
| `nvm ls-remote` | 列出远程可用版本 |
| `nvm alias` | 设置版本别名 |
| `nvm exec` | 使用指定版本运行命令 |
| `nvm which` | 显示 Node 安装路径 |

## 常见问题

### 安装后 nvm 命令找不到

确保你的 shell 配置文件 (~/. Bashrc, ~/. Zshrc 或~/. Profile) 中包含以下内容：

```bash
export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"  # 加载nvm
[ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"  # 加载nvm自动补全
```

### 在不同 shell 中使用 nvm

如果你使用 zsh 或其他 shell，确保相应的配置文件也包含上述内容。

### 代理设置

如果你在中国大陆，可能需要设置代理或使用国内镜像：

```bash
export NVM_NODEJS_ORG_MIRROR=https://npm.taobao.org/mirrors/node
```

## 进阶用法

### 为特定项目自动切换版本

在项目根目录创建 `.nvmrc` 文件，内容为 Node 版本号，然后运行：

```bash
nvm use
```

### 多版本并行运行

```bash
nvm exec <version> <command>  # 使用指定版本运行命令
```

### 查看版本详细信息

```bash
nvm version-remote <version>  # 查看远程版本详细信息
```

