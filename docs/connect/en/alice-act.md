# Connect Quotalogic to Alice AI

**English** · [Русский](../ru/alice-act.md) · [中文](../cn/alice-act.md)

Both setup options require a Quotalogic account with access to a staging workspace.

| Alice version | Where it is managed | How to connect Quotalogic |
| --- | --- | --- |
| **Alice AI for Business** | In the Yandex 360 admin console, an admin allows employees to create plugins and skills. | In **Задачи**, create a personal MCP plugin with a Client ID and upload the skills as separate ZIPs. |
| **Alice AI Pro for Business** | In Yandex Cloud, an admin manages employee profiles and workplaces. | Upload one ZIP with MCP and skills to **Плагины организации** and add it to the workplace profile. |

For personal Alice AI, use the [personal setup](#personal-mcp-plugin-and-separate-skills) in **Действуй!** with the **Алиса Плюс** add-on.

## Personal MCP plugin and separate skills

### 1. Prepare Alice

#### Personal Alice AI

1. Open [Alice AI](https://alice.yandex.ru/) and switch to **Действуй!**. This mode requires the **Алиса Плюс** add-on.
2. If you see **Получить ранний доступ**, click it and review the permissions Yandex requests for Mail, Disk and Calendar before confirming. [Yandex help](https://alice.yandex.ru/support/ru/assistant/chat/agent-act)

#### Alice AI for Business

1. The **Yandex 360 admin** opens the organization console → **Алиса AI для бизнеса → Настройки**. Under **Плагины**, they enable **Создавать и редактировать плагины**.
2. The **employee** opens [Alice AI](https://alice.yandex.ru/), selects the organization space and switches to **Задачи**.

For custom skills, the admin can separately enable **Навыки → Создавать и редактировать навыки**.

### 2. Get a Client ID in Quotalogic

1. Open the staging workspace settings → **Агенты и доступ** (**Agents and access**).
2. Check that **Разрешить подключать агентов** (**Allow connecting agents**) is on. The workspace owner or an admin can turn it on.
3. Below **Как подключить плагин к агенту** (**How to connect the plugin to an agent**), click **Получить Client ID для Алисы** (**Get Alice Client ID**) and copy the value from the field.

### 3. Create a personal plugin

1. Open **Плагины** from the Alice sidebar.

   - In **Действуй!**, click **Создать плагин**.
   - In **Задачи**, click **+** to the right of the search field.

2. Fill in the basic fields.

   | Field in Alice | Value |
   | --- | --- |
   | **Название** | `Quotalogic Staging` |
   | **Описание** | `Work with Quotalogic staging catalogs and proposals.` |
   | **Адрес MCP-сервера** | `https://mcp-staging.quotalogic.io/mcp` |

3. Open **Дополнительно** and fill in the connection settings.

   | Field in Alice | Value |
   | --- | --- |
   | **Протокол подключения** | `Streamable HTTP` |
   | **Способ авторизации** | **OAuth** |
   | **Авторизация OAuth-клиента** | **Без аутентификации** |
   | **Идентификатор клиента (Client ID)** | The Client ID you copied from Quotalogic |
   | **URL авторизации** | `https://mcp-staging.quotalogic.io/authorize` |
   | **URL получения токена** | `https://mcp-staging.quotalogic.io/token` |
   | **Области доступа (Scopes)** | Leave blank |

4. Check that the callback URL is `https://alice.yandex.ru/plugins/oauth/callback`, then click **Создать**.

> **Keep the plugin personal in Alice AI for Business.** Each employee creates their own plugin, gets a separate Client ID and signs in to Quotalogic. If a shared plugin uses the creator's connection, colleagues' requests run under the creator's account and permissions in the workspace they approved.

### 4. Connect and check the plugin

1. In the plugin card, click **Подключить**, review the permissions and confirm the connection.

   If Alice shows **Попробуйте войти через Яндекс Браузер**, click **Продолжить как обычно**.

2. Sign in to Quotalogic, check that the consent page shows the staging workspace and click **Разрешить**.
3. When the Alice plugin card shows **Приложение подключено**, click **Попробовать** and ask “Show my catalogs using Quotalogic Staging.” Alice should return the selected workspace's catalogs. Open **Работала** to see the plugin call.
4. In Quotalogic, check **Агенты и доступ → Мои подключения** (**My connections**) for **Алиса AI** (**Alice AI**).

### 5. Upload skills

1. Download the [Quotalogic repository](https://github.com/intergalactic-planb/plugins) using **Code → Download ZIP** and extract it.
2. Choose the skill you need in `skills/`. ZIP the skill's folder with its `SKILL.md` and the other files it contains. Repeat for each skill you need.
3. In Alice, open **Плагины → Навыки**, click **+**, and upload the skill ZIP.

### Client IDs and signing in again

#### Where to copy the Client ID again

In the same browser, the last issued ID is still in the field in the workspace settings after you refresh the page.

#### What happens when you generate a new Client ID

Get a new ID for each new plugin. The previous ID stays registered and the connected plugin keeps working. Quotalogic revokes your previous Alice connection when you approve a new one with the same account in the same workspace.

#### How long the Client ID lasts

Once connected, the Client ID has no expiry date and stays registered after the connection is revoked. Automatic cleanup can remove an ID more than 24 hours after issue if it has no connection or sign-in in progress. If the ID has expired, generate a new one.

#### How to sign in to Quotalogic again

If you revoked access in Quotalogic and the card still shows **Приложение подключено**, choose **⋯ → Отключить**, then click **Подключить**. On the Quotalogic consent page, check the workspace and click **Разрешить**.

If Alice asks you to sign in again, use the same Client ID. The access token lasts 1 hour and the refresh token lasts 30 days from issue. Each successful refresh gives Alice a new pair with the same lifetimes.

## One ZIP in Alice AI Pro for Business

### 1. Prepare the ZIP

1. Download the [Quotalogic repository](https://github.com/intergalactic-planb/plugins) using **Code → Download ZIP** and extract it.
2. Put a square PNG plugin icon at `assets/quotalogic-icon.png`. In `.codex-plugin/plugin.json`, keep the name and description, remove the `skills` field, and set `interface.logo` and `interface.composerIcon` to `assets/quotalogic-icon.png`.
3. Create a ZIP with this structure at its root. The maximum file size is 64 MB. [Yandex archive format](https://aistudio.yandex.ru/ru/docs/ai-studio/manage/operations/add-organization-plugin)

```text
.codex-plugin/plugin.json
.mcp.json
skills/
assets/quotalogic-icon.png
```

### 2. Add the plugin and assign the profile

1. With the `ai.workProfiles.admin` role, open [Management in Yandex Cloud](https://aistudio.yandex.ru/manage). In **Плагины организации → Добавить плагин**, enter **Quotalogic Staging**, select the ZIP, and click **Добавить**.
2. In **Профили**, open the profile you want to use. Add the package under **Плагины → Плагины организации**, then save the profile.
3. In **Рабочие места**, issue the employee a workplace with this profile or assign the profile to an existing workplace. The employee needs an **Алиса AI Про для бизнеса** subscription and a workplace to use the plugin. [Workplaces](https://aistudio.yandex.ru/ru/docs/ai-studio/manage/operations/manage-runtimes)

### 3. Check access from the workplace

1. Open [Alice AI Pro for Business](https://aistudio.yandex.ru/agents) with the employee's account and select the workplace with the assigned profile.
2. If Alice asks you to connect Quotalogic, sign in with your Quotalogic account, check the staging workspace, and approve access.
3. Ask Alice to show the catalogs using **Quotalogic Staging** and check that the response lists catalogs from the selected workspace.
