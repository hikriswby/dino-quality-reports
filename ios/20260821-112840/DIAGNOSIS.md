# AI 审查（只读，不改红黄绿）

本次卡片颜色仍以 `summary.json` 为准。下文是给研发的初步归因，不是门禁，也不是补丁。

- 模型：`deepseek-chat` · 工具轮次：12 · 平台：ios
- 跑批 SHA：`5b83fc07` · 工作区 HEAD：android `490fba08` / ios `5b83fc07`

## 发现

### 1. 页面级写盘 70-115MB/页，疑似 Lottie 缓存 touch 与原子写放大（perf · 置信度 medium）

chat/class/explore/play 四页写盘分别达 112/89.9/114.9/70MB，远超冷启动 11.5MB。DinoFileCacheStore.lookup 每次命中都 touchAsync 改文件 modificationDate（setAttributes 触发元数据写盘），且 store 用 .atomic 写 tmp+rename 双写放大。LottieFileCache 容量 100MB，页面切换时大量 Lottie 命中+trim 可能造成高频小写。

- 打开：`Packages/DinoEnglishResource/Sources/DinoEnglishResource/Core/DinoFileCacheStore.swift` L86-L96 — lookup 每次命中都 touchAsync 写 modificationDate
- 打开：`Packages/DinoEnglishResource/Sources/DinoEnglishResource/Core/DinoFileCacheStore.swift` L150-L166 — writeAtomically 先写 tmp 再 rename，双写放大
- 打开：`dino-english-ios/Common/LottieCache/LottieFileCache.swift` L42-L44 — init 时 trimToCapacityInBackground，页面切换可能频繁触发
- 证据（artifact）：`perf/page-chat.metrics.json` — disk_write_mb_total=112.0
- 证据（artifact）：`perf/page-explore.metrics.json` — disk_write_mb_total=114.9
- 证据（code）：`Packages/DinoEnglishResource/Sources/DinoEnglishResource/Core/DinoFileCacheStore.swift` — L86-L96 touchAsync 每次 lookup 都写
- 建议：统计 touchAsync 调用频率与单次 setAttributes 写量；评估是否可降频（如仅当文件超过 N 秒未 touch 才更新），或改用内存 LRU 记录访问序、磁盘只保留写入时间。
- 建议看的人：`ios-resource-cache`

### 2. 泄漏 126 根链，root 为 CFString 384B（leak · 置信度 medium）

memgraph 报 leak_count=126、leak_bytes=30352，root 为单个 CFString 0x126a58000 仅 384B。top_classes 显示 CFString 53172 个实例占 3.6MB，malloc 18102 个占 2.6MB。单根链 384B 规模极小，126 个泄漏可能为同一 CFString 的多次引用或系统级缓存，非业务对象泄漏。

- 打开：`perf/memgraph.log` L1-L12 — leak_count=126, root_desc=CFString 0x126a58000
- 证据（artifact）：`perf/memgraph.metrics.json` — leak_count=126, root_retained_mib=0.000366
- 证据（artifact）：`perf/memgraph.log` — root_desc=CFString 0x126a58000
- 建议：用 Instruments Leaks 定位 CFString 0x126a58000 的分配栈；若为 URLSession/系统缓存可忽略，若为业务字符串需查持有者。
- 建议看的人：`ios-perf`

### 3. LottieFileCache URLSession 闭包未全部弱引用（leak · 置信度 medium）

静态扫描命中 LottieFileCache.swift 6 处 URLSession.dataTask 闭包，其中 L105 的 loadJSONAnimation 降级路径闭包未捕获 [weak self]，可能造成 LottieFileCache 单例被闭包强持有（单例本身无泄漏风险，但闭包内 self 若被外部持有则形成环）。其余 5 处均已有 [weak self]。

- 打开：`dino-english-ios/Common/LottieCache/LottieFileCache.swift` L105-L115 — 降级路径 dataTask 闭包未 [weak self]
- 证据（artifact）：`perf/static.log` — L105 命中 URLSession.shared.dataTask 无 weak self
- 建议：L105 降级路径补 [weak self]；确认该路径是否会被 TeacherResourcePreloader 等长生命周期对象持有。
- 建议看的人：`ios-lottie`

## 还缺的数据

- **页面级写盘的具体文件分布与调用栈**：70-115MB/页的写盘量无法仅从 metrics 判断是 touch 元数据写还是 Lottie 文件原子写放大，需要知道写盘热点文件路径
  - 怎么补：在页面跑批时用 fs_usage 或 DTrace 采样 write/rename 系统调用，按文件路径聚合统计
- **CFString 0x126a58000 的分配栈**：126 个泄漏根链指向同一 CFString，需确认是系统缓存还是业务字符串泄漏
  - 怎么补：用 Instruments Leaks 打开 leaks.trace 产物，查看该地址的 allocation callstack

## 不在本轮展开

- CPU 峰值 138% 为冷启动一次性峰值，均值 21.2% 在噪声带内，不展开
- 内存峰值 89.4MiB 与 footprint 209.9MiB 均在正常范围，不展开
- Hitch 0.0ms/s ×0 帧率全绿，不展开
- E2E 17/17 全绿，无行为回归

## 给研发

本轮全绿但写盘信号明显：四页各 70-115MB 写盘值得关注。优先查 DinoFileCacheStore.touchAsync 的调用频率——每次 lookup 都 setAttributes 改 modificationDate，页面内大量 Lottie 命中会放大成高频元数据写。另外 writeAtomically 的 tmp+rename 双写对 100MB 容量缓存也是双倍 IO。建议先做一次 fs_usage 采样确认写盘热点再决定优化方向。泄漏 126 根链规模极小（384B），大概率是系统 CFString 缓存，不必紧张，但建议用 Instruments 确认分配栈。

---

_质量机只读审查专家生成。fail-open，未改 `summary.json`。本地 trace：`ai-review-trace.json`（不发布）。_
