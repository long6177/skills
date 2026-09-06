# git-helper 设计 spec

> 状态：设计定稿（经 grilling 三轮拷问收敛），待实现评审
> 日期：2026-09-06
> 系列文档：`../skill想法1-git状态维护.md`（原始想法）、`../skill调研1-git相关skills调研.md`（生态调研）
> Skill 本体：本仓库根目录 `SKILL.md` + `references/`（英文内容，面向开源分发）

## 1. 定位

面向**有 git 基础但缺乏经验**的 AI 编程者的 git/GitHub 状态维护 skill。核心价值：仓库状态体检、时机性建议、危险操作防呆、概念答疑。用户主动调用后进入"维护模式"，全程让用户参与决策，不全盘代办。

**生态依据**（详见调研文档）：单一"状态维护总管"型 skill 在 100K+ 安装量的头部生态中不存在；.gitignore 治理（全部 <110 安装）、fork+clone 场景（最高 2.9K）、时机性建议、用户参与决策均为空白点；commit message、worktree、危险命令硬拦截为红海，采用**创作期提炼**策略吸收而非竞争。

## 2. 已定决策（grilling 三轮）

### 形式与触发

| 决策 | 结论 |
| --- | --- |
| 形式 | 单 skill + references 分层（"拆分单位是动作不是知识"——playbook 是按需加载的知识，动作类能力提炼进知识库） |
| 触发 | **纯被动**：仅用户主动调用（`$git-helper` 等），description 显式声明不自动触发，避免干扰主开发工作流 |
| 驻留 | 调用一次 = 本会话进入维护模式，后续所有 git 动作按本 skill 准则执行，用户可喊停退出 |
| 语言 | 全英文（开源分发；Conventional Commits 规范直接用英文 canonical 文本） |
| 仓库 | 独立 git 仓库 `skill-git-helper/`，暂不落入 agent skill 目录 |

### 交互准则

| 决策 | 结论 |
| --- | --- |
| 征询粒度 | 风险分级：可逆且局部 → 直接执行并报告；影响历史/远程/他人 → 先征询 |
| commit message | 默认直接提交并在结果中展示 message（可 amend 返工）；仅当用户明确声明"message 须先审批"时改为先示后提（尊重已声明偏好，obra 模式） |
| 概念卡 | **仅用户主动问时**给出（目标用户有 git 基础，不主动科普打断） |
| 体检模式 | 分层：快检（只读命令约 2 秒）→ 异常或用户问题命中场景时才定向深查；输出 = 一行状态表 + ≤3 条建议（每条附 why + 风险级） |
| 仓库画像 | 接管已有仓库时做画像（log 风格、主分支名、remote 结构）用于校准后续建议，但不主动评判用户现有习惯 |

### 风险分级清单

- **直接执行并报告**：只读命令（status/diff/log/branch -v）、`git add`、新建分支、局部 stash、本地 commit（未推送）
- **先征询**：一切 push、worktree 创建（obra consent 模式）、rebase/merge、reset、`clean -f`、`branch -D`、丢弃工作区修改、amend 已推送提交、历史重写、删除 tag/remote

### 快检序列（定稿）

`git status --porcelain` + `git stash list`（工作区）→ `git log --oneline -8` + `git branch -vv`（分支与历史）→ `git remote -v`（远程）→ untracked 文件 ignore 抽样。接管已有仓库时快检前先做画像。

### 结构与篇幅预算（英文）

```
skill-git-helper/
├── SKILL.md                     ≤160 行   准则 + 分级 + 体检 + 路由 + 驻留
├── README.md                              仓库说明
└── references/
    ├── commit.md               ≤160 行   无人区②节奏 + 完整 Conventional Commits + 暂存
    ├── worktree.md             ≤130 行   隔离生命周期：开工 + 收尾
    ├── ignore-governance.md    ≤130 行   无人区①所有权判定规则 + 内置模板
    ├── fork-upstream.md        ≤110 行   无人区③贡献者视角
    ├── repo-ops.md             ≤140 行   无人区④维护者视角（核心模块）
    ├── danger.md               ≤110 行   危险命令 + 补救篇
    └── concepts.md              ≤80 行   概念卡 16 张（Git 10 + GitHub 6）
```

超额自检规则：任何文件超预算即触发"下沉或砍掉"。

### repo-ops.md 范围（维护者视角，与 fork-upstream 的贡献者视角切分）

① 仓库体检清单（README/license/.github/branch protection/Actions 状态）② issue 管理（gh CLI + triage）③ PR review 与合并方式选择 ④ Actions 基础（只到"看懂和基本操作"，不教写 workflow）⑤ release 与 tag。①②③ 为核心，④⑤ 精简。

### 概念卡定稿（16 张，单文件两节，各留 1-2 空位）

- Git 节：detached HEAD、upstream/远程跟踪分支、origin vs upstream、fast-forward vs merge commit、rebase vs merge、force push 与 --force-with-lease、.git/info/exclude、worktree、reflog、checkout 多重身份
- GitHub 节：fork vs clone、PR 生命周期（含 draft）、三种合并方式、GitHub Flow vs Git Flow、Actions 三要素（event/job/step）、Issues+labels+milestone 的 triage 分工

### ignore-governance 判定规则（用户定的"所有权"版本）

本地特有文件 → 优先 `.git/info/exclude`；用户是仓库主要负责人且文件属于"每个协作者机器上都会有"的无用文件 → 进 `.gitignore`。已入库敏感文件的历史清除归 danger.md 补救篇。内置 node/python/IDE/OS 四类精简模板，离线可用。

## 3. 提炼式复用映射（创作期提取，非运行时委托）

| 来源（均 MIT） | 提炼内容 | 去向 | 有意丢弃 |
| --- | --- | --- | --- |
| obra/superpowers@using-git-worktrees (182.8K) | 隔离检测（含 submodule 防误判）、建前征询、原生工具优先、.worktrees/ 约定 + check-ignore 验证、基线测试 | worktree.md 开工篇 | 平台 sandbox fallback |
| obra/superpowers@finishing-a-development-branch (179.5K) | 测试验证后四选项决策（merge/PR/保留/丢弃）、清理流程 | worktree.md 收尾篇 | — |
| github/awesome-copilot@git-commit (44.3K) | diff 分析、智能暂存、机密文件清单、Git Safety Protocol | commit.md / danger.md | — |
| juliusbrussee/caveman-commit (339.7K) | 理念：why over what、祈使语气、剥离噪音 | commit.md 风格小节 | 其格式细节（用官方规范） |
| mattpocock/skills@git-guardrails (321.1K) | 拦截清单骨架（push --force 变体/reset --hard/clean -f/branch -D/checkout .）| danger.md 清单 | hook 安装流程 |
| awesome-copilot@gh-cli (21.9K) / @github-issues (15.5K) / @create-github-pull-request (9K) | gh CLI 规范、issue triage、PR 描述规范 | repo-ops.md / fork-upstream.md | — |

提炼原则：吸收骨架、重写血肉（按"建议 + why + 用户决策"风格）；每个提炼型文件头部标注出处；平台耦合内容不搬。

**三层防护边界**：本 skill 的 danger.md 是教育与确认话术层（会话内约束）；硬拦截属 hook 领域（git-guardrails 已验证的 PreToolUse 路线），README 中作为搭配建议提及，不内置。

## 4. 非目标

- 不做 commit message 生成之外的全自动 git 代办（决策权在用户）
- 不内置 hook / 不做运行时命令拦截
- 不教写 GitHub Actions workflow（repo-ops 只到看懂和基本操作）
- 不在用户未调用时主动触发或主动评判仓库历史

## 5. 待办与后续

- [ ] 实现评审（本次产出，用户过目）
- [ ] 评审通过后落入 agent skill 目录实测（触发、驻留、体检行为）
- [ ] repo-ops.md 按真实使用反馈迭代；概念卡按提问频率增补
- [ ] 决定发布仓库账号（long6177 / JL966）与远端后推送
