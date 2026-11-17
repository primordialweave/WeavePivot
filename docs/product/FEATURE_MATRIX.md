# 织枢 WeavePivot — 功能矩阵 V1.1
> 2026-11-17 · 版本规划；P0 必须，P1 重要，P2 后续

| 模块 | 功能 | 优先级 | 目标版本 | 依赖 | 验收重点 |
|---|---|---|---|---|---|
| Core | 生命周期/菜单栏/快捷键 | P0 | V0.1 | AppKit | 无冲突、可退出 |
| Island | 刘海融合与无刘海替代 | P0 | V0.1 | 窗口 PoC | 定位与多屏 |
| Island | 悬停/展开/收起 | P0 | V0.1 | 事件管理 | 状态可逆 |
| Island | 任务进度与完成提示 | P1 | V0.1 | 任务总线 | 不打扰 |
| Clipboard | 文本/图片历史 | P0 | V0.1 | NSPasteboard | 搜索、删除 |
| Clipboard | 文件/链接历史 | P1 | V0.1 | 数据类型解析 | 正确预览 |
| Clipboard | 暂停/排除应用/清空 | P0 | V0.1 | 隐私策略 | 无残留 |
| Capture | 区域截图 | P0 | V0.1 | 屏幕权限 | Retina/多屏 |
| Capture | 基础标注/OCR（可选） | P2 | V0.1 | Vision | 可取消/重试 |
| Desk | 统一素材暂存 | P0 | V0.1 | 素材模型 | 拖入拖出 |
| Desk | 搜索、分类、多选 | P1 | V0.1 | 索引 | 大量素材 |
| Flow | 用户可见预设动作 | P1 | V0.2 | 任务执行器 | 错误反馈 |
| Archive | ZIP 解压与预览 | P1 | V0.2 | 文件访问 | 安全路径 |
| Dock | 窗口预览与切换 | P1 | V0.2 | Accessibility PoC | 降级策略 |
| Convert | 图片格式转换 | P1 | V0.2 | ImageIO | 无损源文件 |
| Flow | 可配置串行工作流 | P1 | V0.3 | 流程模型 | 中断恢复 |
| Automation | 规则触发 | P2 | V0.3 | 权限/事件总线 | 可关闭 |
| Platform | 基础无障碍、隐私、性能与发布安全 | P0 | V0.1 | 全模块 | 发布验证 |
| Extensions | 插件 API | P2 | V1.x | 安全隔离 | 版本兼容 |
| AI | 本地语义能力 | P2 | V1.x | 模型评估 | 明确隐私 |

## 版本发布边界
### V0.1：可用 MVP
必须完成入口、剪贴板、截图、素材暂存的闭环。OCR 与基础标注可作为可选增强，不阻塞核心发布；用户可见预设工作流不属于 V0.1。Dock 预览和解压缩不属于本版承诺。

### V0.2：日常工具扩展
在 PoC 通过的前提下加入 ZIP 解压、文件转换、用户可见织流预设和可行范围内的 Dock 预览；不得承诺替换系统 Dock。

### V0.3：跨工具自动化
用户可见预设流程升级为可配置流程，加入执行历史、重试、取消、参数校验；复杂可视化编辑器根据测试反馈决定。

### V1.1：稳定发布
完善隐私、性能、无障碍、崩溃恢复、签名公证、分发渠道和回归测试。

## 技术风险与先行 PoC
1. 刘海窗口：顶部坐标、菜单栏、全屏、Spaces、多显示器。
2. 剪贴板：隐私提示、轮询成本、不同数据类型和排除规则。
3. 截图：屏幕权限、缩放因子、OCR、权限撤销。
4. Dock：窗口枚举、缩略图、权限及系统限制。
失败时必须有明确的功能降级和用户提示。

## 依赖关系
Core → Island / Clipboard / Capture；Clipboard + Capture → Desk；Desk + Task Engine → Flow；Archive 与 Dock 各自独立验证后接入统一素材模型。

## 质量与发布 Gate
G0 需求冻结；G1 PoC 通过；G2 设计标注完成；G3 核心闭环可运行；G4 权限/隐私/无障碍/性能测试；G5 Beta 反馈和发布。

## 优先级变更规则
新增功能必须明确用户场景、跨模块价值、权限成本、资源开销、验收标准；不得以概念图中出现某控件作为已承诺实现的依据。

## V1.1 唯一发布范围矩阵
P0/P1/P2 是**优先级**，不能替代交付等级。版本与交付等级共同决定是否阻塞发布。

| ID | 功能 | V0.1 | V0.2 | V0.3 |
|---|---|---|---|---|
| CORE-01 | 原生应用、菜单栏、快捷键 | Must | Maintain | Maintain |
| ISL-01/02 | 织岛定位、状态、无刘海降级 | Must（PoC 决定降级） | Improve | Improve |
| CLIP-01/02 | 文本/图片历史及隐私控制 | Must | Improve | Improve |
| CAP-01 | 区域截图、复制/保存/暂存 | Must | Improve | Improve |
| DESK-01 | 文本/图片暂存、拖入拖出 | Must | Improve | Improve |
| DATA-01/A11Y-01 | 本地清理、权限、键盘/VoiceOver、性能基线 | Must | Improve | Improve |
| CLIP-03 | 文件/链接历史 | Should | Complete | Maintain |
| DESK-02 | 分类、多选、增强搜索 | Should | Complete | Maintain |
| CAP-02 | OCR/基础标注 | Could | Complete | Maintain |
| FLOW-01 | 用户可见预设工作流 | Out | Should | Complete |
| ARCH-01 | ZIP 解压与图片转换 | Out | Should | Maintain |
| DOCK-01 | 窗口预览（需 PoC） | Out | Conditional | Conditional |
| FLOW-02 | 可配置工作流 | Out | Out | Should |

**发布规则**：Must 必须通过；Should 可以延期并记录；Could 不得成为隐性阻塞项；Out 不得提前开发成承诺。性能、无障碍、隐私在 V0.1 就是 Must，V1.0 进一步成熟。

## 需求追踪与变更控制
需求 ID 必须贯穿 PRD、测试用例、GitHub Issue 和 PR。变更版本归属需同步更新 PRODUCT_VISION、PRD、DEVELOPMENT_PLAN、TEST_PLAN 和 ADR-005；单独修改此表不视为完成变更。Dock 相关能力只能基于公开 API 的 PoC 结论进入发布范围。
