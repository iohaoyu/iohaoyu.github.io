# Blog Project Notes

这个仓库是 `iohaoyu/iohaoyu.github.io` 的 Hexo 博客源码仓库。后续维护时优先遵守这些项目约定，避免重复踩坑。

## 分支与发布

- `hexo` 分支是源码分支，日常改主题、文章、配置都在这里提交。
- `main` 分支是 GitHub Pages 发布分支，`https://iohaoyu.com` 读取的是这里的静态产物。
- 只把提交推到 `hexo` 不会更新线上站点。改完源码后，如需线上生效，还要重新生成 `public/` 并同步到 `main`。
- `_config.yml` 里的 deploy 目标目前是：
  - repo: `git@github.com:iohaoyu/iohaoyu.github.io.git`
  - branch: `main`
- 如果本机 GitHub CLI 已登录但 SSH key 未配置，直接用 Hexo deploy 可能仍会因为 SSH 失败。可以用 HTTPS 临时克隆 `main` 分支发布静态产物。

## 常用命令

本地 `node_modules/.bin/hexo` 可能没有执行权限，优先使用直接 Node 调用：

```sh
node node_modules/hexo/bin/hexo clean
node node_modules/hexo/bin/hexo generate
```

生成后重点检查：

```sh
rg -n 'href="/love|>溪若<|/love/' public
rg -n 'nth-child\(7\)|nth-child\(8\)' public/css/style.css
du -h public/images/ocean/ocean.mp4 public/images/ocean/ocean-poster.jpg
```

发布后检查 GitHub Pages：

```sh
gh api repos/iohaoyu/iohaoyu.github.io/pages
gh api repos/iohaoyu/iohaoyu.github.io/pages/builds/latest
```

线上 HTML 可能被 GitHub Pages CDN 或浏览器缓存。需要绕缓存时：

```sh
curl -H Cache-Control:no-cache -L -s https://iohaoyu.com
curl -L -s 'https://iohaoyu.com/?v=<commit>'
```

## 已知历史坑

- `node_modules/` 之前曾经入库，已经从 Git 跟踪中清理，并加入 `.gitignore`。不要再把依赖目录提交进仓库。
- `.gitignore` 已覆盖 `node_modules/`、`public/`、`.deploy*/`、日志文件和本地数据库。
- 首页海浪视频已优化为延迟加载：
  - `themes/ocean/layout/_partial/ocean.ejs`
  - `themes/ocean/source/images/ocean/ocean.mp4`
  - `themes/ocean/source/images/ocean/ocean-poster.jpg`
- 侧边栏已经移除了“溪若”tab，对应页面目录 `source/love/` 已删除。不要在导航里恢复 `/love`。
- 友链和音乐 tab 的图标顺序在 `themes/ocean/source/css/_partial/navbar.styl` 中用 `nth-child` 控制：
  - 第 7 项是友链，图标为文件样式 `\f15b`
  - 第 8 项是音乐，图标为音符 `\f175`

## 修改原则

- 尽量做小而准的改动，不顺手重构无关主题代码。
- 修改主题或生成逻辑后，至少运行 `node node_modules/hexo/bin/hexo generate`。
- 如果用户期待线上变化，必须确认是否已经发布到 `main`，并检查 Pages build 状态。
- 提交源码改动和发布静态产物是两件事，最终汇报时要分别说明。
