# 质量跑批 · 20260818-170924

## 结论：🟡 YELLOW

- 告警等级：`ALERT`
- 等级定义：`BLOCK`=需要阻断/立刻处理，`ALERT`=可继续但需关注，`INFO`=通过

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
| 安装弹窗守护 | ✅ OK | 0s | pid 38129 |
| 编译 debug 包 | ✅ OK | 96s |  |
| 安装到设备 | ✅ OK | 4s |  |
| 登录态自检 | ✅ OK | 14s |  |
| 行为回归 [all-logged-in] | ✅ OK | 1191s |  |
| 性能体检 | ✅ OK | 512s |  |

## 崩溃/ANR

🟢 未捕获到崩溃/ANR（行为回归 + 性能体检全程）。

## 行为回归

通过 **17** / 失败 **0**（来源 `REPORT.md`）

报告：`reports/e2e-run/20260818-171145/REPORT.md`

## Flaky 滚动台账

- 窗口：最近 **14** 天，样本 **5** 次（含本次）
- 当前 flaky：**0**（新增 0 / 持续 0 / 消失 0）
- 本窗口内无 flaky 变化

## 性能体检

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

| 指标 | 本次 | 上次 | 变化 |
|---|---|---|---|
| jank | 11.82% | 11.82 | ⚪ 持平 |
| 主线程最长任务 | 37ms | 37 | ⚪ 持平 |
| 内存增长 | 9MB | 9 | ⚪ 持平 |
| 泄漏 | 2个 | 2 | ⚪ 持平 |

> 环比仅为相邻两次对比提示（含设备噪声），非门禁判定；连续多次同向劣化才需处理。

报告：`/Users/dino/dino-english-android/performance-toolkit/performance-results/run-all/20260818_173143/SUMMARY.md`

## 产物

- 本次跑批：`/Users/dino/dino-english-android/reports/quality-run/20260818-170924/`（run.log、build.log、e2e.log、perf.log）
- 趋势台账：`reports/perf-baseline/perf-trend.csv`


---

## 🤖 AI 根因分析

| 维度 | 结论 |
|---|---|
| 类型 | `infra_env` |
| 置信度 | high |
| 根因 | 跑批整体通过，无业务回归或测试资产问题，仅有输入法保障和APK体积、Flashlight的性能警告，属于环境或基础设施层面的潜在风险。 |
| 建议 | 无需立即处理，但需关注输入法保障的警告，确保后续跑批中inputText注入稳定；同时监控APK体积和Flashlight指标，若持续恶化则需进一步分析。 |

**摘要**：本次跑批整体通过，E2E 17/17全部成功，无业务回归或测试资产问题。仅有输入法保障、APK体积和Flashlight的性能警告，属于环境或基础设施层面的潜在风险，建议持续监控。

_由 DeepSeek (deepseek-chat) 自动生成，仅供参考_
