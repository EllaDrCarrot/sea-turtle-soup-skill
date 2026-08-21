# 海龟汤.skill

一个给通用 AI agent 使用的海龟汤 skill。

它可以陪你玩汤，也可以帮你煲汤、主持、复盘和改汤。

## 它能做什么

- 设计本格、变格、规则怪谈、长叙事、主持人机制创意汤；
- 以玩家视角主持，不提前泄露汤底；
- 检查时间、空间、身份、感知、语言、证据和因果诡计；
- 找出“看似合理但其实存在第二解”的汤底；
- 把一碗逻辑松散的汤，改成能被公平盘出的汤；
- 读取你自己的本地题库，提炼风格、机制和可迁移的设计方法。

## 方法来源

本 skill 的设计方法参考了许二木、老鸭汤、SZ 推理之夜等创作者的公开内容及用户整理材料，重点提炼短汤面信息密度、长叙事人物因果、主持机制和高概念叙事等可迁移方法。

本仓库不包含相关作品的完整汤面汤底、截图、视频或私有训练数据库，也不代表相关创作者的官方立场。各来源内容的版权仍归原作者或相应权利人所有。

## 安装

打开你正在用的 agent（Claude Code、Codex、antigravity、CodeBuddy、Workbuddy 等），告诉它：

    帮我安装这个 skill：https://github.com/EllaDrCarrot/sea-turtle-soup-skill

或者用通用 CLI 安装器：

    npx skills add EllaDrCarrot/sea-turtle-soup-skill

这个仓库遵循通用 Agent Skills 目录格式，根目录直接包含 SKILL.md，也可以直接把本仓库复制到 Agent 的 skills 目录。

## 装好之后怎么用

### 玩一碗汤

    我们来玩海龟汤吧，你来主持，我问你答。

### 出一碗汤

    设计一碗中高难度海龟汤。

### 改一碗汤

    这是我的汤面和汤底。
    请先尝试找出逻辑漏洞、隐藏信息和可能存在的第二解，再给出一版修改后的汤。

### 复盘一局

    这是刚才的提问记录。
    分析玩家在哪里推理出了关键信息，哪条线索没有发挥作用，

## 它学的不是“套路”，而是三种能力

| 能力               | 用来解决什么问题                               |
| ------------------ | ---------------------------------------------- |
| 短汤面与高信息密度 | 用很少文字制造值得追问的异常                   |
| 长叙事与人文因果   | 让人物的选择真的推动故事，而不是最后补一段煽情 |
| 高概念与叙事机制   | 让形式、规则和信息边界成为线索，而不是装饰     |

## 进阶：让它学习你的本地题库

你不需要把题库塞进 SKILL.md，也不需要一开始就微调模型。

准备一个本地目录，例如：

    D:/my-soup-corpus/
    ├── corpus.jsonl
    └── notes.md

corpus.jsonl 每行放一碗汤，至少包含：

    {"id":"001","title":"题目名","surface":"汤面","solution":"汤底","classification":{"primary_mechanism":"空间"}}

然后直接告诉 agent：

    请读取 D:/my-soup-corpus/corpus.jsonl。
    先总结其中最常见的机制、主题和优点，再指出逻辑风险。
    不要复述原题，不要复制人物和道具组合。
    最后用这些抽象规律设计一碗全新的海龟汤，并做第二解审计。

推荐的学习顺序是：

1. 读取少量相关题目；
2. 区分原题事实、可迁移优点和原作风险；
3. 提炼跨题规律；
4. 生成新题；
5. 用公平性和第二解检查新题。

这叫本地检索与规则提炼，不是自动修改模型参数。题库可以一直留在本地，也不应把没有授权的完整作品上传到公开仓库。

更完整的字段和 SQLite 用法见 references/local-database-extension.md。

## 仓库里有什么

    sea-turtle-soup-skill/
    ├── SKILL.md                         # skill 本体
    ├── agents/openai.yaml               # agent 界面信息
    └── references/
        ├── fairness-rubric.md           # 公平性与第二解审计
        ├── host-mechanism-schema.md     # 主持机制与状态机
        ├── reasoning-element-taxonomy.md# 机制、形式、本体和主题分类
        ├── local-database-extension.md  # 本地题库进阶用法
        ├── style-profiles.md            # 三类设计能力
        └── training-corpus-lessons.md   # 跨题可迁移原则
