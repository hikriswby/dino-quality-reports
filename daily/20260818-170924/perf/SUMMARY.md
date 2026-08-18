# 性能一次性体检 · SUMMARY

- 产物目录: `/Users/dino/dino-english-android/performance-toolkit/performance-results/run-all/20260818_173143`
- 应用: `com.prime.dino.english`
- 设备: `AKVR022B19000811` (FRI-AN00)
- 应用状态: `main_shell`

## 步骤总览

> ⚠️ = 运行完成但子脚本返回非 0：通常代表**发现风险项**（静态 CRITICAL / 内存增长超阈值）或单条 maestro flow 偶发 flaky，**不是体检本身失败**。关键指标以下表为准。

| 维度 | 状态 | 耗时 |
|---|---|---|
| 应用状态自检 | ✅ OK | 0s |
| 静态扫描 | ✅ OK | 3s |
| APK体积 | ⚠️ WARN | 0s |
| 内存+Heap | ✅ OK | 160s |
| Shark泄漏 | ✅ OK | 4s |
| Perfetto断言 | ✅ OK | 139s |
| 页面归因[chat] | ✅ OK | 52s |
| 页面归因[class] | ✅ OK | 54s |
| 页面归因[explore] | ✅ OK | 42s |
| 页面归因[play] | ✅ OK | 44s |
| Flashlight | ⚠️ WARN | 11s |

## 关键指标

| 指标 | 值 | 来源 |
|---|---|---|
| 静态风险 | runBlocking×18, SharedPreferences×57 | static-analysis-report.md |
| 内存增长(单次遍历) | 9 MB (1.49%) | maestro-performance-report.md |
| Java heap 直方图 | 🟢 无 Activity/Fragment 多实例 | heap-histogram.md |
| 泄漏(引用链) ⚠️ | 2 个: com.prime.dino.english.splash.SplashActivity | shark-leak-20260818_173428.txt |
| 主线程/jank 🟢 | jank 11.82%, 最长 37ms | trace-assert-runall_20260818_173432.md |

## 动态门禁阈值

| 指标 | 当前阈值 | 来源 | 样本 |
|---|---|---|---|
| jank_pct | > 20.0 | 基础阈值 | 2 |
| leak_count | > 3 | 基础阈值 | 2 |
| fl_fps | < 30.0 | 基础阈值 | 2 |

## 门禁

🟢 通过（已判定：jank 掉帧率 > 20%、泄漏引用链 > 3 条）

## 与上次对比

上次: `20260815_160238` (device `AKVR022B19000811` )

| 指标 | 上次 | 本次 | 变化 |
|---|---|---|---|
| mem_growth_mb | 9.0 | 9 | ⚪ 持平 |
| leak_count | 2.0 | 2 | ⚪ 持平 |
| jank_pct | 11.82 | 11.82 | ⚪ 持平 |
| worst_task_ms | 37.0 | 37 | ⚪ 持平 |

> 🔴 劣化仅为**相邻两次**对比提示（含模拟器噪声），非门禁判定；连续多版同向劣化才需关注。

## 产物索引

- 静态: `performance-results/static-analysis/`
- APK: `performance-results/apk-analysis/`
- 内存+Heap: `performance-results/maestro-test/`（含 heap-histogram.md）
- 泄漏引用链: `performance-results/shark/`
- Perfetto+断言: `performance-results/perfetto/` + `performance-results/trace-assert/`
- Flashlight: `/Users/dino/dino-english-android/performance-toolkit/performance-results/run-all/20260818_173143/flashlight.json`（`flashlight report` 可开网页）
- 趋势台账（入库）: `reports/perf-baseline/perf-trend.csv`

