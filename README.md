# GHCR to DockerHub Sync (With Prefix)

此仓库用于将 **公开的** GHCR 映像同步到 Docker Hub。
为了便于分类管理，所有同步的镜像在 Docker Hub 上都会自动加上 `ghcr-` 前缀。

例如：
*   源：`ghcr.io/user/app:v1`
*   配置文件名：`app`
*   **同步结果**：`docker.io/你的用户名/ghcr-app:v1`

仅同步 `amd64` 和 `arm64` 架构。

## ⚙️ Secrets 配置

请在 Settings -> Secrets -> Actions 中添加：

*   `DOCKERHUB_USERNAME`: 你的 Docker Hub 用户名。
*   `DOCKERHUB_TOKEN`: 你的 Docker Hub Access Token。

> 注意：在生成DOCKERHUB_TOKEN时，需要给予TOKEN read,write和delete权限。

## 🚀 使用方法

### 1. 添加镜像
在 `source_images/` 目录下创建文件。**文件名决定了 Docker Hub 上的仓库名后缀**。

**示例：**
你想同步 `ghcr.io/foo/bar:latest` 到你的 `ghcr-mybar` 仓库。

1. 创建文件：`source_images/mybar`
2. 内容：`ghcr.io/foo/bar:latest`
3. 提交代码。

最终 Docker Hub 地址为：`你的用户名/ghcr-mybar:latest`。

### 2. 自动同步
Push 代码后，GitHub Actions 会自动：
1. 读取源地址。
2. 提取 `amd64` 和 `arm64` 架构。
3. 加上 `ghcr-` 前缀推送到你的 Docker Hub。
4. 记录状态到 `pushed_state/` 防止重复推送。
5. 自动更新DockerHub中仓库的描述信息，说明是从ghcr中同步过来的映像。

## ❓ 常见问题

**Q: 为什么是 `ghcr-app` 而不是 `ghcr/app`？**
A: Docker Hub 不支持在用户名下创建子文件夹（例如 `user/folder/repo` 是非法的）。因此我们使用前缀 `ghcr-` 来区分这些同步过来的镜像。
