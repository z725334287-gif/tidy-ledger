# 下载与整理台账

一个**零依赖单文件网页**，用来记录「下载过什么」和「整理过什么」。

回答两个很实际的问题：

- 这个东西是从哪下的、装在哪、什么版本？
- 上次整理进行到哪一步了，还有什么没做完？

双击 `index.html` 就能用，**不需要装任何东西，也不联网**。

---

## 仓库里有什么

```
index.html    主文件，34 KB，双击就用
README.md     你正在看的这个
SKILL.md      给 AI Agent 看的说明书，装上后 AI 会自动维护这份台账
LICENSE       MIT
```

## 它能做什么

| 页面 | 内容 |
|---|---|
| **概览** | 磁盘占用 TOP 8（横向条形图）+ 整理时间线 |
| **下载记录** | 日期 / 内容 / 来源与许可 / 保存位置 / 体积 / 状态 |
| **整理记录** | 每一轮整理的范围、动作、结果 + 当前目录结构 |
| **待办清单** | 可勾选，未完成数会实时更新，状态存在浏览器里 |
| **规矩与索引** | 整理时踩过的坑 + 相关文件导航 |

其他细节：

- **深色 / 浅色**一键切换，选择会被记住
- **搜索**跨当前页全文过滤，实时显示命中数
- **导出 Markdown** —— 一键把台账导出成 `.md` 存下来
- **打印 / 存 PDF** —— 打印时自动隐藏导航，只留内容
- 路径旁有**复制按钮**
- 响应式，手机和电脑都能看
- 尊重系统的「减弱动态效果」设置

---

## 怎么用

1. 下载这个仓库（或直接保存 `index.html`）
2. 双击打开
3. 改成你自己的内容 —— 所有数据都在文件底部 `<script>` 里，改这几个数组就行：

```js
const DL     = [...];   // 下载记录
const TL     = [...];   // 整理记录
const STRUCT = [...];   // 目录结构
const DISK   = [...];   // 磁盘占用
const TODOS  = [...];   // 待办
const RULES  = [...];   // 自己定的规矩
const IDX    = [...];   // 相关文件索引
```

改完刷新页面即可，不需要构建工具。

---

## 配合 AI 使用（这才是它真正好用的地方）

这个仓库里有一个 **`SKILL.md`**，是一份给 AI Agent 看的说明书。
装上之后，你不用自己动手改任何文件——跟 AI 说一句就行：

| 你说 | AI 做 |
|---|---|
| 「把 XX 下载到 D 盘」 | 下载完自动写进下载记录 |
| 「帮我把 XX 整理一下」 | 先核对安全规矩，整理完追加记录 |
| 「记一下：……」 | 追加到对应板块 |
| 「台账啥情况了」 | 汇报进度与未完成待办 |

**安装方法**（以 WorkBuddy / Claude Code 为例）：

```bash
# WorkBuddy
cp SKILL.md ~/.workbuddy/skills/tidy-ledger/SKILL.md

# Claude Code
cp SKILL.md ~/.claude/skills/tidy-ledger/SKILL.md
```

`SKILL.md` 里写了六条硬规矩（移动与删除同权限、别用 `|= HIDDEN`、别信回收站、
空文件不等于垃圾、桌面快捷方式改不动、存档目录别碰）——每一条都是真实踩坑换来的，
**违反会造成实际损失**。AI 在动手整理前会先过一遍。

---

## 相关项目

找磁盘空间黑洞，比人肉翻文件夹快得多：

| 项目 | 说明 |
|---|---|
| [windirstat/windirstat](https://github.com/windirstat/windirstat) | Windows 上的老牌磁盘分析：树状图、大文件发现、重复文件检测 |
| [dundee/gdu](https://github.com/dundee/gdu) | Go 写的命令行磁盘分析，扫全盘几秒钟 |
| [files-community/Files](https://github.com/files-community/Files) | 现代 Windows 文件管理器，支持多标签 |

想让 AI 自动帮你整理文件（注意隐私风险，务必选本地运行的）：

| 项目 | 说明 |
|---|---|
| [QiuYannnn/Local-File-Organizer](https://github.com/QiuYannnn/Local-File-Organizer) | 本地运行的 AI 文件整理，不上传云端 |
| [ComposioHQ/awesome-claude-skills](https://github.com/ComposioHQ/awesome-claude-skills) | Claude Skills 精选合集 |
| [alirezarezvani/claude-skills](https://github.com/alirezarezvani/claude-skills) | 380+ 个 Claude Code skill，含大量文件与系统管理类 |

---

## 关于隐私

这个仓库里的示例数据**已经做过脱敏**：用户名、计算机名、个人目录名都换成了
`<用户名>` / `<计算机名>` / `<个人库>` 这样的占位符。

页面右上角的 **🛡 脱敏** 按钮可以再把路径里的身份信息藏一层，方便截图或分享给别人看。
脱敏规则用的是通用正则（不会在代码里写死任何真实名字）。

**但请记住**：这个页面默认把数据存在你自己的浏览器里，不会上传到任何地方。
如果你往里面填了真实路径再分享这个 HTML 文件，那些信息就会跟着文件走。

---

## 几条整理文件的经验（血泪版）

这些是真正干活时踩出来的，比"分类方法"更值钱：

1. **移动和删除是同一道权限** —— 都要对源对象的 DELETE 权。没有管理员权限就别硬搬系统目录。
2. **批量改文件属性别用 `|= HIDDEN`** —— 曾经一次把 9 个原本可见的文件夹全藏了。
3. **删东西别指望回收站** —— `SHFileOperation` 可能返回"成功"但文件根本没进去。要走「移到备份目录 + 留还原清单」。
4. **空文件不等于没用** —— `__init__.py` 删了 Python 包就 import 不了；Minecraft 的 `.mca` 空着也是有效区块。
5. **改显示名比改真名安全** —— 用 `desktop.ini` 的 `LocalizedResourceName` 可以让文件夹显示中文标注，
   而真实路径一个字不变，注册表和快捷方式完全不受影响。
6. **游戏存档目录不要碰** —— `.minecraft` 这类目录里藏着正在玩的世界。

---

## 技术说明

- 单个 HTML 文件，CSS 和 JS 全部内联
- **零外部依赖**：不加载任何 CDN、字体、框架，断网可用
- 数据存在 `localStorage`（仅本机，不上传）
- 支持键盘操作，焦点圈可见，对比度符合 WCAG AA

---

## 许可

MIT — 随便用，改个名当自己的台账也行。
