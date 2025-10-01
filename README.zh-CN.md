# workflow-visit-website

[English](./README.md) | 简体中文

这个 GitHub Actions 工作流主要用于通过每日自动访问 [onedrive-index](https://github.com/iRedScarf/onedrive-index) 部署的页面，以在后台调用 Microsoft Graph API，保持 Microsoft 365 E5 开发者订阅的活跃状态。

## 工作原理

  - 工作流按计划定时运行（你可以在 `.github/workflows/visit.yml` 中调整 cron 设置）。
  - 它使用 Playwright 从你配置的网站列表中访问多个网站。
  - 每次运行会：
    - 从列表中随机选择 3–5 个网站（如果少于 3 个，则访问所有）。
    - 在每个网站停留 30–60 秒的随机时间。
    - 可选：尝试点击页面上的 “Home” 链接，并再停留 5 秒。
    - 在访问下一个网站前随机等待 30–90 秒，以模拟真实浏览行为。

所有逻辑实现都在 `/visit-website/visitWebsite.js` 文件中。

如果你想调整延时、增加更多交互或模拟其他浏览行为，可以修改这个文件。

## 配置方法

### 1. 添加你要访问的网站

编辑 `/visit-website/websites.json` 文件，把你希望定时访问的 URL 填进去。

示例：
```json
[
  "https://your-onedrive-index-site-1.com",
  "https://your-onedrive-index-site-2.com"
]
```

### 2. 启用工作流

GitHub Actions 工作流文件 `.github/workflows/visit.yml` 支持手动运行或定时运行。

若要每天自动运行，可在其中的 schedule 部分配置 cron 表达式（例如每天北京时间 04:00 执行）。

### 3. 手动运行（可选）

进入你的仓库 → Actions → 选择该工作流 → 点击 “Run workflow”。

## 注意事项与限制
  - 本项目的主要目的仅限于保持 E5 开发者订阅活跃，不应用于压力测试或爬虫。
  - 注意 GitHub Actions 的免费[使用限制]((https://docs.github.com/zh/actions/reference/limits))：
    - 免费账户每月有 2000 分钟（截至 2025 年）。
    - 本工作流包含随机延时（每站点停留 30–60 秒 + 网站间间隔 30–90 秒）。
    - 在修改访问数量和延时时，请谨慎计算，以免超出 GitHub Actions 免费额度。
  - 对于私有仓库，使用分钟数可能会因 GitHub 计划不同而更快消耗。

## 自定义方法
  - 修改 `/visit-website/visitWebsite.js` 来模拟不同的用户交互。
  - 在 `/visit-website/websites.json` 中添加任意数量的 URL。
  - 可以与其他工作流（例如[workflow-m365-audit-logs](https://github.com/ailoha/workflow-m365-audit-logs)）组合使用，以更全面地保持 E5 活跃度。
