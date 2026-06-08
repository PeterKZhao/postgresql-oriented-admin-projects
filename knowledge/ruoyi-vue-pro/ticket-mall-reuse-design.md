# 门票订单复用 ruoyi-vue-pro mall 模块设计

本文用于指导后续门票中心开发：门票商品、订单、退单和支付退款优先复用 ruoyi-vue-pro 的 mall/trade/pay/member 模块，`ticket_` 只保存文旅票务专属扩展。

## 一、核心结论
复用：

- `product_spu`：门票/年卡/导服等可售商品 SPU。
- `product_sku`：门票规格、价格、库存。
- `trade_cart`：购物车。
- `trade_order`：交易订单主表。
- `trade_order_item`：交易订单明细。
- `trade_after_sale`：退单/售后申请。
- `trade_after_sale_log`：售后日志。
- `pay_order`：支付单。
- `pay_refund`：支付退款单。
- `member_user`：游客/会员用户。

## 二、领域映射

| 票务概念 | 复用表 | 是否需要 `ticket_` 扩展 | 说明 |
|---|---|---:|---|
| 门票产品 | `product_spu` | 是 | `product_spu` 保存通用商品信息，`ticket_product_ext` 保存景区、票种、实名、有效期规则等。 |
| 门票规格/价格 | `product_sku` | 是 | `product_sku` 保存价格库存，`ticket_sku_rule` 保存核销次数、有效期、适用人群、入园规则。 |
| 购物车 | `trade_cart` | 否 | 直接走商城购物车。 |
| 订单主单 | `trade_order` | 否 | 不新建 `ticket_order`。可通过订单类型或订单项关联识别票务订单。 |
| 订单明细 | `trade_order_item` | 否 | 不新建 `ticket_order_item`。每个订单项支付成功后生成对应票务权益。 |
| 支付 | `pay_order` | 否 | 继续使用 ruoyi 支付模块。 |
| 退单申请 | `trade_after_sale` | 否 | 不新建 `ticket_refund`。票务退款申请走售后表。 |
| 支付退款 | `pay_refund` | 否 | 售后审核通过后发起支付退款。 |
| 票务权益 | `trade_order_item`、`member_user` | 是 | `ticket_entitlement` 保存可核销权益、有效期、剩余次数、实名信息快照。 |
| 核销记录 | `ticket_entitlement` | 是 | `ticket_verify_record` 保存每次核销，防止重复核销。 |
| 核验设备 | `iot_` 可参考 | 是 | 简单设备可用 `ticket_verify_device`，复杂设备接入可复用 `iot_`。 |
| 年卡用户权益 | `member_user`、`trade_order_item` | 是 | 年卡订单仍走商城，年卡账户/延期/核销用 `ticket_card_*` 或 `asset_` 扩展。 |

## 三、建议保留的 `ticket_` 扩展表

第一版建议保留这些候选表，后续写 SQL 时再逐表确认：

- `ticket_product_ext`：门票商品扩展，关联 `product_spu.id`。
- `ticket_sku_rule`：门票 SKU 规则扩展，关联 `product_sku.id`。
- `ticket_entitlement`：票务权益，关联 `trade_order.id`、`trade_order_item.id`、`member_user.id`。
- `ticket_entitlement_log`：权益状态、退款锁定、延期、人工调整日志。
- `ticket_verify_code`：核销码或二维码凭证。
- `ticket_verify_record`：核销记录。
- `ticket_verify_device`：核验设备。
- `ticket_session`：场次。
- `ticket_seat_area`、`ticket_seat`、`ticket_seat_lock`：座位和锁座。
- `ticket_card_user`：年卡用户权益。
- `ticket_card_extend_batch`：年卡延期批次。
- `ticket_refund_rule`：票务退款规则配置。

## 四、订单流程

1. 后台在 `product_spu`、`product_sku` 创建门票商品和价格库存。
2. 后台在 `ticket_product_ext`、`ticket_sku_rule` 配置景区、票种、实名、有效期、核销次数、退票规则。
3. 游客使用 `member_user` 身份下单，购物车和订单走 `trade_cart`、`trade_order`、`trade_order_item`。
4. 支付走 `pay_order`。
5. 支付成功后，根据 `trade_order_item` 生成 `ticket_entitlement` 和必要的 `ticket_verify_code`。
6. 入园核销时校验 `ticket_entitlement` 的状态、有效期、实名、剩余次数，再写入 `ticket_verify_record`。
7. 退票时创建 `trade_after_sale`，审核前检查权益剩余次数；审核通过后锁定可退权益并调用 `pay_refund`。
8. `pay_refund` 成功后更新 `trade_after_sale`、`trade_order_item.after_sale_status` 和 `ticket_entitlement`。

## 五、关键不变量

- `ticket_entitlement.used_count <= ticket_entitlement.total_count`。
- 已核销次数不能退款。
- 退款锁定次数不能再次核销。
- `trade_after_sale.refund_price <= trade_order_item.pay_price`。
- `pay_refund.refund_price` 必须等于最终实际支付退款金额。
- 同一个核销码不能重复成功核销。
- 已过期、已取消、已全额退款的权益不能核销。

## 六、codegen-bot 影响

`ticket` 模块仍然可以 codegen，但 SQL 输入里不要放 `ticket_order`、`ticket_order_item`、`ticket_refund`。

建议 `ticket` 第一批只生成：

- 门票商品扩展配置。
- SKU 规则配置。
- 权益和权益日志。
- 核销码、核销记录、核验设备。
- 场次、座位、锁座。
- 年卡权益和延期。

后端服务层再手工接入 mall/trade/pay/member 的订单、售后、支付和会员能力。
