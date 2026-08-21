# 质量跑批 · 20260821-104833

## 给开发（先看这个）

- **存量泄漏** · `com.prime.dino.english.feature.onboarding.impl.ui.welcome.WelcomeActivity` 被持有 · 约 21.2 MB · 证据 [shark-leak.txt](evidence/shark-leak.txt) · 打开 `core/common/src/main/java/com/prime/dino/english/core/common/lifecycle/ForeBackStatusUtils.java`

完整证据与引用链见 [DEV.md](./DEV.md)。

## 结论：🟡 YELLOW

- 告警等级：`ALERT`
- 等级定义：`BLOCK`=需要阻断/立刻处理，`ALERT`=可继续但需关注，`INFO`=通过

- 远端 SHA 未变，本轮跳过全量（只探登录态）
- 有降级步骤：登录态自检、登录态注入、登录态自检(注入后)
- 性能维度有风险项：APK体积、内存+Heap

| 项 | 值 |
|---|---|
| 分支 / 提交 | `dev/feature/0821` @ `490fba08` |
| 应用版本 | 1.5.4 |
| 设备 | Pixel 6 (`19011FDF6003CU`) |
| Maestro 套件 | `all-logged-in` |
| 跑批档位 | `nightly`（每晚全量档：全量回归 + 性能体检） |
| 设备联网 | 是 |

## 跑批步骤

| 步骤 | 状态 | 耗时 | 备注 |
|---|---|---|---|
| 前置检查 | ✅ OK | 0s | Pixel 6 / Android 17 |
| 输入法保障 | ⚠️ WARN | 0s | 真机 inputText 注入可能失效 |
| 同步代码 | ✅ OK | 0s | origin/dev/feature/0821 无新提交 |
| 编译 debug 包 | ✅ OK | 50s |  |
| 安装到设备 | ✅ OK | 4s |  |
| 登录态自检 | ⚠️ WARN | 103s | exit 1 |
| 登录态注入 | ⚠️ WARN | 10s | exit 1 |
| 登录态自检(注入后) | ⚠️ WARN | 103s | exit 1 |
| Google 登录 | ✅ OK | 43s |  |
| 登录态自检(Google登录后) | ✅ OK | 12s |  |
| 行为回归 [all-logged-in] | ✅ OK | 1167s |  |
| 性能体检 | ✅ OK | 771s |  |

## 崩溃/ANR

🟢 未捕获到崩溃/ANR（行为回归 + 性能体检全程）。

## 行为回归

通过 **17** / 失败 **0**（来源 `REPORT.md`）

报告：`/Users/dino/dino-quality/android/reports/e2e-run/20260821-105602/REPORT.md`

## Flaky 滚动台账

- 窗口：最近 **14** 天，样本 **8** 次（含本次）
- 当前 flaky：**0**（新增 0 / 持续 0 / 消失 0）
- 本窗口内无 flaky 变化

## 性能体检

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

| 指标 | 本次 | 上次 | 变化 |
|---|---|---|---|
| APK | 67.8MB | - | 首次 |
| jank | 0.7% | - | 首次 |
| 主线程最长任务 | 0ms | - | 首次 |
| 内存增长 | 16MB | - | 首次 |
| 泄漏 | 2个 | - | 首次 |
| FPS | 59 | - | 首次 |
| gfxinfo jank | 未采到 | - | 未采到 |
| gfxinfo 帧数 | 未采到 | - | 未采到 |

> 样本不足 n=0，噪声带用实测 floor。|δ|≤噪声带记波动；相对跳变过大记条件可能不同。非门禁。
- 门禁 通过

报告：`/Users/dino/dino-quality/android/performance-toolkit/performance-results/run-all/20260821_111536/SUMMARY.md`

## 产物

- 本次跑批：`/Users/dino/dino-quality/reports/quality-run/20260821-104833/`（run.log、build.log、e2e.log、perf.log）
- 趋势台账：`reports/perf-baseline/perf-trend.csv`


---

## 🤖 AI 根因分析

| 维度 | 结论 |
|---|---|
| 类型 | `infra_env` |
| 置信度 | high |
| 根因 | 登录态注入步骤失败，导致登录态自检未通过，但后续 Google 登录成功且行为回归全部通过，表明是测试环境/工具问题而非产品缺陷。 |
| 建议 | 检查登录态注入工具（如 ADB 或 Maestro 的 inputText）在 Android 17 上的兼容性，更新注入方式或使用备用方案；同时确认测试账号的登录态文件是否有效。 |

**摘要**：本次跑批整体通过，E2E 17/17 全绿，无崩溃和 flaky。但登录态注入步骤失败，导致登录态自检未通过，属于测试环境/工具问题（输入法注入在 Android 17 上可能失效），非产品缺陷。建议修复登录态注入工具后重跑。

_由 DeepSeek (deepseek-chat) 自动生成，仅供参考_
