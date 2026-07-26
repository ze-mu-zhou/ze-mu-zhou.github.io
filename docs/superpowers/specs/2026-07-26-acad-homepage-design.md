# 学术主页部署设计（GitHub Pages 路线）

日期：2026-07-26
状态：已实施并上线（https://ze-mu-zhou.github.io）

## 背景与决策

最初计划在自有 Linux VPS 上部署自己和少数熟人的学术主页（子路径方案），
因备案/运维成本放弃自托管，改为模板原生支持的 GitHub Pages 路线。

模板：[RayeRen/acad-homepage.github.io](https://github.com/RayeRen/acad-homepage.github.io)
（AcadHomepage，Jekyll，`github-pages` gem，Pages 原生构建，无需自建 CI）。

方案对比（当时）：

- A. 网页 fork（官方路径）——最傻瓜，适合熟人自助；本人仓库仍需手动网页操作。
- B. **API 建仓 + 干净首提交（采用）**——无需网页操作，历史干净，可全程脚本化。
- C. "Use this template" 按钮——与 A 无实质差异。

范围收敛：只做本人站点；熟人部分后续需要时再出指南。

## 架构

- 托管：GitHub Pages（user site，`ze-mu-zhou/ze-mu-zhou.github.io`，main 分支根目录，legacy 构建）。
- 构建：push 触发 Pages 原生 Jekyll 构建，无自定义 Action。
- Scholar 引用爬虫：暂不启用（已删除 `google_scholar_crawler.yaml`；
  恢复方法：从模板仓库取回该文件，并在 Settings → Secrets 配 `GOOGLE_SCHOLAR_ID`）。
- 无服务器、无备案、零运维成本。

## 已实施

1. API 创建公开仓库 `ze-mu-zhou.github.io`（token scopes: repo, workflow）。
2. 浅克隆模板 → 去 `.git` → 删 crawler workflow。
3. `_config.yml` 按 GitHub 公开资料预填：title/description/repository，
   author.name=zemu、bio/employer=NEU Software Engineering、location=沈阳、
   email、github=ze-mu-zhou；Scholar/Analytics/SEO 验证留空。
4. 干净首提交（保留模板 LICENSE 与出处说明）推 main；Pages 构建约 40 秒，
   站点 200 且 HTML 内容抽查（title/bio/author）通过。

## 遗留（内容侧，非阻塞）

- `_pages/about.md` 已填入真实内容（2026-07-26）：NEU 软工本科、9 项竞赛/获奖、
  CVE-2026-31323；教育经历起始年 2024.09 系从学号邮箱推断，待本人确认。
- Publications/Invited Talks/Internships 暂无内容，板块已移除，需要时照模板加回。
- 头像/favicon 仍是模板默认图（`images/`，可用 favicon-generator 生成后替换）。
- Google Analytics / 各站长平台 SEO 验证未配。
- Scholar 爬虫按上述方法可随时补开。
