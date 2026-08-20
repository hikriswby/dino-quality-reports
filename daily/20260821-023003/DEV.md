# 给开发

每条都尽量回答：类型、新/旧、打开哪、建议动作。
完整证据在 `evidence/`。

## 泄漏

- 类型：存量（Shark APPLICATION LEAK，未超门禁也会列出，因为能修）
- 泄漏对象：`com.prime.dino.english.splash.SplashActivity`
- 建议：先看引用链里 `Leaking: YES` 的类；若被 `ForeBackStatusUtils.delaySendRun` 持有，改闭包不要捕获 Activity。

- 打开：`core/common/src/main/java/com/prime/dino/english/core/common/lifecycle/ForeBackStatusUtils.java`
- 泄漏类路径：`app/src/main/java/com/prime/dino/english/splash/SplashActivity.kt`

```
┬───
│ GC Root: System class
│
├─ android.provider.FontsContract class
│    Leaking: NO (DinoEnglishApp↓ is not leaking and a class is never leaking)
│    ↓ static FontsContract.sContext
├─ com.prime.dino.english.DinoEnglishApp instance
│    Leaking: NO (Application is a singleton)
│    mBase instance of android.app.ContextImpl
│    ↓ Application.mActivityLifecycleCallbacks
│                  ~~~~~~~~~~~~~~~~~~~~~~~~~~~
├─ java.util.ArrayList instance
│    Leaking: UNKNOWN
│    Retaining 2.5 MB in 1177 objects
│    ↓ ArrayList[14]
│               ~~~~
├─ com.prime.dino.english.core.common.lifecycle.ForeBackStatusUtils$1 instance
│    Leaking: UNKNOWN
│    Retaining 2.5 MB in 1163 objects
│    ↓ ForeBackStatusUtils$1.delaySendRun
│                            ~~~~~~~~~~~~
├─ com.prime.dino.english.core.common.lifecycle.ForeBackStatusUtils$1$$ExternalSyntheticLambda0 instance
│    Leaking: UNKNOWN
│    Retaining 2.5 MB in 1161 objects
│    f$1 instance of com.prime.dino.english.splash.SplashActivity with mDestroyed = true
│    ↓ ForeBackStatusUtils$1$$ExternalSyntheticLambda0.f$1
│                                                      ~~~
╰→ com.prime.dino.english.splash.SplashActivity instance
​     Leaking: YES (Activity#mDestroyed is true)
​     Retaining 2.5 MB in 1160 objects
​     mApplication instance of com.prime.dino.english.DinoEnglishApp
​     mBase instance of androidx.appcompat.view.ContextThemeWrapper
```

## 行为回归失败

### `smoke-app-launch`

- 类型：产品或用例契约
- 断言：`Assertion is false: id: .*btn_get_started.* is visible`
- 建议：先对照断言里的 id/文案，确认是页面改了还是产品 bug；改 `maestro/` 下对应 yaml 或对应页面。

- flow 文件：`/Users/dino/dino-quality/android/maestro/smoke/smoke-app-launch.yaml`
- 日志：[evidence/smoke-app-launch.log](evidence/smoke-app-launch.log)

![smoke-app-launch](evidence/smoke-app-launch-0.png)

- UI Dump：[evidence/smoke-app-launch-hierarchy.json](evidence/smoke-app-launch-hierarchy.json)

```

Waiting for flows to complete...
[Failed] smoke-app-launch (24s) (Assertion is false: id: .*btn_get_started.* is visible)

1/1 Flow Failed

```

### `smoke-logged-in-shell`

- 类型：产品或用例契约
- 断言：`Assertion is false: id: .*tab_bar.* is visible`
- 建议：先对照断言里的 id/文案，确认是页面改了还是产品 bug；改 `maestro/` 下对应 yaml 或对应页面。

- flow 文件：`/Users/dino/dino-quality/android/maestro/smoke/smoke-logged-in-shell.yaml`
- 日志：[evidence/smoke-logged-in-shell.log](evidence/smoke-logged-in-shell.log)

![smoke-logged-in-shell](evidence/smoke-logged-in-shell-0.png)

- UI Dump：[evidence/smoke-logged-in-shell-hierarchy.json](evidence/smoke-logged-in-shell-hierarchy.json)

```

Waiting for flows to complete...
[Failed] smoke-logged-in-shell (1m 16s) (Assertion is false: id: .*tab_bar.* is visible)

1/1 Flow Failed

```

### `smoke-onboarding`

- 类型：产品或用例契约
- 断言：`Assertion is false: id: .*btn_get_started.* is visible`
- 建议：先对照断言里的 id/文案，确认是页面改了还是产品 bug；改 `maestro/` 下对应 yaml 或对应页面。

- flow 文件：`/Users/dino/dino-quality/android/maestro/smoke/smoke-onboarding.yaml`
- 日志：[evidence/smoke-onboarding.log](evidence/smoke-onboarding.log)

![smoke-onboarding](evidence/smoke-onboarding-0.png)

- UI Dump：[evidence/smoke-onboarding-hierarchy.json](evidence/smoke-onboarding-hierarchy.json)

```

Waiting for flows to complete...
[Failed] smoke-onboarding (24s) (Assertion is false: id: .*btn_get_started.* is visible)

1/1 Flow Failed

```

## 崩溃

- stage `性能体检`：`进程消失（start pid 4739，无需日志解析）`

## 大图资源（存量，非本次回归）

可压缩/降分辨率；不作为今晚 ALERT。

| KB | 尺寸 | 路径 |
|---|---|---|
| 829 | 2436x1125 | `feature/home/impl/src/main/res/drawable-xxhdpi/home_bg_scene.webp` |
| 227 | 438x660 | `feature/home/impl/src/main/res/drawable-nodpi/home_welcome_gift_dino.webp` |
| 246 | 496x520 | `feature/home/impl/src/main/res/drawable-nodpi/home_welcome_gift_reward_skin.webp` |
| 246 | 496x520 | `feature/home/impl/src/main/res/drawable-nodpi/home_welcome_gift_reward_figure.webp` |
| 310 | 1716x969 | `feature/home/impl/src/main/res/drawable-nodpi/home_welcome_gift_dialog_background.webp` |
| 1527 | 3072x2295 | `feature/home/impl/src/main/res/drawable-sw600dp-xxhdpi/home_bg_scene.webp` |
| 445 | 2048x1536 | `feature/dinoclass/impl/src/main/res/drawable-sw600dp-nodpi/dino_class_topic_lesson_bg.webp` |
| 270 | 2436x1125 | `feature/dinoclass/impl/src/main/res/drawable-nodpi/dino_class_topic_lesson_bg.webp` |

清单：[big-images.txt](evidence/big-images.txt)

## 采集盲区（≠ 产品健康）

未采到的页面不能判定，也不能用整机指标代替。

- Chat CPU峰值 **未采到**（WARN，不能判定该页；冷启动/整机 CPU 不能代替）
- Class CPU峰值 **未采到**（WARN，不能判定该页；冷启动/整机 CPU 不能代替）
- Explore CPU峰值 **未采到**（WARN，不能判定该页；冷启动/整机 CPU 不能代替）
- Play CPU峰值 **未采到**（WARN，不能判定该页；冷启动/整机 CPU 不能代替）

## 跑批本身（质量机）

- 输入法保障
- 页面性能盲区：Chat、Class、Explore、Play

上质量机看 `/Users/dino/dino-quality/reports/quality-run/20260821-023003/run.log`。


---

- **存量泄漏** · `com.prime.dino.english.splash.SplashActivity` 被持有 · 约 2.5 MB · 证据 [shark-leak.txt](evidence/shark-leak.txt) · 打开 `core/common/src/main/java/com/prime/dino/english/core/common/lifecycle/ForeBackStatusUtils.java`
- **行为回归失败** · `smoke-app-launch` · `Assertion is false: id: .*btn_get_started.* is visible` · flow `/Users/dino/dino-quality/android/maestro/smoke/smoke-app-launch.yaml`
- **行为回归失败** · `smoke-logged-in-shell` · `Assertion is false: id: .*tab_bar.* is visible` · flow `/Users/dino/dino-quality/android/maestro/smoke/smoke-logged-in-shell.yaml`
- **行为回归失败** · `smoke-onboarding` · `Assertion is false: id: .*btn_get_started.* is visible` · flow `/Users/dino/dino-quality/android/maestro/smoke/smoke-onboarding.yaml`
- **崩溃** @ `性能体检` · `进程消失（start pid 4739，无需日志解析）`
- **采集盲区** · Chat、Class、Explore、Play 页未采到，该页性能未知（不是产品失败，也不能当健康）。

完整证据与引用链见 [DEV.md](./DEV.md)。
