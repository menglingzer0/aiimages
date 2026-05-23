# 文生图 · Cloudflare Workers（功能增强版） 🎨☁️
---

## 🙏 致谢

本项目基于并借鉴了以下优秀开源项目的工作，在此表示感谢：

- huarzone/Text2img-Cloudflare-Workers — https://github.com/huarzone/Text2img-Cloudflare-Workers
- zhumengkang/cf-ai-image — https://github.com/zhumengkang/cf-ai-image  

---
一个功能强大、开箱即用的在线「文生图 / 图生图」服务，完全基于 Cloudflare Workers AI 平台构建，侧重隐私保护、易用性与企业级安全配置。

![应用截图（浅色）](https://github.com/huarzone/Text2img-Cloudflare-Workers/raw/main/public/top.png)

---

## ✨ 功能亮点

本项目在融合多个开源方案的基础上，进行了大量增强与体验优化，主要特性：

- **全能模型支持**
  - 支持文生图（SDXL、FLUX）、图生图、局部重绘等主流模型。
- **本地文件上传**
  - 支持从本地直接上传图片用于图生图 / 局部重绘，免去外链繁琐操作。
- **批量生成与下载**
  - 支持一次性生成多张图片、画廊预览，并提供 ZIP 批量下载。
- **智能参数建议**
  - 根据选定模型自动推荐合适的步数与引导系数，降低使用门槛。
- **企业级安全配置**
  - **环境变量密码**：通过 Cloudflare 环境变量设置访问密码，安全且易管理。
  - **IP 速率限制**：内置基于 IP 的请求频率限制，防止滥用。
  - **R2 临时存储**：上传图片存放于 R2 并自动过期，保护用户隐私与存储空间。
- **优秀用户体验**
  - 明/暗主题切换、响应式设计、移动端友好。
  - 实时生成进度条（带超时），一键复制生成参数以便分享与复现。

---

## 🚀 部署教程（快速上手）

> 前置条件：一个有效的 Cloudflare 账号。以下步骤在 Cloudflare 控制台中操作。


### 第 1 步：部署 Worker 并上传代码
1. 创建 Worker 服务  
   - Cloudflare 控制台 → Workers & Pages → 创建应用程序 → 创建 Worker。为 Worker 选择唯一子域名并部署。
2. 上传代码文件  
   - 点击“编辑代码”进入在线编辑器。将本项目的 `worker.js` 代码完整粘贴到默认文件（如 `index.js` 或 `_worker.js`）中，替换原有内容。  
   - 新建文件 `index.html`，将本项目的 `index.html` 内容完整粘贴进去。
---

### 第 2 步：准备（Cloudflare 控制台）
1. 创建 R2 存储桶（用于图片上传）
   - Cloudflare 控制台 → R2 → 创建存储桶（示例名称：`ai-image`）。
2. 创建 KV 命名空间（用于速率限制）
   - Cloudflare 控制台 → Workers & Pages → KV → 创建命名空间（示例名称：`AI_RATE_LIMITER`）。

---

### 第 3 步：绑定与环境变量（关键）
1. 绑定（Settings → Bindings）
   - 添加以下绑定（三次点击“添加绑定”）：

     | 绑定类型 | 变量名称 | 说明 |
     |---|---:|---|
     | Workers AI | AI | （无需选择命名空间） |
     | R2 存储桶 | IMAGE_BUCKET | 选择第 1 步创建的 R2 存储桶（例如 `ai-image`） |
     | KV 命名空间 | RATE_LIMITER_KV | 选择第 1 步创建的 KV 命名空间（例如 `AI_RATE_LIMITER`） |

2. 设置环境变量（Settings → Variables）
   - 添加访问密码（可选）：
     - 变量名称：`PASSWORDS`
     - 变量值：例如 `10000`
     - 提示：若不设置此变量或留空，则站点公开无需密码。

示例：部署完成后，Worker 地址通常形如：
https://<你的名称>.<你的子域>.workers.dev

---

### 第 4 步：完成部署
1. 在编辑器右上角点击 “部署”。  
2. 等待部署完成后，通过你的 Worker 地址访问并开始使用。

---

## 常用配置摘要（便于复制粘贴） 🧾

示例环境变量与绑定（仅供参考）
```
绑定：
- AI                -> Workers AI（内置）
- IMAGE_BUCKET      -> R2 存储桶（选择 ai-image-uploads）
- RATE_LIMITER_KV   -> KV 命名空间（选择 AI_RATE_LIMITER）

环境变量：
- PASSWORDS = 10000   # 访问密码（可选）
```

---

## 🙏 致谢

本项目基于并借鉴了以下优秀开源项目的工作，在此表示感谢：

- zhumengkang/cf-ai-image — https://github.com/zhumengkang/cf-ai-image  
- huarzone/Text2img-Cloudflare-Workers — https://github.com/huarzone/Text2img-Cloudflare-Workers

---
