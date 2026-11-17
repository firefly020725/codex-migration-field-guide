# 官方能力与相关工作

检索日期：2026-11-17。本文比较公开说明与本案例的需求；没有安装或执行下列社区工具。

**官方已有跨机 Handoff，社区也已有迁移经验和工具。** 本案例不是“发现了唯一可行的迁移方法”。它补充的是：在 Windows 目标用户已有数据的情况下，迁移大型多媒体会话，保留原身份，分别核验旧数据、历史、项目、媒体与后续工作分支。

要区分三个时间：功能发布的时间、本案例在 2026 年 10 月的实测时间、本文 11 月的检索时间。当前网页的内容不能自动当作当时某个安装包的全部能力；我们当时没有找到某个入口，也不能推出官方从未提供相关方案。

## 1 官方已经提供哪些能力

| 入口 | 公开说明覆盖什么 | 与本案例的关系 |
| --- | --- | --- |
| [Remote connections / Handoff](https://learn.chatgpt.com/docs/remote-connections#hand-off-a-chat-between-hosts) | 在本地与已连接远端主机之间交接聊天和 Git 状态；两侧需要匹配同一 Git 仓库及子目录的已保存项目，并创建或使用工作树 | 应优先评估的官方途径。公开说明没有明确承诺本案例要求的全部组合：保留原 ID、导入完整会话族、修复旧绝对媒体路径及与目标已有本地数据增量共存；不能据此反推这些能力必定不存在 |
| [Import from another agent](https://learn.chatgpt.com/docs/import) | 文档列出的桌面导入来源为 Claude Code、Claude Cowork、Cursor；CLI 列出 Claude Code、Cursor | 这个入口不能直接当作“原生 Codex 备份包导入”说明，文档没有把该类备份列作来源 |
| [App Server](https://learn.chatgpt.com/docs/app-server) | 提供会话读取、恢复、分叉和历史分页等接口；恢复与分叉的身份语义不同 | 是理解客户端行为的接口资料，不等于跨设备迁移的完整支持承诺。只读返回 `notLoaded` 或未加载的空历史，不能单独证明数据丢失 |
| [`migrate-rollouts` 的官方实现 PR](https://github.com/openai/codex/pull/37348) | 把本地旧格式 rollout 转成分页历史相关格式；默认 dry-run，显式应用才写入 | 这是本地存储格式迁移，不是两台电脑之间的会话包搬运。不要因命令里有 migration 就混为一谈 |
| [Network File Systems](https://learn.chatgpt.com/docs/environments/network-file-systems) | 讨论共享 HOME、SQLite 日志方式及相关数据库迁移 | 主要是 Linux/NFS 部署背景，不能直接当成 Windows NTFS、用户权限和 MSIX 桌面状态的搬家教程 |
| [Work：跨设备继续](https://learn.chatgpt.com/docs/get-started-with-work#continue-work-across-devices) | 说明适用任务的跨设备继续方式及云端工作方式 | 与既有本地 Codex 历史的离线接入不是同一个前提；不能把新的跨设备能力等同于旧本地档案已自动迁入 |

其中 Handoff 不是此次检索才出现的新功能：官方更新日志在 **2026-06-18、版本 26.616** 已记录本地与远端匹配项目间的任务交接，早于本案例。当时未验证该入口能否满足本案例的全部保留条件；本次补列当前官方说明，供读者优先评估。[官方更新日志](https://learn.chatgpt.com/docs/changelog)

截至本次检索，以上文档没有给出一个明确覆盖本案例全部验收条件的原生离线导出/增量导入流程。这是对所查公开资料的范围判断，不是对所有内部接口、账号功能或未来能力的否定。

## 2 已有社区实践

以下“支持”均按作者公开说明整理，**不代表我们已实测、安全审计或推荐直接运行**。同样叫 migration/sync，可能分别采用文件复制、索引更新、数据库写入、路径改写或新 ID 克隆；选型时先看实际语义。

| 工作 | 公开做法与接近之处 | 使用前需要区分的边界 |
| --- | --- | --- |
| [Windows 用户间迁移经验，Discussion #46461](https://github.com/openai/codex/discussions/46461) | 2026-09-18 的用户经验，涉及同机 Windows 用户、会话、归档、项目和界面可见性 | 最接近本案的先例之一；记录了 CLI 版本，但不能等同于本案桌面构建，也不能把复制索引或桌面状态的做法直接套到已有重要数据的目标 |
| [kroxiksut/codexSync](https://github.com/kroxiksut/codexSync) | GUI/CLI 同步与交接，涉及备份、冲突、会话、项目路径映射；作者声明 Windows 到 Windows 已测试 | 不负责搬运项目文件；Linux/macOS 为实验性。会话规则区分相同内容、前缀增长与分叉，不把分叉历史直接合并；本次所见版本包含 10 月案例之后的更新 |
| [ColinKiiim/codex-session-migration-skill](https://github.com/ColinKiiim/codex-session-migration-skill) | 技能与脚本组合，处理路径迁移、单会话打包、索引与可见性；作者提供跨系统验证记录 | 部分流程克隆新 ID，并在界面确认后归档来源；不能泛称“始终保留原 ID”。验证范围以其 [VALIDATION.md](https://github.com/ColinKiiim/codex-session-migration-skill/blob/main/VALIDATION.md) 为准 |
| [Kirayor/codex_local_migrate](https://github.com/Kirayor/codex_local_migrate) | 本地导出/导入会话、归档、索引及选定状态，提供 dry-run 和备份思路 | 代码包含数据库插入/替换路径，与本案保留目标库、由原生发现生成相关状态的路径不同；不能据 README 认定完整媒体或大型子会话族已验证 |
| [Jac0bXu/codex-sync](https://github.com/Jac0bXu/codex-sync) | 通过 SSH/rsync 同步会话的扩展，强调不覆盖与不删除 | 主要面向 rollout 同步，Windows 另需 rsync；仓库已归档。与上面的 `codexSync` 是不同项目，不要混淆 |

codexSync 的[会话规则](https://github.com/kroxiksut/codexSync/blob/15e47838d08e75f919cf7a223a98cef4780357b2/docs/en/SESSIONS.md)和[项目规则](https://github.com/kroxiksut/codexSync/blob/15e47838d08e75f919cf7a223a98cef4780357b2/docs/en/PROJECTS.md)尤其值得对照：它把历史分叉与项目路径映射分别处理。这与我们的经验“同 ID 不保证历史仍相同”“能读目录不等于项目已关联”相呼应，但不意味着两套实现完全相同。

为了让比较可回查，本次检索记录了以下代码快照；日期是对应提交日期，按 UTC 记录，**不是工具首次发布日或完整测试日**：

| 项目 | 本次参考代码快照 |
| --- | --- |
| kroxiksut/codexSync | [2026-10-25，15e4783](https://github.com/kroxiksut/codexSync/commit/15e47838d08e75f919cf7a223a98cef4780357b2) |
| ColinKiiim/codex-session-migration-skill | [2026-07-05，6dd2a42](https://github.com/ColinKiiim/codex-session-migration-skill/commit/6dd2a4220143c0b9969494d9572832a968df2382) |
| Kirayor/codex_local_migrate | [2026-05-21，a0a8df9](https://github.com/Kirayor/codex_local_migrate/commit/a0a8df9621b7c6b7e2850b765030687cdf1c8243) |
| Jac0bXu/codex-sync | [2026-06-19，6d5cdb8](https://github.com/Jac0bXu/codex-sync/commit/6d5cdb84f2da3b20d938cc419b43fe8aae130a14) |

本文只链接和比较，没有复制第三方实现。未来若复用代码，应对选定提交另行核对许可证；不能因为能公开下载就默认可以任意再发布。

## 3 官方仓库里的用户反馈，不等于官方方案

- [#47196：便携、完整的项目迁移需求](https://github.com/openai/codex/issues/47196)，2026-09-22。用户希望把历史、工具状态、附件及关系一起携带，说明这一组合需求还有人在提出；它不是官方路线图或交付承诺。
- [#37673：大 JSONL 行与格式迁移反馈](https://github.com/openai/codex/issues/37673)，2026-08-09。用户在特定 CLI 版本和 macOS 上报告大记录迁移问题。这提示测试媒体大行的价值，但我们未复现，也不据此断言当前版本有同一缺陷。
- [#36663：升级后的项目关系反馈](https://github.com/openai/codex/issues/36663)，2026-08-03。它提示项目可见性应独立验收，但不能作为本案项目未关联的原因证明。

## 4 我们应怎样定位这份经验

读者应先检查官方 Handoff 或现有工具是否符合自己的场景。若适合，不必重演我们的全部手工过程。

这份材料提供的是另一个视角：一个已有真实目标数据、主会话约 3.65 GB、会话族 69 个、外部媒体较多的 Windows 案例，怎样定义成功、发现误报、保留限制并结束迁移。尤其值得复用的是验收清单和合成例子，而不是绑定真实机器的一次性管理员脚本。

我们没有做工具排名，也不声称覆盖全部方案。欢迎以版本、日期、最小合成复现及公开来源补充这份比较。
