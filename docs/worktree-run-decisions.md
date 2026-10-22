# worktree-run 决策记录

> 维护文档，**不随 skill 分发**：安装到用户本地的只有 `skills/worktree-run/` 下的 SKILL.md 和 README.md，本文留在仓库 `docs/`。
> SKILL.md 只写规则、不写理由；理由、被否掉的替代方案和验收证据都在这里，避免下一轮优化把已经讨论过的东西再讨论一遍。
> 对应提交：`89e5709`（2026-10-22）。

## 目标与边界

- **目标**：同一条 prompt 交给不同的 `LLM × harness`，各跑一份独立结果，由人对比后自选。
- **不做**：不评分、不选优、不调度、不评测框架、不引入容器/虚拟机。
- **约束**（已定，勿再推翻）：轻量、不引入重依赖、不干扰日常业务、最终评判交给人。
- **现实前提**：worktree 只隔离工作目录和 `HEAD`，`.git`（refs、objects、reflog、stash）全共享 → 没有真正的结构性隔离，只能"轻结构 + 明令禁止"。

## 决策

| # | 决策 | 理由 | 已否掉的替代 | 代价 / 残余风险 |
|---|---|---|---|---|
| 1 | worktree 建在仓库内 `<repo>/.worktrees/<h>-<l>` | 运行期 agent 的 cwd 是**仓库根**，各 harness 的 workspace 按会话 cwd 锚定；放仓库外等于把整条腿搬出沙箱 | 仓库外同级目录；每条腿在 worktree 里另开会话 | 在仓库根递归会看到别的腿 → 靠规则 + ignore 缓解 |
| 2 | base 固定成 40 位 SHA，缺省 `HEAD` | 两条腿必须同一起跑线，事后可追溯 | 直接用分支名/标签 | 无 |
| 3 | commit subject = **用户请求**（≤60 字符，尽量贴近原话），body = 改动 | 让"列分支"零信息增量；答案只存在于 body | subject 描述改动（旧版做法）；固定无信息 subject（草稿做法） | 各腿措辞必然不同；agent 真把解法写进 subject 时人一眼可见，多了再收紧 |
| 4 | subject 末尾 `[commit N]` 自增 | 一次 run 允许多条 commit，需要区分 | 只有第一条带请求 | 泄漏"改了几轮"（有意接受：计数属普通上下文） |
| 5 | 只在**最后一条** commit 的 body 追加 `branch/tools/skills/mcp/Base` | 对比需要的元数据一次给全，避免每条都吵 | 每条都带；完全不带 | 无 |
| 6 | per-worktree 设 `format.pretty=oneline` | 让裸 `git log` / `git show` / `git log --all` 结构性地打不出 body，不再依赖 agent 自觉 | 只靠规则文字；改全局 config（会污染用户自己的 git） | 需 git≥2.20 + `extensions.worktreeConfig`；`--format=%B` 显式读取仍会漏 |
| 7 | `.worktrees/` 写进 `.git/info/exclude` | 不改用户的 `.gitignore`；ripgrep 系工具（含 Claude Code 的 Grep）默认尊重它，最常用的递归搜索扫不到别的腿 | 写 `.gitignore` | `grep -r` / `find` / `Get-ChildItem -Recurse` 不认 ignore |
| 8 | 历史里的 `[worktree-run]` commit 也是禁区（含 base 祖先） | winner 合并进 base 后，它的 diff/body 依旧是答案 | 只禁"base 之后" | 依赖 agent 识别 subject 前缀；base 工作树本身仍可自由读（只看 commit 关闭） |
| 9 | 同组合重复运行 → **删旧分支、按新 base 重建** | 一轮结束的判据是"人比对完并 merge 了 winner"，其余分支已无意义；同名分支无法双检出 | `-2/-3` 后缀保留旧结果；复用旧分支（起点过期） | 未评判就重跑会丢结果（README 已提示先 merge）；分支被 live worktree 检出时必须停下问 |
| 10 | 规则只管一次 run，结束后全部失效 | 不把禁区带进用户后续的业务会话 | 常驻规则 | 无 |
| 11 | 结束时只加一行 `branch: …`；真读到禁区时加一行 `Note:` | 噪声最小，`Note` 是唯一可留痕的证据 | 不写 Note；中断任务 | 无 |
| 12 | 不装 `commit-msg` 钩子 | 钩子只能规范格式，挡不住读取，且 `--no-verify` 可绕 | 钩子 + per-worktree `core.hooksPath`（已实测可行） | 格式错误靠提交后 `git log -1 --format=%s` 自查 |

## 明确不做的

- 容器 / VM / 独立 clone：重依赖，违背"轻量"约束（真要真隔离就 clone，README 已写）。
- harness 级 `PreToolUse` / 权限 deny：每个 harness 一套配置，等于把 skill 绑死在具体 harness 上。
- PATH 里放 `git` shim：Windows 下 pwsh/cmd/bash 各一套，脆。
- 自动评分 / 对比 skill（arena、council、模型 PK 之类）：评判权归人，用户明确不要。
- 依赖 worktrunk（`wt`）等外部二进制：用户不要新依赖；它解决的是"多环境+端口+进程生命周期"，与本 skill 的定位不同。

## 已知残余风险（讨论过，别再当新问题）

- `git show <别的腿>`、`git diff <别的腿>` 仍会出 **diff 内容**；`--format=%B`、`git cat-file` 显式读取仍会漏 body。
- `git stash` 是仓库级共享的 → 直接禁用（避免套用别的腿的暂存）。
- `.git` 完全共享：分支、reflog、objects、计数都可见（有意接受，靠 subject 结构保证零信息增量）。
- 文件级规则（不许在仓库根递归、不许读 `.git`、未跟踪文件只按精确路径访问）是**约定**，不是强制。
- 一次运行内的"停止条件"只覆盖失败/中断；agent 自行放弃任务不在本 skill 管辖范围。

## 验收证据

- 临时仓库 18/18：per-worktree 配置生效、主树 config 不受影响、裸 `git log --all` 隐藏 body、subject 形态、清理后无残留。
- `codex_attemp` 真实仓库 16/16：建腿 → 提交 → 移除 → toggle 清理；同组合重跑走"删旧重建"且旧提交不可达；分支被检出的情况下 `git branch -D` 被 git 拒绝。
- 体积预算：SKILL.md ≤ ~700 words（纯规则）；README ~450 words（给人看，讲背景与取舍）。

## 维护约定

1. **SKILL.md 不放解释性文字**（用户明确要求：agent 应专注任务，规则要短、可执行）。
2. 新规则写进 SKILL.md 对应段落；**理由写进本文件**，两边不重复。
3. 改读取/隔离策略前先看本文"已否掉的替代"，避免绕回原路。
4. 改完用临时仓库跑一遍验收清单（见上），再动真实仓库。
