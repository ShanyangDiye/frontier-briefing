# 简报站点部署与维护说明

## 本地目录
- 站点源文件：`D:\ZP\ZCode_data\leisure\frontier-briefing\`（原「前沿简报」文件夹已改名，避免 URL 中文转义）
  - `index.html` — 总索引（每日更新，最新一期排在最上）
  - `2026-11-22.html` 等 — 各期页面（按日期命名，发布后永不改名/覆盖）
  - `.nojekyll` — GitHub Pages 跳过 Jekyll 构建的标记
- 部署脚本：`D:\ZP\ZCode_data\tools\deploy_briefing.ps1`
- 授权脚本：`D:\ZP\ZCode_data\tools\gh_device_auth.ps1`

## 当前网络结论（2026-11-22 实测）
- `api.github.com` 直连可用（建仓库/传文件/开 Pages 的 API 都走它）
- `github.com` 网页、OAuth 授权、git 推送在本机直连**不通**（仅新加坡 IP `20.205.243.166` 间歇可用）
- 推论：手机/浏览器大概率也无法访问 github.com 与 `*.github.io`

## 上线路径（三选一）
1. **GitHub 设备码授权（自动化程度最高）**
   我在后台重试获取设备码（`device_code.json`）。拿到后把 `https://github.com/login/device?user_code=XXXX-XXXX` 发给你，
   你用任何能访问 github.com 的浏览器（手机流量也算）打开并点击授权即可，之后全自动部署。
2. **手动 PAT（备选）**
   浏览器打开 `https://github.com/settings/tokens/new?scopes=public_repo`（已预选最小权限），
   生成 `ghp_` 开头的 classic token 发给我，我写入 `tools\gh_token.txt` 后一键部署。
3. **换腾讯云 EdgeOne Pages（若 github.com 手机也打不开，推荐）**
   注册/登录腾讯云 → 开通 EdgeOne Pages → 个人中心生成 API Token 发我，国内直连稳定，
   部署与每日更新由我通过其 API 完成。

## 拿到 GitHub token 后的部署/日常更新
```powershell
# 全量同步本地文件夹到仓库（存在则更新、不存在则创建）+ 开启 Pages
powershell -NoProfile -ExecutionPolicy Bypass -File D:\ZP\ZCode_data\tools\deploy_briefing.ps1
# token 缺省自动读取 tools\gh_token.txt；也可 -Token ghp_xxx 显式传入
```
每日流程：新增 `YYYY-MM-DD.html`、更新 `index.html`（加一行本期条目 + 前后期导航），再跑一次上述脚本。

## 注意
- 仓库为 public（GitHub Pages 免费版要求）；内容均为公开资讯摘要。
- `*.github.io` 在大陆网络可能无法直接访问，上线后请在手机实测；
  若打不开，改走路径 3（EdgeOne Pages），本地源文件完全复用。
- token 明文保存在 `tools\gh_token.txt`（最小权限 public_repo，只动这一个仓库），泄露时可随时在 GitHub 撤销。
