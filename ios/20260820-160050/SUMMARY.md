# Dino English iOS 质量跑批 · 20260820-160050

## 给开发（先看这个）

- **泄漏** · 125 · 根 `CFString`
- **采集盲区** · Chat、Class、Explore、Play 页未采到，该页性能未知（不是产品失败，也不能当健康）。

完整证据见 [DEV.md](./DEV.md)。

- 结论：🟢 GREEN
- 平台/设备：模拟器 iPhone 17 Pro · 版本 1.5.2
- 分支/提交：dev-1.5.2 @ 5b83fc07 · 套件 logged-in
- 行为回归：通过 2 / 失败 0
- 性能体检：5/12 项采集成功（盲区：Chat、Class、Explore、Play 页未采到，不能用整机 CPU 代替）（7 项跳过）

## 性能指标（本次 vs 上次）

- 样本不足 n=0，噪声带用实测 floor

- CPU峰值 **未采到**（本轮环境不可用）
- CPU均值 **未采到**（本轮环境不可用）
- 内存峰值 **未采到**（本轮环境不可用）
- Hitch Time Ratio **未采到**（本轮环境不可用）
- Hitch 次数 **未采到**（本轮环境不可用）
- Chat CPU峰值 **未采到**（连续 9 次，**Chat 页性能是盲区**；本轮环境不可用。冷启动/整机 CPU 不能代替该页）
- Class CPU峰值 **未采到**（连续 6 次，**Class 页性能是盲区**；本轮环境不可用。冷启动/整机 CPU 不能代替该页）
- Explore CPU峰值 **未采到**（连续 6 次，**Explore 页性能是盲区**；本轮环境不可用。冷启动/整机 CPU 不能代替该页）
- Play CPU峰值 **未采到**（连续 6 次，**Play 页性能是盲区**；本轮环境不可用。冷启动/整机 CPU 不能代替该页）
- Time Profiler 跳过
- 内存图 已采集（模拟器作业，不当泄漏门禁）

未采到的页面不能判定，也不能用冷启动/整机 CPU 代替。

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

