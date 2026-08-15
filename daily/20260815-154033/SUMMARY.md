# 质量跑批 · 20260815-154033

## 结论：🟡 YELLOW

- 有降级步骤：输入法保障
- 性能维度有风险项：APK体积、Flashlight

| 项 | 值 |
|---|---|
| 分支 / 提交 | `dev/feature/0815` @ `1b674930` |
| 应用版本 | 1.5.3 |
| 设备 | FRI-AN00 (`AKVR022B19000811`) |
| Maestro 套件 | `all-logged-in` |
| 跑批档位 | `nightly`（每晚全量档：全量回归 + 性能体检） |
| 设备联网 | 是 |

## 跑批步骤

| 步骤 | 状态 | 耗时 | 备注 |
|---|---|---|---|
| 前置检查 | ✅ OK | 0s | FRI-AN00 / Android 15 |
| 输入法保障 | ⚠️ WARN | 0s | 真机 inputText 注入可能失效 |
| 安装弹窗守护 | ✅ OK | 0s | pid 34961 |
| 编译 debug 包 | ✅ OK | 54s |  |
| 安装到设备 | ✅ OK | 4s |  |
| 登录态自检 | ✅ OK | 14s |  |
| 行为回归 [all-logged-in] | ✅ OK | 1222s |  |
| 性能体检 | ✅ OK | 503s |  |

## 崩溃/ANR

🟢 未捕获到崩溃/ANR（行为回归 + 性能体检全程）。

## 行为回归

通过 **17** / 失败 **0**（来源 `REPORT.md`）

报告：`reports/e2e-run/20260815-154209/REPORT.md`

## 性能体检

| 维度 | 状态 | 耗时 |
|---|---|---|
| 应用状态自检 | ✅ OK | 0s |
| 静态扫描 | ✅ OK | 3s |
| APK体积 | ⚠️ WARN | 0s |
| 内存+Heap | ✅ OK | 156s |
| Shark泄漏 | ✅ OK | 3s |
| Perfetto断言 | ✅ OK | 137s |
| 页面归因[chat] | ✅ OK | 51s |
| 页面归因[class] | ✅ OK | 55s |
| 页面归因[explore] | ✅ OK | 42s |
| 页面归因[play] | ✅ OK | 43s |
| Flashlight | ⚠️ WARN | 10s |

| 指标 | 本次 | 上次 | 变化 |
|---|---|---|---|
| jank | 14.08% | - | 首次 |
| 主线程最长任务 | 42ms | - | 首次 |
| 内存增长 | 10MB | - | 首次 |
| 泄漏 | 2个 | - | 首次 |

> 环比仅为相邻两次对比提示（含设备噪声），非门禁判定；连续多次同向劣化才需处理。

报告：`/Users/dino/dino-english-android/performance-toolkit/performance-results/run-all/20260815_160238/SUMMARY.md`

## 产物

- 本次跑批：`/Users/dino/dino-english-android/reports/quality-run/20260815-154033/`（run.log、build.log、e2e.log、perf.log）
- 趋势台账：`reports/perf-baseline/perf-trend.csv`

