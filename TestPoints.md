# 测试点列表

| 测试类         | 测试场景描述            | 输入值                          | 预期输出                            | 类型     |
|--------------|---------------------|-------------------------------|-----------------------------------|--------|
| User         | 正常借阅书籍            | 库存充足的Book，合法用户               | 借书成功，库存减少，借阅记录生成                 | 正常     |
| User         | 借无库存书             | 库存为0的Book，合法用户                | 抛出BookNotAvailableException              | 异常     |
| User         | 信用不足时借书           | 信用分低于阈值                       | 抛出InsufficientCreditException           | 异常     |
| User         | 账户被冻结时操作借书        | 账户状态为FROZEN                     | 抛出AccountFrozenException               | 异常     |
| User         | 自动续借功能            | 启用自动续借，借阅过期             | 自动续借执行，预约/续借记录变化                | 正常/异常  |
| User         | 用户类型边界           | 普通用户/超级用户                     | 权限限制分别表现正确                       | 边界     |
| User         | 超额借书/还书           | 已借满上限后借书                        | 抛出InvalidOperationException             | 边界/异常  |
| VIPUser      | 信用修复功能            | 信用不良用户，不同信用分                   | 信用分恢复如预期                           | 正常/边界  |
| Library      | 搜索存在/不存在的书籍       | 有效/无效关键词                        | 正确返回/空列表                           | 正常/边界  |
| Library      | 借书还书全流程           | 用户、有效Book                      | 借出、归还过程完整                          | 正常     |
| Library      | 借书超期罚款            | 借出后超期未还                        | 抛出OverdueFineException                  | 异常     |
| InventoryService| 添加/移除书籍          | 新Book/已有Book                     | 成功入库/减少，或错误提示                       | 正常/异常  |
| Reservation  | 正常预约/重复预约         | 同一用户、同一Book                    | 预约成功，重复预约被禁止或提示                     | 正常/边界  |
| BorrowRecord | 是否超期/正常记录         | 不同借阅到期日                       | isOverdue 返回正确                          | 正常/边界  |
| NotificationService| 邮件/短信通知        | 正确/错误手机号、邮件                   | 成功/抛出SMSException/EmailException         | 正常/异常  |
| ExternalLibraryAPI| 外部查询接口        | 合法/非法关键词                       | 返回匹配结果/空或错误                         | 正常/边界  |
| CreditRepairService| 信用修复          | 非信用不良用户/已修复用户                 | 无变化/恢复成功                             | 正常/边界  |
| AutoRenewalService| 正常/失败自动续借    | 可续借/已到期借阅                       | 返回true/false，续借状态如预期                     | 正常/异常  |
| 异常类（所有Exception）| 构造与捕获测试| new XxxException("msg")           | message 能正确获取、可被catch                   | 正常/异常  |
