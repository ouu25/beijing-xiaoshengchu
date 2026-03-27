# 项目：北京小升初升学索引

## 网站
- 域名：xiaoshengchu.org
- 部署：Cloudflare Pages（自动从 GitHub 部署）
- 仓库：github.com/ouu25/beijing-xiaoshengchu
- 本地路径：~/Documents/Project/beijing-xiaoshengchu

## 技术栈
- 纯静态 HTML，单文件 index.html（约52KB）
- 所有数据内嵌为 JS 常量（SCHOOLS、EXAMS、TL）
- 字体：fonts.loli.net（国内 CDN 镜像，不用 Google Fonts）
- 无框架、无构建步骤，push 即部署

## 数据结构
- SCHOOLS：36所学校数组，每所包含 school_id、name、short_names、district、tier(T1/T2/T3)、tags、channels、notes、zone、website、honors
- EXAMS：7条考试记录数组，含 question_recalls 回忆题
- TL：时间线对象，按 haidian/xicheng 分区，每区按月排列事件

## 内容覆盖
- 36所学校（海淀23 + 西城13），每所有：层级、学区/区域、入学渠道(≥2条)、成绩数据、官网链接、金帆金鹏
- 7条考试记录（早培、2+4、八少八素、分班考等）
- 2026时间线（海淀 + 西城）
- 备考指南13个模块：一派二派、西城入学、KET/PET/FCE、数学竞赛、市三好/区三好、金帆金鹏、五六年级节奏、简历制作、面试问题、六小强DZ渠道、志愿填报策略、上岸案例、1+3项目

## 基础设施
- Cloudflare：域名注册(xiaoshengchu.org) + DNS + Pages + SSL（Full模式）
- 搜索引擎：Google Search Console ✓、Bing Webmasters ✓、百度跳过（不愿提交个人信息）
- SEO：sitemap.xml、robots.txt、OG标签、emoji favicon（📚）

## 页面结构
- 5个导航标签：学校、时间线、考试、备考指南、关于
- 学校列表：桌面端表格（层级/学校/学区/关键数据/官网），手机端卡片
- 学校详情：点击行弹出 overlay，ESC/点蒙层关闭，背景滚动锁定
- 备考指南：手风琴折叠，默认展开第一个
- 搜索：支持学校名、T1/T2、学区、渠道等关键词，Ctrl+K 聚焦
- 学区筛选：下拉框过滤区域一至五、具体学区名

## 设计原则
- 信息密度优先，工具感，不要花哨动画
- 暖米色背景(#fafaf9)，Noto Sans SC 字体
- 不写公众号来源署名（版权考虑）
- 不写"不接受机构赞助"（语气不合适）
- 数据标注可信度：official/verified/reported/estimated

## 数据来源（不在页面上标注）
- 教委官网、学校公告
- 公众号文章（小倪聊升学、曾哥点评、老马说升学、莲妈读书，约9000篇）
- 2024初中入学白皮书 PDF
- 2024海淀区大派位 PDF
- 网络搜索补充

## 工作流
- 修改 index.html 后 git push，Cloudflare Pages 自动部署（1-2分钟生效）
- 数据量大的批量更新用 Python 脚本替换，避免逐条 str_replace
- 中文引号「」代替""（JS字符串内）
