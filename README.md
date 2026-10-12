# make-knowledge-cards

把粘贴的文章或本地 Markdown/TXT 文件提炼成有原文依据的 Markdown 知识卡片，便于理解和复习。

## 项目解决什么问题

文章往往同时包含重要概念、重复解释和次要细节，直接复制段落很难形成便于复习的卡片。本项目提供一套 Skill 指令，让 AI 助手筛选重要知识、拆分独立知识点并整理为统一格式，同时保留原文中的条件和限制。

这是供 AI 助手读取的指令包，实际提炼由助手完成；项目没有独立运行的转换程序。

## 主要功能

- **支持文本输入**：粘贴正文，或读取指定的本地 `.md`、`.markdown`、`.txt` 文件。
- **筛选与去重**：围绕文章主旨提取重要知识，一张卡片只讲一个知识点，合并同义和重复内容。
- **统一卡片结构**：标题、核心知识、简明解释，以及原文例子或带参考答案的自测问题；每张附来源定位。
- **以原文为依据**：不补写背景或编造例子，保留适用条件和观点归属；遇到影响结论的矛盾时指出冲突，不擅自裁决。
- **按内容决定数量**：即使用户指定数量，信息不足时也少生成或不生成，并说明原因。
- **Markdown 输出**：默认在对话中回复，也可按要求保存为本地 Markdown 文件。输出语言跟随用户要求，未指定时跟随原文。

暂不支持网页抓取、PDF、Anki 和图形界面，也不进行外部资料检索或事实核验。

## 安装方法

需要能读取 Skill 指令的 AI 助手；处理本地文件时，助手还需具有相应文件访问权限。日常使用无需安装 Python 或其他项目依赖。

作为个人 Codex Skill 安装：

1. 从本项目取得完整的 `skills/make-knowledge-cards` 文件夹，保留其中的 `SKILL.md` 和 `agents/openai.yaml`。
2. 将该文件夹复制到 `$CODEX_HOME/skills/`；若未设置 `CODEX_HOME`，则使用用户主目录下的 `.codex/skills/`。Windows 默认位置为 `%USERPROFILE%\.codex\skills\make-knowledge-cards`。
3. 在客户端重新加载技能后，通过 `$make-knowledge-cards` 调用。

安装后的目录结构应为：

```text
<Codex 技能目录>/
└── make-knowledge-cards/
    ├── SKILL.md
    └── agents/
        └── openai.yaml
```

也可以不安装，直接使用下一节的项目内调用方式。仅将 Skill 放在本项目的 `skills/` 目录中，不意味着客户端会自动发现它。

## 使用方法

安装后，粘贴文章：

```text
请使用 $make-knowledge-cards，将以下文章转换为知识卡片：
（粘贴正文）
```

或提供本地文件路径；以下路径需替换为实际存在、助手可读取的文件：

```text
请使用 $make-knowledge-cards，把本地 notes/article.md 转成知识卡片。
```

需要保存结果时，补充“将结果保存为 cards.md”。可以提出卡片数量或输出语言偏好，但数量不会优先于原文依据和去重要求。

未安装时，在当前项目中直接要求助手读取指令。以下示例使用项目已有的测试文章：

```text
请读取 skills/make-knowledge-cards/SKILL.md，按该 Skill 将
tests/inputs/science.md 转成知识卡片。
```

## 输入输出示例

以下示例没有原文例子，因此使用自测问题。两句表达同一知识点，合并为一张卡片。

输入正文：

> 相关关系不能单独证明因果关系。两件事同时变化，不足以证明一件事导致另一件事。

输出：

### 1. 相关不能单独证明因果

- **核心知识**：两件事同时变化，不足以证明一件事导致另一件事。
- **简明解释**：仅凭共同变化，不能认定存在因果关系。
- **自测问题**：观察到两件事同时变化，能否单独据此认定因果关系？
  **参考答案**：不能。
- **来源**：粘贴文本，第 1 段。

## 开发与验证

- 技能：[SKILL.md](skills/make-knowledge-cards/SKILL.md)
- 界面配置：[openai.yaml](skills/make-knowledge-cards/agents/openai.yaml)
- 人工试做的输入、输出和完善记录：[tests/RESULTS.md](tests/RESULTS.md)

使用官方 skill-creator 的 `init_skill.py` 初始化，并用其 `quick_validate.py` 检查。复查命令（Python 环境需提供 PyYAML；将 `<skill-creator>` 替换为本地技能目录）：

```text
python -X utf8 <skill-creator>/scripts/quick_validate.py skills/make-knowledge-cards
```

人工行为测试与结构检查分别进行；结构通过不代表所有文章的提炼质量都已验证。

## 许可证

采用 [MIT License](LICENSE)，可复制、修改和分发。
