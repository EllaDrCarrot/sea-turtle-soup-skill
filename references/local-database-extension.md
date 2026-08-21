# 本地题库与二次开发

本文件只在使用者明确提供本地题库，并要求检索、研究、复盘或二次训练时读取。

## 先区分两种“训练”

### 检索增强与规则提炼

这是推荐方式。把题库作为本地事实库，按主题、机制、难度和来源检索相关记录，再让 skill 提炼抽象规律或辅助创作。它不修改模型参数，易于更新、撤回和审计。

### 模型参数微调

这是独立的机器学习工程，不是本 skill 的安装步骤。需要单独准备训练框架、模型、评估集、版权确认和隐私处理。不要把完整第三方作品直接上传给外部服务，也不要把主持人的临场圆场当作标准答案。

## 推荐目录

    local-corpus/
    ├── README.md
    ├── INDEX.json
    ├── records/
    │   ├── learning1.jsonl
    │   ├── learning2.jsonl
    │   └── other.jsonl
    ├── analysis/
    │   ├── style-profiles.md
    │   └── taxonomy-summary.json
    └── private/
        └── source-notes/

private 目录不应提交到公开仓库。公开仓库只放获得授权的内容和抽象分析。

## 推荐记录结构

每行一个 JSON 对象。至少保留以下字段：

    {
      "id": "SOURCE-001",
      "title": "题目标题",
      "surface": "连贯汤面",
      "solution": "连贯汤底",
      "reasoning": {
        "available": true,
        "key_points": ["关键推理点"]
      },
      "host_manual": null,
      "mechanism": {
        "available": false,
        "type": null,
        "phase_rules": []
      },
      "classification": {
        "ontology": "classic",
        "primary_mechanism": "时间",
        "secondary_mechanisms": [],
        "forms": ["standard"],
        "themes": []
      },
      "learning": {
        "strengths": [],
        "best_for": [],
        "risks": []
      },
      "provenance": {
        "source": "来源说明",
        "license": "授权或使用边界"
      },
      "status": {
        "surface_complete": true,
        "solution_complete": true,
        "confidence": "high"
      }
    }

字段可扩展，但 surface、solution、classification、status 的含义应保持稳定。

## 二次学习流程

1. 先确认题库来源、授权范围和是否允许改编。
2. 统一每条记录的汤面、汤底、推理记录和主持机制边界。
3. 给每题标注一个主机制，辅助机制不超过两个。
4. 把优点、风险和不确定事实分开保存。
5. 按当前任务检索小批量相关记录，不要每次加载整个数据库。
6. 生成来源对比和抽象规则，不复制原题的表面组合。
7. 用公平性清单和第二解搜索验证新题。
8. 将稳定、跨题复现的规则写入 references；把单题结论留在数据库分析文件。

## SQLite 选项

题库较大时可以使用 SQLite。建议保留 JSONL 作为可读、可迁移的主档案，SQLite 作为检索副本。

    CREATE TABLE soups (
      id TEXT PRIMARY KEY,
      source TEXT NOT NULL,
      title TEXT,
      surface TEXT NOT NULL,
      solution TEXT NOT NULL,
      ontology TEXT,
      primary_mechanism TEXT,
      forms_json TEXT,
      themes_json TEXT,
      reasoning_json TEXT,
      host_manual_json TEXT,
      mechanism_json TEXT,
      learning_json TEXT,
      status_json TEXT
    );

    CREATE INDEX idx_soups_source ON soups(source);
    CREATE INDEX idx_soups_mechanism ON soups(primary_mechanism);
    CREATE INDEX idx_soups_ontology ON soups(ontology);

向量检索可以作为辅助，但不能替代结构化过滤。最终交给 skill 的记录应包含完整汤面、汤底和标签，便于逐条审计。

## 提供给 skill 的请求模板

可以这样请求：

    请读取 local-corpus/ 中与“空间诡计 + 经典本格 + 中高难度”相关的 10 条记录。
    只提炼可迁移结构，不复述原文；输出机制分布、共同优点、常见风险和
    一套新的事实链，最后运行公平性与第二解审计。

如果只想玩题，不要让 skill 读取 solution；如果要审题或研究，才同时读取 solution 和相关分析字段。

## 版本和评估

每次更新题库或 skill 时记录：

- 数据版本和新增/删除数量；
- 标签定义是否变化；
- 新题在公平性、唯一性、物理可行性和主持稳定性上的结果；
- 哪些规则是跨来源结论，哪些只是单题观察。

至少保留一组不参与提炼的评估题，用来检查二次开发是否真的提高了推理质量，而不是只让输出更像某个来源。
