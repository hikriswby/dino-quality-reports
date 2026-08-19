# 质量跑批 · 20260819-113932

## 给开发（先看这个）

- 本轮没有需要改产品代码的项。健康度看卡片「本次 vs 上次」。

完整证据与引用链见 [DEV.md](./DEV.md)。

## 结论：🟢 GREEN

- 告警等级：`INFO`
- 等级定义：`BLOCK`=需要阻断/立刻处理，`ALERT`=可继续但需关注，`INFO`=通过

- 全部通过，无保留项

| 项 | 值 |
|---|---|
| 分支 / 提交 | `dev/feature/0815` @ `1b674930` |
| 应用版本 | 1.5.3 |
| 设备 | Pixel 6 (`19011FDF6003CU`) |
| Maestro 套件 | `all-logged-in` |
| 跑批档位 | `nightly`（每晚全量档：全量回归 + 性能体检） |
| 设备联网 | 是 |

## 跑批步骤

| 步骤 | 状态 | 耗时 | 备注 |
|---|---|---|---|
| 前置检查 | ✅ OK | 0s | Pixel 6 / Android 17 |
| 输入法保障 | ⚠️ WARN | 0s | 真机 inputText 注入可能失效 |
| 编译 debug 包 | ✅ OK | 54s |  |
| 安装到设备 | ✅ OK | 4s |  |
| 登录态自检 | ✅ OK | 14s |  |
| 行为回归 [all-logged-in] | ✅ OK | 1167s |  |
| 性能体检 | ✅ OK | 727s |  |

## 崩溃/ANR

🟢 未捕获到崩溃/ANR（行为回归 + 性能体检全程）。

## 行为回归

通过 **17** / 失败 **0**（来源 `REPORT.md`）

报告：`reports/e2e-run/20260819-114106/REPORT.md`

## Flaky 滚动台账

- 窗口：最近 **14** 天，样本 **9** 次（含本次）
- 当前 flaky：**0**（新增 0 / 持续 0 / 消失 0）
- 本窗口内无 flaky 变化

## 性能体检

| 维度 | 状态 | 耗时 |
|---|---|---|
| 应用状态自检 | ✅ OK | 0s |
| 静态扫描 | ✅ OK | 2s |
| APK体积 | ⚠️ WARN | 1s |
| 内存+Heap | ✅ OK | 156s |
| Shark泄漏 | ✅ OK | 2s |
| Perfetto断言 | ✅ OK | 136s |
| 页面归因[chat] | ✅ OK | 49s |
| 页面归因[class] | ✅ OK | 50s |
| 页面归因[explore] | ✅ OK | 41s |
| 页面归因[play] | ✅ OK | 45s |
| Flashlight | ✅ OK | 241s |
| gfxinfo旁路 | ✅ OK | 0s |

| 指标 | 本次 | 上次 | 变化 |
|---|---|---|---|
| jank | 0.56% | - | 首次 |
| 主线程最长任务 | 30ms | - | 首次 |
| 内存增长 | 2MB | - | 首次 |
| 泄漏 | 0个 | - | 首次 |
| FPS | 59.6 | - | 首次 |

> 样本不足 n=0，噪声带用实测 floor。|δ|≤噪声带记波动；相对跳变过大记条件可能不同。非门禁。

报告：`/Users/dino/dino-english-android/performance-toolkit/performance-results/run-all/20260819_120039/SUMMARY.md`

## 产物

- 本次跑批：`/Users/dino/dino-english-android/reports/quality-run/20260819-113932/`（run.log、build.log、e2e.log、perf.log）
- 趋势台账：`reports/perf-baseline/perf-trend.csv`

