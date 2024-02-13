---
title: hugo模板自定义修改
description: 如何在模板主题(stack为例)基础上，进行自定义修改满足个人需求
slug: hugo_template_customize
date: 2024-02-09 00:35:00+0800
image: imgs/hugo.png
categories:
    - techStudy
tags:
    - hugo
    - blog
    - FrontEndDevelopment
    - web
# math: 
# license: 
hidden: false
comments: true

draft: false

links:
  - title: hugo官方文档
    description: hugo官方文档
    website: https://gohugo.io/documentation/
    image: https://gohugo.io/favicon-32x32.png

---

## 前言

本文所在的博客是基于hugo的stack主题搭建的，但是在使用过程中，发现有一些地方不太符合个人的需求，因此需要对模板进行一些自定义修改。
例如一些页面的样式，或者新增一些页面、功能或组件。

这就需要在主题的基础上进行一些自定义修改，本文以在左侧选项菜单栏新增一个`随笔(musings)`页面和新增`password`与`passwordPoint`变量进行博文前端加密为例，说明如何在新建的页面中使用自定义的模板。

### 新建随笔页面

理论上是直接在content目录下新建`.md`文件就行了，根据不同主题的情况，我使用的`stack`主题中，每个页面是一个文件夹，里面包含一个`index.md`文件，因此需要新建一个文件夹，然后在里面新建`index.md`文件。

在`content/page/`目录下新建一个`musings`文件夹，然后在里面新建`index.md`文件，内容如下：

```toml
---
title: "随笔"
date: 2023-09-08
layout: "musings"
slug: "musings"
menu:
    main:
        weight: 4
        params: 
            icon: note
---
```

其中`layout: "musings"`表示使用`stack`主题中尚未创建(待会我会创建)的`musings.html`模板。

### 新建模板

如果使用`git moudule`的方式引入主题，博客根目录下会有一个`layouts`目录,`themes`目录下也会有一个`layouts`目录，这两个目录的区别是前者是用来存放自定义模板的，后者是存放主题模板的。一般前一个目录下的模板会覆盖后一个目录下的模板，自定义模板时，将文件复制到前一个目录下进行修改即可。

模板文件一般放在`layouts`目录下，`_default`目录下存放优先默认模板，因此在`layouts/_default`目录下新建一个`musings.html`文件，查找并显示`tag`为`musings`的文章，内容如下：

```html
{{ define "body-class" }}template-archives{{ end }}

{{ define "main" }}
<h1 class="big-title" style="color: white;">随笔</h1>


{{ $pages := where .Site.RegularPages "Type" "in" .Site.Params.mainSections }}
{{ $notHidden := where .Site.RegularPages "Params.hidden" "!=" true }}
{{ $filtered := ($pages | intersect $notHidden) }}

{{ range $filtered.GroupByDate "2006" }}
{{ $id := lower (replace .Key " " "-") }}
<div class="archives-group" id="{{ $id }}">
    <h2 class="archives-date section-title"><a href="{{ $.RelPermalink }}#{{ $id }}">{{ .Key }}</a></h2>
    <div class="article-list--compact">
        {{ range .Pages }}
        {{ if in .Params.tags "musings" }}
        <article>
            <a href="{{ .RelPermalink }}">
                <div class="article-details">
                    <h2 class="article-title">
                        {{- .Title -}}
                    </h2>
                    <footer class="article-time">
                        <time datetime='{{ .Date.Format "2006-01-02T15:04:05Z07:00" }}'>
                            {{- .Date.Format (or .Site.Params.dateFormat.published "Jan 02, 2006") -}}
                        </time>
                    </footer>
                </div>

                {{- $image := partialCached "helper/image" (dict "Context" . "Type" "articleList") .RelPermalink
                "articleList" -}}
                {{ if $image.exists }}
                <div class="article-image">
                    {{ if $image.resource }}
                    {{- $Permalink := $image.resource.RelPermalink -}}
                    {{- $Width := $image.resource.Width -}}
                    {{- $Height := $image.resource.Height -}}

                    {{- if (default true .Page.Site.Params.imageProcessing.cover.enabled) -}}
                    {{- $thumbnail := $image.resource.Fill "120x120" -}}
                    {{- $Permalink = $thumbnail.RelPermalink -}}
                    {{- $Width = $thumbnail.Width -}}
                    {{- $Height = $thumbnail.Height -}}
                    {{- end -}}

                    <img src="{{ $Permalink }}" width="{{ $Width }}" height="{{ $Height }}" alt="{{ .Title }}"
                        loading="lazy">
                    {{ else }}
                    <img src="{{ $image.permalink }}" loading="lazy" alt="Featured image of post {{ .Title }}" />
                    {{ end }}
                </div>
                {{ end }}
            </a>
        </article>
        {{ end }}
        {{ end }}
    </div>
</div>
{{ end }}

{{ partialCached "footer/footer" . }}
{{ end }}
```

### 成功

这样就完成了一个新页面的创建。


### 修改 layouts\partials\article\article.html

```html

{{ if isset .Params "password" }}
<div id="password-protected-content"
    style="border: 1px solid #ccc; padding: 10px; border-radius: 5px; background-color: #f9f9f9; text-align: center;">
    <p style="font-size: 16px; margin-bottom: 10px;">{{ with .Params.passwordPoint }}{{ . }}{{ else
        }}这是密码保护的内容，请输入密码访问：{{ end }}</p>
    <input type="password" id="password-input" style="padding: 5px; margin-bottom: 10px; width: 200px;">
    <button onclick="verifyPassword()"
        style="padding: 5px 10px; background-color: #007bff; color: #fff; border: none; border-radius: 3px; cursor: pointer;">提交</button>
</div>
<div id="article-content" style="display: none;">
    <article class="{{ if .Params.image }}has-image {{ end }}main-article">
        {{ partial "article/components/header" . }}
        <!-- 以下部分仅在密码验证通过后显示 -->
        <div id="content">
            {{ partial "article/components/content" . }}
        </div>
        <!-- End of password-protected content -->
        {{ partial "article/components/footer" . }}
        {{ if or .Params.math .Site.Params.article.math }}
        {{ partialCached "article/components/math.html" . }}
        {{ end }}
    </article>
</div>
{{ else }}
<div id="article-content">
    <article class="{{ if .Params.image }}has-image {{ end }}main-article">
        {{ partial "article/components/header" . }}
        {{ partial "article/components/content" . }}
        {{ partial "article/components/footer" . }}
        {{ if or .Params.math .Site.Params.article.math }}
        {{ partialCached "article/components/math.html" . }}
        {{ end }}
    </article>
</div>
{{ end }}

<script>
    function verifyPassword() {
        var inputPassword = document.getElementById('password-input').value;
        var correctPassword = '{{ with .Params.password }}{{ . }}{{ else }}{{ "" }}{{ end }}'; // 获取文章 Front Matter 中定义的密码
        if (inputPassword === correctPassword) {
            document.getElementById('password-protected-content').style.display = 'none'; // 隐藏密码输入框
            document.getElementById('article-content').style.display = 'block'; // 显示文章内容
        } else {
            alert('密码错误，请重新输入！');
        }
    }
</script>

```

- 其中 `{{ if isset .Params "password" }}`可以判断`password`是否存在；

- `{{ with .Params.passwordPoint }}{{ . }}{{ else }}这是密码保护的内容，请输入密码访问：{{ end }}`则可以获取md前边的`passwordPoint`变量，如果没有则显示默认的提示信息；

### 成功

这样就完成了一个加密功能的实现。