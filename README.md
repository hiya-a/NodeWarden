# NodeWarden
一个基于 Cloudflare Workers 的 Bitwarden 兼容服务器实现，专为个人用户设计。

[English](./README_EN.md) | 中文

---

> **免责声明**  
> 本项目仅供学习交流使用。我们不对任何数据丢失负责，强烈建议定期备份您的密码库。  
> 本项目与 Bitwarden 官方无关，请勿向 Bitwarden 官方反馈问题。

---

## 特性
- ✅ 完全免费，不需要在服务器上部署，再次感谢大善人！
- ✅ 完整的密码、笔记、卡片、身份信息管理
- ✅ 文件夹和收藏功能
- ✅ 文件附件支持（基于 R2 存储）
- ✅ 导入/导出功能
- ✅ 网站图标获取
- ✅ 登录限速保护（5 次失败后锁定 15 分钟）
- ✅ API 访问频率限制（60 次/分钟）
- ✅ 端到端加密（服务器无法查看明文）
- ✅ 兼容所有 Bitwarden 官方客户端

---

## 快速开始

### 一键部署

点击下方按钮部署到 Cloudflare Workers：

[![Deploy to Cloudflare Workers](https://deploy.workers.cloudflare.com/button)](https://deploy.workers.cloudflare.com/?url=https://github.com/shuaiplus/nodewarden)

**部署步骤：**

1. 使用 GitHub 登录并授权
2. 登录 Cloudflare 账户
3. **重要**：设置 `JWT_SECRET` 为强随机字符串（推荐使用 `openssl rand -hex 32` 生成）
4. KV 存储和 R2 存储桶将自动创建
5. 点击 Deploy 等待部署完成

> ⚠️ **再次提醒**：请务必使用强随机的 `JWT_SECRET`，使用默认或弱密钥可能导致账户被入侵，**后果自负！**

### 配置客户端

部署完成后，在任意 Bitwarden 客户端中：

1. 打开设置（⚙️）
2. 选择「自托管环境」
3. 服务器 URL 填入：`https://你的项目名`
4. 保存并返回登录页面

**首次注册**：直接访问 Workers 地址，在网页上完成账户注册。

---

## 手动部署

```bash
# 克隆项目
git clone https://github.com/shuaiplus/nodewarden.git
cd nodewarden

# 安装依赖
npm install

# 登录 Cloudflare
npx wrangler login

# 创建 KV 存储
npx wrangler kv namespace create VAULT
# 将输出的 id 填入 wrangler.toml 的 [[kv_namespaces]]

# 创建 R2 存储桶（用于文件附件）
npx wrangler r2 bucket create nodewarden-attachments

# 设置 JWT 密钥（请使用强随机字符串）
npx wrangler secret put JWT_SECRET
# 建议使用：openssl rand -hex 32

# 部署
npm run deploy
```

---

## NodeWarden vs Vaultwarden

NodeWarden 专注于**个人用户**的核心功能，保持代码简洁。以下是与 Vaultwarden 的功能对比：

| 功能 | NodeWarden | Vaultwarden | 说明 |
|------|:----------:|:-----------:|------|
| 密码/笔记/卡片/身份 | ✅ | ✅ | 完整支持 |
| 文件夹 & 收藏 | ✅ | ✅ | 完整支持 |
| 文件附件 | ✅ | ✅ | 使用 R2 存储，100MB 限制 |
| 导入/导出 | ✅ | ✅ | 完整支持 |
| 网站图标 | ✅ | ✅ | 代理获取 |
| 登录限速 | ✅ | ✅ | 防暴力破解 |
| 单用户模式 | ✅ | ✅ | 个人使用 |
| Bitwarden Send | ❌ | ✅ | 安全分享功能 |
| 两步验证 (2FA) | ❌ | ✅ | TOTP/WebAuthn 等 |
| 紧急访问 | ❌ | ✅ | 紧急联系人访问 |
| 组织/团队 | ❌ | ✅ | 多用户协作 |
| 实时同步 (WebSocket) | ❌ | ✅ | 多设备即时推送 |
| 邮件通知 | ❌ | ✅ | 需要 SMTP |
| 修改主密码 | ❌ | ✅ | 重新加密数据 |
| Admin 管理页 | ❌ | ✅ | 后台管理 |

> **💡 选择建议**  
> 如果你只需要个人密码管理，NodeWarden 足够使用且部署更简单。  
> 如果需要团队功能或高级特性，建议使用 [Vaultwarden](https://github.com/dani-garcia/vaultwarden)。

---

## 更新指南

如果你通过一键部署按钮安装，代码会被 fork 到你的 GitHub 账户。要获取最新更新：

### 方法 1：手动同步（推荐）

```bash
# 在你的 fork 仓库中
git remote add upstream https://github.com/shuaiplus/nodewarden.git
git fetch upstream
git merge upstream/main
git push origin main
```

### 方法 2：GitHub Actions 自动同步

项目已内置自动同步配置，在你的 fork 仓库中：

1. 进入 **Actions** 标签页
2. 如果看到提示"Workflows aren't being run on this forked repository"，点击 **I understand my workflows, go ahead and enable them**
3. 自动同步将每天运行一次（UTC 时间凌晨 2 点）
4. 也可以点击 **Sync Fork with Upstream** → **Run workflow** 手动触发

> **⚠️ 注意**：如果你修改了代码，自动同步可能会产生冲突，需要手动解决。

---

## 限制（本人认为完全没必要的功能）

- 不支持两步验证
- 不支持组织/团队功能
- 不支持修改主密码
- 文件附件大小限制 100MB

---

## 技术栈

- **运行环境**：Cloudflare Workers
- **数据存储**：Cloudflare KV
- **文件存储**：Cloudflare R2
- **开发语言**：TypeScript
- **加密算法**：客户端 AES-256-CBC，JWT 使用 HS256

---

## 常见问题

**Q: 如何备份数据？**  
A: 在客户端中选择「导出密码库」，保存 JSON 文件。

**Q: 忘记主密码怎么办？**  
A: 无法恢复，这是端到端加密的特性。建议妥善保管主密码。

**Q: 可以多人使用吗？**  
A: 不建议。本项目为单用户设计，多人使用请选择 Vaultwarden。

---

## 开源协议

MIT License

---

## 致谢

- [Bitwarden](https://bitwarden.com/) - 原始设计和客户端
- [Vaultwarden](https://github.com/dani-garcia/vaultwarden) - 服务器实现参考
- [Cloudflare Workers](https://workers.cloudflare.com/) - 无服务器平台