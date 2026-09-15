# El Psy Kongroo

使用 Quarto 构建的个人学习博客。编辑源码后运行 `quarto render`，发布结果统一生成到 `docs/`。

## 哪些文件需要同步到 GitHub？

| 文件 / 目录 | 是否同步 | 用途 |
| --- | --- | --- |
| `index.qmd` | 是 | 首页内容、文章列表设置 |
| `posts/` | 是 | 文章源码与文章附属资源 |
| `styles.scss` | 是 | 全站视觉样式 |
| `preferences-head.html` | 是 | 主题、文章展示切换与本地偏好保存 |
| `_quarto.yml` | 是 | 网站、主题与构建配置 |
| `images/` | 是 | 原始图片，供下次构建使用 |
| `docs/` **整个目录** | 是 | GitHub Pages 使用的完整静态网站 |
| `.gitignore`、`.gitattributes` | 是 | 忽略规则与跨平台文本格式 |
| `README.md`、`LICENSE` | 是 | 项目说明与许可证 |
| `.quarto/` | 否 | Quarto 的本地缓存和临时文件 |
| `.venv/`、`__pycache__/` | 否 | 本地 Python 环境和缓存 |
| 根目录 `DMP_files/` | 否 | 旧的单篇渲染产物，当前网站不引用它 |
| `.preview/` | 否 | 本地预览和截图 |

`images/` 与 `docs/images/` 都要保留：前者是源码资源，后者是部署副本。

### 为什么 docs/ 每次会出现很多变化？

Quarto 除了生成 HTML，还会生成配套的 CSS、JavaScript、图片、索引与站点地图。修改主题后，CSS 文件名中的内容哈希会变化，通常表现为旧文件删除、新文件增加。这些属于同一次构建，应一起提交。

不要单独挑选 `docs/index.html`，也不要忽略 `docs/site_libs/`，否则线上页面可能缺少样式或交互。`docs/` 内的文件由 Quarto 管理，不要手工修改。构建工具版本变化也可能带来较多产物变化。

## 每次更新的固定流程

在项目根目录执行：

```powershell
# 1. 编辑 index.qmd、posts/、styles.scss 等源码后，构建整站
quarto render

# 2. 本地检查网页
quarto preview --no-browser
# 打开终端显示的地址，检查后按 Ctrl+C 停止预览

# 3. 收集源码与发布结果的变更，包括过期产物的删除
git add -A

# 4. 提交前确认没有混入其他工作
git diff --cached --stat
git diff --cached --check

# 5. 提交并同步
git commit -m "Update blog"
git push
```

也可以在 GitHub Desktop 中检查 Changes 后提交并 Push。清理后的忽略规则会排除本地缓存；发布产物数量较多是正常现象。

本仓库按“提交 `docs/` 发布”的方式维护。GitHub 仓库的 Pages 发布目录应对应当前发布分支的 `/docs`；本地文件无法确认远端设置，需在仓库 Settings → Pages 查看。

## 本次整理

- 首页关闭独立分类筛选栏：目前只有一个主题，文章卡片继续显示主题标签。未来积累多个主题后，可将 `index.qmd` 中的 `categories` 恢复为 `true`。
- “最新文章”采用纯文字标题与细分隔线，保持现有色彩和插画。
- 文章页继承 `_quarto.yml` 的全站主题，统一维护样式。
- 只渲染 `index.qmd` 和 `posts/*.qmd`，说明文档不会成为网页文章。
- `.quarto/` 与旧 `DMP_files/` 已从项目同步范围移除；`.quarto/` 只由 Quarto 在构建时按需生成，旧的 `DMP_files/` 不再使用。
- `docs/DMP.html` 是没有对应源码的旧页面；整站构建会移除它，当前文章地址为 `docs/posts/DMP.html`。

## 主题与文章展示

导航栏的调色盘图标可选择牧濑 红莉栖、战场原 黑仪、维多利加或赫萝主题。主题配色与深浅模式独立，可自由组合；原文与插画不变。

“最新文章”的排序、筛选和展示图标位于同一组工具栏中。四个图标从左到右对应大卡片（宽幅）、中卡片（桌面两列）、小卡片（桌面三列，手机紧凑行）与纯文字标题列表，悬停显示名称。选中底块滑动切换，列表带轻微淡入位移动效。窄屏工具栏会换行，保持阅读和点击空间；系统启用“减少动态效果”时停用这些动效。

主题和文章展示偏好保存在当前浏览器的本地存储中，刷新或进入文章页后保留。浏览器禁用本地存储时，本次页面仍可切换。没有 JavaScript 时保留普通文章列表。

排序和筛选使用固定网格列，不随选项文字长度改变位置；原生下拉菜单的展开方向和弹出外观由浏览器、操作系统决定。
