# Project Model Advisor

一个基于 Markdown 的 Codex Skill：在开始项目、进入新阶段或明确询问模型选择时，给出低开销的模型层级、推理强度和升级边界建议，并在条件未变化时沿用已有结论。

当前版本：`0.3.0-trial`（个人试用版）。它已经通过结构验证和三项隔离回答检查，但尚未通过完整跨模型基准，也没有证明具体费用节省比例。

项目维护者：[@TolyMarkee](https://github.com/TolyMarkee)  
协同开发：OpenAI Codex（AI coding assistant；不代表 OpenAI 对本项目的官方背书）

建议的 GitHub repository description：

> A low-overhead Codex Skill for task-aware model selection, reasoning-level recommendations, and reusable project context.

## 能做什么

- 根据任务难度、错误影响、工具和上下文要求给出临时推荐；
- 先排除不兼容的模型/配置，再考虑成本；
- 遵守用户指定的模型、预算和“禁止升级”约束；
- 区分推理不足与网络、权限、文件缺失等环境问题；
- 在项目初始化、交接或重大变化时生成简短上下文卡；
- 条件没有变化时沿用原建议，减少重复分析。

它不会自行切换 Codex/ChatGPT 的模型，不会让已经开始的当前回答改由更便宜的模型计费，也不能保证跨任务自动保留全部上下文。

## 仓库结构

```text
project-model-advisor-github/
|-- README.md                    使用与发布指南
|-- VERSION                      发布版本
|-- CHECKSUMS.sha256             Skill 文件校验值
`-- project-model-advisor/       可安装的 Skill 本体
    |-- SKILL.md
    |-- agents/openai.yaml
    `-- references/
```

不要把仓库根目录的 `README.md`、`VERSION` 或 `CHECKSUMS.sha256` 复制进 Skill 目录。安装目标应当是内层 `project-model-advisor/` 文件夹。

## 安装到 Codex

### Windows（PowerShell）

先下载或克隆本仓库。在仓库根目录运行：

```powershell
$skillSource = (Resolve-Path -LiteralPath '.\project-model-advisor').Path
$skillTarget = Join-Path $env:USERPROFILE '.codex\skills\project-model-advisor'
if (Test-Path -LiteralPath $skillTarget) {
    throw "目标已存在：$skillTarget。请先备份旧版本，再安装。"
}
Copy-Item -LiteralPath $skillSource -Destination $skillTarget -Recurse
```

### macOS / Linux

在仓库根目录运行：

```bash
skill_target="${CODEX_HOME:-$HOME/.codex}/skills/project-model-advisor"
test ! -e "$skill_target" || { echo "目标已存在，请先备份：$skill_target"; exit 1; }
mkdir -p "$(dirname "$skill_target")"
cp -R ./project-model-advisor "$skill_target"
```

安装后重新启动 Codex，或者至少新建一个任务，让 Skill 目录重新被发现。若自动触发不稳定，使用下面的显式调用方式。

### 更新已有版本

不要直接覆盖唯一副本。先将现有的 `project-model-advisor` 文件夹改名为带日期的备份，再复制新版本。确认新版本可用后，再自行决定是否保留备份。

## 使用方法

最可靠的方法是在请求中显式写出 Skill 名称：

```text
使用 $project-model-advisor。
我要做的任务是：……
当前可选模型/档位是：……
预算或限制是：……
请给出适合当前阶段的配置；如果已有建议仍适用，就直接沿用。
```

几个常用示例：

```text
使用 $project-model-advisor。我要修改一个普通的单文件 Python 脚本，能运行单元测试。请简短推荐模型层级和推理强度。
```

```text
使用 $project-model-advisor。上一轮建议均衡档/medium；任务、预算和可用模型都没变，结果也能用。是否需要重新选模型？
```

```text
使用 $project-model-advisor。我只能使用 A、B、C。A 不支持工具，B 支持文件和 shell，C 具有更强推理。我要做机械替换并运行现成测试，请推荐配置。
```

```text
使用 $project-model-advisor。请为这个新项目给出当前阶段配置，并生成最短上下文卡：目标……；限制……；下一步……
```

Skill 默认应给出三类信息：推荐配置、主要依据与不确定性、观察到什么情况才升级或停止。它给出建议后，仍需要你在支持切换的界面中手动选择模型/推理档位，再开始后续任务。

## 如何确认安装成功

新建 Codex 任务并输入：

```text
使用 $project-model-advisor。用三行告诉我：整理一份格式固定的短表格，应该从什么模型层级和推理强度开始？
```

成功的基本迹象：回答简短；不会声称已经替你切换模型；不会因为文件多或文本长就自动选择最强档；会给出明确的升级或停止条件。

如果 Codex 的可用 Skill 列表中能看到 `project-model-advisor`，说明目录已被发现。目录未被发现时，依次检查：文件夹名是否为 `project-model-advisor`、其根目录是否直接包含 `SKILL.md`、是否误套了多层文件夹、安装后是否重新打开了任务。

## 验证与测试边界

`references/eval-cases.md` 保存测试输入，`references/eval-rubric.md` 保存评分规则，二者刻意分开，避免答题时直接看到预期结论。维护者应按 `references/eval-suite.md` 的程序运行测试并保留原始输出。

当前 `0.3.0-trial` 只完成了与第三轮修改直接相关的三项隔离回答检查；其余用例在本版本中标记为未运行。不要把历史记录中的旧分数当成实测成本或跨模型性能证据。

## 发布到 GitHub

1. 在 GitHub 创建空仓库，例如 `cost-aware-model-advisor`。
2. 将本目录中的 `README.md`、`VERSION`、`CHECKSUMS.sha256` 和内层 `project-model-advisor/` 一并提交。
3. 建议使用标签 `v0.3.0-trial`，并在 Release 中附上纯 Skill ZIP。
4. 发布说明中保留“personal trial / 未完成跨模型成本校准”的状态。
5. 公开发布前选择许可证。未添加许可证时，GitHub 展示源码不等于授权他人复制、修改或再发布。

官方 OpenAI Skills API 也支持使用[目录文件或单个 ZIP 创建 Skill](https://developers.openai.com/api/reference/python/resources/skills/methods/create)，并支持[创建不可变版本](https://developers.openai.com/api/reference/cli/resources/skills/subresources/versions/methods/create)；这是 API 托管路径，与把文件夹安装到个人 Codex 的本地路径不同。不要在仓库或命令示例中提交 API Key。

## 许可证

本包暂未附带许可证。公开给别人复用前，请由版权所有者选择许可证：

- MIT：条款简短，允许修改、再发布和商用，要求保留版权与许可声明；
- Apache-2.0：同样宽松，并包含更明确的专利授权条款；
- 不添加许可证：默认保留全部权利，别人通常只能查看，不能获得通用复用授权。

许可证是法律选择，不应由打包工具代替作者决定。

---

## English

### Project summary

`cost-aware-model-advisor` is a Markdown-based Codex Skill for choosing a suitable model capability tier and reasoning level for each task. It aims to reduce unnecessary model escalation while preserving explicit constraints, verification boundaries, and reusable project context.

Maintainer: [@TolyMarkee](https://github.com/TolyMarkee)  
Co-developed with OpenAI Codex, an AI coding assistant. This project is not an official OpenAI product or endorsement.

Suggested repository description:

> A cost-aware Codex Skill for task-aware model selection, reasoning-level recommendations, and reusable project context.

### What it does

- recommends a provisional model capability tier and reasoning level;
- checks required tools, input types, context and supported settings before cost;
- preserves explicit model, budget and no-upgrade constraints;
- distinguishes reasoning problems from missing files, permissions, tools and network failures;
- creates a short context card only at project boundaries or material changes;
- reuses the previous recommendation when the task and constraints are unchanged.

It does not switch the host model automatically, make the current turn cheaper retroactively, or guarantee complete memory across conversations.

### Install in Codex

The install target is the inner `project-model-advisor/` directory, not the repository root.

On Windows PowerShell, run from the repository root:

```powershell
$skillSource = (Resolve-Path -LiteralPath '.\project-model-advisor').Path
$skillTarget = Join-Path $env:USERPROFILE '.codex\skills\project-model-advisor'
if (Test-Path -LiteralPath $skillTarget) {
    throw "Target already exists: $skillTarget"
}
Copy-Item -LiteralPath $skillSource -Destination $skillTarget -Recurse
```

On macOS or Linux:

```bash
skill_target="${CODEX_HOME:-$HOME/.codex}/skills/project-model-advisor"
test ! -e "$skill_target" || { echo "Target already exists: $skill_target"; exit 1; }
mkdir -p "$(dirname "$skill_target")"
cp -R ./project-model-advisor "$skill_target"
```

Restart Codex or start a new task after installation. Explicit invocation is the most reliable:

```text
Use $project-model-advisor.
My task is: ...
Available models or tiers: ...
Budget and constraints: ...
Recommend the suitable current configuration briefly. If the previous recommendation still applies, reuse it.
```

### Repository topics

Suggested topics:

`codex` `ai-agents` `llm` `model-selection` `model-routing` `cost-optimization` `prompt-engineering` `developer-tools`

### Validation status

Version `0.3.0-trial` passed structural validation and three isolated response checks related to the third revision. It is a personal trial release, not a complete cross-model benchmark and not proof of a specific cost saving.

The evaluation inputs and rubric are separated under `project-model-advisor/references/`. Do not treat historical self-assessments as measured performance evidence.

### Publishing and license

Before publishing, choose a license. No license is included in this release. MIT is a simple option for broad reuse; Apache-2.0 is another permissive option with explicit patent terms. Without a license, public visibility does not grant general permission to copy, modify or redistribute.

