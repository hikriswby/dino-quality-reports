# 质量跑批 · 20260821-023003

## 给开发（先看这个）

- **存量泄漏** · `com.prime.dino.english.splash.SplashActivity` 被持有 · 约 2.5 MB · 证据 [shark-leak.txt](evidence/shark-leak.txt) · 打开 `core/common/src/main/java/com/prime/dino/english/core/common/lifecycle/ForeBackStatusUtils.java`
- **行为回归失败** · `smoke-app-launch` · `Assertion is false: id: .*btn_get_started.* is visible` · flow `/Users/dino/dino-quality/android/maestro/smoke/smoke-app-launch.yaml`
- **行为回归失败** · `smoke-logged-in-shell` · `Assertion is false: id: .*tab_bar.* is visible` · flow `/Users/dino/dino-quality/android/maestro/smoke/smoke-logged-in-shell.yaml`
- **行为回归失败** · `smoke-onboarding` · `Assertion is false: id: .*btn_get_started.* is visible` · flow `/Users/dino/dino-quality/android/maestro/smoke/smoke-onboarding.yaml`
- **崩溃** @ `性能体检` · `进程消失（start pid 4739，无需日志解析）`
- **采集盲区** · Chat、Class、Explore、Play 页未采到，该页性能未知（不是产品失败，也不能当健康）。

完整证据与引用链见 [DEV.md](./DEV.md)。

## 结论：🔴 RED

- 告警等级：`BLOCK`
- 等级定义：`BLOCK`=需要阻断/立刻处理，`ALERT`=可继续但需关注，`INFO`=通过

- 跑批中断：Google 登录
- 行为回归 3 条失败（smoke-app-launch、smoke-logged-in-shell、smoke-onboarding）
- 崩溃/ANR 1 起：崩溃@性能体检（进程消失（start pid 4739，无需日志解析））
- 主线程最卡：`Choreographer#doFrame` ×12 最长 28ms

| 项 | 值 |
|---|---|
| 分支 / 提交 | `dev/feature/0821` @ `490fba08` |
| 应用版本 | 1.5.4 |
| 设备 | ELE-AL00 (`8KE5T19802015934`) |
| Maestro 套件 | `smoke` |
| 跑批档位 | `nightly`（每晚全量档：全量回归 + 性能体检） |
| 设备联网 | 是 |

## 跑批步骤

| 步骤 | 状态 | 耗时 | 备注 |
|---|---|---|---|
| 前置检查 | ✅ OK | 0s | ELE-AL00 / Android 10 |
| 输入法保障 | ⚠️ WARN | 0s | 真机 inputText 注入可能失效 |
| 安装弹窗守护 | ✅ OK | 0s | pid 65003 |
| 同步代码 | ✅ OK | 0s | origin/dev/feature/0821 (490fba08)（2 个提交） |
| 编译 debug 包 | ✅ OK | 1240s |  |
| 安装到设备 | ✅ OK | 7s |  |
| 登录态自检 | ⚠️ WARN | 131s | exit 1 |
| 登录态注入 | ⚠️ WARN | 131s | exit 1 |
| 登录态自检(注入后) | ⚠️ WARN | 127s | exit 1 |
| Google 登录 | ⏱️ TIMEOUT | 127s | 超过 120s 上限被终止 |
| 行为回归 [smoke] | ❌ FAIL | 759s | 3 条 flow 失败（脚本退出码为 0，见 REPORT.md） |
| 性能体检 | ✅ OK | 1595s |  |

## 崩溃/ANR

- 🔴 崩溃 @ `性能体检`（2026-08-21 03:43:19）：进程消失（start pid 4739，无需日志解析）

## 行为回归

通过 **0** / 失败 **3**（来源 `REPORT.md`）

失败 flow：
- `smoke-app-launch`
- `smoke-logged-in-shell`
- `smoke-onboarding`

报告：`/Users/dino/dino-quality/android/reports/e2e-run/20260821-030400/REPORT.md`

## Flaky 滚动台账

- 窗口：最近 **14** 天，样本 **7** 次（含本次）
- 当前 flaky：**0**（新增 0 / 持续 0 / 消失 0）
- 本窗口内无 flaky 变化

## 性能体检

| 维度 | 状态 | 耗时 |
|---|---|---|
| 应用状态自检 | ⚠️ WARN | 0s |
| 静态扫描 | ✅ OK | 56s |
| APK体积 | ⚠️ WARN | 2s |
| 内存+Heap | ✅ OK | 137s |
| Shark泄漏 | ✅ OK | 33s |
| Perfetto断言 | ⚠️ WARN | 137s |
| 页面归因[chat] | ⚠️ WARN | 91s |
| 页面归因[class] | ⚠️ WARN | 90s |
| 页面归因[explore] | ⚠️ WARN | 90s |
| 页面归因[play] | ⚠️ WARN | 90s |
| Flashlight | ⚠️ WARN | 864s |

| 指标 | 本次 | 上次 | 变化 |
|---|---|---|---|
| APK | 67.6MB | - | 首次 |
| jank | 未采到 | - | 未采到 |
| 主线程最长任务 | 28ms | - | 首次 |
| 内存增长 | 0MB | - | 首次 |
| 泄漏 | 1个 | - | 首次 |
| FPS | 0 | - | 首次 |
| gfxinfo jank | 未采到 | - | 未采到 |
| gfxinfo 帧数 | 未采到 | - | 未采到 |
| Chat CPU峰值 | 未采到 | - | 盲区，不能判定 |
| Class CPU峰值 | 未采到 | - | 盲区，不能判定 |
| Explore CPU峰值 | 未采到 | - | 盲区，不能判定 |
| Play CPU峰值 | 未采到 | - | 盲区，不能判定 |

> 样本不足 n=0，噪声带用实测 floor。|δ|≤噪声带记波动；相对跳变过大记条件可能不同。非门禁。
- 门禁 未判定（应用状态 `welcome`，指标不具代表性）

报告：`/Users/dino/dino-quality/android/performance-toolkit/performance-results/run-all/20260821_031644/SUMMARY.md`

## 产物

- 本次跑批：`/Users/dino/dino-quality/reports/quality-run/20260821-023003/`（run.log、build.log、e2e.log、perf.log）
- 趋势台账：`reports/perf-baseline/perf-trend.csv`


---

## 🤖 AI 根因分析

| 维度 | 结论 |
|---|---|
| 类型 | `test_asset` |
| 置信度 | high |
| 根因 | Google 登录超时导致登录态注入失败，所有依赖登录态的 smoke 用例因未登录而无法通过，属于测试资产与账号环境不匹配问题。 |
| 建议 | 人工在设备 8KE5T19802015934 上完成一次 Google 登录并导出登录态，或改用免登录的 smoke 用例；同时检查 Google 账号是否可用，避免风控拦截。 |

**摘要**：本次跑批因 Google 登录超时导致登录态注入失败，3 条 smoke 用例（app-launch、logged-in-shell、onboarding）均因未登录而失败，属于测试资产/环境问题，非业务回归。建议人工登录一次设备并导出登录态，或改用免登录用例。

_由 DeepSeek (deepseek-chat) 自动生成，仅供参考_
