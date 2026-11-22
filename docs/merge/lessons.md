# 这次踩过的坑（别再来一遍）

记录 2026-11-22 那次「把社区版手机自动化融合进官方 1.3.0 并自签名」的尝试。结论先行：**没做成，代价是一次本机环境的重装和一次数据恢复**。数据没丢，但这条路不值得这么走。

## 一、起点与决策失误

原本的需求只有一句话：想要一个「自己名字、自己图标、能自己控制更新」的版本。

实际做法：fork 官方 → 把社区版 `puppy-bai/dsh-Go`（基线 1.0.13）的手机自动化移植进官方 1.3.0 → 自建证书签名 → 卸载重装。

**失误在于顺序**：为了换图标和名字，先动了「签名 + 卸载重装」这个不可逆的环节，而不是先把代码合到能编译。签名不同 → Android 拒绝覆盖 → 必须卸载 → 卸载清空整个 Linux 环境 → 只能靠备份恢复。一次判断失误带来一次全量数据迁移风险，收益却只有一个图标。

## 二、七轮 CI 的时间线

每一轮约 20–25 分钟，全部靠 GitHub Actions（手机上没法本地编译）。

| 轮 | 错误量 | 根因 |
|---|---|---|
| 1 | 103 个 Dart 错误 | 把 `app/lib/phone/phone_channel.dart`、`phone_state.dart` 取了社区旧版，官方 1.3.0 的 UI 要的字段全缺 |
| 2 | 5 个 | 只剩 `app_rules_page.dart`（社区版页面）引用社区的 `visualPackages` / `revokeVisual` |
| 3 | Dart 全过，Kotlin 60+ | 两套 Kotlin API 对不上（详见下节） |
| 4 | 仍 60+ | 只回退了 `phone/` 一个目录，`companion/` 里还有社区文件 |
| 5 | 仍 520 行报错 | **推送漏了**：见「坑 4」 |
| 6 | **1 个** | 一个孤儿文件引用不存在的常量 `ACTION_COMPANION_TASK` |
| 7 | **通过** | 清掉 13 个孤儿文件后编出 APK（75.9 MB，签名指纹正确） |

## 三、具体技术坑

### 坑 1：Kotlin 层是两套不兼容的 API

社区版改的是 1.0.13 结构，官方 1.3.0 在同一层做了大改（语音、后台预览、确认三档、悬浮条）。典型冲突：

```
CompanionMapPlatform.kt  Unresolved 'SystemHandlers'          官方新增类，社区版没有
SystemImePrompt.kt       PromptButton is private               可见性冲突
NotificationGate.kt      Unresolved 'overlay_allow_task*'       官方 strings 无此键
PhoneAndroidShell.kt     Unresolved 'onlyKeys'/'strictText'     社区版 SystemApi 才有
PhoneControl.kt          Unresolved 'beginTask'/'interrupt'     官方 PhoneCore 新方法
PhonePrompt.kt           Unresolved 'visualScope'/'allowRound'
```

**不是加文件能解决的**，得按官方新签名重写社区那批类的调用点。60+ 处，无法本地验证（手机上没 Flutter/Android SDK），只能靠 CI 一轮一轮试。

### 坑 2：Dart 与 Kotlin 必须同版本配对

`phone_channel.dart` / `phone_state.dart` 是官方 UI 和原生通道之间的契约。只回退其中一个，就会「UI 要的字段在模型里不存在」。判断方法：`grep` 官方版本里有没有该成员（如 `previewFrames`、`anyTaskBusy`、`parallelism`）。

### 坑 3：社区版新增文件散落在多个目录

不止 `phone/`，`companion/`、`text/` 下也有。回退时只按一个目录做，必然漏。正确做法：

```bash
git diff --name-status <上游 v1.3.0 提交> HEAD -- app/android/app/src/main/kotlin | grep '^A'
```

列出所有新增文件，逐个判断——依赖已删实现的要一起删。

### 坑 4：用「相对基线的 diff」决定推送什么，会漏

这是第 5 轮失败的原因，也是最隐蔽的一个：

- 本地把 Kotlin 回退成官方版后，**与基线（官方 1.3.0）没有差异**
- 于是 diff 列表里不含这些文件 → 判定「不用推送」
- 结果远端还是社区版内容，CI 拿着旧代码编译，报的错和上一轮一模一样

正确做法：**按内容哈希比对远端与本地**，不一致就推。

```python
# 远端 tree 里每个 blob 的 sha 与本地 git hash-object 对比
# blob sha = sha1("blob <len>\0" + content)
```

那次补推了 30 个文件才真正生效。

### 坑 5：远端会留下「孤儿文件」

本地删了、远端没删的文件会继续参与编译。第 6 轮只剩一个错误，就是孤儿文件 `CompanionReminderAndroid.kt` 引用了不存在的常量。清理办法：远端文件列表 − 本地文件列表 = 孤儿，逐个删除。那次清了 13 个。

### 坑 6：本地化是分片生成的，别手改

`app/lib/l10n/app_*.arb` 和 `gen/` 都是生成物，真源在 `app/lib/l10n/parts/<area>_{zh,en}.arb`。构建前必须跑：

```bash
cd app && python3 tool/l10n_merge.py
```

否则新增的 `automation_*.arb` 里的键不会出现在生成物里，编译报缺 getter。

### 坑 7：签名与包名

- 社区版用的是 **Android Debug 证书**（SHA256 `48175d40…`），官方用发布证书，两者不能互相覆盖。跨签名 **必须卸载**，卸载会删对话、插件、`/root` 和整个 Linux 运行时。
- 自建证书 SHA256 `43:55:B8:9C:…:D7:A4`，有效期到 2054。**jks 和密码丢了就无法覆盖升级**。
- 包名还是 `io.github.ccch1mneyyy.dsh_app`，全仓有 670 个文件提到它（多数是 `package` 声明）。改 `applicationId` 前必须先排查 Manifest 里的 authority、无障碍服务组件名、快捷方式等硬编码。

### 坑 8：品牌素材不在 AGPL 范围内

上游 `ASSETS.md` 明确：源码 AGPL-3.0，但鱼尾标、Deepy 小鲸鱼、小粉鱼、鲸娘、DeepSeek 图标、`docs/assets` 展示图**都不在授权范围**，且写明「以修改版或衍生作品再发布时，请换成自己的名称和素材」。

所以：代码可以开源、可以署名二创；带上原图标和鲸鱼发出去就是侵权。自用测试无所谓，对外发布必须全换。

## 四、如果还要做，正确顺序

1. **先合代码，后动签名**。代码合到能编译、能出包，再谈换证书。
2. **一阶段只做「官方 1.3.0 + 品牌替换」**，不碰 Kotlin 层。这一阶段一晚上能完成。
3. 二阶段再谈融合，且**必须在有 Flutter 环境的电脑上做**（本地编译 2 分钟 vs CI 20 分钟），按函数接、每接一块编译一次。
4. 融合顺序：坐标手势（独立）→ `run` 多步（动 PhoneCore）→ `task` 自动任务（动四处，最险）。
5. 手机操控改动必须真机验证：授权弹窗、前后台包名守卫、失败不重放、视觉权限撤销。编译通过不等于能用。

## 五、成本与收益核对

| | 实际 |
|---|---|
| 投入 | 约 3 小时，7 轮 CI，1 次全量备份 + 卸载重装 |
| 得到 | 一个自签名的官方 1.3.0（能装，功能正常） |
| 没得到 | 社区版的坐标手势 / `run` 多步 / 自动任务 |
| 风险兑现 | 附件引用失效 6 次，其余数据完好 |

**如果只是想要「自己的版本」**：官方版能自动更新，数据不丢，图标换不了但启动画面、壁纸、主题都能自定义——这个需求其实不用自己造包。

**如果确实要二创**：代码开源这步已经完成了（仓库 public、LICENSE/NOTICE/AGENTS.md 齐全），要发布安装包再替换素材和包名即可，不必重走编译融合这条路。
