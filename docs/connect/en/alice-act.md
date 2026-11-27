# Connect Quotalogic to Alice AI

**English** · [Русский](../ru/alice-act.md) · [中文](../cn/alice-act.md)

Create a personal plugin for the Quotalogic staging server in **Действуй!** mode. You need a Quotalogic account with access to the staging workspace and **Алиса Плюс** with access to **Действуй!**. If you see **Получить ранний доступ**, open it and review the permissions Yandex requests for Mail, Disk and Calendar before confirming. [Yandex help](https://alice.yandex.ru/support/ru/assistant/chat/agent-act)

## Get an Alice Client ID

1. Open your staging workspace settings in Quotalogic and choose **Агенты и доступ** (**Agents and access**).
2. Check that agent access is on. The workspace owner or an admin can turn it on with **Разрешить подключать агентов** (**Allow connecting agents**).
3. Below **Как подключить плагин к агенту** (**How to connect the plugin to an agent**), click **Получить Client ID для Алисы** (**Get Alice Client ID**) and copy the value from the field. After refreshing the page in the same browser, you can copy the last issued ID again. Use the button again for each new personal plugin.

Generating a new Client ID leaves the previous one registered. If the previous ID has no connection or sign-in in progress, automatic cleanup can remove it once it is more than 24 hours old. If the ID has expired, generate a new one. A Client ID used to create a connection has no expiry date. It remains registered even after the connection is revoked.

## Create the plugin

1. Open Alice AI at [alice.yandex.ru](https://alice.yandex.ru/) and switch to **Действуй!**.
2. In the sidebar, open **Плагины** and click **Создать плагин**.
3. Fill in the form.

   | Field in Alice | Value |
   | --- | --- |
   | **Название** | `Quotalogic Staging` |
   | **Описание** | `Work with Quotalogic staging catalogs and proposals.` |
   | **Адрес MCP-сервера** | `https://mcp-staging.quotalogic.io/mcp` |

4. Open **Дополнительно** and fill in the connection settings.

   | Field in Alice | Value |
   | --- | --- |
   | **Протокол подключения** | `Streamable HTTP` |
   | **Способ авторизации** | **OAuth** |
   | **Авторизация OAuth-клиента** | **Без аутентификации** |
   | **Идентификатор клиента (Client ID)** | The Client ID you copied from Quotalogic |
   | **URL авторизации** | `https://mcp-staging.quotalogic.io/authorize` |
   | **URL получения токена** | `https://mcp-staging.quotalogic.io/token` |
   | **Области доступа (Scopes)** | Leave blank |

5. Check that Alice shows `https://alice.yandex.ru/plugins/oauth/callback` as the callback URL, then click **Создать**.

## Sign in and check the connection

1. In the plugin card, click **Подключить**. Review the requested permissions and confirm with **Подключить**.
2. If Alice shows **Попробуйте войти через Яндекс Браузер**, click **Продолжить как обычно** to open the sign-in page in your current browser.
3. Sign in to Quotalogic, check the workspace on the consent page and click **Разрешить**.
4. Back in Alice, the plugin card shows **Приложение подключено**. Click **Попробовать** and ask “Show my catalogs using Quotalogic Staging.” Alice should return your workspace's catalogs. Open **Работала** in the task to see which plugin Alice called.
5. In **Мои подключения** (**My connections**), the connection appears as **Алиса AI** (**Alice AI**). Your connected plugin keeps working when you generate a new Client ID. The previous Alice connection is revoked when you approve a new Alice connection with the same account in the same workspace.

The access token lasts 1 hour and the refresh token lasts 30 days from issue. Each successful refresh gives Alice a new pair with the same lifetimes. If Alice asks you to sign in to Quotalogic again, use the same Client ID.
