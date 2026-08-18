# 质量跑批 · 20260818-180216

## 结论：🟡 YELLOW

- 告警等级：`ALERT`
- 等级定义：`BLOCK`=需要阻断/立刻处理，`ALERT`=可继续但需关注，`INFO`=通过

- 有降级步骤：输入法保障

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
| 安装弹窗守护 | ✅ OK | 0s | pid 69143 |
| 编译装机 | ⏭️ SKIP | 0s | --skip-build |
| 登录态自检 | ✅ OK | 15s |  |
| 行为回归 [all-logged-in] | ✅ OK | 1186s |  |
| 性能体检 | ⏭️ SKIP | 0s | --skip-perf |

## 崩溃/ANR

🟢 未捕获到崩溃/ANR（行为回归 + 性能体检全程）。

## 行为回归

通过 **17** / 失败 **0**（来源 `REPORT.md`）

报告：`reports/e2e-run/20260818-180247/REPORT.md`

## Flaky 滚动台账

- 窗口：最近 **14** 天，样本 **6** 次（含本次）
- 当前 flaky：**0**（新增 0 / 持续 0 / 消失 0）
- 本窗口内无 flaky 变化

## 性能体检

未执行或未取到结果。

## 产物

- 本次跑批：`/Users/dino/dino-english-android/reports/quality-run/20260818-180216/`（run.log、build.log、e2e.log、perf.log）
- 趋势台账：`reports/perf-baseline/perf-trend.csv`


---

## 🤖 AI 根因分析

| 维度 | 结论 |
|---|---|
| 类型 | `test_asset` |
| 置信度 | high |
| 根因 | 输入法保障步骤警告，真机 inputText 注入可能失效，但所有 E2E 测试均通过，无实际失败。 |
| 建议 | 无需修复。输入法保障为已知的测试环境警告，不影响测试结果。建议在后续跑批中监控该警告是否频繁出现，若频繁出现可考虑优化输入法注入方案。 |

**摘要**：本次跑批整体通过，17 个 E2E 用例全部成功，无崩溃或性能问题。唯一警告是输入法保障步骤提示真机 inputText 注入可能失效，但未影响测试结果。建议持续关注该警告，无需立即处理。

_由 DeepSeek (deepseek-chat) 自动生成，仅供参考_
