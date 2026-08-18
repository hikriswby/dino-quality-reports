# 质量跑批 · 20260818-183033

## 结论：🟡 YELLOW

- 告警等级：`ALERT`
- 等级定义：`BLOCK`=需要阻断/立刻处理，`ALERT`=可继续但需关注，`INFO`=通过

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
| 安装弹窗守护 | ✅ OK | 0s | pid 88011 |
| 编译 debug 包 | ✅ OK | 61s |  |
| 安装到设备 | ✅ OK | 4s |  |
| 登录态自检 | ✅ OK | 16s |  |
| 行为回归 [all-logged-in] | ✅ OK | 1715s |  |
| 性能体检 | ✅ OK | 718s |  |

## 崩溃/ANR

🟢 未捕获到崩溃/ANR（行为回归 + 性能体检全程）。

## 行为回归

通过 **17** / 失败 **0**（来源 `REPORT.md`）

报告：`reports/e2e-run/20260818-183217/REPORT.md`

## Flaky 滚动台账

- 窗口：最近 **14** 天，样本 **7** 次（含本次）
- 当前 flaky：**0**（新增 0 / 持续 0 / 消失 0）
- 本窗口内无 flaky 变化

## 性能体检

| 维度 | 状态 | 耗时 |
|---|---|---|
| 应用状态自检 | ✅ OK | 0s |
| 静态扫描 | ✅ OK | 3s |
| APK体积 | ⚠️ WARN | 0s |
| 内存+Heap | ✅ OK | 223s |
| Shark泄漏 | ✅ OK | 5s |
| Perfetto断言 | ✅ OK | 199s |
| 页面归因[chat] | ✅ OK | 109s |
| 页面归因[class] | ✅ OK | 67s |
| 页面归因[explore] | ✅ OK | 46s |
| 页面归因[play] | ✅ OK | 49s |
| Flashlight | ⚠️ WARN | 12s |

| 指标 | 本次 | 上次 | 变化 |
|---|---|---|---|
| jank | 1.06% | 11.82 | 🟢 改善 -10.76 |
| 主线程最长任务 | 42ms | 37 | 🔴 劣化 +5 |
| 内存增长 | 12MB | 9 | 🔴 劣化 +3 |
| 泄漏 | 2个 | 2 | ⚪ 持平 |

> 环比仅为相邻两次对比提示（含设备噪声），非门禁判定；连续多次同向劣化才需处理。

报告：`/Users/dino/dino-english-android/performance-toolkit/performance-results/run-all/20260818_190058/SUMMARY.md`

## 产物

- 本次跑批：`/Users/dino/dino-english-android/reports/quality-run/20260818-183033/`（run.log、build.log、e2e.log、perf.log）
- 趋势台账：`reports/perf-baseline/perf-trend.csv`


---

## 🤖 AI 根因分析

| 维度 | 结论 |
|---|---|
| 类型 | `infra_env` |
| 置信度 | high |
| 根因 | 性能维度存在风险项（APK体积、Flashlight），但E2E全部通过，无崩溃ANR，判定为环境或性能监控告警，非业务回归。 |
| 建议 | 关注APK体积和Flashlight告警，检查近期提交是否引入体积增加或性能问题；输入法WARN为已知环境问题，可忽略。 |

**摘要**：本次跑批E2E全部通过，无崩溃ANR，但性能维度有风险项（APK体积、Flashlight）。内存增长和最长任务耗时略有恶化，但卡顿率大幅改善，整体判定为环境/性能监控告警，非业务回归。建议关注APK体积和Flashlight告警，排查近期提交。

_由 DeepSeek (deepseek-chat) 自动生成，仅供参考_
