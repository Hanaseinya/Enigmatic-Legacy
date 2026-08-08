# 构建工作流说明

这个目录下的 `build.yml` 用来做 GitHub Actions 构建和 GitHub Release。

## 触发方式

- `pull_request`
  - 总是执行构建校验
  - 不发布 GitHub Release

- `push`
  - 只有最新一次 commit message 里包含特定关键字时才会构建

- `workflow_dispatch`
  - 手动触发时总是执行构建
  - 不自动发布 GitHub Release

## commit message 关键字

- `build-action`
  - 执行构建
  - 上传构建产物到 Actions Artifacts
  - 不发布 GitHub Release

- `build-release`
  - 执行构建
  - 上传构建产物
  - 发布 GitHub Release

## 日志控制

- 默认会带上 `--stacktrace`
- 如果 commit message 里包含 `--info`
  - 构建命令会追加 `--info`

- 如果 commit message 里包含 `--debug`
  - 构建命令会追加 `--info --debug`

示例：

```text
chore: trigger ci; build-action --debug
```

```text
chore: publish fork build; build-release --info
```

## 当前 CI 行为

- CI 构建时会传入 `-PciBuildOnly=true`
- 这会跳过 `Modrinth` 和 `CurseForge` 相关的发布配置
- 只执行本地 Forge 模组构建和 GitHub Release
- GitHub Release 采用“存在则更新，不存在则创建”的策略
- 创建 release 时会显式绑定到当前 commit SHA

## 产物命名

上传到 GitHub Actions / GitHub Release 的文件名格式：

```text
EnigmaticLegacy-v<版本号>-mc<MC版本>-forge-vincentzyu-fork.jar
```

例如：

```text
EnigmaticLegacy-v2.30.1-mc1.20.1-forge-vincentzyu-fork.jar
```
