# Codex Migration Field Guide

**非官方案例与验收指南 · 2026-11-17**

一次 Windows Codex 大型会话搬家：在目标已有数据的前提下，保留原会话 ID、历史、项目归属及可恢复图片。案例完成于 2026-10-19，实测桌面版本为 `26.1013.3656.0`。

> 19,872 个冻结文件 · 69 个会话 · 约 37.29 GiB 冻结包 · 保留目标原有 329 个会话 · 最终回填 1,066 个媒体文件

本稿提供经验、判断方法、可直接发给 Codex 的提示词和空白模板；不包含真实会话、数据库、截图或认证信息。文件核验、界面抽查和未解决问题分别记录。

官方已有跨机 Handoff，社区也有迁移工具。本例补充一个具体场景的经验，不主张首创；请先看[官方能力与相关工作](docs/RELATED_WORK.zh-CN.md)，判断是否已有更合适的路径。

## 从哪里开始

| 你要解决的问题 | 阅读入口 |
| --- | --- |
| 想让自己的 Codex 根据本机情况开始帮忙 | [复制这段提示词](prompts/START_HERE.zh-CN.md) |
| 想知道官方和社区已经做了什么 | [官方能力与相关工作](docs/RELATED_WORK.zh-CN.md) |
| 想了解这次实际怎样搬、哪里走了弯路 | [完整案例复盘](docs/CASE_STUDY.zh-CN.md) |
| 想为自己的迁移拟定可检验的步骤 | [迁移验收清单](templates/acceptance-checklist.md) |
| 想记录文件、媒体映射、缺失和分支 | [清单示例说明](examples/README.md)与[合成清单](examples/manifest.synthetic.json) |

## 直接交给你的 Codex

[打开并复制完整提示词](prompts/START_HERE.zh-CN.md)，补充你已知的迁移目标和机器情况即可。它会引导 Codex 先只读盘点、核对当前可用方案，再形成具体计划；确认后在约定范围内推进。未知信息可以留空，由 Codex 根据实机检查并在必要时提问。

提示词保留了这次经历中的判断要点，让接收方依据自己的版本、数据和权限选择做法。本仓库以文档与提示词为交付内容，工具开发不在当前计划内。

## 最值得带走的四件事

1. **文件、聊天、图片、项目是不同的验收层。** 复制成功、原生历史可见、实际续聊和原图片可见，需要各自的证据。
2. **已有目标数据也要验收。** 本例增量接入，保留目标数据库，没有用来源整套数据目录覆盖目标。
3. **成功追加不是历史损坏。** 恢复设置和新消息会增长会话；检查冻结前缀和已有消息身份，而非要求整文件永远不变。
4. **相同 ID 不代表同一份活跃历史。** 两边续聊后会分开增长，必须明确哪一份继续工作。

## 这个案例没有证明什么

未证明跨版本、跨系统通用；未逐个恢复执行全部子智能体；未把图片查看通过当作新的生图、编辑、编码与音频生产能力通过。剩余 349 条历史缺失引用有记录，不能叫全部图片已恢复。

旧路径兼容副本是受限的案例 workaround，不是让读者直接照抄的 `C:\Users` 目录修复指令。正式运行过的脚本含机器身份和精确对象约束，没有作为通用工具发布。

## English overview

This unofficial field guide documents one Windows-to-Windows migration of a large local Codex project. It preserved the original main thread identity and the destination user's existing data, then validated native history, manual continuation, project membership, selected child-thread relationships, and recovered media.

The frozen bundle contained 69 threads and about 37.29 GiB of files. Media recovery ultimately covered 1,066 files; UI validation used representative samples plus five individually checked supplemental images. Some historical references remain unresolved.

The key contribution is a version-scoped acceptance method and a candid account of failed assumptions. Official local/remote handoff, cloud task continuity, third-party sync, and local archive migration have different requirements; consult [related work](docs/RELATED_WORK.zh-CN.md) before choosing a path. This is not a universal importer. All examples here are synthetic. No real user data is included.

A [copyable starter prompt in Chinese](prompts/START_HERE.zh-CN.md) helps your own Codex inspect its environment, propose a concrete migration plan, and adapt the method and acceptance checks to your requirements. Developing a migration utility is outside the current project scope.

## 分享与贡献

欢迎补充明确版本、操作系统、目标原有数据情况、成功边界及来源链接。请只提交最小合成复现，不上传整套 `.codex`、真实 rollout、数据库、令牌或未脱敏截图。

由 [firefly020725](https://github.com/firefly020725) 与 Codex 协作整理；运行和界面观察由项目使用者参与验证，文字中的产品能力以链接证据为准。

## 许可证

本仓库的原创文档、提示词、模板与合成示例采用 [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/)，完整条款见 [LICENSE](LICENSE)。转载或改编时请署名 firefly020725，保留本仓库与许可证链接，并注明改动。外链资料及第三方项目遵循各自的许可。

Original documentation, prompts, templates, and synthetic examples are licensed under CC BY 4.0. Attribute firefly020725, link to this repository and the license, and indicate changes.
