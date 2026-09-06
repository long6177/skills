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

### 3.1 逐节溯源（section 级）

三类来源：**① 提炼自其他 skill**（骨架跟随、血肉重写）／**② 改编自官方规范或社区通用知识**（非抄自 skill，也非原创，如 Conventional Commits 规范本身）／**③ 原创**（主要为调研发现的无人区内容）。

| 文件 | ① 提炼自其他 skill | ② 改编自官方/社区通用知识 | ③ 原创 |
| --- | --- | --- | --- |
| worktree.md | **提炼浓度最高**。开工篇几乎整体来自 obra/superpowers@using-git-worktrees：隔离检测命令（`GIT_DIR` vs `GIT_COMMON`）、submodule 防误判、建前征询、原生工具优先、`.worktrees/` 目录优先级 + `check-ignore` 强制验证、装依赖 + 基线测试。收尾篇来自 obra@finishing-a-development-branch：先验证测试再给选项、四选项决策菜单、`worktree remove/prune` + `branch -d` 清理。速查表压缩改编自 obra 的同名表。重写：全部表述换成"话术 + 风险级"风格，删平台耦合内容 | — | "为什么用 worktree"的引言（原始想法痛点 2 场景：自己阅读 vs Agent 干活）；"拿不准就先给分支尖端打 tag"的兜底建议 |
| commit.md | 暂存一节（§3）来自 awesome-copilot@git-commit：三个 diff 分析命令、逻辑分组、`add -p`、机密文件清单。风格小节（§4）提炼自 caveman-commit 的理念：why over what、祈使语气、无句号、无 AI 署名、50/72 字符 | §5 Conventional Commits 完整规范改编自 conventionalcommits.org 官方规范（"内置完整规范"决策的落地处） | §1 何时提交、§2 粒度自检问题——这是无人区②的核心；"发现 untracked 堆积先转 ignore 治理"的胶水逻辑；"直提后示 + 可 amend 返工"的交互设计 |
| danger.md | 清单骨架来自 mattpocock/skills@git-guardrails 的拦截列表（force push、reset --hard、clean -f、branch -D、checkout ./restore .，即第 1-5 条）；第 6-9 条（amend 已推送、共享分支 rebase、--no-verify、删远程分支/tag）改写自 awesome-copilot@git-commit 的 Git Safety Protocol | — | 每条的"更安全替代"（`--force-with-lease`、先 stash、`clean -n` 干跑、`restore -p`）和全部确认话术；整节"补救篇"（先吊销凭据 → filter-repo/BFG → 通知协作者重新 clone）；文末 hook 边界说明 |
| repo-ops.md | §2 issue 管理：`gh` 命令组合来自 awesome-copilot@gh-cli，triage 流程（复现→标签→优先级→指派）来自 @github-issues | — | §1 仓库体检清单表；§3 三种合并方式的取舍表（含"AI 重度工作流默认 squash"的判断）；§4 Actions 基础与 flaky 识别原则；§5 release 流程 |
| ignore-governance.md | —（生态空白，无可提炼对象） | 内置四类模板是社区通行的 gitignore 模式（文末注明 github/gitignore 为完整参考）；`rm --cached` 处理是 git 标准知识 | **几乎全部**：用户定下的所有权判定规则的形式化、安全/协作/个人三分类及各自的处置路径、与 danger.md 补救篇的衔接 |
| fork-upstream.md | PR 描述规范小段（what/why/how tested、用仓库模板）提炼自 awesome-copilot@create-github-pull-request | `origin`/`upstream` 命名约定、`--ff-only` 同步策略是社区标准实践 | fork 前三问；"只 rebase 自己的 PR 分支"规则；"完事前同步检查"；PR 礼仪大部分（小步、CI 绿、不中途 force-push、烂尾礼貌关闭） |
| concepts.md | —（没有其他 skill 做过概念卡） | 卡片知识本身是 git/GitHub 通用语义 | 16 张卡的选题（grilling 三轮定稿）、"≤5 行 + 预留空位"的机制、每张卡"和你什么有关"的写法 |
| SKILL.md | 一条模式借自 obra："尊重用户已声明偏好，不再重复询问"（其 worktree skill 的 consent 设计） | — | 维护模式、风险分级清单（grilling Q3=B 决策的形式化）、快检序列、输出格式（一行状态表 + ≤3 条建议）、路由表、hook 边界声明 |

提炼浓度：worktree.md ≈ danger.md（骨架）> commit.md ≈ repo-ops.md > fork-upstream.md；**ignore-governance.md 与 concepts.md 为纯原创**，对应调研发现的两个最大空白点。与各文件头部的出处注释互为印证：注释管"哪一行抄了谁"，本表管"为什么这么分工"。

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
