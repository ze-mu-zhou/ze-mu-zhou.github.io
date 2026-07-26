# 学术主页部署设计（GitHub Pages 路线）

日期：2026-07-26（2026-07-26 晚更新：迁移 al-folio）
状态：已实施并上线（https://ze-mu-zhou.github.io）

## 背景与决策

最初计划在自有 Linux VPS 上部署自己和少数熟人的学术主页（子路径方案），
因备案/运维成本放弃自托管，改为 GitHub Pages。第一版用 AcadHomepage 模板，
随后为更高的设计品质迁移至 [al-folio](https://github.com/alshedivat/al-folio)
（Jekyll，GitHub Actions 构建发布到 gh-pages 分支，零服务器运维）。

方案对比（当时）：

- A. 网页 fork（官方路径）——最傻瓜，适合熟人自助。
- B. **API 建仓 + 干净首提交（采用）**——无需网页操作，全程脚本化。
- C. "Use this template" 按钮——与 A 无实质差异。

范围：只做本人站点；熟人部分后续需要时再出指南。

## 架构（al-folio 版）

- 托管：GitHub Pages（user site），**Pages 源 = gh-pages 分支**（al-folio 的
  deploy.yml 用 JamesIves/github-pages-deploy-action 发布 `_site` 到 gh-pages）。
- 构建：push main → Actions（Ruby 3.3.5 + Python + Node + imagemagick + purgecss）
  → gh-pages。模板自带的大量无关 workflow（lighthouse/prettier/codeql 等）已删除，
  仅保留 deploy.yml。
- 页面：about（简介 + announcements + socials）、cv（_data/cv.yml 结构化渲染）、
  news（announcement 聚合）、404。blog/publications/projects/repositories/
  teaching/profiles/dropdown/books/plugins 等空板块页面已删除。
- Scholar 爬虫：未启用（al-folio 的 update-citations workflow 已一并删除；
  需要时从模板仓库取回并配置）。

## 已实施

1. API 建仓、首版 AcadHomepage 上线、内容中文化（详见 git 历史）。
2. 迁移 al-folio：模板内容整体替换（保留 .git 历史），_config.yml
   （url/baseurl 置空/姓名/简介/lang=cn/keywords）、_data/socials.yml
   （邮箱 + GitHub）、_pages/about.md 中文内容、_news/ 5 条动态、
   _data/cv.yml（教育/奖项/CVE）。
3. 头像仍为模板默认 `assets/img/prof_pic.jpg`，待本人提供照片替换。

## 内容口径备注

- 专业：信息安全（本人 2026-07-26 更正；此前按 GitHub bio 误写为软件工程）。
- 荣誉与奖项按类型合并展示（蓝桥杯 / 安全竞赛 / 数学建模 / 软件设计 / 英语）。
- 蓝桥杯：十六届=2025（Java 省二）、十七届=2026（Python 省一 + 国赛三等）。
- 教育经历起始 2024.09 系从学号邮箱推断，待本人确认。
- _news 日期为月份级近似值（用于排序），可按实际证书日期修订。
- "软件杯""外研杯"按本人提供的名称直录。

## 遗留（内容侧，非阻塞）

- 头像/favicon 仍为模板默认，待照片。
- 真名未提供，站点显示名为 zemu。
- Google Analytics / SEO 验证未配；Scholar 引用集成未开。
