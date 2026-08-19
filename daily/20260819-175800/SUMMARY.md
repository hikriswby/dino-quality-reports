# 质量跑批 · 20260819-175800

## 给开发（先看这个）

- 本轮没有需要改产品代码的项。健康度看卡片「本次 vs 上次」。

完整证据与引用链见 [DEV.md](./DEV.md)。

## 结论：🔴 RED

- 告警等级：`BLOCK`
- 等级定义：`BLOCK`=需要阻断/立刻处理，`ALERT`=可继续但需关注，`INFO`=通过

- 跑批中断：行为回归 [smoke]

| 项 | 值 |
|---|---|
| 分支 / 提交 | `dev/feature/0815` @ `1b674930` |
| 应用版本 | 1.5.3 |
| 设备 | Pixel 6 (`19011FDF6003CU`) |
| Maestro 套件 | `smoke` |
| 跑批档位 | `nightly`（每晚全量档：全量回归 + 性能体检） |
| 设备联网 | 是 |

## 跑批步骤

| 步骤 | 状态 | 耗时 | 备注 |
|---|---|---|---|
| 前置检查 | ✅ OK | 0s | Pixel 6 / Android 17 |
| 输入法保障 | ⚠️ WARN | 0s | 真机 inputText 注入可能失效 |
| 编译装机 | ⏭️ SKIP | 0s | --skip-build |
| 登录态自检 | ⚠️ WARN | 4s | exit 1 |
| Google 登录 | ⚠️ WARN | 4s | exit 1 |
| 行为回归 [smoke] | ❌ FAIL | 0s | exit 127 |
| 性能体检 | ⚠️ WARN | 0s | exit 127 |

## 崩溃/ANR

🟢 未捕获到崩溃/ANR（行为回归 + 性能体检全程）。

## 行为回归

未执行或未取到结果。

## Flaky 滚动台账

- 窗口：最近 **14** 天，样本 **2** 次（含本次）
- 当前 flaky：**0**（新增 0 / 持续 0 / 消失 0）
- 本窗口内无 flaky 变化

## 性能体检

未执行或未取到结果。

## 产物

- 本次跑批：`/Users/dino/dino-quality/reports/quality-run/20260819-175800/`（run.log、build.log、e2e.log、perf.log）
- 趋势台账：`reports/perf-baseline/perf-trend.csv`


---

## 🤖 AI 根因分析

| 维度 | 结论 |
|---|---|
| 类型 | `infra_env` |
| 置信度 | high |
| 根因 | 跑批脚本引用的 maestro/run.sh 文件不存在，导致行为回归和性能体检步骤以 exit 127 失败，跑批中断。 |
| 建议 | 检查 nightly-quality-android.sh 脚本中第 318 行引用的 maestro/run.sh 路径是否存在，确认脚本文件是否被误删或路径配置错误，并修复后重新运行跑批。 |

**摘要**：本次跑批因脚本文件缺失而中断：nightly-quality-android.sh 引用的 maestro/run.sh 不存在，导致行为回归和性能体检步骤以 exit 127 失败。属于基础设施问题，非业务代码回归，建议修复脚本路径后重跑。

_由 DeepSeek (deepseek-chat) 自动生成，仅供参考_
