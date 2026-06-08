# 项目技能体系结构

本文用于规划本项目后续 Codex skills 的结构。目标是让需求分析、数学建模、ruoyi-vue-pro 表设计、codegen-bot 生成和业务实现形成稳定流程。

## 一、设计原则

1. 技能只保存高频、易错、需要一致执行的流程。
2. 不把所有需求说明都塞进 `SKILL.md`，详细资料放到 `references/`。
3. 每个技能只解决一个清晰问题，避免一个技能包办整个项目。
4. 先复用已有技能，再创建新技能。
5. 需求先形成模型卡，再进入 SQL 和 Java 实现。
6. 复杂业务必须先写不变量和测试样例，再生成代码。

## 二、已有基础技能

### 1. `java-math-modeling`

用途：

- 门票、核销、退款、年卡、座位、积分、库存、停车、路线、租赁费用等数学/业务模型。
- 输出变量、单位、状态、不变量、异常场景、Java 服务建议和测试样例。

定位：

- 所有复杂业务的核心建模技能。
- 后续领域技能都应该调用或引用它。

### 2. `ruoyi-mysql-style`

用途：

- 设计和评审 ruoyi-vue-pro/codegen-bot 兼容的 MySQL 表。
- 检查表前缀、审计字段、租户字段、索引、中文注释、软删除、唯一约束。

定位：

- 所有新增 SQL 表的基础技能。
- 和 `java-math-modeling` 配合使用：先建模，再设计表。

## 三、建议新增的项目技能

### 1. `travel-requirement-analysis`

用途：

- 逐项读取 `requirements.txt`。
- 判断每个需求是 CRUD、业务状态模型、数学模型、复用 ruoyi 表，还是需要新增扩展表。
- 生成统一模型卡。

触发场景：

- “分析 requirements.txt”
- “逐项讨论需求”
- “把某个需求保存成模型卡”
- “判断这个需求是否需要数学建模”

建议结构：

```text
travel-requirement-analysis/
├── SKILL.md
├── agents/openai.yaml
└── references/
    ├── model-card-template.md
    ├── requirement-classification.md
    └── ruoyi-reuse-first-rules.md
```

核心引用资料：

- `skills/project-planning/model-card-template.md`
- 项目本地需求拆分文档，例如 `docs/requirements/skill-split.md`
- 项目本地已有表覆盖矩阵，例如 `docs/requirements/existing-table-coverage.md`

优先级：最高。它会成为后续逐项需求讨论的入口技能。

### 2. `travel-ticketing-modeling`

用途：

- 门票、年卡、核销、退票、座位、批量下单、分销、供应商、核验设备。
- 固定执行“mall/trade/pay/member 复用优先”的票务模型。

触发场景：

- “门票模型”
- “核销模型”
- “退单/退款”
- “年卡”
- “座位预订”
- “批量下单”

建议结构：

```text
travel-ticketing-modeling/
├── SKILL.md
├── agents/openai.yaml
└── references/
    ├── ticket-mall-reuse-design.md
    ├── ticket-entitlement-invariants.md
    ├── refund-state-machine.md
    └── seat-locking-model.md
```

核心引用资料：

- `knowledge/ruoyi-vue-pro/ticket-mall-reuse-design.md`
- 项目本地已有表覆盖矩阵，例如 `docs/requirements/existing-table-coverage.md`

优先级：高。票务是本项目最复杂、最容易出错的业务。

### 3. `travel-content-i18n-modeling`

用途：

- 景区官网、小程序内容、信息发布、多语言、栏目、广告、排行、UGC 展示。
- 处理内容发布、翻译、审核、回退、SEO、缓存。

触发场景：

- “多语言化”
- “信息发布”
- “官网内容”
- “广告栏”
- “热门排行”
- “UGC 展示”

建议结构：

```text
travel-content-i18n-modeling/
├── SKILL.md
├── agents/openai.yaml
└── references/
    ├── multilingual-model-card.md
    ├── content-publish-state.md
    └── ranking-rules.md
```

核心引用资料：

- `knowledge/model-cards/site-multilingual.md`

优先级：中高。它适合先支持低风险内容类模块。

### 4. `travel-map-route-modeling`

用途：

- 手绘地图、图层、POI、路线、LBS、最近点、救援导航、周边服务。
- 处理距离、路线、可达性、地图比例尺、定位和搜索。

触发场景：

- “手绘地图”
- “POI”
- “图层搜索”
- “旅游线路”
- “救援导航”
- “附近厕所/医院/停车场”

建议结构：

```text
travel-map-route-modeling/
├── SKILL.md
├── agents/openai.yaml
└── references/
    ├── poi-model.md
    ├── route-graph-model.md
    ├── nearest-point-rules.md
    └── map-layer-rules.md
```

优先级：中高。地图涉及路线和距离，需要单独建模。

### 5. `travel-member-marketing-modeling`

用途：

- 会员、积分、等级、权益、优惠券、全员营销、达人佣金。
- 固定复用 `member_`、`promotion_`、`trade_`、`pay_`。

触发场景：

- “会员中心”
- “积分”
- “等级”
- “权益”
- “优惠券”
- “全员营销”
- “达人佣金”

建议结构：

```text
travel-member-marketing-modeling/
├── SKILL.md
├── agents/openai.yaml
└── references/
    ├── member-reuse-rules.md
    ├── points-invariants.md
    ├── coupon-rules.md
    └── commission-model.md
```

优先级：中。会员和营销复杂，但可以在票务后推进。

### 6. `travel-ops-commercial-modeling`

用途：

- 零售、餐饮、POS、库存、停车、招商租赁、合同费用。
- 固定复用 `erp_`、`product_`、`trade_`、`pay_`、`crm_`。

触发场景：

- “零售系统”
- “餐饮系统”
- “收银系统”
- “停车计费”
- “招商租赁”
- “租金/滞纳金”

建议结构：

```text
travel-ops-commercial-modeling/
├── SKILL.md
├── agents/openai.yaml
└── references/
    ├── pos-shift-model.md
    ├── inventory-reuse-rules.md
    ├── parking-fee-model.md
    └── lease-billing-model.md
```

优先级：中低。范围很大，建议后期根据实际开发再拆成 POS、停车、租赁三个技能。

### 7. `ruoyi-codegen-bot-workflow`

用途：

- 使用 `codegen-bot` 针对 ruoyi-vue-pro 生成 CRUD。
- 支持多模块 `CODEGEN_MODULES`。
- 处理 SQL 输入、生成目录、后端/前端同步和检查。

触发场景：

- “用 codegen-bot 生成”
- “多模块生成”
- “根据 SQL 生成 ruoyi 模块”
- “检查 codegen 输出”

建议结构：

```text
ruoyi-codegen-bot-workflow/
├── SKILL.md
├── agents/openai.yaml
└── references/
    ├── codegen-bot-usage.md
    ├── multi-module-codegen.md
    └── generated-code-review.md
```

核心引用资料：

- `knowledge/ruoyi-vue-pro/github-projects-guide.md`

优先级：中高。等第一批 SQL 草案确定后再创建最合适。

## 四、技能调用顺序

### 需求逐项讨论

```text
travel-requirement-analysis
-> java-math-modeling
-> ruoyi-mysql-style
```

### 门票/年卡/核销

```text
travel-ticketing-modeling
-> java-math-modeling
-> ruoyi-mysql-style
-> ruoyi-codegen-bot-workflow
```

### 内容/多语言/官网

```text
travel-content-i18n-modeling
-> ruoyi-mysql-style
-> ruoyi-codegen-bot-workflow
```

### 地图/路线/LBS

```text
travel-map-route-modeling
-> java-math-modeling
-> ruoyi-mysql-style
```

### 会员/营销

```text
travel-member-marketing-modeling
-> java-math-modeling
-> ruoyi-mysql-style
```

### POS/停车/租赁

```text
travel-ops-commercial-modeling
-> java-math-modeling
-> ruoyi-mysql-style
```

## 五、推荐创建顺序

1. `travel-requirement-analysis`
2. `travel-ticketing-modeling`
3. `ruoyi-codegen-bot-workflow`
4. `travel-content-i18n-modeling`
5. `travel-map-route-modeling`
6. `travel-member-marketing-modeling`
7. `travel-ops-commercial-modeling`

第一阶段不要一次性创建太多技能。先把第 1 个和第 2 个做扎实，后续遇到重复任务再创建领域技能。

## 六、项目文档与技能引用关系

| 项目文档 | 建议归属技能 |
|---|---|
| `skills/project-planning/model-card-template.md` | `travel-requirement-analysis` |
| 项目本地 `docs/requirements/skill-split.md` | `travel-requirement-analysis` |
| 项目本地 `docs/requirements/existing-table-coverage.md` | `travel-requirement-analysis`、`ruoyi-mysql-style` |
| `knowledge/ruoyi-vue-pro/ticket-mall-reuse-design.md` | `travel-ticketing-modeling` |
| `knowledge/model-cards/site-multilingual.md` | `travel-content-i18n-modeling` |
| `knowledge/ruoyi-vue-pro/github-projects-guide.md` | `ruoyi-codegen-bot-workflow` |

## 七、下一步落地建议

下一步先创建 `travel-requirement-analysis` 技能，把模型卡模板、需求分类规则和 ruoyi 复用优先规则放进去。这样后续讨论 `requirements.txt` 的每一个细项时，都能统一输出结构。
