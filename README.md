# 🚀 Docker Puller & Exporter

基于 **GitHub Actions** 的 Docker 镜像离线代下与导出工具。

专为群晖（Synology DSM Container Manager）、威联通（QNAP）、爱快、PVE、unRAID 等国内受限网络环境设计。通过海外 GitHub Runner 高速拉取镜像，自动打包为标准 `.tar` 离线文件并发布到 Releases，实现浏览器直接高速下载与一键离线导入。

---

## ✨ 功能特性

- ⚡ **高速无阻**：借助 GitHub Actions 海外骨干网络拉取镜像，彻底告别 Docker Hub 连接超时与网络限制。
- 📦 **免安装 Docker**：无需在本地电脑安装臃肿的 Docker Desktop 或配置虚拟机，网页端点一下即可打包。
- 💻 **多架构支持**：支持指定平台架构（默认 `linux/amd64`，适配群晖 DS220+、x86 PC，亦支持 `linux/arm64` 树莓派/Apple 芯片等）。
- 🔗 **Release 直链分发**：打包后自动上传至 Releases，配合各大 GitHub 加速源可跑满家庭宽带。
- 🔄 **群晖无损更新**：与群晖 Container Manager 的“导入”和“重置”流程完美契合，更新镜像保留所有容器设置与挂载数据。

---

## 🛠️ 首次使用配置（仅需一次）

为确保 Actions 有权限自动创建 Release 并上传镜像文件，需开启仓库写入权限：

1. 打开本仓库页面顶部的 **Settings**。
2. 点击左侧导航栏 **Actions** $\rightarrow$ **General**。
3. 滑动至页面最底部的 **Workflow permissions**，勾选 **`Read and write permissions`**。
4. 点击 **Save** 保存。

---

## 📖 使用步骤

### 1. 触发代下任务
1. 点击本仓库顶部的 **Actions** 标签页。
2. 在左侧列表中点击 **`Docker 镜像代下导出`**。
3. 点击右侧出现的 **`Run workflow`** 下拉菜单：
   - **Docker 镜像完整名称**：填入你需要下载的镜像（如 `jellyfin/jellyfin:latest`、`linuxserver/qbittorrent:latest`）。
   - **硬件架构**：群晖 x86 设备（如 DS220+、DS920+ 等）保持默认的 `linux/amd64` 即可。
4. 点击绿色 **`Run workflow`** 按钮开始运行（通常 1~2 分钟内完成）。

### 2. 下载打包好的镜像
1. 运行完成后（显示绿色对勾 $\checkmark$），进入仓库主页右侧的 **Releases**。
2. 找到刚生成的发布版本，点击下载 `.tar` 镜像文件。
   > **加速下载小技巧**：如果浏览器直接下载 GitHub 文件较慢，可右键复制该 `.tar` 的下载链接，在前面加上代理前缀（例如 `https://ghproxy.cn/`）即可实现国内满速下载。

---

## 📁 导入群晖并无损更新容器

以群晖 **Container Manager**（DSM 7.2+）为例：

### 第一步：导入离线映像
1. 打开群晖 **Container Manager** $\rightarrow$ 点击左侧 **【映像】**。
2. 点击顶部 **【操作】** $\rightarrow$ **【导入】** $\rightarrow$ **【从文件添加】**。
3. 选择刚刚下载的 `.tar` 镜像文件上传导入。

### 第二步：重置并应用新版本（无损更新）
1. 导入完成后，点击左侧 **【容器】** 页面。
2. 找到对应正在运行的容器，先将其 **【停止】**。
3. 选中该容器，点击顶部 **【操作】** $\rightarrow$ **【重置】**（Reset）。
   > **注意**：重置会使用新镜像重新构建容器，已配置的**端口映射、环境变量、文件夹装载路径都会完整保留**；只要数据保存在挂载的本地文件夹中，数据完全不受影响。
4. 重置完成后，点击 **【启动】** 容器即可完成更新。

### 第三步：清理旧映像（可选）
回到 **【映像】** 页面，点击顶部的 **【移除未使用映像】**，清理被替换的旧版镜像以释放 NAS 硬盘空间。

---

## 💡 常见问题

- **Q：GitHub Actions 免费吗？**  
  A：完全免费。公开（Public）仓库享有不限时长的 GitHub Actions 免费额度；私有仓库每月也有 2000 分钟免费额度，平时拉镜像完全用不完。
- **Q：镜像解压/打包后文件有多大限制？**  
  A：GitHub Releases 单个文件上传上限为 2 GB，大部分常见服务镜像（Jellyfin、qbittorrent、Alist 等）均能轻松打包。

---

## 📄 开源协议

本项目基于 [MIT License](LICENSE) 开源。
