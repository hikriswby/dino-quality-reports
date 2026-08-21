# 性能一次性体检 · SUMMARY

- 产物目录: `/Users/dino/dino-quality/android/performance-toolkit/performance-results/run-all/20260821_111536`
- 应用: `com.prime.dino.english`
- 设备: `19011FDF6003CU` (Pixel 6)
- 应用状态: `main_shell`

## 步骤总览

> ⚠️ = 运行完成但子脚本返回非 0：通常代表**发现风险项**（静态 CRITICAL / 内存增长超阈值）或单条 maestro flow 偶发 flaky，**不是体检本身失败**。关键指标以下表为准。

| 维度 | 状态 | 耗时 |
|---|---|---|
| 应用状态自检 | ✅ OK | 0s |
| 静态扫描 | ✅ OK | 14s |
| APK体积 | ⚠️ WARN | 0s |
| 内存+Heap | ⚠️ WARN | 157s |
| Shark泄漏 | ✅ OK | 3s |
| Perfetto断言 | ✅ OK | 137s |
| 页面归因[chat] | ✅ OK | 49s |
| 页面归因[class] | ✅ OK | 49s |
| 页面归因[explore] | ✅ OK | 43s |
| 页面归因[play] | ✅ OK | 46s |
| Flashlight | ✅ OK | 270s |

## 关键指标

| 指标 | 值 | 来源 |
|---|---|---|
| APK 安装大小 | 67.8 MB | `app-debug.apk` |
| 静态风险 | runBlocking×18, SharedPreferences×76 | static-analysis-report.md |
| 内存增长(单次遍历) | 16 MB (2.31%) | maestro-performance-report.md |
| Java heap 直方图 | 🟢 无 Activity/Fragment 多实例 | heap-histogram.md |
| 泄漏(引用链) ⚠️ | 2 个: com.prime.dino.english.feature.onboarding.impl.ui.welcome.WelcomeActivity, com.p | shark-leak-20260821_111829.txt |
| 主线程/jank 🟢 | jank 0.7%, 最长 0ms | trace-assert-runall_20260821_111833.md |
| Flashlight | status=SUCCESS avgFPS=59.0 avgRAM=667.7MB | flashlight.json |

## 门禁

🟢 通过（已判定：jank 掉帧率 > 20%、泄漏引用链 > 3 条、平均 FPS < 30）

## 与上次对比

无可比基线：趋势台账里还没有**同一台设备且处于主壳**的历史记录。
跨设备或跨应用状态的数字不在一个量纲上，比出来的劣化是假的，故不做对比。

## 产物索引

- 静态: `performance-results/static-analysis/`
- APK: `performance-results/apk-analysis/`
- 内存+Heap: `performance-results/maestro-test/`（含 heap-histogram.md）
- 泄漏引用链: `performance-results/shark/`
- Perfetto+断言: `performance-results/perfetto/` + `performance-results/trace-assert/`
- Flashlight: `/Users/dino/dino-quality/android/performance-toolkit/performance-results/run-all/20260821_111536/flashlight.json`（`flashlight report` 可开网页）
- 趋势台账（入库）: `/Users/dino/dino-quality/android/reports/perf-baseline/perf-trend.csv`

