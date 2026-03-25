# 数据字典

本文档定义了所有数据文件的字段规范。

---

## 1. 学校档案 (`data/schools/`)

每所学校一个 JSON 文件，命名规则：`{district}_{school_short_name}.json`

例：`haidian_rdfz.json`（海淀-人大附中）

### 字段定义

| 字段 | 类型 | 必填 | 说明 |
|------|------|------|------|
| `school_id` | string | ✅ | 唯一标识，格式 `{区拼音}_{校简称拼音}` |
| `name` | string | ✅ | 学校全称 |
| `short_names` | string[] | ✅ | 常用简称/别名 |
| `district` | enum | ✅ | 所属区：`haidian`, `xicheng`, `dongcheng`, `chaoyang` |
| `school_type` | enum | ✅ | `public`（公办）, `private`（民办）, `hybrid`（公办民助） |
| `tier` | enum | | 家长圈层级：`T1`, `T2`, `T3`, `T4`（非官方） |
| `tags` | string[] | | 标签，如 `六小强`, `早培校`, `集团校` |
| `address` | string | | 学校地址 |
| `website` | string | | 官网地址 |
| `enrollment_data` | object[] | | 按年份的招生数据（见下） |
| `exit_data` | object[] | | 升学出口数据（见下） |
| `notes` | string | | 补充说明 |
| `last_updated` | date | ✅ | 最后更新日期 |

### enrollment_data 子结构

| 字段 | 类型 | 说明 |
|------|------|------|
| `year` | int | 招生年份 |
| `total_enrollment` | int | 招生总人数 |
| `class_count` | int | 开班数 |
| `class_types` | object[] | `[{name, size, description}]` |
| `source_confidence` | enum | `official`, `verified`, `reported`, `estimated` |
| `source_notes` | string | 数据来源说明 |

### exit_data 子结构

| 字段 | 类型 | 说明 |
|------|------|------|
| `graduation_year` | int | 毕业年份 |
| `zhongkao_avg` | float | 中考平均分 |
| `top_school_rate` | float | 市重点高中录取比例 (0-1) |
| `direct_admission_rate` | float | 直升/校额到校比例 (0-1) |
| `source_confidence` | enum | 数据可信度 |

---

## 2. 入学渠道 (`data/channels/`)

按学校和年份组织，命名规则：`{school_id}_{year}.json`

例：`haidian_rdfz_2025.json`

### 字段定义

| 字段 | 类型 | 必填 | 说明 |
|------|------|------|------|
| `school_id` | string | ✅ | 关联学校 |
| `year` | int | ✅ | 适用年份 |
| `channels` | object[] | ✅ | 该校当年所有渠道（见下） |
| `last_updated` | date | ✅ | 最后更新日期 |

### channels 子结构

| 字段 | 类型 | 必填 | 说明 |
|------|------|------|------|
| `channel_type` | enum | ✅ | `lottery`（派位）, `selection`（点招）, `early_talent`（早培）, `specialty`（特长）, `direct_feed`（直升）, `policy_reserve`（政保）, `other` |
| `transparency` | enum | ✅ | `open`（公开）, `semi_open`（半公开）, `hidden`（非公开） |
| `est_quota` | int | | 估算名额 |
| `selection_method` | string | | 筛选方式描述 |
| `timeline` | object | | `{registration_start, exam_date, notification_date}` |
| `requirements` | string | | 报名条件/门槛 |
| `source_count` | int | ✅ | 独立信息来源数量 |
| `source_confidence` | enum | ✅ | 数据可信度 |
| `notes` | string | | 补充说明 |

---

## 3. 考试记录 (`data/exams/`)

命名规则：`{school_id}_{year}_{exam_sequence}.json`

例：`haidian_rdfz_2025_01.json`（人大附2025年第1场考试）

### 字段定义

| 字段 | 类型 | 必填 | 说明 |
|------|------|------|------|
| `exam_id` | string | ✅ | 唯一标识 |
| `school_id` | string | ✅ | 关联学校 |
| `channel_type` | enum | ✅ | 关联渠道类型 |
| `year` | int | ✅ | 年份 |
| `exam_date` | date | | 考试日期 |
| `format` | enum | ✅ | `written`（笔试）, `computer`（机考）, `interview`（面谈）, `activity`（综合活动）, `mixed`（混合） |
| `subjects` | string[] | ✅ | 考察科目/维度 |
| `difficulty` | enum | | `curriculum`（课内）, `extended`（拓展）, `competition_intro`（竞赛入门）, `competition_mid`（竞赛中等）, `competition_hard`（竞赛高难） |
| `duration_min` | int | | 考试时长（分钟） |
| `participant_est` | int | | 估计参加人数 |
| `pass_rate_est` | float | | 估计通过率 (0-1) |
| `question_recalls` | object[] | | 题目回忆（见下） |
| `prep_recommendations` | object[] | | 备考建议（见下） |
| `source_confidence` | enum | ✅ | 数据可信度 |
| `last_updated` | date | ✅ | 最后更新日期 |

### question_recalls 子结构

| 字段 | 类型 | 说明 |
|------|------|------|
| `subject` | string | 所属科目 |
| `topic_tags` | string[] | 知识点标签 |
| `summary` | string | 题目概述（非原题） |
| `difficulty_rating` | int(1-5) | 难度评分 |
| `similar_resources` | string[] | 相似题目出处 |

### prep_recommendations 子结构

| 字段 | 类型 | 说明 |
|------|------|------|
| `stage` | string | 备考阶段 |
| `period` | string | 适用时期 |
| `focus` | string | 重点内容 |
| `materials` | object[] | `[{name, type, level}]` |

---

## 4. 时间线事件 (`data/timeline/`)

按区和年份组织，命名规则：`{district}_{year}.json`

例：`haidian_2025.json`

### 字段定义（每个文件包含 events 数组）

| 字段 | 类型 | 必填 | 说明 |
|------|------|------|------|
| `event_name` | string | ✅ | 事件名称 |
| `event_type` | enum | ✅ | `policy`（政策）, `exam`（考试）, `notification`（MD/通知）, `result`（结果公示）, `prep`（备考节点） |
| `school_id` | string | | 关联学校（全区事件可空） |
| `district` | enum | ✅ | 适用区域 |
| `date_expected_start` | date | ✅ | 预计开始日期 |
| `date_expected_end` | date | | 预计结束日期 |
| `date_actual` | date | | 实际日期（事后更新） |
| `action_required` | string | | 家长需要做什么 |
| `historical_dates` | object[] | | `[{year, date}]` 历年同一事件日期 |
| `source_confidence` | enum | ✅ | 数据可信度 |
| `notes` | string | | 补充说明 |

---

## 通用规则

### 数据可信度 (source_confidence)

| 值 | 含义 | 要求 |
|------|------|------|
| `official` | 官方公示 | 来自学校官网、教委文件 |
| `verified` | 多源验证 | ≥3 个独立来源交叉确认 |
| `reported` | 单源待验 | 1-2 个来源，尚需更多验证 |
| `estimated` | 推算 | 基于历年数据或间接信息推断 |

### 文件命名

- 全部使用小写字母和下划线
- 学校 ID 格式：`{区拼音}_{校简称拼音}`
- 日期格式：`YYYY-MM-DD`(ISO 8601)

### 贡献指南

提交数据时请确保：
1. 所有必填字段已填写
2. `source_confidence` 已正确标注
3. 不包含任何个人身份信息
4. `last_updated` 已更新为提交日期
