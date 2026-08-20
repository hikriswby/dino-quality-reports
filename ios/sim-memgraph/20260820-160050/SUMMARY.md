# Dino English iOS 模拟器内存图 · 20260820-160050

## 给开发（先看这个）

- 本轮没有需要改产品代码的项。健康度看卡片「本次 vs 上次」。

完整证据见 [DEV.md](./DEV.md)。

- 结论：🟢 GREEN
- 平台/设备：模拟器 iPhone 17 Pro · 版本 1.5.2
- 分支/提交：dev-1.5.2 @ 5b83fc07 · 套件 logged-in · 档位 sim-memgraph
- 行为回归：通过 2 / 失败 0
- 本轨只采模拟器内存图，不采真机 CPU / Hitch / 页面

## 内存图

- 内存占用 162.3 MiB（峰值 184 MiB） (首次基线)
- leaks 计数 125（含系统对象，不当门禁）
- 业务 ROOT LEAK：无（系统根已过滤）

模拟器 leaks 计数含系统对象，不当产品泄漏门禁。

## 步骤

| 步骤 | 状态 | 耗时 | 备注 |
| --- | --- | --- | --- |
| 前置检查 | OK | 0 | iPhone 17 Pro / iOS 26.3 |
| 构建模拟器 Debug 包 | OK | 37 |  |
| 安装到模拟器 | OK | 2 |  |
| 构建装机 | OK | 0 | Dino AI.app |
| 补装已有产物 | OK | 2 |  |
| 构建装机 | OK | 0 | 补装 Dino AI.app |
| 登录态自检 | OK | 18 |  |
| 行为回归 第1轮 | OK | 0 | 通过 2 / 失败 0 / infra 0 |
| 行为回归 | OK | 0 | 通过 2 / 失败 0 / infra   |
| 崩溃检查 | OK | 0 | 无新增 |
| 性能体检·静态扫描 | OK | 2 |  |
| 性能体检·体积 | OK | 2 |  |
| 性能体检·内存 | OK | 76 |  |
| 性能体检·泄漏 | OK | 81 |  |
| 性能体检·CPU | SKIP | 0 | 模拟器无 Activity monitoring service，仅真机可用 |
| 性能体检·页面[chat] | SKIP | 0 | 模拟器无 Activity monitoring service，仅真机可用 |
| 性能体检·页面[class] | SKIP | 0 | 模拟器无 Activity monitoring service，仅真机可用 |
| 性能体检·页面[explore] | SKIP | 0 | 模拟器无 Activity monitoring service，仅真机可用 |
| 性能体检·页面[play] | SKIP | 0 | 模拟器无 Activity monitoring service，仅真机可用 |
| 性能体检·内存图 | OK | 20 | 内存图峰值162.3MiB 泄漏125 根链384B(CFString) |
| 性能体检·TimeProfiler | SKIP | 0 | 模拟器 memgraph 轨不跑冷启动 --launch |
| 性能体检·帧率 | SKIP | 0 | Hitches 仅真机可用（模拟器不支持） |

