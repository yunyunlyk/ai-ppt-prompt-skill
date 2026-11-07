# 变更记录

版本号遵循[语义化版本](https://semver.org/lang/zh-CN/)，git tag 即版本事实来源。每一版在 GitHub Releases 上都有对应发行版。

## [1.0.0] - 2026-11-07

首个正式发行版。

### 新增

- `SKILL.md`：PPT 请求路由（11 种输入情形 → 对应模板）、输出规范、交付前自检，以及「文案 → 配图 → 动效」的固定顺序。
- `references/framework.md`：四段式框架模板、极简三段式、结构清单法、大纲试探；金字塔 / SCQA / PREP / STAR 四模型拆解。
- `references/image.md`：按标题反推配图提示词（真实摄影、主体 1/3 留白 2/3、单一主体）；人物双重曝光改图指令。
- `references/visual.md`：对比四维度（明暗 / 大小 / 色彩 / 焦点）、产品图分区提示词、废图三步补救、封面 ≠ 内页、动效克制。
- `references/video.md`：工具内动效镜头的四类输入（提示词 / 方向 / 镜头参数 / 关键帧）、图片转视频提示词。
- `examples/framework-filled.md`：「主题定框架」的一次完整输出样例。
- `README.md`：DSH / Claude Code / Codex 三种安装方式、更新、计划任务自动更新。
- `LICENSE`：MIT，附上游出处。

### 说明

- 框架与配图部分整理自《2026-AI辅助做PPT提示词》（旁门左道PPT 出品）。
- 流程顺序、对比原则、废图补救与动效工作台整理自技术胖《AI制作PPT全流程攻略 PPT制作提分指南》（[BV1pFMi6tEig](https://www.bilibili.com/video/BV1pFMi6tEig/)）。
- 本技能只做路由、预填与判断标准，提示词骨架保持原样。
