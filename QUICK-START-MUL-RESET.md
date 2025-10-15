# 多网站自动化生成 - 快速开始指南

## 🚀 5分钟快速上手

### 步骤1️⃣：准备环境

确保已安装：
- ✅ Node.js 18+
- ✅ Git
- ✅ OpenAI API密钥（在 `.env` 文件中配置）

```bash
# 检查Node.js版本
node --version

# 检查Git
git --version
```

---

### 步骤2️⃣：创建配置文件

创建或编辑 `websites-config.csv` 文件：

```csv
theme,domain,siteName,adsenseCode
Automotive & Mobility,autosite.com,AutoSite,ca-pub-1234567890123456
Travel & Adventure,travelhub.com,TravelHub,ca-pub-2345678901234567
Technology & Innovation,techvision.com,TechVision,ca-pub-3456789012345678
```

**字段说明**：
- `theme` - 网站主题（例如：Automotive & Mobility）
- `domain` - 域名（例如：example.com，不要加http://）
- `siteName` - 网站名称（例如：Example Site）
- `adsenseCode` - Google AdSense代码（可选，格式：ca-pub-xxxxxxxxxxxxxxxx）

---

### 步骤3️⃣：运行批量生成

**方法1：使用npm命令**
```bash
npm run mul-reset-site
```

**方法2：使用批处理文件（Windows）**
```bash
scripts\mul-reset-site.bat
```

**方法3：直接运行脚本**
```bash
node scripts/mul-reset-site.js
```

---

### 步骤4️⃣：等待完成

脚本会自动：
1. ✅ 读取配置文件中的所有网站
2. ✅ 依次处理每个网站（生成40篇文章、配置、图标等）
3. ✅ 每个网站生成后自动部署到GitHub
4. ✅ 显示详细的进度和结果

**预计时间**：
- 单个网站：20-35分钟
- 3个网站：约1-2小时
- 5个网站：约2-3小时

---

## 📊 配置示例

### 基础配置（最简单）

```csv
theme,domain,siteName,adsenseCode
Health & Wellness,healthylife.com,HealthyLife,ca-pub-1234567890123456
```

### 多个网站配置

```csv
theme,domain,siteName,adsenseCode
Automotive & Mobility,autoworld.com,AutoWorld,ca-pub-1111111111111111
Travel & Adventure,wanderlust.com,Wanderlust,ca-pub-2222222222222222
Technology & AI,aitoday.com,AIToday,ca-pub-3333333333333333
Finance & Investing,smartmoney.com,SmartMoney,ca-pub-4444444444444444
Food & Cooking,tastybites.com,TastyBites,ca-pub-5555555555555555
```

### 完整配置（包含GitHub仓库）

```csv
theme,domain,siteName,adsenseCode,repoUrl,branch
Automotive,auto.com,AutoSite,ca-pub-1111111111111111,https://github.com/user/auto.git,main
Travel,travel.com,TravelSite,ca-pub-2222222222222222,https://github.com/user/travel.git,main
```

---

## 🎯 高级用法

### 只处理前3个网站

```bash
npm run mul-reset-site -- --limit=3
```

### 从第5个网站开始处理

```bash
npm run mul-reset-site -- --start=5
```

### 处理第3到第7个网站

```bash
npm run mul-reset-site -- --start=3 --limit=5
```

### 使用自定义配置文件

```bash
npm run mul-reset-site -- --config=my-sites.csv
```

---

## 📋 执行流程概览

```
┌─────────────────────────────────────────────────────────┐
│  读取 websites-config.csv                                │
└──────────────────┬──────────────────────────────────────┘
                   │
                   ▼
┌─────────────────────────────────────────────────────────┐
│  网站 1: AutoSite                                        │
├─────────────────────────────────────────────────────────┤
│  1. 验证配置                                             │
│  2. 备份当前配置                                         │
│  3. 写入新配置                                           │
│  4. 执行 reset-site (13步骤)                             │
│     • 清空文章                                           │
│     • AI生成主题配置                                     │
│     • AI生成40篇文章                                     │
│     • 生成图标                                           │
│  5. 部署到GitHub                                         │
└──────────────────┬──────────────────────────────────────┘
                   │
                   ▼  等待10秒
┌─────────────────────────────────────────────────────────┐
│  网站 2: TravelHub                                       │
├─────────────────────────────────────────────────────────┤
│  重复上述步骤...                                         │
└──────────────────┬──────────────────────────────────────┘
                   │
                   ▼  等待10秒
┌─────────────────────────────────────────────────────────┐
│  网站 3: TechVision                                      │
├─────────────────────────────────────────────────────────┤
│  重复上述步骤...                                         │
└──────────────────┬──────────────────────────────────────┘
                   │
                   ▼
┌─────────────────────────────────────────────────────────┐
│  生成总结报告                                            │
│  • 成功: 3/3                                             │
│  • 失败: 0/3                                             │
│  • 总用时: 75 分钟                                       │
└─────────────────────────────────────────────────────────┘
```

---

## ✅ 检查清单

执行前确认：

- [ ] 已安装Node.js 18+
- [ ] 已安装Git
- [ ] `.env` 文件中配置了 `OPENAI_API_KEY`
- [ ] 已创建 `websites-config.csv` 文件
- [ ] 配置文件格式正确（逗号分隔，包含表头）
- [ ] OpenAI账户有足够的API配额
- [ ] 网络连接稳定

---

## ❗ 常见问题

### Q1: 脚本中途停止了怎么办？

A: 使用 `--start` 参数从停止的位置继续：
```bash
# 如果在第3个网站停止，从第4个继续
npm run mul-reset-site -- --start=4
```

### Q2: 如何只测试一个网站？

A: 创建只有一行数据的配置文件，或使用 `--limit=1`：
```bash
npm run mul-reset-site -- --limit=1
```

### Q3: OpenAI API配额不足怎么办？

A:
- 检查OpenAI账户余额
- 分批处理网站
- 增加网站之间的等待时间

### Q4: Git推送失败怎么办？

A:
```bash
# 检查Git配置
git config --list

# 配置用户信息（如果需要）
git config user.name "Your Name"
git config user.email "your@email.com"
```

### Q5: 如何跳过部署步骤？

A: 在配置文件中不提供 `repoUrl` 列，或临时注释掉部署代码。

---

## 📝 配置文件模板

复制以下模板，根据需要修改：

### 模板1：基础版（2个网站）
```csv
theme,domain,siteName,adsenseCode
主题名称1,域名1.com,网站名称1,ca-pub-1234567890123456
主题名称2,域名2.com,网站名称2,ca-pub-2345678901234567
```

### 模板2：标准版（5个网站）
```csv
theme,domain,siteName,adsenseCode
Automotive & Mobility,auto1.com,AutoSite1,ca-pub-1111111111111111
Travel & Adventure,travel1.com,TravelSite1,ca-pub-2222222222222222
Technology & Innovation,tech1.com,TechSite1,ca-pub-3333333333333333
Health & Wellness,health1.com,HealthSite1,ca-pub-4444444444444444
Finance & Investment,finance1.com,FinanceSite1,ca-pub-5555555555555555
```

### 模板3：完整版（含GitHub）
```csv
theme,domain,siteName,adsenseCode,repoUrl,branch
Automotive & Mobility,auto1.com,AutoSite1,ca-pub-1111111111111111,https://github.com/user/auto.git,main
Travel & Adventure,travel1.com,TravelSite1,ca-pub-2222222222222222,https://github.com/user/travel.git,main
Technology & Innovation,tech1.com,TechSite1,ca-pub-3333333333333333,https://github.com/user/tech.git,gh-pages
```

---

## 🎉 完成后

生成完成后，每个网站会：

✅ 包含40篇AI生成的高质量文章（25篇立即发布 + 15篇定时发布）
✅ 完整的网站配置（导航、页脚、SEO等）
✅ 自定义的网站图标
✅ 优化的内链系统
✅ 自动部署到GitHub

你可以：
1. 访问 GitHub 查看提交记录
2. 运行 `npm run build` 构建生产版本
3. 部署到你的托管平台（Netlify、Vercel等）

---

## 📚 更多资源

- [详细使用指南](MUL-RESET-SITE-GUIDE.md) - 完整的功能说明和故障排除
- [项目README](README.md) - 项目总体介绍
- [单网站生成](scripts/reset-site.js) - 单个网站的生成流程

---

**祝你使用愉快！** 🚀

有问题请查看 [MUL-RESET-SITE-GUIDE.md](MUL-RESET-SITE-GUIDE.md) 的故障排除部分。
