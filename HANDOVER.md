# 接手说明（给电脑上的自己）

仓库：`https://github.com/yanyuchen-resume/dsh-Go`（私有仓库，需用你的 GitHub 账号 clone）

这个仓库是 **官方 ccch1mneyyy/dsh-Go v1.3.0 的非官方修改版**，来源与署名见 `NOTICE.md`。
本文件说明当前状态、卡在哪、以及怎么继续。

## 一、当前状态（2026-11-22 下午）

| 项 | 状态 |
|---|---|
| 基底 | 官方 v1.3.0 完整代码 |
| 品牌替换 | 已换启动图标（`mipmap-*/ic_launcher*.png`）、应用名 `Harness Pocket`、`NOTICE.md` + `README.self.md` |
| 包名 | 仍是 `io.github.ccch1mneyyy.dsh_app`（**未改**，因为 670 个文件里有包名引用，改之前要先排查 manifest 里的 authority/组件名） |
| 签名 | 自建证书（不在仓库里，注意保管） |
| Dart 层 | 编译通过 ✅ |
| Kotlin 层 | 已回退为官方实现，社区自动化代码已移除 → 应该能过 |
| 社区自动化能力 | **当前不在代码里**。Dart 侧 `automation_*.dart` 文件仍在但对应通道不存在，运行时显示「自动化不可用」 |

## 二、为什么会走到这一步

原计划是把社区版 `puppy-bai/dsh-Go` v1.0.19 的手机自动化（坐标手势、`run` 多步、`task` 自动任务、`send` 发送范围）合并进官方 1.3.0。三方合并做完了，得到 38 个冲突文件 / 118 个冲突块，但 **Kotlin 层两套 API 对不上**，共 60+ 处错误：

```
CompanionMapPlatform.kt  Unresolved reference 'SystemHandlers'        官方新增类，社区版没有
SystemImePrompt.kt       PromptButton is private in PhonePromptUi     可见性冲突
NotificationGate.kt      Unresolved 'overlay_allow_task_no_ask'       官方 strings 无此键
PhoneAndroidShell.kt     Unresolved 'onlyKeys' / 'strictText'         社区版 SystemApi 才有
PhoneControl.kt          Unresolved 'beginTask' / 'interrupt'         官方 PhoneCore 的新方法
PhoneDisplaySelection.kt Unresolved 'TaskControls' / 'setPausedIfCurrent'
PhonePrompt.kt           Unresolved 'visualScope' / 'allowRound'
PhonePromptUi.kt         Unresolved 'ALLOW_TASK'
PhonePrompts.kt          Unresolved 'owner' / 'ALLOW_VISUAL_FOR_TASK'
```

根因：社区版改的是 1.0.13 的 Kotlin 层，官方 1.3.0 在这层大改（语音、后台预览、确认三档、悬浮条）。要真正合并，得按官方新 API 重写社区那批类的调用点，属于几天的人工工作，手机上没法编译验证，只能靠 CI 一轮 20 分钟。

## 三、电脑上怎么跑起来

```bash
git clone git@github.com:yanyuchen-resume/dsh-Go.git
cd dsh-Go/app
flutter pub get

# 签名：把手机上的 dshgo-self.jks 拷到 app/android/app/，再写 app/android/key.properties
# storeFile=self-release.jks
# storePassword=...
# keyAlias=dshgo-self
# keyPassword=...

python3 tool/l10n_merge.py      # 生成本地化
flutter build apk --release --flavor arm64 --target-platform android-arm64 \
  --build-name 1.3.1 --build-number <自定> \
  --dart-define=DSH_APP_VERSION=1.3.1+<自定>
```

环境版本必须对齐官方 CI：**Flutter 3.47.7、JDK 17、Android SDK 36**。
官方 CI 参考：`.github/workflows/ci.yml`；本项目的发版流水线：`.github/workflows/build-self.yml`。

## 四、要继续合并该怎么做

1. 先把社区版代码拉下来对照：
   `git clone https://github.com/puppy-bai/dsh-Go.git`
2. 社区版的自动化改动集中在 `app/android/.../phone/` 下的 `Automation*.kt`、`RoutinePlan.kt`、`TextSelector.kt`、`SendScope.kt`、`a11y/ElementSelector.kt`、`a11y/VisualFrame.kt`（这些是新增文件，接入成本低）
3. 难点是它们要挂到官方 `PhoneCore` / `PhoneToolCatalog` / `SafetyGate` / `MainScreenDriver` 上，而这三个文件官方也改过。**不要整文件覆盖**，按函数逐个接
4. 每接一个，本地 `flutter build apk` 验证一次，比 CI 快十倍
5. 冲突细节见仓库 `docs/merge/冲突报告.md`

## 五、合规边界（发布前必查）

- 代码 AGPL-3.0，改和发都行，但必须开源 + 保留原作者署名（`NOTICE.md` 已写好）
- **上游品牌素材不在 AGPL 范围内**：DSH Go 的鱼尾标、Deepy 小鲸鱼、小粉鱼、鲸娘、DeepSeek 图标、`docs/assets` 里的展示图。发布版必须全部替换
- 当前图标是自己做的角色头像（AI 生成的立绘抠图），与上游鲸娘相似度偏高，公开前建议再拉开差异（发色/服装/名字）
- 包名未改，与官方相同，装在同一位用户手机上会冲突；对外发布前改成自己的

## 六、还缺的东西

- `dshgo-self.jks` + 密码：在手机 `下载/DSH-Go/selfbuild/keystore/`，需要单独传到电脑。**丢了就无法覆盖升级**
- Actions Secrets 已配好，若改走电脑本地构建，可忽略
