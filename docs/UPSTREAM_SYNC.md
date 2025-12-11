# 上游仓库自动同步

## 概述

本仓库配置了自动同步上游仓库（[xhc2008/CABM](https://github.com/xhc2008/CABM)）的 GitHub Actions 工作流。

## 工作流说明

### 自动执行

- **执行时间**: 每天凌晨 2:00 UTC（北京时间 10:00）
- **工作流文件**: `.github/workflows/sync-upstream.yml`
- **触发方式**:
  - 定时触发（每天自动运行）
  - 手动触发（在 GitHub Actions 页面点击 "Run workflow"）

### 工作流程

1. **检查更新**: 比较本地 main 分支与上游 main 分支的最新提交
2. **如果有更新**:
   - 创建临时同步分支（格式：`sync-upstream-YYYYMMDD-HHMMSS`）
   - 尝试自动合并上游更改
3. **合并成功**: 自动创建 Pull Request，包含同步的更改
4. **合并冲突**: 创建 Issue，提供手动解决冲突的详细步骤
5. **无更新**: 工作流结束，不执行任何操作

## 同步成功后的操作

当工作流成功创建 Pull Request 后：

1. 仔细审查 PR 中的更改
2. 确保更改与本仓库的定制内容兼容
3. 运行测试确保功能正常
4. 如果一切正常，合并 PR

## 处理合并冲突

如果自动合并失败，工作流会创建一个 Issue 并提供详细的手动解决步骤：

### 手动解决步骤

```bash
# 1. 克隆仓库
git clone https://github.com/ICEE-BING/CABM.git
cd CABM

# 2. 添加上游远程仓库
git remote add upstream https://github.com/xhc2008/CABM.git

# 3. 获取上游更改
git fetch upstream

# 4. 创建同步分支
git checkout -b sync-upstream-manual

# 5. 合并上游更改
git merge upstream/main

# 6. 解决冲突
# 使用你喜欢的编辑器解决冲突文件
git add .
git commit -m "Resolve merge conflicts with upstream"

# 7. 推送并创建 PR
git push origin sync-upstream-manual
```

### 注意事项

- 仔细检查冲突文件，确保保留本仓库的定制功能
- 特别关注配置文件和定制化代码
- 解决冲突后务必进行测试
- 确保所有功能正常工作后再合并

## 手动触发同步

如果需要立即同步上游更改：

1. 访问仓库的 [Actions 页面](https://github.com/ICEE-BING/CABM/actions)
2. 选择 "Sync Upstream Repository" 工作流
3. 点击 "Run workflow" 按钮
4. 选择 main 分支
5. 点击 "Run workflow" 确认

## 查看同步历史

- 成功的同步会以 Pull Request 的形式记录
- 失败的同步会以 Issue 的形式记录
- 所有执行记录可在 Actions 页面查看

## 标签说明

- `sync-upstream`: 所有与上游同步相关的 PR 和 Issue
- `automated`: 自动创建的 PR
- `merge-conflict`: 存在合并冲突的同步尝试
- `help-wanted`: 需要手动处理的情况

## 常见问题

### Q: 为什么选择 2:00 UTC？

A: 这个时间对应北京时间 10:00，是工作日的常规工作时间，便于及时处理同步后的 PR 或冲突。

### Q: 可以修改同步频率吗？

A: 可以。编辑 `.github/workflows/sync-upstream.yml` 文件中的 cron 表达式：
- `0 2 * * *` - 每天一次
- `0 */12 * * *` - 每 12 小时一次
- `0 2 * * 1` - 每周一一次

### Q: 会自动合并 PR 吗？

A: 不会。所有 PR 都需要手动审查和合并，以确保更改不会破坏定制功能。

### Q: 如果长期不同步会怎样？

A: 时间越长，可能出现的冲突越多。建议定期检查并合并同步 PR。

## 相关资源

- [上游仓库](https://github.com/xhc2008/CABM)
- [GitHub Actions 文档](https://docs.github.com/en/actions)
- [贡献指南](../CONTRIBUTING.md)
