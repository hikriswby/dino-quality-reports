# Dino English iOS 质量跑批 · 20260818-200957

## 给开发（先看这个）

- **行为回归失败** · `home-chat-hub` · `{'type': 'assertion', 'message': "Element not visible: text='课堂' (cause: context deadline exceeded: no elements match selector)"}`
- **行为回归失败** · `home-chat-hub` · `{'type': 'network', 'message': 'Failed to create session for app: com.prime.dino.english (cause: failed to create session: Post "http://localhost:8266/session":`

完整证据见 [DEV.md](./DEV.md)。


- 结论：🟢 GREEN
- 平台/设备：真机 iPhone X (真机) · 版本 1.5.2
- 分支/提交：dev-1.5.2 @ 5b83fc07 · 套件 logged-in
- 行为回归：通过 17 / 失败 0
- 性能体检：9/11 项采集成功（1 项降级）（1 项跳过）

## 步骤

| 步骤 | 状态 | 耗时 | 备注 |
| --- | --- | --- | --- |
| 前置检查 | OK | 0 | 真机 2b7c46ad546d61859b0303e9a8bccf3dee779d5e |
| git fetch | OK | 6 |  |
| checkout dev-1.5.2 | OK | 2 |  |
| 构建装机 | SKIP | 0 | 真机复用已装包 |
| 登录态自检 | OK | 18 |  |
| 行为回归 第1轮 | OK | 0 | 通过 17 / 失败 0 |
| 行为回归 | OK | 0 | 通过 17 / 失败 0 |
| 崩溃检查 | OK | 0 | 无新增 |
| 性能体检·静态扫描 | OK | 2 |  |
| 性能体检·体积 | OK | 2 |  |
| 性能体检·内存 | OK | 77 |  |
| 性能体检·泄漏 | OK | 76 |  |
| 性能体检·内存图 | SKIP | 0 | 真机进程无法纯 CLI 附加（需 Instruments/LLDB 会话） |
| 性能体检·CPU | OK | 73 | CPU峰值107.7%/均值22.2% 内存峰值82.8MiB 写盘5.5MB |
| 性能体检·页面[chat] | OK | 103 | CPU峰值20.4%/均值1.5% 内存峰值45.9MiB 写盘77.9MB |
| 性能体检·页面[class] | OK | 103 | CPU峰值11.8%/均值1.1% 内存峰值46.0MiB 写盘48.9MB |
| 性能体检·页面[explore] | OK | 101 | CPU峰值7.8%/均值1.2% 内存峰值46.0MiB 写盘65.2MB |
| 性能体检·页面[play] | WARN | 319 | exit 1 |
| 性能体检·帧率 | OK | 267 |  |

