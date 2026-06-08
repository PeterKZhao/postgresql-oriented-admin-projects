# ruoyi-vue-pro 辅助仓库使用指南

本文基于 2026-06-06 对以下三个 GitHub 仓库的检查整理，用于后续启动 `requirements.txt` 中智慧景区/智慧文旅平台开发时作为执行指南。

- https://github.com/FutureTechQuant/codegen-bot
- https://github.com/FutureTechQuant/Clone-ruoyi-vue-pro-Bot
- https://github.com/FutureTechQuant/Clone-ruoyi-ui-admin-vue3-Bot

## 结论

这三个仓库可以帮助我们节省时间，但职责不同：

- `codegen-bot` 是最有价值的开发加速工具，适合把业务表 SQL 转换成 ruoyi-vue-pro 的后端模块、管理端页面和基础 CRUD。
- `Clone-ruoyi-vue-pro-Bot` 适合一次性初始化后端基座仓库，处理品牌替换、目录重构、模块拆分和 CI。
- `Clone-ruoyi-ui-admin-vue3-Bot` 适合一次性初始化管理端前端仓库，并配置 GitHub Pages、Cloudflare 域名和构建流程。

它们不能直接完成完整业务系统。门票核销、年卡规则、订单退款、地图导览、LBS、OTA 对接、支付、停车、慢直播、智能客服、营销分销等业务逻辑仍需要手工开发。

## 仓库功能说明

### 1. codegen-bot

用途：基于 ruoyi-vue-pro 的代码生成能力，把 `sql/schema/*.sql` 中的业务表自动生成后端和前端基础代码。

主要能力：

- 创建临时 MySQL 数据库。
- 导入 ruoyi-vue-pro 基础 SQL。
- 导入本仓库 `sql/schema` 下的业务 SQL。
- 自动识别新增业务表。
- 推断模块名和表前缀。
- 支持通过 `CODEGEN_MODULES` 一次生成多个模块。
- 调用 ruoyi-vue-pro 的代码生成服务。
- 生成后端模块代码、Mapper、Service、Controller、VO、SQL、前端页面等。
- 把 admin controller 复制生成 app controller。
- 把生成代码同步到后端和前端目标仓库。
- 附带 GitHub Actions 工作流，可在 CI 中运行代码生成。

适合生成的模块：

- 景区资源管理：景区、场馆、景点、POI、服务点、图层、手绘地图点位。
- 文旅内容管理：资讯、攻略、活动、文化、美食、乡村游、周边服务。
- UGC 管理：游记、打卡、评论、评价、内容审核。
- 票务基础数据：票种、年卡、核销设备、核销记录、供应商、分销商。
- 导游导服：导游资料、预约、评价、核销记录。
- 会员和积分扩展表。
- 停车、救援、交通、公共服务点位等基础数据。

局限：

- 只能生成基础 CRUD，不能自动生成复杂业务流程。
- 不能自动完成支付、退款、库存扣减、核销、防重复提交、分布式事务等关键逻辑。
- 不能自动判断 app 接口的安全边界。
- 不能自动完成前端复杂交互、地图组件、路线规划、直播播放等页面。

需要修正的风险：

- `publish.sh` 会删除并重建 GitHub 仓库，正式使用前必须移除默认删除逻辑或增加强确认。
- `clone_admin_controller_to_app.py` 会去掉 `@PreAuthorize`，并保留 create/update/delete/get/page 接口，直接用于游客端有安全风险。
- 当前 `CODEGEN_BASE_PACKAGE` 没有完全贯穿到配置和模板中，如果后端基座已经从 `cn.iocoder.yudao` 改成 `cn.iocoder.future`，需要先修正包名生成逻辑。
- 示例 SQL 使用了 `createtime`、`updatetime`、`tenantid`，建议改为 ruoyi-vue-pro 常用字段 `create_time`、`update_time`、`tenant_id`。
- `publish_sync.py` 按官方 yudao 目录结构同步，如果后端已经被重构为 `future-*` 和 `modules/...` 目录结构，需要改造同步路径。

### 2. Clone-ruoyi-vue-pro-Bot

用途：自动创建一个新的后端基座仓库。

主要能力：

- 通过 GitHub Actions 创建新仓库。
- 克隆 ruoyi-vue-pro 后端代码。
- 删除不需要的原始仓库文件。
- 将 `yudao`、`ruoyi` 等品牌名替换为 `future`。
- 重构 Maven 模块目录，例如拆成 `platform`、`apps`、`modules/core`、`modules/biz`、`modules/extend`。
- 将模块拆成 `api` 和 `biz` 结构。
- 启用常用业务模块依赖，例如 member、pay、mall、crm、erp、report、bpm 等。
- 修改 `application-local.yaml`，接入环境变量形式的数据库和 Redis 配置。
- 添加 Maven CI 和部署工作流。

适用场景：

- 我们决定长期维护一个自己的 ruoyi-vue-pro 后端派生版本。
- 需要统一品牌、包名、模块目录和 CI。
- 不追求频繁无痛同步 ruoyi-vue-pro 上游更新。

风险：

- 会让项目结构明显偏离 ruoyi-vue-pro 原始结构，后续合并上游更新会更难。
- 存在硬编码组织名、仓库名、分支名和配置。
- 工作流会删除已有同名仓库，正式使用前必须关闭或增加确认。
- 如果使用这个重构版后端，`codegen-bot` 的同步逻辑需要改造后才能匹配。

### 3. Clone-ruoyi-ui-admin-vue3-Bot

用途：自动创建管理端 Vue3 前端基座仓库。

主要能力：

- 通过 GitHub Actions 创建新前端仓库。
- 克隆 `yudao-ui-admin-vue3`。
- 删除原始仓库元数据和 README。
- 替换品牌名和 API 地址。
- 添加 GitHub Pages 构建部署 workflow。
- 自动配置 Cloudflare CNAME。
- 自动配置 GitHub Pages 自定义域名和 HTTPS。

适用场景：

- 快速得到一个可部署的管理端前端仓库。
- 需要把管理端部署到 GitHub Pages 或绑定自定义域名。
- 需要配合 `codegen-bot` 生成的前端页面做持续构建。

风险：

- 硬编码了 `backend.fscitech.net`、`admin.fscitech.net` 等域名。
- 默认会删除同名仓库，正式使用前必须确认。
- 只能初始化和部署前端，不能生成业务页面之外的复杂交互。
- 景区地图、导览、票务核销、座位选择等页面仍需要手工实现。

## 推荐开发流程

### 阶段 0：选择基座策略

先确定后端使用哪种结构：

方案 A：保留 ruoyi-vue-pro 官方结构。

- 优点：最容易使用 `codegen-bot`，也更容易跟随上游。
- 缺点：品牌、目录结构、模块边界没有完全按我们的偏好整理。

方案 B：使用 `Clone-ruoyi-vue-pro-Bot` 生成的 `future-*` 重构结构。

- 优点：目录更清晰，品牌统一，适合长期自维护。
- 缺点：需要先改造 `codegen-bot`，让生成代码同步到新的目录结构。

建议：如果目标是快速启动开发，先用方案 A；如果目标是长期产品化平台，再考虑方案 B。

### 阶段 1：按 requirements.txt 拆模块

建议先拆成以下模块：

- `tourism`：景区、场馆、景点、资讯、攻略、活动、文化、美食、乡村游、周边服务。
- `map`：手绘地图、图层、POI、路线、点位分类、定位搜索。
- `ticket`：门票、年卡、票种、座位、核销、退款、供应商、分销商、核验设备。
- `ugc`：游记、打卡、评论、评价、内容审核。
- `guide`：导游资料、导游预约、服务核销、评价。
- `parking`：停车场、车位、停车券、停车缴费记录。
- `commerce`：复用 ruoyi-vue-pro mall/trade/promotion/pay，再扩展景区商城。
- `member`：复用 ruoyi-vue-pro member，再扩展积分、等级、权益、礼品兑换。
- `staff`：工作人员端的核销、统计、消息和工作台。

### 阶段 2：先写 SQL，再生成 CRUD

每个模块先写 SQL，放入 `sql/schema`。

建表建议：

- 表名使用统一前缀，例如 `tourism_scenic`、`ticket_product`、`map_poi`。
- 每次生成尽量只处理一个模块前缀。
- 字段注释必须认真写，因为代码生成会用它们作为页面标签和 VO 描述。
- 基础字段建议使用 ruoyi-vue-pro 风格：
  - `id`
  - `creator`
  - `create_time`
  - `updater`
  - `update_time`
  - `deleted`
  - `tenant_id`
- 多租户表必须保留 `tenant_id`。
- 需要审核、上下架、发布状态的表，应显式设计 `status`、`audit_status`、`publish_status` 等字段。

### 阶段 3：运行代码生成

推荐使用 `codegen-bot` 的流程：

1. 导入 ruoyi-vue-pro 基础 SQL。
2. 导入当前模块 SQL。
3. 运行代码生成。
4. 检查生成的后端模块、前端页面、菜单 SQL、错误码。
5. 只同步确认后的生成代码。
6. 不直接运行带删除仓库逻辑的 `publish.sh`。

多模块生成示例：

```bash
CODEGEN_MODULES="ticket:ticket_,map:map_,ugc:ugc_,tourism:tourism_" \
bash tools/codegen/run.sh
```

单模块生成示例：

```bash
CODEGEN_MODULE_NAME=ticket \
CODEGEN_TABLE_PREFIX=ticket_ \
bash tools/codegen/run.sh
```

生成后必须人工检查：

- Controller 是否暴露了不应该给游客端使用的接口。
- 权限注解是否正确。
- 租户字段是否正确生效。
- 列表查询条件是否合理。
- 字典字段是否需要接入 ruoyi 字典。
- 枚举、错误码、菜单编号是否需要调整。
- 前端页面是否需要补充上传、地图选择、富文本、视频、图片、开关、排序等控件。

### 阶段 4：补业务逻辑

代码生成完成后，需要重点手工开发：

- 门票购买后的权益发放。
- 年卡有效期、实名制、延期、退款。
- 二维码/身份证/PDA 核销。
- 订单退款和库存回滚。
- 分销商、供应商、OTA 接口。
- 景区预约、场馆座位预订。
- 手绘地图点位渲染和图层控制。
- LBS 搜索、附近服务点、救援电话。
- UGC 内容审核流。
- 会员积分、优惠券、权益发放。
- 工作人员端扫码核销和统计。

### 阶段 5：前端和部署

管理端：

- 使用 `Clone-ruoyi-ui-admin-vue3-Bot` 初始化前端仓库。
- 使用 `codegen-bot` 同步生成的管理端页面。
- 对地图、图片、视频、富文本、座位图、核销等复杂页面做手工增强。

后端：

- 使用 Maven CI 验证编译。
- 接入 MySQL、Redis、对象存储、短信、支付、地图服务等环境变量。
- 不在开发早期启用自动删除仓库、自动覆盖生产部署等危险步骤。

## 使用这些仓库前必须做的安全改造

1. 关闭默认删除仓库逻辑。
2. 把组织名、仓库名、分支名、域名、API 地址全部改成参数。
3. 修正 `codegen-bot` 与后端目录结构不一致的问题。
4. 修正 app controller 生成逻辑，禁止默认暴露 create/update/delete。
5. 对生成代码做人工 review 后再提交。
6. 给每个业务模块分配正式错误码号段。
7. 给每个模块建立最小化测试，至少覆盖新增、修改、删除、分页、权限和租户隔离。

## 推荐实际执行顺序

1. 先确定使用官方 ruoyi 结构还是 future 重构结构。
2. 初始化后端和前端基座仓库。
3. 从 `requirements.txt` 中选择第一个低风险模块，例如景区资源、场馆、活动或 POI。
4. 编写该模块 SQL。
5. 使用 `codegen-bot` 生成 CRUD。
6. 人工修正权限、字段、字典、菜单和前端控件。
7. 编译后端、构建前端。
8. 再进入门票、订单、核销、支付等高风险模块。

## 总体判断

这三个仓库适合作为开发加速器，而不是完整解决方案。

最推荐的用法是：用 `codegen-bot` 批量生成低风险基础数据模块，用 clone bot 初始化和部署基座，然后把真正复杂的业务流程交给人工设计和实现。这样既能节省大量重复 CRUD 时间，又能避免把关键业务逻辑交给不可靠的自动生成脚本。
