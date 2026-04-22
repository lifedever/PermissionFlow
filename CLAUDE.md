# PermissionFlow 维护指引

本仓库是 fork 自 [`jaywcjlove/PermissionFlow`](https://github.com/jaywcjlove/PermissionFlow)，由 `lifedever` 维护，主要消费方是 [PasteMemo](https://github.com/lifedever/PasteMemo-app)。

## 发版规范

**所有改动通过 SemVer tag 发布，不接受消费方用 commit revision 或 branch pin。**

完整规则见 [VERSIONING.md](./VERSIONING.md)（中文：[VERSIONING.zh.md](./VERSIONING.zh.md)）。

简化流程：

1. feature 分支开发 → merge / fast-forward 到 `main`
2. 在 `main` 打 SemVer tag（不加 `v` 前缀，如 `0.2.0`）
3. `git push origin main && git push origin <tag>`
4. 通知 PasteMemo 侧 `swift package update`，并一起 commit `Package.swift` + `Package.resolved`

版本号：

- Patch (`0.1.0 → 0.1.1`)：只修 bug
- Minor (`0.1.0 → 0.2.0`)：新增 API；**0.x 阶段 minor 也可能 breaking，changelog 要写清**
- Major：1.0 后才进入严格 SemVer

当前版本：`0.1.0`。

## 改动隔离原则（避免和 upstream 冲突）

rebase upstream 时，改动越集中在 fork 独有文件越好。

**fork 独有（随便改）**：
- `FORK.md`
- `VERSIONING.md` / `VERSIONING.zh.md`
- `CLAUDE.md`（本文件）
- `pastememo-enhancements` 分支上新加的代码文件

**upstream 共有（克制）**：
- `README.md` / `README.zh.md` / `Package.swift` / `Sources/**` 原有文件

如果必须改 upstream 共有文件：
- 首选"增量"（追加新 API、追加新文件），不要替换原有逻辑
- 必要的小修（比如 URL 占位符）可以直接改，冲突风险低
- 大的结构性调整先跟上游提 PR，拿不回来再 fork 内消化

## 改了库 → 别忘了下游

PasteMemo（`/Users/gefangshuai/Documents/Dev/myspace/PasteMemo/opensource`）通过 SPM 远程依赖消费本库。库里加/改公开 API 后：

1. 打新 tag push 到远程
2. 提醒用户在 PasteMemo 侧 `swift package update`
3. 如果是 breaking change，在 PasteMemo 侧的调用点也要一并更新

不要只改库不管下游。

## 与上游同步

```bash
git fetch upstream
git rebase upstream/main
```

rebase 前先切到 `main`，rebase 后考虑是否要打新 patch tag（如果有实质改动的话）。

## 与用户沟通用中文。
