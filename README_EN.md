# NodeWarden

An alternative implementation of the Bitwarden server API running on Cloudflare Workers, designed for personal use.

English | [中文](./README.md)

---

## ⚠️ Important Notice


> **Disclaimer**  
> This project is for educational purposes only. We are not responsible for any data loss. Regular backups are strongly recommended.  
> This project is not associated with Bitwarden. Do not report issues to Bitwarden's official support channels.

---

## Features

- ✅ Full password, note, card, and identity management
- ✅ Folders and favorites
- ✅ File attachments (R2 storage, 100MB limit)
- ✅ Import/Export functionality
- ✅ Website icons
- ✅ Login rate limiting (lockout after 5 failed attempts for 15 minutes)
- ✅ API rate limiting (60 requests/minute)
- ✅ End-to-end encryption (server cannot access plaintext)
- ✅ Compatible with all official Bitwarden clients

---

## Quick Start

### One-Click Deploy

Click the button below to deploy to Cloudflare Workers:

[![Deploy to Cloudflare Workers](https://deploy.workers.cloudflare.com/button)](https://deploy.workers.cloudflare.com/?url=https://github.com/shuaiplus/nodewarden)

**Deployment Steps:**

1. Sign in with GitHub and authorize
2. Log in to your Cloudflare account
3. **Important**: Set `JWT_SECRET` to a strong random string (use `openssl rand -hex 32`)
4. KV storage and R2 bucket will be auto-provisioned
5. Click Deploy and wait for completion

> ⚠️ **Reminder**: Always use a strong random `JWT_SECRET`. Never use example values or simple strings!

### Client Setup

After deployment, open any Bitwarden client:

1. Click Settings (⚙️)
2. Select "Self-hosted environment"
3. Enter Server URL: `https://your-project.workers.dev`
4. Save and return to login page

**First-time registration**: Visit your Workers URL directly to register an account.

---

## Manual Deployment

```bash
# Clone
git clone https://github.com/shuaiplus/nodewarden.git
cd nodewarden

# Install
npm install

# Login to Cloudflare
npx wrangler login

# Create KV storage
npx wrangler kv namespace create VAULT
# Copy the id to wrangler.toml [[kv_namespaces]]

# Create R2 bucket (for file attachments)
npx wrangler r2 bucket create nodewarden-attachments

# Set JWT secret (use a strong random string)
npx wrangler secret put JWT_SECRET
# Recommended: openssl rand -hex 32

# Deploy
npm run deploy
```

---

## NodeWarden vs Vaultwarden

NodeWarden focuses on **personal users** with core features, keeping the codebase minimal. Here's a comparison with Vaultwarden:

| Feature | NodeWarden | Vaultwarden | Notes |
|---------|:----------:|:-----------:|-------|
| Passwords/Notes/Cards/Identity | ✅ | ✅ | Full support |
| Folders & Favorites | ✅ | ✅ | Full support |
| File Attachments | ✅ | ✅ | R2 storage, 100MB limit |
| Import/Export | ✅ | ✅ | Full support |
| Website Icons | ✅ | ✅ | Proxy fetch |
| Login Rate Limiting | ✅ | ✅ | Brute-force protection |
| Single User Mode | ✅ | ✅ | Personal use |
| Bitwarden Send | ❌ | ✅ | Secure sharing |
| Two-Factor Auth (2FA) | ❌ | ✅ | TOTP/WebAuthn etc |
| Emergency Access | ❌ | ✅ | Emergency contacts |
| Organizations/Teams | ❌ | ✅ | Multi-user collaboration |
| Real-time Sync (WebSocket) | ❌ | ✅ | Instant multi-device push |
| Email Notifications | ❌ | ✅ | Requires SMTP |
| Change Master Password | ❌ | ✅ | Re-encrypt vault |
| Admin Panel | ❌ | ✅ | Backend management |

> **💡 Recommendation**  
> If you only need personal password management, NodeWarden is sufficient and easier to deploy.  
> For team features or advanced capabilities, consider [Vaultwarden](https://github.com/dani-garcia/vaultwarden).

---

## Update Guide

If you deployed via the one-click button, the code is forked to your GitHub account. To get the latest updates:

### Method 1: Manual Sync (Recommended)

```bash
# In your forked repository
git remote add upstream https://github.com/shuaiplus/nodewarden.git
git fetch upstream
git merge upstream/main
git push origin main
```

### Method 2: GitHub Actions Auto-Sync

The project includes built-in auto-sync configuration. In your forked repository:

1. Go to the **Actions** tab
2. If you see "Workflows aren't being run on this forked repository", click **I understand my workflows, go ahead and enable them**
3. Auto-sync will run daily at 2:00 AM UTC
4. You can also manually trigger by clicking **Sync Fork with Upstream** → **Run workflow**

> **⚠️ Note**: If you've modified the code, auto-sync may cause merge conflicts that require manual resolution.

---

## Limitations

- Single user only (personal use)
- No two-factor authentication
- No organization/team support
- Cannot change master password
- File attachment size limit: 100MB

---

## Tech Stack

- **Runtime**: Cloudflare Workers
- **Data Storage**: Cloudflare KV
- **File Storage**: Cloudflare R2
- **Language**: TypeScript
- **Encryption**: Client-side AES-256-CBC, JWT with HS256

---

## Security Recommendations

1. **Strong JWT_SECRET**: Generate with `openssl rand -hex 32`
2. **Regular Backups**: Export your vault and store securely
3. **HTTPS Access**: Cloudflare Workers provides HTTPS by default
4. **Access Control**: Use Cloudflare WAF rules or IP whitelist

---

## FAQ

**Q: How to backup data?**  
A: In the client, select "Export Vault" and save the JSON file.

**Q: Forgot master password?**  
A: Cannot be recovered due to end-to-end encryption. Keep your master password safe.

**Q: Can multiple people use it?**  
A: Not recommended. This project is designed for single user. Use Vaultwarden for multi-user scenarios.

---

## License

MIT License

---

## Acknowledgments

- [Bitwarden](https://bitwarden.com/) - Original design and clients
- [Vaultwarden](https://github.com/dani-garcia/vaultwarden) - Server implementation reference
- [Cloudflare Workers](https://workers.cloudflare.com/) - Serverless platform
