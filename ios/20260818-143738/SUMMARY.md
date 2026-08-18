# Dino English iOS 质量跑批 · 20260818-143738

- 结论：🔴 RED
  - 行为回归 1 条失败（profile-from-chat）
- 平台/设备：模拟器 iPhone 17 Pro · 版本 1.5.2
- 分支/提交：dev-1.5.2 @ 5b83fc07 · 套件 logged-in
- 行为回归：通过 16 / 失败 1
  - 失败：profile-from-chat
- 性能体检：5/11 项采集成功（6 项跳过）

## 步骤

| 步骤 | 状态 | 耗时 | 备注 |
| --- | --- | --- | --- |
| 前置检查 | OK | 0 | iPhone 17 Pro / iOS 26.3 |
| git fetch | OK | 4 |  |
| checkout dev-1.5.2 | OK | 2 |  |
| 构建装机 | SKIP | 0 | --skip-build |
| 补装已有产物 | OK | 2 |  |
| 构建装机 | OK | 0 | 补装 Dino AI.app |
| 登录态自检 | OK | 17 |  |
| 行为回归 第1轮 | OK | 0 | 通过 16 / 失败 1： profile-from-chat |
| 行为回归 | OK | 0 | 通过 16 / 失败 1： profile-from-chat |
| 崩溃检查 | OK | 0 | 无新增 |
| 性能体检·静态扫描 | OK | 2 |  |
| 性能体检·体积 | OK | 2 |  |
| 性能体检·内存 | OK | 77 |  |
| 性能体检·泄漏 | OK | 71 |  |
| 性能体检·内存图 | OK | 16 | 内存图峰值142.1MiB 泄漏123 根链320B(CFString) |
| 性能体检·CPU | SKIP | 0 | 模拟器无 Activity monitoring service，仅真机可用 |
| 性能体检·页面[chat] | SKIP | 0 | 模拟器无 Activity monitoring service，仅真机可用 |
| 性能体检·页面[class] | SKIP | 0 | 模拟器无 Activity monitoring service，仅真机可用 |
| 性能体检·页面[explore] | SKIP | 0 | 模拟器无 Activity monitoring service，仅真机可用 |
| 性能体检·页面[play] | SKIP | 0 | 模拟器无 Activity monitoring service，仅真机可用 |
| 性能体检·帧率 | SKIP | 0 | Hitches 仅真机可用（模拟器不支持） |

