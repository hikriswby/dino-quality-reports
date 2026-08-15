# 性能一次性体检 · SUMMARY

- 产物目录: `/Users/dino/dino-english-android/performance-toolkit/performance-results/run-all/20260815_152316`
- 应用: `com.prime.dino.english`
- 设备: `AKVR022B19000811` (FRI-AN00)
- 应用状态: `welcome`

> 🚨 **本次数据不具代表性**：应用停在欢迎页/登录页（登录态失效），下列 jank / FPS / 内存全部采自欢迎页。
> 不要用于版本间对比，也不要据此判断性能好坏。台账已标注 `app_state=welcome`。
> 恢复方式：设备上手动登录一次 → `./maestro/common/scripts/debug-auth-export.sh`

## 步骤总览

> ⚠️ = 运行完成但子脚本返回非 0：通常代表**发现风险项**（静态 CRITICAL / 内存增长超阈值）或单条 maestro flow 偶发 flaky，**不是体检本身失败**。关键指标以下表为准。

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

## 关键指标

| 指标 | 值 | 来源 |
|---|---|---|
| 静态风险 | runBlocking×18, SharedPreferences×57 | static-analysis-report.md |
| 内存增长(单次遍历) | 3 MB (1.05%) | maestro-performance-report.md |
| Java heap 直方图 | 🟢 无 Activity/Fragment 多实例 | heap-histogram.md |
| 泄漏(引用链) ⚠️ | 2 个: com.prime.dino.english.splash.SplashActivity | shark-leak-20260815_152534.txt |
| 主线程/jank 🟢 | jank 1.5%, 最长 19ms | trace-assert-runall_20260815_152538.md |

## 门禁

⏸️ 未判定：应用状态为 `welcome`，指标不具代表性，判红没有意义。

## 与上次对比

无可比基线：趋势台账里还没有**同一台设备且处于主壳**的历史记录。
跨设备或跨应用状态的数字不在一个量纲上，比出来的劣化是假的，故不做对比。

## 产物索引

- 静态: `performance-results/static-analysis/`
- APK: `performance-results/apk-analysis/`
- 内存+Heap: `performance-results/maestro-test/`（含 heap-histogram.md）
- 泄漏引用链: `performance-results/shark/`
- Perfetto+断言: `performance-results/perfetto/` + `performance-results/trace-assert/`
- Flashlight: `/Users/dino/dino-english-android/performance-toolkit/performance-results/run-all/20260815_152316/flashlight.json`（`flashlight report` 可开网页）
- 趋势台账（入库）: `reports/perf-baseline/perf-trend.csv`

