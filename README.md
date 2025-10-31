# R2 二进制文件部署

这是一个使用 GitHub Actions 自动将最新的二进制文件部署到 Cloudflare R2 的项目。当前支持 `cloudflared`、`sing-box` 和 `xray`，但项目设计上具有良好的扩展性，未来可以轻松加入更多其他软件。

## 工作原理

项目通过 GitHub Actions 工作流实现全自动化。每个工作流负责一个软件，它会自动从该软件的官方发布页面获取最新版本的 `amd64` 和 `arm64` 架构的二进制文件，然后使用 `rclone` 将它们上传到 Cloudflare R2 存储桶中。最后，它会调用 Cloudflare API 清除缓存，以确保用户可以立即访问到最新版本。

## 当前支持的软件

*   **Cloudflared**: 部署 `cloudflared` 的二进制文件。
*   **Sing-box**: 部署 `sing-box` 的二进制文件。
*   **Xray**: 部署 `Xray-core` 的二进制文件。

## 如何使用

所有工作流都配置为手动触发 (`workflow_dispatch`)。

1.  在你的 GitHub 仓库页面，点击 **Actions** 标签。
2.  从左侧列表中选择你想要运行的工作流（例如 `Deploy Cloudflared Binaries to R2`）。
3.  点击 **Run workflow** 按钮来启动部署流程。

## 配置要求

为了使工作流能够成功运行，你需要在你的 GitHub 仓库的 `Settings > Secrets and variables > Actions` 中配置以下 `secrets`：

*   `R2_ACCESS_KEY_ID`: 你的 Cloudflare R2 访问密钥 ID。
*   `R2_SECRET_ACCESS_KEY`: 你的 Cloudflare R2 秘密访问密钥。
*   `R2_ACCOUNT_ID`: 你的 Cloudflare 账户 ID。
*   `CF_ZONE_ID`: 用于提供文件访问的 Cloudflare Zone ID。
*   `CF_API_TOKEN`: 一个拥有清除缓存权限的 Cloudflare API 令牌。
