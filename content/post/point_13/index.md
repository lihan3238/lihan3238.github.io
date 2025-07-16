---
title: Hugo+GitHubPages 报错 timeout 超时
slug: point_13
date: 2025-04-05 03:47:00+0800
image: hugo.png
categories:
    - techStudy
tags:
    - computer
    - point
    - hugo
    - golang
#weight: 1       # You can add weight to some posts to override the default sorting (date descending)
comments: true

#license: flase
#math: true
#toc: true
#style: 
#keywords:
#readingTime:
---

# 问题

![err](err.png)

```
Run hugo \
hugo: collected modules in 1126 ms
Start building sites … 
hugo v0.123.8-5fed9c591b694f314e5939548e11cc3dcb79a79c+extended linux/amd64 BuildDate=2024-03-07T13:14:42Z VendorInfo=gohugoio

ERROR render of "home" failed: "/tmp/hugo_cache_runner/modules/filecache/modules/pkg/mod/github.com/!cai!jimmy/hugo-theme-stack/v3@v3.30.0/layouts/index.html:9:15": execute of template failed: template: index.html:9:15: executing "main" at <partial "article-list/default" .>: error calling partial: "/tmp/hugo_cache_runner/modules/filecache/modules/pkg/mod/github.com/!cai!jimmy/hugo-theme-stack/v3@v3.30.0/layouts/partials/article-list/default.html:3:7": execute of template failed: template: partials/article-list/default.html:3:7: executing "partials/article-list/default.html" at <partial "article/components/header" .>: error calling partial: partial "article/components/header" timed out after 30s. This is most likely due to infinite recursion. If this is just a slow template, you can try to increase the 'timeout' config setting.
Total in 70757 ms
Error: error building site: render: failed to render pages: render of "home" failed: "/tmp/hugo_cache_runner/modules/filecache/modules/pkg/mod/github.com/!cai!jimmy/hugo-theme-stack/v3@v3.30.0/layouts/index.html:9:15": execute of template failed: template: index.html:9:15: executing "main" at <partial "article-list/default" .>: error calling partial: "/tmp/hugo_cache_runner/modules/filecache/modules/pkg/mod/github.com/!cai!jimmy/hugo-theme-stack/v3@v3.30.0/layouts/partials/article-list/default.html:3:7": execute of template failed: template: partials/article-list/default.html:3:7: executing "partials/article-list/default.html" at <partial "article/components/header" .>: error calling partial: partial "article/components/header" timed out after 30s. This is most likely due to infinite recursion. If this is just a slow template, you can try to increase the 'timeout' config setting.
Error: Process completed with exit code 1.
```

Hugo + Stack 部署在 GitHub Pages 下，一次正常更新推文后，突然出现部署失败，报错如上。

# 解决 

由于在`.gitignore`中添加了`public/` `resources/`，导致在 GitHub Actions 每次会重新生成所有图片等资源，如果过多就会导致超时。

将`config\_default\hugo.toml`中的`timeout`设置为`120`（单位为秒），可以解决问题。

```toml
timeout = 120
```

# 一劳永逸

正确调用 Github Actions 的 Cache 功能，避免每次都重新生成所有资源。

1. 在`config/_default/caches.toml`中添加缓存配置：

```toml

[images]
  dir = ':cacheDir/images'

```

2. 在`.github/workflows/hugo.yml`中添加缓存配置：

```yaml

            ······

      - name: Cache Restore
        id: cache-restore
        uses: actions/cache/restore@v4
        with:
          path: ${{ runner.temp }}/hugo_cache
          key: hugo-${{ github.run_id }}
          restore-keys: |
            hugo-

            ······

      - name: Cache Save
        uses: actions/cache/save@v4
        with:
          path: ${{ runner.temp }}/hugo_cache
          key: ${{ steps.cache-restore.outputs.cache-primary-key }}

            ······

```

完整的`.github/workflows/hugo.yml`文件如下：

```yaml

name: Deploy Hugo site to Pages

on:
  push:
    branches: ["main"]
  workflow_dispatch:

permissions:
  contents: read
  pages: write
  id-token: write

concurrency:
  group: "pages"
  cancel-in-progress: false

defaults:
  run:
    shell: bash

jobs:
  build:
    runs-on: ubuntu-latest
    timeout-minutes: 10
    env:
      HUGO_VERSION: 0.128.0
    steps:
      - name: Install Hugo CLI
        run: |
          wget -O ${{ runner.temp }}/hugo.deb https://github.com/gohugoio/hugo/releases/download/v${HUGO_VERSION}/hugo_extended_${HUGO_VERSION}_linux-amd64.deb \
          && sudo dpkg -i ${{ runner.temp }}/hugo.deb
      - name: Install Dart Sass
        run: sudo snap install dart-sass
      - name: Checkout
        uses: actions/checkout@v4
        with:
          submodules: recursive
      - name: Setup Pages
        id: pages
        uses: actions/configure-pages@v5

      - name: Install Node.js dependencies
        run: "[[ -f package-lock.json || -f npm-shrinkwrap.json ]] && npm ci || true"

      - name: Cache Restore
        id: cache-restore
        uses: actions/cache/restore@v4
        with:
          path: ${{ runner.temp }}/hugo_cache
          key: hugo-${{ github.run_id }}
          restore-keys: |
            hugo-

      - name: Build with Hugo
        env:
          HUGO_CACHEDIR: ${{ runner.temp }}/hugo_cache
          HUGO_ENVIRONMENT: production
        run: |
          hugo --minify --baseURL "${{ steps.pages.outputs.base_url }}/" 

      - name: Upload artifact
        uses: actions/upload-pages-artifact@v3
        with:
          path: ./public

      - name: Cache Save
        uses: actions/cache/save@v4
        with:
          path: ${{ runner.temp }}/hugo_cache
          key: ${{ steps.cache-restore.outputs.cache-primary-key }}

  deploy:
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    runs-on: ubuntu-latest
    needs: build
    timeout-minutes: 5
    steps:
      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v4

```




# 参考

![craftycto A Gnarly Hugo-Cloudflare Build Problem, Resolved](https://craftycto.com/micro/hugo-cloudflare-build/)