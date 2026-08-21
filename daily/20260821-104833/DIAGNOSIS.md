# AI 审查（只读，不改红黄绿）

本次卡片颜色仍以 `summary.json` 为准。下文是给研发的初步归因，不是门禁，也不是补丁。

- 模型：`deepseek-chat` · 工具轮次：12 · 平台：android
- 跑批 SHA：`490fba08` · 工作区 HEAD：android `490fba08` / ios `5b83fc07`

## 发现

### 1. WelcomeActivity 被 CredentialManager$GetCredentialTransport 持有导致 21.2MB 泄漏（leak · 置信度 high）

Shark 显示 GC Root 为 native 全局变量，android.credentials.CredentialManager$GetCredentialTransport.mContext 持有已销毁的 WelcomeActivity（mDestroyed=true），retaining 21.2MB/12766 对象。WelcomeActivity 在 merged login 模式下通过 AccountRoute.LOGIN_FRAGMENT_PATH 嵌入 LoginAccountFragment，该 Fragment 在 L889 调用 googleAuthManager.signIn(activity) 把宿主 Activity 传入 CredentialManager。CredentialManager 的 GetCredentialTransport 在授权流程结束后未释放对 Activity 的引用。

- 打开：`feature/account/impl/src/main/java/com/prime/dino/english/feature/account/impl/internal/GoogleAuthManagerImpl.kt` L64-L70 — CredentialManager.create(context) 使用 @ApplicationContext，但 signIn 传入的是 Activity，检查 GetCredentialTransport 生命周期
- 打开：`feature/account/impl/src/main/java/com/prime/dino/english/feature/account/impl/ui/login/LoginAccountFragment.kt` L889 — googleAuthManager.signIn(activity) 传入宿主 Activity，确认授权完成后是否有 cancel/释放
- 打开：`feature/onboarding/impl/src/main/java/com/prime/dino/english/feature/onboarding/impl/ui/welcome/WelcomeActivity.kt` L283-L306 — openMergedLoginFragment 嵌入登录 Fragment，确认 Activity 销毁时 Fragment 是否同步移除
- 证据（artifact）：`perf/shark/shark-leak-20260821_111829.txt` — CredentialManager$GetCredentialTransport.mContext -> WelcomeActivity (mDestroyed=true), 21.2MB
- 证据（code）：`feature/account/impl/src/main/java/com/prime/dino/english/feature/account/impl/internal/GoogleAuthManagerImpl.kt` — signIn(activity) 将 Activity 传入 CredentialManager
- 建议：检查 GoogleAuthManagerImpl 中 getCredentialWithUiLaunchTimeout 的 withTimeoutOrNull 超时后是否取消 GetCredentialRequest；确认 CredentialManager 在 Activity onDestroy 时是否有 cancel 机制；考虑在 LoginAccountFragment onDestroyView 时取消进行中的 signIn 协程。
- 建议看的人：`android-account`

### 2. ForeBackStatusUtils$1.delaySendRun 捕获 SplashActivity 导致 1.9MB 泄漏（leak · 置信度 high）

Shark 第二条链：FontsContract.sContext -> DinoEnglishApp.mActivityLifecycleCallbacks -> ArrayList[14] -> ForeBackStatusUtils$1 -> delaySendRun -> ExternalSyntheticLambda0.f$1 -> SplashActivity (mDestroyed=true)。ForeBackStatusUtils.java L99-L117 的 check(activity) 把传入的 activity 捕获进 delaySendRun lambda，该 lambda 被 handler.postDelayed 持有 1 秒。若 Activity 在 1 秒内销毁，lambda 仍持有已销毁 Activity。SplashActivity 是冷启动首屏，销毁后立即进入 WelcomeActivity，触发此竞态。

- 打开：`core/common/src/main/java/com/prime/dino/english/core/common/lifecycle/ForeBackStatusUtils.java` L99-L117 — check(activity) 将 activity 捕获进 delaySendRun，postDelayed 1 秒；Activity 销毁后 lambda 仍持有引用
- 打开：`app/src/main/java/com/prime/dino/english/splash/SplashActivity.kt` L1-L120 — 冷启动首屏，销毁后进入 WelcomeActivity，触发竞态
- 证据（artifact）：`perf/shark/shark-leak-20260821_111829.txt` — ForeBackStatusUtils$1.delaySendRun -> SplashActivity (mDestroyed=true), 1.9MB
- 证据（code）：`core/common/src/main/java/com/prime/dino/english/core/common/lifecycle/ForeBackStatusUtils.java` — delaySendRun 捕获 activity 且不判空
- 建议：delaySendRun 不应捕获具体 activity，改为在 run 时从 LISTENERS 或当前 resumed activity 获取；或 postDelayed 前用 WeakReference 包装 activity；或在 onActivityDestroyed 时 removeCallbacks(delaySendRun)。
- 建议看的人：`android-core-common`

### 3. LocalizationManager 主线程 runBlocking 读取 DataStore 有 ANR 风险（anr_risk · 置信度 medium）

静态扫描显示 18 处 runBlocking，其中 LocalizationManager.kt L74 在 initialize() 中主线程 runBlocking 读 DataStore，L99/L112 的 persistLocaleDisplayName/getStoredLocaleDisplayName 也在主线程 runBlocking。DataStore 读磁盘在冷启动路径上可能阻塞主线程。AuthTokenStorage.kt L61 和 AccountPreferenceStorage.kt L77/L347 也有 runBlocking，但需确认是否在主线程。

- 打开：`core/common/src/main/java/com/prime/dino/english/core/common/locale/LocalizationManager.kt` L74 — initialize() 主线程 runBlocking 读 DataStore
- 打开：`core/common/src/main/java/com/prime/dino/english/core/common/locale/LocalizationManager.kt` L99-L112 — persistLocaleDisplayName/getStoredLocaleDisplayName 主线程 runBlocking
- 打开：`core/common/src/main/java/com/prime/dino/english/core/common/auth/AuthTokenStorage.kt` L61 — 启动时同步恢复 runBlocking，确认是否主线程
- 证据（artifact）：`perf/static-analysis/static-analysis-report.md` — 18 处 runBlocking，LocalizationManager 3 处、AuthTokenStorage 1 处、AccountPreferenceStorage 2 处
- 建议：将 LocalizationManager.initialize 的 runBlocking 改为挂起函数或异步初始化；persistLocaleDisplayName/getStoredLocaleDisplayName 改为 suspend 或切 IO 线程。
- 建议看的人：`android-core-common`

### 4. APK 体积 67.8MB 触发 WARN，需确认是否含 debug 膨胀（perf · 置信度 medium）

APK 体积 67.8MB 触发 WARN。当前跑的是 debug 包，debug 包含调试符号、LeakCanary、Timber DebugTree 等膨胀，不能直接等同商店包体积。但需确认是否有异常增长（如新增大资源/so）。

- 证据（artifact）：`run/SUMMARY.md` — apk_mb=67.8, APK体积 WARN
- 建议：对比 release 包体积或历史 debug 包基线，确认 67.8MB 是否异常；检查是否有新增大 so/资源。
- 建议看的人：`android-build`

### 5. 内存增长 16MB/2.31% 触发 WARN，与两处泄漏相关（perf · 置信度 medium）

内存增长 16MB/2.31% 触发 WARN，与 Shark 发现的两处泄漏（21.2MB + 1.9MB）直接相关。泄漏的 WelcomeActivity 和 SplashActivity 在冷启动流程中创建后未释放，导致内存持续增长。修复泄漏后该指标应回落。

- 证据（artifact）：`run/SUMMARY.md` — mem_growth_mb=16.0, mem_growth_pct=2.31, 内存+Heap WARN
- 证据（artifact）：`perf/shark/shark-leak-20260821_111829.txt` — 两处泄漏共 retaining 23.1MB
- 建议：优先修复两处泄漏，修复后重跑验证内存增长是否回落。
- 建议看的人：`android-account`

## 还缺的数据

- **LoginAccountFragment 中 Google signIn 的调用上下文（协程作用域、是否在 onDestroy 时取消）**：判断 CredentialManager 泄漏是系统 API 行为还是调用方未取消协程导致
  - 怎么补：读 feature/account/impl/src/main/java/com/prime/dino/english/feature/account/impl/ui/login/LoginAccountFragment.kt L860-L910 附近代码
- **ForeBackStatusUtils 的 LISTENERS 中 RemoteLanguageCoordinator.foregroundListener 是否在 App 销毁时移除**：确认 ForeBackStatusUtils$1 泄漏是否还叠加了 listener 未移除的问题
  - 怎么补：检查 RemoteLanguageCoordinator 是否有对应的 removeListener 调用
- **AuthTokenStorage/AccountPreferenceStorage 中 runBlocking 是否在主线程执行**：静态扫描只报 runBlocking 数量，无法区分主线程/IO 线程
  - 怎么补：读 AuthTokenStorage.kt L61 和 AccountPreferenceStorage.kt L77/L347 的调用上下文

## 不在本轮展开

- SharedPreferences 76 处静态计数：盘点性质，不逐条展开
- Handler 83 处静态计数：盘点性质，不逐条展开
- Bitmap 19 处静态计数：盘点性质，不逐条展开
- Thread.sleep 5 处：RetryInterceptor 和 TTS 播放器中的 sleep 属预期行为，非本轮回归引入

## 给研发

本轮 E2E 17/17 全绿，但 Shark 发现两处真实泄漏（共 23.1MB），且内存增长 16MB 与泄漏直接相关。优先处理：1) WelcomeActivity 被 CredentialManager 持有（21.2MB，高优），检查 GoogleAuthManagerImpl 的 signIn 超时/取消逻辑；2) ForeBackStatusUtils.delaySendRun 捕获 SplashActivity（1.9MB），lambda 不应持有具体 Activity。两处都是冷启动路径，修复后内存增长指标应明显回落。LocalizationManager 的主线程 runBlocking 是潜在 ANR 风险，建议后续改为异步。APK 体积 WARN 需对比 release 包确认。

---

_质量机只读审查专家生成。fail-open，未改 `summary.json`。本地 trace：`ai-review-trace.json`（不发布）。_
