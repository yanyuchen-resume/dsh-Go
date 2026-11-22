# DSH Go 自建版（非官方 · 二创）

> 本版是 **第三方修改版**，由 `yanyuchen-resume` 维护，与原作者 `ccch1mneyyy` 无隶属关系，也不代表官方。
> 官方项目：[ccch1mneyyy/dsh-Go](https://github.com/ccch1mneyyy/dsh-Go)

DeepSeek Harness 的 Android 客户端。手机上直接跑，也能遥控你电脑上的 DSH。

## 这个版本是什么

在官方 **1.3.0** 的基础上，并入另一个社区分支 `puppy-bai/dsh-Go`（**1.0.19**）的手机自动化能力：

- **坐标手势与按文字点击**：`gesture` / `tap_xy` / `input`，按可见文字定位
- **`run` 多步任务**：`schema:2`，声明应用范围、选择器、等待、`if`、有限 `repeat`、`yield`
- **自动任务系统**：`task` 工具 + 任务页，参数校验、分支、循环、审核后复用
- **发送范围授权**：`send:{app,recipient,text}`，同一任务内跨步骤复用
- **按应用视觉权限**：首次按应用确认，可在应用名单撤销

同时保留官方 1.3.0 的功能（语音朗读与通话、酒馆玩法包、后台保护、使用统计、会话搜索等）。

## 和官方/社区版的区别

| | 官方 | 社区版 | 本版 |
|---|---|---|---|
| 签名证书 | 作者发布证书 | Android 调试证书 | 自建证书 |
| 更新源 | 官方仓库 | 社区仓库 | 本仓库 |
| 手机自动化 | 基础 | 强 | 强 |
| 语音/酒馆新功能 | 有 | 无 | 有 |
| 数据迁移 | — | — | 跨签名首装需卸载一次 |

## 安装

1. 只支持 **arm64-v8a**，Android 8.0 以上。
2. 与官方版、社区版签名都不同，**首次安装需要先卸载旧版**，卸载会清空对话与 Linux 环境，请先备份。
3. 此后本仓库同一证书签名的版本可以覆盖升级，数据保留。

## 自行构建

见 `.github/workflows/build-self.yml`：需要 `ANDROID_KEYSTORE_BASE64`、`ANDROID_STORE_PASSWORD`、`ANDROID_KEY_ALIAS`、`ANDROID_KEY_PASSWORD` 四个 Actions Secrets，以及 Flutter 3.47.7 + JDK 17。

## 许可与署名

- 本仓库与修改部分以 **AGPL-3.0** 发布，全文见 [LICENSE](LICENSE)。
- 上游项目：`ccch1mneyyy/dsh-Go`（AGPL-3.0）；手机自动化代码来自 `puppy-bai/dsh-Go`（AGPL-3.0）。两位作者的著作权均予保留。
- 修改清单、合并方式与**品牌素材的使用限制**见 [NOTICE.md](NOTICE.md)。上游的品牌图形、桌宠与 DeepSeek 图标不在 AGPL 授权范围内，发布衍生版时应替换为自己的素材。
- 第三方组件许可见 [THIRD_PARTY_NOTICES.md](THIRD_PARTY_NOTICES.md)。

## 免责

软件按「现状」提供，不附带任何担保。手机操控会让 AI 读取屏幕并执行点击、输入、发送等操作，风险由使用者自行承担。
