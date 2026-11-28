# 简报站点部署与维护说明

## 技能化（2026-11-27 起）
- 本流程已固化为可调用技能：`C:\Users\mjc_2\.agents\skills\frontier-briefing\SKILL.md`
- 内容含：窗口/命名约定、四领域细分方向、**A/B/C 三层来源矩阵**（对标豆包+AMiner 覆盖范围：arXiv + 期刊 TOC 直扫（含无预印本期刊）+ Zenodo 等学术仓储 + 科学媒体 + 国际防务媒体 + 厂商新闻稿 + 政府/军方合同演习层 + 财经通讯社 + 国内来源 + 会议/基金）、质量与去重纪律、页面模板与部署验证步骤。
- 对标依据（用户提供的豆包/AMiner 截图）：豆包=厂商官方稿+政府公告+军媒+合同演习，窗口 09:00→09:00；AMiner=关键词订阅覆盖 arXiv + 期刊（如 Optical and Quantum Electronics）+ Zenodo（CERN）仓储。

## 线上信息（已上线 ✅ 2026-11-22）
- **站点地址（手机收藏这个）**：https://ShanyangDiye.github.io/frontier-briefing/
- **仓库**：https://github.com/ShanyangDiye/frontier-briefing （public，Pages 为 legacy 分支构建）
- 已部署：第 1 期（2026-11-22，27 条）、第 2 期（2026-11-23，10 条）、第 3 期（2026-11-24，5 条新闻，重更新版）、第 4 期（2026-11-25，11 条）、第 5 期（2026-11-26，10 条）、第 6 期（2026-11-27，10 条论文）、第 7 期（2026-11-28，5 条）；已验证直连 200、首页含各期入口
- 索引页（2026-11-27 起）为日历 UI：绿色=已发布可点击、黄色=今天、灰=未来/未发布；每期在 index.html 的 ISSUES 数组追加 {d,no,n,stat} 即自动渲染
- **窗口约定（2026-11-24 起）**：每期窗口为北京时间 D 日 09:00 至 D+1 日 09:00，与 arXiv 公告时刻对齐——每期包含起点 D 日 09:00 公告批次；页面按窗口结束日命名
- **来源约定（2026-11-24 起，用户要求）**：除 arXiv 外，须覆盖期刊正式发表（Nature/Science/PRL/IEEE 等，看摘要或新闻报道即可，注明期刊与 DOI）+ 科学媒体（EurekAlert/phys.org/ScienceDaily/Nanowerk 等）+ 行业/厂商动态；第 3 期起新增"期刊"徽章类型
- 第 3 期特例：起点批次已随第 2 期发布故未重复收录；第 4 期起为标准节奏（含 11-24 09:00 批次）

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
