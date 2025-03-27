# ZeroDBlog

ZeroDBlog 是一个基于Nuxt3和Nuxt Content的博客系统。无需数据库存储，通过Github API 更新数据，基于Github和Vercel自动化搭建博客。

## 快速开始

### 创建项目

通过`Fork`或`Use this template`按钮创建项目即可。

同步模版工作流：

```yaml [.github/workflows/sync.yml]
name: Sync with template

on:
  # cronjob trigger
  schedule:
    # 12:00 per day
    - cron: '0 0 12 * * ?'
  # manual trigger
  workflow_dispatch:

jobs:
  sync:
    runs-on: ubuntu-latest
    permissions:
      contents: write
      pull-requests: write

    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Sync Template
        uses: AndreasAugustin/actions-template-sync@v2.5.1
        with:
          source_repo_path: sunshj/ZeroDBlog
          source_gh_token: ${{ secrets.GITHUB_TOKEN }}
```

### 必要环境变量

```env
# Seo
NUXT_SITE_URL="https://<your-domain>"
NUXT_SITE_NAME="My Blog"

# Github Repo
NUXT_GITHUB_REPO="nuxt-blog"

# Auth Utils (32位字符)
NUXT_SESSION_PASSWORD="********************************"
# Github OAuth
NUXT_OAUTH_GITHUB_CLIENT_ID="********************"
NUXT_OAUTH_GITHUB_CLIENT_SECRET="****************************************"

```

### 创建Github OAuth App

1. 访问 https://github.com/settings/developers
2. 点击 New OAuth App
3. 填写应用信息
   - Application name: ZeroDBlog
   - Homepage URL: `<your-blog-url>`
   - Authorization callback URL: `<your-blog-url>`/auth/github
4. 获取 Client ID 和 Client Secret

### 在线编写博客

访问 `<your-blog-url>`/dashboard，使用Github账号登录，即可在线编写博客。

> 点击推送按钮后，会自动将修改后的文章提交到Github仓库并触发Vercel 部署，等待大约1分钟，即可查看修改后的文章。

## 配置

### 添加Giscus评论系统

1. 需要创建一个公开的Github仓库用于存储评论，例如`blog-comments`。
2. 访问 https://giscus.app/zh-CN ，填写仓库和Discussion分类，在`启用 giscus`中获取以下数据:
   - data-repo
   - data-repo-id
   - data-category
   - data-category-id
3. 修改博客仓库中`app/app.config.ts`中`giscus`配置项(或者在`<your-blog-url>/dashboard/config`中修改)，将获取到的数据填入对应的字段中。

### 配置图床

1. 需要创建一个公开的Github仓库用于存储图片，例如`blog-images`。
2. 修改博客仓库中`app/app.config.ts`中`imageBed`配置项(或者在`<your-blog-url>/dashboard/config`中修改)，将仓库名、图片存放目录填入对应的字段中。
