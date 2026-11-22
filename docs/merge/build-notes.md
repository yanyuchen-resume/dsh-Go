# 自建 DSH Go：手机上就能跑完的流程

目标：官方 1.3.0 的全部功能 + 社区版 1.0.19 的手机自动化增强（坐标手势、`run` 多步、`task` 自动任务、`send` 发送范围），用自己的证书签名，以后自己发版、覆盖升级不丢数据。

没电脑也能做，全部在 GitHub 手机网页 + Actions 上完成。

## 现在的进度

| 步骤 | 状态 |
|---|---|
| 三方合并（官方 1.0.13 为基线，官方 1.3.0 ← 社区 1.0.19） | 已执行，38 个冲突文件 / 约 118 个冲突块 |
| 冲突报告 | `冲突报告.md` |
| 第一版补丁 | `merge-community.patch`（127 文件，+11178/-5925），已用 `git apply --check` 验证能干净打在官方 1.3.0 上 |
| 编译流水线 | `build-self.yml`（含 l10n 重生成、签名、artifact） |
| 证书生成流水线 | `make-keystore.yml` |
| 更新源 | 已改成占位 `YOUR_GITHUB_NAME/dsh-Go`，**上传前必须替换成你的 GitHub 用户名** |
| 编译验证 | **没做过**，手机上没有 Flutter/Android SDK，只能靠 GitHub Actions 跑出第一个结果 |

## 第一版补丁的取舍（要改就改这里）

第一版是「先跑通」策略，不是终版：

- `phone/**/*.kt`、`app/lib/phone/*`、`app/lib/ui/phone/*`、`phone_prompt_strings.xml` → **取社区版**（自动化能力在这一层）
- 其余冲突 → **取官方 1.3.0**
- 代价：官方 1.3.0 对 `phone/` 的部分改动（悬浮条细节、确认三档的按钮文案）会被社区版覆盖掉。编译通过后，再按 `冲突报告.md` 逐块补回官方语义。

这是可接受的中间态：先让 Actions 编出一个能装的包，再谈功能完整性。

## 为什么会有这么多冲突

社区版的自动化是往 `phone/` 那层 Kotlin 里改的（`PhoneCore.kt` 单文件 +600 行），而官方 1.3.0 也动了同一批文件（悬浮条、确认三档、语音）。两边都要，就必须逐块人工合。

冲突分布：

- `phone/*.kt` 20 个文件（`PhoneCore` 15 块、`MainScreenDriver` 13 块、`PhoneControl` 11 块、`SafetyGate` 10 块）—— 核心，必须逐块合
- `l10n` 5 个（生成文件 + .arb）—— 取并集，补键
- 文档 / runtime / 签名配置 13 个 —— 简单裁决

## 裁决原则（逐块时按这个来）

1. **两边的安全提示都要保留**：官方的「允许一次 / 本轮允许 / 取消」和社区版的「本任务免确认 / 允许这次发送」是两套确认语义，合并后按钮要覆盖两边场景。
2. **官方的新功能不许丢**：悬浮条、不打扰模式、语音、后台保护都来自官方 1.3.0 的 `phone/` 改动。
3. **社区版的能力要接上**：`Automation*` 那批新增类（63 个文件，无冲突，已自动并入）需要被 `PhoneCore` / `PhoneToolCatalog` 调用到。
4. **编译优先**：编不过的合并毫无意义。每完成一批就跑 Actions 编译。

## 手机上的操作顺序

1. GitHub 网页 fork `ccch1mneyyy/dsh-Go`（只 fork main，别改设置）。
2. 在你的 fork 里，用网页「Add file → Upload files」上传 `build-self.yml`、`make-keystore.yml` 到 `.github/workflows/`，`merge-community.patch` 到 `selfbuild/`。
   - 手机网页上传单个文件没问题；先建目录用「Create new file」输入 `selfbuild/x` 也能顺手建。
3. **仓库设 Private**（Settings → General → Danger Zone → Change visibility），证书和密码才安全。
4. Actions → Make keystore (one-off) → Run workflow，填密码；跑完下载 artifact，取出 `dshgo-self.jks.b64` 里的 base64 文本。
5. Settings → Secrets and variables → Actions，加四个：
   - `ANDROID_KEYSTORE_BASE64`：上面那段 base64
   - `ANDROID_STORE_PASSWORD`、`ANDROID_KEY_ALIAS`、`ANDROID_KEY_PASSWORD`
6. Actions → Build DSH Go (self) → Run workflow → 版本名填 `1.3.1`。
7. 编译失败就把报错日志复制给我，我改补丁；成功后下载 artifact 里的 APK。
8. 装之前先做备份（已备好：`/sdcard/Download/DSH-Go/backup/`），因为自建版签名与现在这台上的社区版不同，必须先卸载。
9. 装上自建版后跑 `restore.sh` 回填数据。

## 后续发版

改 `app/pubspec.yaml` 的版本号 → 再跑一次 Actions → 覆盖安装。因为签名固定是你自己的，以后升级不会再丢数据。

## 注意

- AGPL-3.0：自己用随便改；**要分发（发群、发网盘）就必须同样开源并保留原作者署名**。
- 官方 CI 用 Flutter 3.47.7 + JDK 17，`build-self.yml` 已对齐，别自己换版本。
- 编译产物 arm64 专用；`x64` flavor 只给模拟器。
