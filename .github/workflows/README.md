# 自动同步上游更新

这个 GitHub Actions 工作流会自动将上游仓库（shuaiplus/nodewarden）的更新同步到你的 fork。

## 功能特性

- ✅ 每天自动检查并同步上游更新
- ✅ 支持手动触发同步
- ✅ 自动处理简单的合并
- ⚠️ 遇到冲突时会提醒你手动处理

## 如何启用

1. 在你 fork 的仓库中，进入 **Actions** 标签页
2. 点击 **I understand my workflows, go ahead and enable them**
3. 完成！工作流会自动运行

## 手动触发

1. 进入 **Actions** 标签页
2. 选择 **Sync Fork with Upstream**
3. 点击 **Run workflow** → **Run workflow**

## 注意事项

- 如果你修改了代码，可能会产生合并冲突
- 遇到冲突时，工作流会失败，需要你手动解决
- 建议不要修改核心代码，只修改配置文件

## 禁用自动同步

如果你不想自动同步：

1. 进入 **Actions** 标签页
2. 选择 **Sync Fork with Upstream**
3. 点击右上角的 **···** → **Disable workflow**
