# ai-ppt-prompt

一个给 AI agent 用的技能包：只要你提到 PPT / 演示文稿，它就自动判断你手里有什么，套用对应的提示词模板，把占位符填成你的实际内容，直接产出可用成品——而不是把一份带 `[方括号]` 的空模板丢回给你。

## 它解决什么

「AI 做 PPT」的提示词网上一堆，真用起来问题是两个：**该用哪套**、**填什么值**。这个技能把四套模板做成路由：

| 你手里的东西 | 它做什么 | 输出 |
| --- | --- | --- |
| 只有主题（「帮我做份预算申请汇报」） | 主题定框架 | 四段式预填 + 页级大纲 |
| 一篇文档 / 长文 / 复盘材料 | 文档梳逻辑 | 按金字塔 / SCQA / PREP / STAR 重组的大纲 |
| 说「逻辑乱」 | 四模型选型 | 结构建议 + 章节重排 |
| 要封面配图 / 找不到图 | 配图反推 | 3 组真实摄影风格的生图提示词 |
| 要人物海报 / 双重曝光 | 图片编辑指令 | 改图指令 |
| 有静态图要动起来当背景 | 图生视频 | 一条结构化视频提示词 |

## 安装

技能就是一个目录，拷进去就生效，不需要装依赖、不需要改配置。

### DeepSeek Harness

```powershell
# Windows PowerShell
$dest = Join-Path $env:DSH_HOME 'skills\ai-ppt-prompt'
New-Item -ItemType Directory -Force $dest | Out-Null
git clone --depth 1 https://github.com/yunyunlyk/ai-ppt-prompt-skill.git "$env:TEMP\ai-ppt-prompt-skill"
Copy-Item "$env:TEMP\ai-ppt-prompt-skill\*" $dest -Recurse -Force
Remove-Item "$env:TEMP\ai-ppt-prompt-skill" -Recurse -Force
```

```bash
# macOS / Linux
dest="$DSH_HOME/skills/ai-ppt-prompt"
mkdir -p "$dest"
git clone --depth 1 https://github.com/yunyunlyk/ai-ppt-prompt-skill.git /tmp/ai-ppt-prompt-skill
cp -r /tmp/ai-ppt-prompt-skill/. "$dest"/
rm -rf /tmp/ai-ppt-prompt-skill
```

`$DSH_HOME` 就是你的 DSH 数据目录；不确定就先执行 `echo $env:DSH_HOME`（macOS / Linux 用 `echo $DSH_HOME`）。目录名必须是 `ai-ppt-prompt`，要和 `SKILL.md` 里的 `name` 一致。

不想用 git 也行：下载仓库 zip，把 `SKILL.md` 和 `references/` 一起放到 `$DSH_HOME/skills/ai-ppt-prompt/`。**别只拷 `SKILL.md`**，三份参考模板在 `references/` 里。

### Claude Code / Codex 等同样吃 `SKILL.md` 的 agent

放到各自的 skills 目录即可，例如 `~/.claude/skills/ai-ppt-prompt/`、`~/.codex/skills/ai-ppt-prompt/`。目录结构与 frontmatter 通用。

### 验证装好了

在 DSH 里让模型加载这个技能（`skill` 工具，name 传 `ai-ppt-prompt`），能返回技能正文、且 base directory 指向你刚拷的目录，就是成功。DSH 有目录监听，装完下一步就生效，不用重启。

### 更新

没有自动更新，重新执行一次安装命令覆盖即可（`git clone --depth 1` + `Copy-Item -Force` / `cp -r`）。改动过的本地副本会被覆盖，要留就自己先备份。

## 使用

不用记命令，按正常说话方式提需求就会触发。

**要框架**

> 我要给大老板做一份明年研发预算申请汇报，20 分钟，帮我搭个框架

它按「主题定框架」走，把这四段（核心诉求 / 身份背景 / 受众与目标 / 限制条件）填成你的实际值，再输出页级大纲表格。缺关键信息时它最多问一个问题，其余按默认推断并在末尾一行注明假设。

**文档变 PPT**

> 这是我这季度的复盘文档，帮我转成 PPT 大纲，用 SCQA

按「文档梳逻辑」走，每页给结论型标题 + 3–5 条要点，适合图示的页面标 `[建议配图：xx]`。

**配图**

> 封面标题是「让增长回到两位数」，给我配图提示词

按「配图反推」走，出 3 组真实摄影风格提示词，每组都是「风格关键词 + 主体画面 + 场景描述 + 构图逻辑」，且强制主体占 1/3、留白 2/3。

**要拿去别的工具用**

> 只要提示词，我自己拿去 Midjourney 跑

这时它输出的是一整段可直接粘贴的提示词文本，不带解说。

## 目录结构

```text
SKILL.md              路由 + 输出规范 + 自检（模型先读这个）
references/
├── framework.md      主题定框架 / 文档梳逻辑 / 四模型选型（完整填空模板）
├── image.md          配图反推 / 人物双重曝光
└── video.md          图生视频
examples/
└── framework-filled.md  一次真实走完「主题定框架」的输出样例
```

想知道装完到底会得到什么，先看 [examples/framework-filled.md](examples/framework-filled.md)。

## 什么时候它不接

- 工具操作类问题（PPT 转 PDF、文件打不开）——直接答，不套模板。
- 你要的是成片本身（.pptx / 网页版幻灯片）——那是做稿工具的事，这个技能只管「说什么、怎么配图」。
- 没有具体主题的闲聊。

## 来源与许可

提示词骨架整理自《2026-AI辅助做PPT提示词》（旁门左道PPT 出品），本仓库做的是结构化、路由与预填改造，引用原文骨架时保留出处。本仓库代码与改造部分以 MIT 许可发布，详见 [LICENSE](LICENSE)。
