# HotelByte Public Docs Agent Guide

本文件覆盖 `hotelbyte-com/docs` 独立仓库。这里同时包含人工文档、供应商认证证据和 Upptime 自动状态数据；不要依赖其他仓库说明来判断其真实性。

## 职责

- 维护公开说明、错误处理指南、供应商认证材料，以及 `status.hotelbyte.com` 的 Upptime 配置与可审计历史。
- 区分人工结论、原始证据和机器人生成物；不把旧日志、模拟结果或本地文件当成当前生产事实。
- 完成变更后在 feature branch 提交并 push；workflow dispatch、Pages 部署、状态探测、incident 操作或其他对外发布必须有明确任务。

## 先读入口

1. `README.md`：仓库用途与对客户/供应商的问题清单。
2. `.upptimerc.yml`：状态站点、探测 URL、调度和展示配置的人工维护入口。
3. `certification/<supplier>/README.md`、submission/summary 文件及其原始 report/log：认证结论的证据链。
4. `error-handling-en.md` 与 `error-handling-zh.md`：双语错误处理契约。
5. `.github/workflows/`：Upptime/Pages 执行面；修改前读取嵌套指导。

## 内容、生成物与发布边界

- Upptime 自动维护 `api/`、`graphs/`、`history/`、README 状态摘要以及带生成警告的 workflow。不要手工美化或修补这些输出；配置变更优先落在 `.upptimerc.yml`，再由自动化生成。
- 认证报告必须写清 supplier、环境、场景、时间、请求/响应证据、通过/失败/未验证项。不得把搜索成功外推为可订、可取消、可结算或已上线。
- 既有原始认证证据视为历史记录，不原地改写结果。复测使用新的日期/运行目录并从新原始证据生成总结。
- 不新增 app secret、password、token、Authorization、签名、可重放 curl、客户 PII 或内部管理链接。提交前对新增/修改证据做人工脱敏检查。
- 双语契约变化应同步维护中英文版本，或明确记录暂未同步的语言 gap。

## 最小验证

- 任意变更：运行 `git diff --check`，逐项阅读 changed files，并确认没有意外改动机器人产物。
- `.upptimerc.yml` 变更：核对 YAML 结构、站点 slug/URL、expected status、调度和公开展示影响；未读取 workflow 结果前不得宣称状态页已发布或探测已生效。
- 认证材料变更：从原始 report/log 逐条核对 summary 的场景数、结果、时间和环境；明确哪些 live 步骤未运行。
- Markdown/链接变更：核对仓库内相对链接目标存在；外部链接或线上状态只有实际访问验证后才能声称可用。

## Code Review Rules

- 阻止无证据的“100% 完成”“production ready”“已上线”等结论；安全路径是引用具体 run artifact，并列出失败与未验证项。
- 阻止改写历史原始证据、把 mock/旧日志冒充当前 live 结果，或用空结果冒充成功。
- 阻止新增凭据、签名、可重放请求、客户 PII 和不必要的内部链接；证据保留最小必要字段并脱敏。
- 阻止直接编辑 Upptime 生成物来改变公开状态；安全路径是修改配置/真实信源并等待自动化产出可核验证据。
- 阻止状态配置或 workflow 变化被描述为已部署，除非有对应 Actions/Pages/公开站点证据。
