# 步骤 1：构建 PWA service-worker 的 js 脚本
> 采用rollup.config.js配置编译脚本，对js进行编译和压缩成mini

```bash
#bash
npm run build:js
```

> 目录是在[\_javascript/pwa](_javascript/pwa) 构建 service-worker 必须的脚本。包含sw.js, app.js。是service-worker的基本脚本。然后在layouts/_default/baseof.html里引入最终生成的app.min.js

# 步骤2：配置sw.js 的 swconfi配置文件

> 配置的 js 数据对象过滤在：[layouts/js/list.js.js](layouts/js/list.js.js)，生成配置文件名是在自定义输出格式 toml 配置。然后使用该格式输出的话，在[content/js/_index.md](../../content/js/_index.md)文件里使用`outputs:- JS`的front-matter头
- 如下：hugo.toml

```toml
#hugo.toml

[outputFormats.JS]
mediaType = "text/javascript"
baseName = "swconfig"         # 输出文件名为 swconfig.js
isPlainText = true
path = "/"                    # 输出到 / 目录
[outputFormats.JSONData]
mediaType = "application/json"
baseName = "swconfig"          # 输出文件名为 data.json
isPlainText = true
path = "/"                     # 输出到 json 目录
# Options to make hugo output files
# 用于 Hugo 输出文档的设置
[outputs]
home = ["HTML", "RSS", "JSON"]
page = ["HTML", "MarkDown"]
section = ["HTML", "RSS"]
taxonomy = ["HTML", "RSS"]
json = ["JSONData"]            # 启用自定义 JSON 输出
js = ["JS"]                    # 启用自定义 JS 输出
```
```yaml
#content/js/_index.md
---
title: "JS"
outputs:
  - JS
---
```
# 步骤3，安装notification.html模板，有使用到bootstrap类库和css样式
> service-worker注册的通知提醒对话框模板在[layouts/partials/notification.html](layouts/partials/notification.html) 在baseof.html引入了通知有新的内容去更新最新内容

> pwa的可安装网页配置是在 [static/site.webmanifest](static/site.webmanifest), 相对路径需要修改

# 启用service-worker
> 默认在开发环境下不启用，只有生产环境启用。在开发sw需要开启时，使用配置环境变量命令编译hugo：HUGO_EVIRONMENT=production hugo server开启service-worker

# 步骤 4：使用该主题的pwa配置

在themes外层主项目里，配置变量
```toml
[params.pwa]
enabled = true
cache = { enabled = true, deny_paths = [
  "/admin",
  "/private",
], static_files = [
  "/assets/images/boy.webp",
  "/js/theme.min.js",
  "/css/style.min.css",
] }
```