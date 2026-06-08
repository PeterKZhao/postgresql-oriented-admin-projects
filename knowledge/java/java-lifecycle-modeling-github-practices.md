# Java 生命周期开发、业务设计与数学建模 GitHub 实践指南

本文整理适合学习的 GitHub 项目，用于指导后续基于 `requirements.txt` 的智慧景区/智慧文旅平台开发。重点关注三类能力：

- Java 软件开发生命周期能力
- 基于业务设计的开发能力
- 数学建模、优化建模和模型验证能力

## 总体学习路线

推荐学习顺序：

1. 先学习 DDD 和业务建模。
2. 再学习 Hexagonal/Clean Architecture，把业务逻辑和技术细节隔离。
3. 然后学习模块化单体，保证大型系统按业务模块演进。
4. 再学习架构测试和属性测试，防止代码结构和业务规则被破坏。
5. 最后学习数学建模、优化建模、单位建模和金额建模。

对我们的项目来说，`codegen-bot` 适合生成 CRUD 和基础管理页面，但核心业务流程必须按业务设计和模型设计手工实现。

## 一、业务设计与 DDD 实践

### 1. ddd-by-examples/library

GitHub：

https://github.com/ddd-by-examples/library

推荐程度：最高

适合学习：

- Domain-Driven Design
- Event Storming
- User Story Mapping
- 聚合设计
- 领域事件
- CQRS
- 模块边界
- 架构测试
- 从业务需求到代码实现的完整过程

对我们的价值：

- 帮助我们把智慧景区需求拆成业务模块。
- 帮助识别聚合，例如门票、订单、年卡、核销、景区、活动、会员。
- 帮助避免只按数据库 CRUD 开发，而忽略业务规则。

在项目中的使用方式：

- 用它的方法拆分 `requirements.txt`。
- 每个模块先写业务规则，再写表结构。
- 对门票、核销、退款、会员、积分等高风险模块，先画业务流程和聚合边界。

### 2. citerus/dddsample-core

GitHub：

https://github.com/citerus/dddsample-core

适合学习：

- 经典 DDD Java 示例
- Entity、Value Object、Repository、Domain Service 的使用方式
- 业务流程如何落到代码

对我们的价值：

- 适合作为 DDD 基础示例。
- 可以学习领域对象如何表达业务含义，而不是只写贫血模型。

## 二、Java 分层架构与生命周期开发

### 1. thombergs/buckpal

GitHub：

https://github.com/thombergs/buckpal

推荐程度：很高

适合学习：

- Hexagonal Architecture
- Clean Architecture
- Use Case 分层
- Domain 和 Adapter 分离
- Controller、Service、Repository 边界
- 测试友好的 Java 结构

对我们的价值：

- 后续使用 ruoyi-vue-pro 时，不能把复杂业务逻辑写在 Controller 或 Mapper 中。
- 可以参考它的结构，把核心业务逻辑放在 Application Service / Domain Service。

推荐落地方式：

- Controller 只处理 HTTP 入参和出参。
- Application Service 负责事务和业务编排。
- Domain Model 负责核心规则。
- Mapper/Repository 只负责数据访问。

### 2. spring-projects/spring-modulith

GitHub：

https://github.com/spring-projects/spring-modulith

推荐程度：很高

适合学习：

- Spring Boot 模块化单体
- 业务模块边界
- 模块依赖验证
- 模块文档生成
- 领域事件

对我们的价值：

智慧文旅平台会有很多模块，例如：

- `tourism`：景区、场馆、资讯、攻略、活动
- `map`：手绘地图、POI、图层、路线
- `ticket`：门票、年卡、核销、退款
- `member`：会员、积分、权益
- `ugc`：游记、打卡、评论、审核
- `parking`：停车、车位、停车券
- `staff`：工作人员端、扫码核销、统计

Spring Modulith 的思想可以帮助我们保持模块边界清晰。

### 3. spring-projects/spring-petclinic

GitHub：

https://github.com/spring-projects/spring-petclinic

适合学习：

- Spring Boot 应用生命周期
- Maven 构建
- 数据库 profile
- 测试
- Testcontainers
- Docker Compose
- 基础 CI 思路

对我们的价值：

- 适合作为 Spring Boot 工程化基础参考。
- 不适合直接参考复杂业务建模，但适合学习项目启动、测试、运行和部署。

## 三、架构测试与规则验证

### 1. TNG/ArchUnit-Examples

GitHub：

https://github.com/TNG/ArchUnit-Examples

适合学习：

- 架构规则测试
- 禁止错误依赖
- 分层边界测试
- 模块边界测试

对我们的价值：

可以用 ArchUnit 写规则，例如：

- `domain` 不能依赖 `controller`
- `ticket` 模块不能直接调用 `map` 模块内部实现
- Controller 不能直接调用 Mapper
- 业务模块不能互相循环依赖
- 核心业务不能依赖前端 VO

推荐做法：

在每个重要模块加入架构测试，防止后续开发时系统变成无边界的大泥球。

### 2. jlink/how-to-specify-it

GitHub：

https://github.com/jlink/how-to-specify-it

适合学习：

- Property-based Testing
- jqwik
- 用属性描述业务不变量
- 用随机数据验证数学模型

对我们的价值：

适合测试业务规则和数学规则，例如：

- 退款金额不能大于原支付金额。
- 核销次数不能超过可用次数。
- 年卡有效期不能为负。
- 积分余额不能小于 0。
- 优惠券折扣后金额不能小于 0。
- 路线容量不能超过景区容量。

## 四、数学建模与优化建模

### 1. optimatika/ojAlgo

GitHub：

https://github.com/optimatika/ojAlgo

推荐程度：很高

适合学习：

- 线性代数
- 统计
- 线性规划 LP
- 二次规划 QP
- 混合整数规划 MIP
- 优化模型
- 矩阵计算

对我们的价值：

适合处理：

- 景区容量优化
- 导游排班
- 票务库存分配
- 停车资源分配
- 活动场次安排
- 多目标资源调度

推荐使用方式：

- 先把业务问题建模成变量、约束、目标函数。
- 再用 ojAlgo 求解。
- 不要直接在 Controller 中写数学计算。
- 数学模型应独立成服务，并保留输入、输出和模型版本。

### 2. google/or-tools

GitHub：

https://github.com/google/or-tools

推荐程度：很高

适合学习：

- 组合优化
- 约束规划
- 车辆路径 VRP
- 旅行商 TSP
- 排班
- 最短路
- 最大流
- 线性求解器

对我们的价值：

适合处理：

- 游客推荐线路
- 导游路线分配
- 车辆/摆渡车调度
- 停车资源调度
- 活动人员排班
- 多景区行程规划

注意：

OR-Tools 功能强大，但建模复杂。建议只在确实需要优化求解时使用，不要把简单 CRUD 问题复杂化。

### 3. jgrapht/jgrapht

GitHub：

https://github.com/jgrapht/jgrapht

适合学习：

- 图结构
- 路径搜索
- 最短路径
- 网络流
- 连通性
- 图算法

对我们的价值：

适合处理：

- 手绘地图路线
- 景点之间的路径
- 最近厕所/停车场/救援点搜索
- 景区交通网络
- 游览线路推荐

注意：

新版本可能需要较高 JDK 版本。ruoyi-vue-pro 当前以 JDK 17 为主时，需要选择兼容 JDK 17 的版本。

## 五、单位、金额与精度建模

### 1. unitsofmeasurement/unit-api

GitHub：

https://github.com/unitsofmeasurement/unit-api

适合学习：

- JSR-385 单位建模
- 长度、时间、速度、面积、容量等单位表达
- 防止单位混用

对我们的价值：

适合处理：

- 距离
- 游览时长
- 路线长度
- 场馆容量
- 停车时长
- 交通时间

推荐做法：

涉及单位的模型不要只用裸 `double` 或 `BigDecimal`，应明确字段单位。例如米、公里、分钟、小时必须清楚区分。

### 2. JavaMoney/jsr354-ri

GitHub：

https://github.com/JavaMoney/jsr354-ri

适合学习：

- 金额建模
- 货币建模
- 金额计算
- 舍入规则

对我们的价值：

适合处理：

- 门票价格
- 退款
- 优惠券
- 分销佣金
- 会员余额
- 积分抵扣
- 多渠道订单金额

推荐做法：

- 金额计算不要使用 `double`。
- Java 中普通业务金额可以使用 `BigDecimal`。
- 更复杂的货币场景可以参考 JavaMoney 的建模方式。

## 六、落地到 ruoyi-vue-pro 的建议

### 1. 代码生成只负责低风险重复工作

适合 codegen 的内容：

- 景区基础资料
- 场馆资料
- 景点资料
- POI 点位
- 活动资料
- 攻略资讯
- UGC 审核后台
- 导游资料
- 供应商资料
- 核销设备资料

不建议只靠 codegen 的内容：

- 支付
- 退款
- 年卡
- 核销
- 库存
- 分销
- 会员积分
- 路线推荐
- 停车调度
- OTA 对接

这些需要手工业务设计和测试。

### 2. 推荐模块结构

建议拆成：

- `tourism`
- `map`
- `ticket`
- `ugc`
- `guide`
- `parking`
- `commerce`
- `member`
- `staff`

每个模块应有：

- SQL 表结构
- 业务规则文档
- 生成的 CRUD
- 手工业务 Service
- 单元测试
- 集成测试
- 架构测试

### 3. 每个复杂业务都要写模型说明

例如门票核销模型应说明：

- 输入：订单、票种、核销码、核销设备、核销人、时间
- 输出：核销结果、剩余次数、核销记录
- 规则：是否过期、是否已退款、是否超过次数、是否允许该设备核销
- 异常：重复核销、无效码、过期票、退款票
- 测试：正常核销、重复核销、过期核销、退款后核销、跨租户核销

### 4. 数学模型要版本化

例如路线推荐、容量优化、排班模型，应记录：

- 模型版本
- 输入变量
- 输出结果
- 约束条件
- 目标函数
- 算法说明
- 精度和舍入规则
- 测试样例

## 七、推荐学习计划

第一阶段：业务建模

- 学 `ddd-by-examples/library`
- 学 `citerus/dddsample-core`
- 输出我们项目的领域词汇表和模块划分

第二阶段：代码架构

- 学 `thombergs/buckpal`
- 学 `spring-projects/spring-modulith`
- 确定 ruoyi-vue-pro 的模块边界和 Service 分层

第三阶段：测试与约束

- 学 `TNG/ArchUnit-Examples`
- 学 `jlink/how-to-specify-it`
- 给 ticket、member、ugc、map 等模块写架构规则和业务不变量测试

第四阶段：数学建模

- 学 `ojAlgo`
- 学 `OR-Tools`
- 学 `JGraphT`
- 先从路线推荐、停车调度、导游排班中选一个小模型试点

第五阶段：正式开发

- 用 `codegen-bot` 生成基础 CRUD
- 手工实现核心业务逻辑
- 用测试验证规则
- 用 ArchUnit 保持架构边界

## 总结

这些 GitHub 项目不是直接拿来替换 ruoyi-vue-pro 的，而是用于提升我们的开发方法。

最重要的原则：

- CRUD 可以生成。
- 业务规则不能随便生成。
- 数学模型必须显式建模、测试和版本化。
- 复杂业务必须先设计，再编码。
- 架构边界必须用测试保护。

后续开发智慧文旅平台时，应把这些实践和 `codegen-bot` 结合：先用业务设计拆模块，再用代码生成节省重复工作，最后用 DDD、数学建模和测试补齐核心业务能力。
