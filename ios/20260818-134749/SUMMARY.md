# Dino English iOS 质量跑批 · 20260818-134749

- 结论：🔴 RED
  - 行为回归 2 条失败（profile-from-chat）
- 平台/设备：模拟器 iPhone 17 Pro · 版本 1.5.2
- 分支/提交：dev-1.5.2 @ 5b83fc07 · 套件 logged-in
- 行为回归：通过 32 / 失败 2
  - 失败：profile-from-chat

## 步骤

| 步骤 | 状态 | 耗时 | 备注 |
| --- | --- | --- | --- |
| 前置检查 | OK | 0 | iPhone 17 Pro / iOS 26.3 |
| git fetch | OK | 6 |  |
| checkout dev-1.5.2 | OK | 2 |  |
| 构建装机 | SKIP | 0 | --skip-build |
| 补装已有产物 | OK | 2 |  |
| 构建装机 | OK | 0 | 补装 Dino AI.app |
| 登录态自检 | OK | 18 |  |
| 行为回归 第1轮 | OK | 0 | 通过 16 / 失败 1： profile-from-chat |
| 行为回归 第2轮 | OK | 0 | 通过 16 / 失败 1： profile-from-chat |
| 行为回归 | OK | 0 | 通过 32 / 失败 2： profile-from-chat |
| 崩溃检查 | OK | 0 | 无新增 |

## Flaky 统计

🔴 稳定失败（1 条，每轮都失败，属真回归）：
  - profile-from-chat

