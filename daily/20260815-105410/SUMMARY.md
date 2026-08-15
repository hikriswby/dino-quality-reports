# 质量跑批 · 20260815-105410

## 结论：🔴 RED

- 行为回归 3 条失败（smoke-app-launch、smoke-logged-in-shell、smoke-onboarding）
- 性能未采集到数据：内存增长、泄漏引用链（不代表正常，需查日志）

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
| 安装弹窗守护 | ✅ OK | 0s | pid 58677 |
| 编译 debug 包 | ✅ OK | 316s |  |
| 安装到设备 | ✅ OK | 4s |  |
| 登录态自检 | ⚠️ WARN | 112s | exit 1 |
| 登录态注入 | ⚠️ WARN | 10s | exit 1 |
| 登录态自检(注入后) | ⚠️ WARN | 104s | exit 1 |
| Google 登录 | ⚠️ WARN | 106s | exit 1 |
| 行为回归 [smoke] | ❌ FAIL | 747s | 3 条 flow 失败（脚本退出码为 0，见 REPORT.md） |
| 性能体检 | ✅ OK | 479s |  |

## 崩溃/ANR

🟢 未捕获到崩溃/ANR（行为回归 + 性能体检全程）。

## 行为回归

通过 **0** / 失败 **3**（来源 `REPORT.md`）

失败 flow：
- `smoke-app-launch`
- `smoke-logged-in-shell`
- `smoke-onboarding`

报告：`reports/e2e-run/20260815-110537/REPORT.md`

## 性能体检

| 维度 | 状态 | 耗时 |
|---|---|---|
| 应用状态自检 | ⚠️ WARN | 0s |
| 静态扫描 | ✅ OK | 2s |
| APK体积 | ⚠️ WARN | 0s |
| 内存+Heap | ⚠️ WARN | 117s |
| Perfetto断言 | ⚠️ WARN | 118s |
| 页面归因[chat] | ⚠️ WARN | 55s |
| 页面归因[class] | ⚠️ WARN | 54s |
| 页面归因[explore] | ⚠️ WARN | 55s |
| 页面归因[play] | ⚠️ WARN | 55s |
| Flashlight | ⚠️ WARN | 4s |

| 指标 | 本次 | 上次 | 变化 |
|---|---|---|---|

> 环比仅为相邻两次对比提示（含设备噪声），非门禁判定；连续多次同向劣化才需处理。

报告：`/Users/dino/dino-english-android/performance-toolkit/performance-results/run-all/20260815_111808/SUMMARY.md`

## 产物

- 本次跑批：`/Users/dino/dino-english-android/reports/quality-run/20260815-105410/`（run.log、build.log、e2e.log、perf.log）
- 趋势台账：`reports/perf-baseline/perf-trend.csv`

