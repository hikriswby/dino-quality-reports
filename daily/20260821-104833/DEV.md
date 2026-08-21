# 给开发

每条都尽量回答：类型、新/旧、打开哪、建议动作。
完整证据在 `evidence/`。

## 泄漏

- 类型：存量（Shark APPLICATION LEAK，未超门禁也会列出，因为能修）
- 泄漏对象：`com.prime.dino.english.feature.onboarding.impl.ui.welcome.WelcomeActivity`
- 建议：先看引用链里 `Leaking: YES` 的类；若被 `ForeBackStatusUtils.delaySendRun` 持有，改闭包不要捕获 Activity。

- 打开：`core/common/src/main/java/com/prime/dino/english/core/common/lifecycle/ForeBackStatusUtils.java`
- 泄漏类路径：`feature/onboarding/impl/src/main/java/com/prime/dino/english/feature/onboarding/impl/ui/welcome/WelcomeActivity.kt`

```
┬───
│ GC Root: Global variable in native code
│
├─ android.credentials.CredentialManager$GetCredentialTransport instance
│    Leaking: UNKNOWN
│    Retaining 21.2 MB in 12833 objects
│    mContext instance of com.prime.dino.english.feature.onboarding.impl.ui.welcome.WelcomeActivity with mDestroyed = true
│    ↓ CredentialManager$GetCredentialTransport.mContext
│                                               ~~~~~~~~
╰→ com.prime.dino.english.feature.onboarding.impl.ui.welcome.WelcomeActivity instance
​     Leaking: YES (Activity#mDestroyed is true)
​     Retaining 21.2 MB in 12766 objects
​     mApplication instance of com.prime.dino.english.DinoEnglishApp
​     mBase instance of androidx.appcompat.view.ContextThemeWrapper

1933321 bytes retained by leaking objects
Signature: f5ef01864f361ac99918b9e88f300106048d999e
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
│    Retaining 1.9 MB in 1364 objects
│    ↓ ArrayList[14]
│               ~~~~
├─ com.prime.dino.english.core.common.lifecycle.ForeBackStatusUtils$1 instance
│    Leaking: UNKNOWN
│    Retaining 1.9 MB in 1352 objects
│    ↓ ForeBackStatusUtils$1.delaySendRun
│                            ~~~~~~~~~~~~
├─ com.prime.dino.english.core.common.lifecycle.ForeBackStatusUtils$1$$ExternalSyntheticLambda0 instance
│    Leaking: UNKNOWN
│    Retaining 1.9 MB in 1350 objects
│    f$1 instance of com.prime.dino.english.splash.SplashActivity with mDestroyed = true
│    ↓ ForeBackStatusUtils$1$$ExternalSyntheticLambda0.f$1
│                                                      ~~~
╰→ com.prime.dino.english.splash.SplashActivity instance
​     Leaking: YES (Activity#mDestroyed is true)
​     Retaining 1.9 MB in 1349 objects
​     mApplication instance of com.prime.dino.english.DinoEnglishApp
​     mBase instance of androidx.appcompat.view.ContextThemeWrapper
```

## 行为回归

通过，无失败断言需要修。

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

## 跑批本身（质量机）

- 输入法保障

上质量机看 `/Users/dino/dino-quality/reports/quality-run/20260821-104833/run.log`。


---

- **存量泄漏** · `com.prime.dino.english.feature.onboarding.impl.ui.welcome.WelcomeActivity` 被持有 · 约 21.2 MB · 证据 [shark-leak.txt](evidence/shark-leak.txt) · 打开 `core/common/src/main/java/com/prime/dino/english/core/common/lifecycle/ForeBackStatusUtils.java`

完整证据与引用链见 [DEV.md](./DEV.md)。
- AI 审查（不改色）：[DIAGNOSIS.md](./DIAGNOSIS.md)
