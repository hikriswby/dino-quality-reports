# 质量跑批 · 20260815-150950

## 结论：🔴 RED

- 行为回归 1 条失败（smoke-logged-in-shell）

| 项 | 值 |
|---|---|
| 分支 / 提交 | `dev/feature/0815` @ `1b674930` |
| 应用版本 | 1.5.3 |
| 设备 | FRI-AN00 (`AKVR022B19000811`) |
| Maestro 套件 | `smoke` |
| 跑批档位 | `nightly`（每晚全量档：全量回归 + 性能体检） |
| 设备联网 | 是 |

## 跑批步骤

| 步骤 | 状态 | 耗时 | 备注 |
|---|---|---|---|
| 前置检查 | ✅ OK | 0s | FRI-AN00 / Android 15 |
| 输入法保障 | ⚠️ WARN | 0s | 真机 inputText 注入可能失效 |
| 安装弹窗守护 | ✅ OK | 0s | pid 20772 |
| 编译 debug 包 | ✅ OK | 81s |  |
| 安装到设备 | ✅ OK | 4s |  |
| 登录态自检 | ⚠️ WARN | 103s | exit 1 |
| 登录态注入 | ⚠️ WARN | 14s | exit 1 |
| 登录态自检(注入后) | ⚠️ WARN | 103s | exit 1 |
| Google 登录 | ⚠️ WARN | 55s | exit 1 |
| 行为回归 [smoke] | ❌ FAIL | 400s | 1 条 flow 失败（脚本退出码为 0，见 REPORT.md） |
| 性能体检 | ✅ OK | 465s |  |

## 崩溃/ANR

🟢 未捕获到崩溃/ANR（行为回归 + 性能体检全程）。

## 行为回归

通过 **2** / 失败 **1**（来源 `REPORT.md`）

失败 flow：
- `smoke-logged-in-shell`

报告：`reports/e2e-run/20260815-151631/REPORT.md`

## 性能体检

| 维度 | 状态 | 耗时 |
|---|---|---|
| 应用状态自检 | ⚠️ WARN | 0s |
| 静态扫描 | ✅ OK | 2s |
| APK体积 | ⚠️ WARN | 0s |
| 内存+Heap | ✅ OK | 117s |
| Shark泄漏 | ✅ OK | 19s |
| Perfetto断言 | ⚠️ WARN | 93s |
| 页面归因[chat] | ⚠️ WARN | 54s |
| 页面归因[class] | ⚠️ WARN | 54s |
| 页面归因[explore] | ⚠️ WARN | 55s |
| 页面归因[play] | ⚠️ WARN | 54s |
| Flashlight | ⚠️ WARN | 14s |

| 指标 | 本次 | 上次 | 变化 |
|---|---|---|---|
| jank | 1.5% | - | 首次 |
| 主线程最长任务 | 19ms | - | 首次 |
| 内存增长 | 3MB | - | 首次 |
| 泄漏 | 2个 | - | 首次 |

> 环比仅为相邻两次对比提示（含设备噪声），非门禁判定；连续多次同向劣化才需处理。

报告：`/Users/dino/dino-english-android/performance-toolkit/performance-results/run-all/20260815_152316/SUMMARY.md`

## 产物

- 本次跑批：`/Users/dino/dino-english-android/reports/quality-run/20260815-150950/`（run.log、build.log、e2e.log、perf.log）
- 趋势台账：`reports/perf-baseline/perf-trend.csv`

