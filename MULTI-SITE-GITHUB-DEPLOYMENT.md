# 多网站批量生成与 GitHub 自动部署指南

本指南详细介绍如何使用多网站生成系统批量创建网站并自动部署到 GitHub Pages。

## 📋 功能概述

系统现在支持：
- ✅ 从 CSV 文件读取多个网站配置
- ✅ 自动生成每个网站的内容
- ✅ 自动创建 GitHub 仓库（每个网站独立仓库）
- ✅ 自动配置 Astro 部署设置
- ✅ 自动推送代码到 GitHub
- ✅ 自动触发 GitHub Pages 部署
- ✅ 批量处理多个网站

## 🚀 快速开始

### 1. 前置准备

#### 安装 GitHub CLI

**Windows:**
```bash
winget install --id GitHub.cli
```

或访问 https://cli.github.com/ 下载安装

**验证安装:**
```bash
gh --version
```

#### 登录 GitHub

```bash
gh auth login
```

选择：
- GitHub.com
- HTTPS
- Login with a web browser

### 2. 准备 CSV 配置文件

编辑 `websites-config.csv` 文件：

```csv
theme,domain,siteName,adsenseCode
Automotive & Mobility,autotech.example.com,AutoTech Hub,ca-pub-1234567890123456
Travel & Adventure,travelblog.example.com,Travel Explorer,ca-pub-2345678901234567
Technology & Innovation,techvision.example.com,Tech Vision,ca-pub-3456789012345678
```

**字段说明:**
- `theme`: 网站主题（必填）
- `domain`: 网站域名（必填）
- `siteName`: 网站名称（必填，将用于生成仓库名）
- `adsenseCode`: Google AdSense 代码（可选）

### 3. 执行批量生成和部署

**生成所有网站:**
```bash
npm run mul-reset-site
```

**从第 2 个网站开始生成:**
```bash
npm run mul-reset-site -- --start=2
```

**只生成前 3 个网站:**
```bash
npm run mul-reset-site -- --limit=3
```

**生成第 2-4 个网站:**
```bash
npm run mul-reset-site -- --start=2 --limit=3
```

**使用自定义配置文件:**
```bash
npm run mul-reset-site -- --config=my-websites.csv
```

## 📊 执行流程

每个网站的完整流程：

### 1. 网站生成阶段
1. ✅ 清空 HTML 文章
2. ✅ 删除所有现有文章
3. ✅ 更新主题配置
4. ✅ 更新文章配置并重置追踪
5. ✅ 生成文章
6. ✅ 同步配置到模板
7. ✅ 添加新文章到网站
8. ✅ 生成新主题方向
9. ✅ 生成定时发布文章
10. ✅ 设置文章定时发布
11. ✅ 生成 AI 图标
12. ✅ 生成图标文件
13. ✅ 更新网站图标

### 2. GitHub 自动部署阶段
1. ✅ **创建 GitHub 仓库**
   - 根据网站名称自动生成仓库名
   - 检查名称冲突并自动调整
   - 创建公开仓库

2. ✅ **配置部署文件**
   - 自动创建 `.github/workflows/deploy.yml`
   - 配置 GitHub Actions 工作流

3. ✅ **配置 Astro**
   - 更新 `astro.config.mjs`
   - 设置正确的 `site` 和 `base` 路径
   - 确保静态输出模式

4. ✅ **构建网站**
   - 执行 `npm run build`
   - 生成静态文件到 `dist` 目录

5. ✅ **推送到 GitHub**
   - 初始化 Git 仓库（如需要）
   - 添加远程仓库
   - 提交所有文件
   - 推送到 GitHub

6. ✅ **触发 GitHub Pages**
   - GitHub Actions 自动运行
   - 构建并部署到 GitHub Pages

## 🔧 仓库命名规则

系统会自动根据网站名称生成仓库名：

| 网站名称 | 生成的仓库名 |
|---------|------------|
| AutoTech Hub | autotech-hub |
| Travel Explorer | travel-explorer |
| Tech Vision | tech-vision |
| My Cool Site! | my-cool-site |

**规则:**
- 转换为小写
- 移除特殊字符
- 空格替换为连字符
- 如果名称冲突，自动添加后缀 `-1`, `-2` 等

## 📁 生成的文件结构

每个网站部署后的结构：

```
your-repo/
├── .github/
│   └── workflows/
│       └── deploy.yml          # GitHub Actions 工作流
├── src/
│   ├── content/                # 网站内容
│   ├── pages/                  # 页面
│   └── ...
├── astro.config.mjs            # Astro 配置（已更新）
├── package.json
└── dist/                       # 构建输出（部署到 Pages）
```

## 🌐 访问部署的网站

每个网站部署后可以通过以下 URL 访问：

```
https://{github-username}.github.io/{repo-name}/
```

例如：
- 用户名: `jaydonke`
- 仓库名: `autotech-hub`
- 网站 URL: `https://jaydonke.github.io/autotech-hub/`

## 📊 监控部署状态

### 方法 1: GitHub Actions 页面

访问每个仓库的 Actions 页面：
```
https://github.com/{username}/{repo-name}/actions
```

### 方法 2: 使用 GitHub CLI

```bash
# 查看最近的 workflow 运行
gh run list --repo jaydonke/autotech-hub --limit 5

# 查看特定 run 的详情
gh run view <run-id> --repo jaydonke/autotech-hub

# 实时查看 run 日志
gh run watch <run-id> --repo jaydonke/autotech-hub
```

## ⚠️ 常见问题

### Q1: 仓库名称冲突

**问题:** 生成的仓库名已存在

**解决:** 系统会自动在名称后添加数字后缀 (`-1`, `-2` 等)

### Q2: 构建失败

**问题:** `npm run build` 失败

**检查:**
1. 确保所有依赖已安装: `npm install`
2. 检查 `astro.config.mjs` 配置是否正确
3. 查看构建日志中的具体错误

### Q3: GitHub Pages 404 错误

**问题:** 网站显示 404

**解决步骤:**
1. 检查仓库的 Settings > Pages 是否已启用
2. Source 应设置为 "GitHub Actions"
3. 确保 `astro.config.mjs` 中的 `base` 路径正确
4. 等待几分钟让 DNS 生效

### Q4: 部署超时

**问题:** 等待部署时间过长

**说明:** GitHub Actions 首次部署可能需要 5-10 分钟，这是正常的

## 🔐 环境变量和密钥

如果你的网站需要环境变量（如 OpenAI API Key）：

1. 在 GitHub 仓库设置中添加 Secrets:
   - Settings > Secrets and variables > Actions
   - 添加 `OPENAI_API_KEY` 等密钥

2. 更新 `.github/workflows/deploy.yml`:
```yaml
- name: Build with Astro
  run: npm run build
  env:
    NODE_ENV: production
    OPENAI_API_KEY: ${{ secrets.OPENAI_API_KEY }}
```

## 📈 批量处理示例

### 示例 1: 生成 10 个网站

```bash
# 准备包含 10 个网站的 CSV 文件
npm run mul-reset-site -- --limit=10
```

**预期时间:** 每个网站约 3-5 分钟，10 个网站约 30-50 分钟

### 示例 2: 分批处理

```bash
# 第一批：网站 1-5
npm run mul-reset-site -- --start=1 --limit=5

# 第二批：网站 6-10
npm run mul-reset-site -- --start=6 --limit=5

# 第三批：网站 11-15
npm run mul-reset-site -- --start=11 --limit=5
```

### 示例 3: 重新部署单个网站

```bash
# 只处理第 3 个网站
npm run mul-reset-site -- --start=3 --limit=1
```

## 🎯 最佳实践

1. **小批量测试**
   - 先用 `--limit=1` 测试单个网站
   - 确认流程正常后再批量处理

2. **避免 API 限流**
   - 系统会在网站之间自动等待 10 秒
   - 不要同时运行多个批量任务

3. **备份配置**
   - 系统会自动备份配置到 `config-backups/`
   - 保留原始 CSV 文件的副本

4. **监控资源**
   - GitHub 有 API 频率限制
   - 注意 GitHub Actions 的使用配额

5. **命名规范**
   - 使用清晰、描述性的网站名称
   - 避免使用特殊字符和过长的名称

## 🛠️ 故障排除

### 检查 GitHub CLI 状态

```bash
gh auth status
```

### 重新登录

```bash
gh auth logout
gh auth login
```

### 查看详细日志

系统会输出详细的彩色日志：
- 🔵 蓝色：信息
- 🟢 绿色：成功
- 🟡 黄色：警告
- 🔴 红色：错误

### 手动清理

如果需要清理失败的部署：

```bash
# 删除远程仓库（谨慎使用！）
gh repo delete {username}/{repo-name} --yes

# 重置本地 Git
rm -rf .git
git init
```

## 📞 获取帮助

如果遇到问题：

1. 查看 GitHub Actions 日志
2. 检查控制台输出的错误信息
3. 确认所有前置条件已满足
4. 查看本文档的常见问题部分

## 🎉 成功示例

完整的成功输出应该类似：

```
═══════════════════════════════════════════════════════════
   网站 1/3: AutoTech Hub
═══════════════════════════════════════════════════════════

📋 验证网站配置...
   主题: Automotive & Mobility
   域名: autotech.example.com
   网站名: AutoTech Hub

🚀 执行网站生成流程...
[1/13] 清空HTML文章
   ✅ 清空HTML文章 完成
   ...

📦 自动部署到GitHub...
╔═══════════════════════════════════════════════════════╗
║          GitHub 自动部署流程                          ║
╚═══════════════════════════════════════════════════════╝

📦 创建 GitHub 仓库...
   用户名: jaydonke
   仓库名: autotech-hub
   创建仓库: autotech-hub
✅ 仓库创建成功: jaydonke/autotech-hub

⚙️  配置 Astro 用于 GitHub Pages...
✅ Astro 配置已更新
   Site: https://jaydonke.github.io
   Base: /autotech-hub

🔨 构建网站...
✅ 网站构建成功

🚀 推送代码到 GitHub...
✅ 代码推送成功

🎉 部署完成！
   仓库: https://github.com/jaydonke/autotech-hub.git
   网站: https://jaydonke.github.io/autotech-hub
   Actions: https://github.com/jaydonke/autotech-hub/actions

🎉 网站 "AutoTech Hub" 生成并部署成功！
   用时: 4.2 分钟
```

---

**享受你的批量网站部署吧！** 🚀
