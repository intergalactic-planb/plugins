# 将 Quotalogic 连接到 Alice AI

[English](../en/alice-act.md) · [Русский](../ru/alice-act.md) · **中文**

两种连接方式都需要有权访问测试工作区的 Quotalogic 账号。

| Alice 版本 | 管理位置 | 如何连接 Quotalogic |
| --- | --- | --- |
| **企业版 Alice AI** | 管理员在 Yandex 360 管理后台允许员工创建插件和技能。 | 在 **Задачи** 模式下使用 Client ID 创建个人 MCP 插件，再分别上传技能 ZIP。 |
| **企业版 Alice AI Pro** | 管理员在 Yandex Cloud 管理员工的配置和工作席位。 | 将包含 MCP 和技能的单个 ZIP 上传到 **Плагины организации**，再将插件添加到工作席位的配置。 |

个人版 Alice AI 可通过 **Алиса Плюс** 附加选项进入 **Действуй!** 模式，按[个人连接步骤](#个人-mcp-插件和单独上传的技能)操作。

## 个人 MCP 插件和单独上传的技能

### 1. 准备 Alice

#### 个人版 Alice AI

1. 打开 [Alice AI](https://alice.yandex.ru/)，切换到 **Действуй!** 模式。此模式需要开通 **Алиса Плюс** 附加选项。
2. 如果看到 **Получить ранний доступ**，请点击它，查看 Yandex 为邮件、网盘和日历申请的权限，再确认授权。 [Yandex 帮助](https://alice.yandex.ru/support/ru/assistant/chat/agent-act)

#### 企业版 Alice AI

1. **Yandex 360 管理员**打开组织管理后台 → **Алиса AI для бизнеса → Настройки**，在 **Плагины** 区域勾选 **Создавать и редактировать плагины**。
2. **员工**打开 [Alice AI](https://alice.yandex.ru/)，选择组织空间并切换到 **Задачи** 模式。

如需使用自定义技能，管理员可以单独启用 **Навыки → Создавать и редактировать навыки**。

### 2. 在 Quotalogic 获取 Client ID

1. 打开测试工作区设置 → **Агенты и доступ**（**Agents and access**）。
2. 确认 **Разрешить подключать агентов**（**Allow connecting agents**）已开启。工作区所有者或管理员可以开启此选项。
3. 在 **Как подключить плагин к агенту**（**How to connect the plugin to an agent**）链接下方点击 **Получить Client ID для Алисы**（**Get Alice Client ID**），然后复制字段中的值。

### 3. 创建个人插件

1. 在 Alice 侧边栏打开 **Плагины**。

   - 在 **Действуй!** 模式下点击 **Создать плагин**。
   - 在 **Задачи** 模式下点击搜索框右侧的 **+**。

2. 填写基本字段。

   | Alice 中的字段 | 值 |
   | --- | --- |
   | **Название** | `Quotalogic Staging` |
   | **Описание** | `Work with Quotalogic staging catalogs and proposals.` |
   | **Адрес MCP-сервера** | `https://mcp-staging.quotalogic.io/mcp` |

3. 打开 **Дополнительно**，填写连接设置。

   | Alice 中的字段 | 值 |
   | --- | --- |
   | **Протокол подключения** | `Streamable HTTP` |
   | **Способ авторизации** | **OAuth** |
   | **Авторизация OAuth-клиента** | **Без аутентификации** |
   | **Идентификатор клиента (Client ID)** | 从 Quotalogic 复制的 Client ID |
   | **URL авторизации** | `https://mcp-staging.quotalogic.io/authorize` |
   | **URL получения токена** | `https://mcp-staging.quotalogic.io/token` |
   | **Области доступа (Scopes)** | 留空 |

4. 确认回调地址为 `https://alice.yandex.ru/plugins/oauth/callback`，然后点击 **Создать**。

> **在企业版 Alice AI 中，请将插件保留为个人插件。** 每位员工分别创建自己的插件、获取独立的 Client ID，并登录自己的 Quotalogic 账号。如果共享插件使用创建者的连接，其他员工的请求就会以创建者的身份和权限在创建者授权的工作区中执行。

### 4. 连接并检查插件

1. 在插件卡片上点击 **Подключить**，查看申请的权限并确认连接。

   如果看到 **Попробуйте войти через Яндекс Браузер**，请点击 **Продолжить как обычно**。

2. 登录 Quotalogic，确认授权页面显示的是测试工作区，然后点击 **Разрешить**。
3. Alice 的插件卡片显示 **Приложение подключено** 后，点击 **Попробовать**，然后询问“请通过 Quotalogic Staging 显示我的目录”。Alice 会列出所选工作区的目录。在 **Работала** 中可以查看插件调用。
4. 在 Quotalogic 的 **Агенты и доступ → Мои подключения**（**My connections**）中查看是否出现 **Алиса AI**（**Alice AI**）。

### 5. 上传技能

1. 在 [Quotalogic 仓库](https://github.com/intergalactic-planb/plugins)中点击 **Code → Download ZIP**，下载并解压。
2. 在 `skills/` 中选择需要的技能。将技能文件夹连同其中的 `SKILL.md` 和其他文件打包为独立的 ZIP。每个所需的技能都单独打包。
3. 在 Alice 中打开 **Плагины → Навыки**，点击 **+**，然后上传技能 ZIP。

### Client ID 和重新登录

#### 从哪里再次复制 Client ID

在同一浏览器中刷新页面后，上次签发的 ID 仍会显示在工作区设置的字段中。

#### 获取新的 Client ID 后会发生什么

每个新插件都需要获取一个新的 ID。旧 ID 仍会保留，已连接的插件可以继续工作。当您使用同一账号在同一工作区确认新的 Alice 连接时，Quotalogic 会撤销上一个连接。

#### Client ID 的有效期

建立连接后，Client ID 不会过期，连接被撤销后也会保留。如果 ID 尚未建立连接，也没有正在进行的登录，系统可能在签发超过 24 小时后自动清理该 ID。如果 ID 已过期，请重新获取。

#### 如何重新登录 Quotalogic

如果您已在 Quotalogic 中撤销访问权限，但卡片仍显示 **Приложение подключено**，请选择 **⋯ → Отключить**，然后点击 **Подключить**。在 Quotalogic 授权页面核对工作区，再点击 **Разрешить**。

如果 Alice 要求重新登录，请继续使用原来的 Client ID。访问令牌自签发起有效 1 小时，刷新令牌自签发起有效 30 天。每次刷新成功后，Alice 都会获得一对新的令牌，有效期从签发时重新计算。

## 在企业版 Alice AI Pro 中使用一个 ZIP

### 1. 准备 ZIP

1. 在 [Quotalogic 仓库](https://github.com/intergalactic-planb/plugins)中点击 **Code → Download ZIP**，下载并解压。
2. 将正方形的 PNG 插件图标放在 `assets/quotalogic-icon.png`。在 `.codex-plugin/plugin.json` 中保留名称和描述，移除 `skills` 字段，并将 `interface.logo` 和 `interface.composerIcon` 设置为 `assets/quotalogic-icon.png`。
3. 将以下结构打包到 ZIP 的根目录。文件大小上限为 64 MB。 [Yandex 压缩包格式](https://aistudio.yandex.ru/ru/docs/ai-studio/manage/operations/add-organization-plugin)

```text
.codex-plugin/plugin.json
.mcp.json
skills/
assets/quotalogic-icon.png
```

### 2. 添加插件并分配配置

1. 使用拥有 `ai.workProfiles.admin` 角色的账号打开 [Yandex Cloud 管理界面](https://aistudio.yandex.ru/manage)。在 **Плагины организации → Добавить плагин** 中输入 **Quotalogic Staging**，选择 ZIP，然后点击 **Добавить**。
2. 在 **Профили** 中打开所需的配置。在 **Плагины → Плагины организации** 中添加插件包，然后保存配置。
3. 在 **Рабочие места** 中为员工分配采用此配置的工作席位，或将此配置分配给现有席位。员工需要 **Алиса AI Про для бизнеса** 订阅和工作席位才能使用插件。 [工作席位](https://aistudio.yandex.ru/ru/docs/ai-studio/manage/operations/manage-runtimes)

### 3. 从工作席位检查访问权限

1. 使用员工账号打开 [企业版 Alice AI Pro](https://aistudio.yandex.ru/agents)，并选择已分配配置的工作席位。
2. 如果 Alice 提示连接 Quotalogic，请登录自己的 Quotalogic 账号，核对测试工作区并确认授权。
3. 请 Alice 使用 **Quotalogic Staging** 显示目录，并核对返回的目录是否来自所选工作区。
