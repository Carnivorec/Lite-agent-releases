# Lite-agent 发布仓库

本仓库只用于发布 Lite-agent 的编译产物，源码保存在私有仓库中。发布内容包括：

- **Release**：各平台二进制文件（`Lite-agent-<系统>-<架构>`）和 `SHA256SUMS` 校验文件。
- **安装脚本**：仓库根目录下的 `install.sh`（Linux / macOS）和 `install.ps1`（Windows），由发布流程从源码仓库自动同步，与同一次发布的二进制文件保持一致。
- **Docker 镜像**：`ghcr.io/carnivorec/lite-agent`，提供 `latest` 和具体版本两种标签。

## 默认安全设置

- **自动更新默认关闭**：需要在安装命令中加上 `--enable-auto-update` 才会开启。开启后只从本仓库获取更新。
- **远程控制默认关闭**：终端、文件管理、命令执行和 MCP 默认全部关闭，需要在节点本机通过 `--enable-remote-control` 开启。管理面板不能远程打开此开关。

## 安装

建议在 Lite 管理面板中生成安装命令，命令会自动带上节点地址和 token。手动安装示例：

```bash
wget -qO- https://raw.githubusercontent.com/Carnivorec/Lite-agent-releases/main/install.sh | sudo bash -s -- -e https://你的主控地址 -t 节点token
```

## 发布流程（维护者）

在本仓库的 Actions 页面手动运行 **Release Lite-agent**，填写：

- `version`：四段版本号，例如 `2.3.11.0`。
- `source_ref`：私有源码仓库的分支、标签或提交，默认为 `main`。

工作流通过只读部署密钥（Secret `LITE_AGENT_DEPLOY_KEY`）拉取私有源码，编译全部平台，发布 Release，推送 Docker 镜像，并同步安装脚本。测试在私有仓库中运行，本仓库的公开日志不输出源码内容。
