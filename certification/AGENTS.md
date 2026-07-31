# Certification Evidence Rules

此目录保存供应商认证与复测证据。结论必须可回溯到同一 supplier、环境和 run 的原始 artifact。

- 先读 supplier 自己的 README/submission/summary，再读其引用的 report/log；不要跨 supplier 或跨日期拼接证据。
- 既有日志和报告是历史记录。复测创建新的日期/运行目录，不覆盖旧成功、失败或请求/响应。
- 新增证据前移除 password、app secret、token、Authorization、签名、可重放 curl、客户 PII 和不必要的内部地址；脱敏后保留场景、时间、状态和关联 ID 等最小审计字段。
- summary 分开陈述搜索、价格、check-avail、预订、查询、取消和结算；任何未执行步骤都标为未验证。
- 最小验证：`git diff --check`，逐条从原始 artifact 复算 summary 的通过/失败/未验证清单，并人工检查敏感字段。

## Code Review Rules

- 阻止无原始 run anchor 的认证结论、跨 run 拼接以及失败步骤被省略。
- 阻止历史 artifact 原地改写；安全路径是追加新的复测目录和带来源链接的总结。
- 阻止任何可用凭据、签名、可重放请求或客户 PII 进入新证据。
