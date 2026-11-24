# 简报站点部署与维护说明

## 线上信息（已上线 ✅ 2026-11-22）
- **站点地址（手机收藏这个）**：https://ShanyangDiye.github.io/frontier-briefing/
- **仓库**：https://github.com/ShanyangDiye/frontier-briefing （public，Pages 为 legacy 分支构建）
- 已部署：第 1 期（2026-11-22，27 条）、第 2 期（2026-11-23，10 条）、第 3 期（2026-11-24，3 条新闻）；已验证直连 200、首页含各期入口
- 周末窗口规律：周六窗口内 arXiv 无公告、行业媒体基本无发布，属天然空窗；第 2 期采用"窗口闭合后首个公告批次（北京时间 09:00）并入并逐条标注"的口径
- 第 3 期为过渡新闻期（窗口内公告批次已并入第 2 期，下一批次 11-24 09:00 落入第 4 期窗口）；自第 4 期起恢复"每期一批"正常节奏，每期禁收清单见各期检索记录

## 本地目录
- 站点源文件：`D:\ZP\ZCode_data\leisure\frontier-briefing\`
  - `index.html` — 总索引（每日更新，最新一期排最上）
  - `2026-11-22.html` 等 — 各期页面（按日期命名，发布后永不改名/覆盖，旧链接永久有效）
  - `.nojekyll` — 跳过 Jekyll 构建标记
- 部署脚本：`D:\ZP\ZCode_data\tools\deploy_briefing.ps1`（幂等：文件存在则更新、不存在则创建）
- GitHub token：`D:\ZP\ZCode_data\tools\gh_token.txt`（classic，仅 public_repo 最小权限）

## 每日更新流程（由 ZCode 执行）
1. 检索四领域（里德堡原子 / 拓扑材料 / 射频前端 / 雷达）过去一天资讯；
2. 生成新页 `YYYY-MM-DD.html`，更新 `index.html`（新增本期条目、前后期导航）；
3. 重跑 `deploy_briefing.ps1`（token 自动读取），Pages 约 1–2 分钟后生效；
4. 用户手机刷新收藏的固定网址即可看到最新一期。

## 网络实测结论（2026-11-22）
- 本机网络：`api.github.com` 稳定可达（建仓/传文件/开 Pages 全走它，无需本地 git）；
- `github.com`（网页/授权/git 推送）直连不通，仅个别 IP 间歇可用；
- `*.github.io` **直连可用**（站点访问不受影响）；
- 若日后某网络打不开 github.io，可整体迁腾讯云 EdgeOne Pages，本地源文件完全复用。

## 安全说明
- token 明文存于本机 `tools\gh_token.txt`，仅能操作公开仓库（public_repo）；
- 如需撤销：GitHub → Settings → Developer settings → Personal access tokens 删除后重建，替换 token 文件即可。
