# 将 Quotalogic 连接到 Alice AI

[English](../en/alice-act.md) · [Русский](../ru/alice-act.md) · **中文**

在 **Действуй!** 模式中为 Quotalogic 测试服务器创建个人插件。您需要有权访问测试工作区的 Quotalogic 账号，以及已开通 **Действуй!** 模式的 **Алиса Плюс**。如果看到 **Получить ранний доступ**，请点击它，查看 Yandex 为邮件、网盘和日历申请的权限，再确认授权。 [Yandex 帮助](https://alice.yandex.ru/support/ru/assistant/chat/agent-act)

## 获取 Alice Client ID

1. 在 Quotalogic 中打开测试工作区设置，选择 **Агенты и доступ**（**Agents and access**）。
2. 确认工作区已开启代理访问权限。工作区所有者或管理员可通过 **Разрешить подключать агентов**（**Allow connecting agents**）开启。
3. 在 **Как подключить плагин к агенту**（**How to connect the plugin to an agent**）链接下方，点击 **Получить Client ID для Алисы**（**Get Alice Client ID**），并复制字段中的值。在同一浏览器中刷新页面后，仍可从该字段复制上次获取的 Client ID。每创建一个个人插件，请再次点击此按钮获取新的值。如果保存的 Client ID 已过期而无法登录，请点击按钮获取新值。

## 创建插件

1. 在 [alice.yandex.ru](https://alice.yandex.ru/) 打开 Alice AI，切换到 **Действуй!** 模式。
2. 在侧边栏打开 **Плагины**，点击 **Создать плагин**。
3. 填写表单。

   | Alice 中的字段 | 值 |
   | --- | --- |
   | **Название** | `Quotalogic Staging` |
   | **Описание** | `Work with Quotalogic staging catalogs and proposals.` |
   | **Адрес MCP-сервера** | `https://mcp-staging.quotalogic.io/mcp` |

4. 打开 **Дополнительно**，填写连接设置。

   | Alice 中的字段 | 值 |
   | --- | --- |
   | **Протокол подключения** | `Streamable HTTP` |
   | **Способ авторизации** | **OAuth** |
   | **Авторизация OAuth-клиента** | **Без аутентификации** |
   | **Идентификатор клиента (Client ID)** | 从 Quotalogic 复制的 Client ID |
   | **URL авторизации** | `https://mcp-staging.quotalogic.io/authorize` |
   | **URL получения токена** | `https://mcp-staging.quotalogic.io/token` |
   | **Области доступа (Scopes)** | 留空 |

5. 确认 Alice 显示的回调地址为 `https://alice.yandex.ru/plugins/oauth/callback`，然后点击 **Создать**。

## 登录并检查连接

1. 在插件卡片上点击 **Подключить**。查看申请的权限，再点击 **Подключить** 确认。
2. 如果 Alice 显示 **Попробуйте войти через Яндекс Браузер**，点击 **Продолжить как обычно**，在当前浏览器中打开登录页面。
3. 登录 Quotalogic，在授权页面核对工作区并点击 **Разрешить**。
4. 返回 Alice 后，插件卡片会显示 **Приложение подключено**。点击 **Попробовать**，然后询问“请通过 Quotalogic Staging 显示我的目录”。Alice 会列出您所在工作区的目录。在任务中展开 **Работала**，可以查看 Alice 调用了哪个插件。
5. 在 **Мои подключения**（**My connections**）中，连接会显示为 **Алиса AI**（**Alice AI**）。再次登录 Alice 会替换您在此工作区中的上一个 Alice 连接。
