# Чат Claude

[English](../en/claude-chat.md) · **Русский** · [中文](../cn/claude-chat.md)

Для вкладки **Chat** приложения Claude. Для вкладки **Code**: [Приложение Claude](claude-desktop.md).

Для плагинов нужна платная подписка Claude: Pro, Max, Team или Enterprise. Репозиторий `intergalactic-planb/plugins` должен быть публичным.

## Установить

1. В боковом меню приложения откройте **Plugins**.
2. Нажмите **Add**, затем **Add marketplace**, затем **Add from a repository**.
3. В поле **URL** введите `intergalactic-planb/plugins` и выберите строку **Use "intergalactic-planb/plugins"**.
4. Найдите в списке `Quotalogic` и откройте его карточку.

## Войти в Quotalogic

1. В карточке плагина, в разделе **Connectors**, нажмите **Connect** у `quotalogic`.
2. В окне **Add custom connector** нажмите **Continue**.
3. В следующем окне ничего не меняйте (выбраны **Sign in now** и **Register automatically**) и нажмите **Add**.
4. На странице «Finish connecting a connector?» нажмите **Continue connecting**.
5. В браузере войдите в Quotalogic, проверьте рабочее пространство и подтвердите.
6. В карточке плагина появятся **Added** и **Connected**. Кнопка **Finish setup** исчезнет сама.
7. Начните новый чат и спросите: «Покажи мои каталоги». Появится список ваших каталогов.

## Обновить

Откройте карточку Quotalogic в **Plugins** и нажмите **Update**. После обновления начните новый чат.

## Выключить и удалить

Откройте карточку Quotalogic в **Plugins**, нажмите **...**, затем **Remove**. Подключение `quotalogic` (Custom connector) остаётся в списке **Plugins**: откройте его **...** и удалите отдельно, если оно больше не нужно.

## Если не работает

| Что видите | Что делать |
| --- | --- |
| «Marketplace source format is invalid» при добавлении | Закройте окно. **Add**, **Manage marketplaces**: если `quotalogic-plugins` уже есть, нажмите **...**, **Check for updates**. Если плагина всё равно нет в списке, нажмите **...**, **Remove** и добавьте заново с шага 2 |
| В карточке `quotalogic` со статусом **Not connected** | Нажмите **Connect** и пройдите раздел «Войти в Quotalogic» |
| «Нет инструментов Quotalogic» в чате | Откройте карточку Quotalogic в **Plugins** и проверьте, что `quotalogic` в статусе **Connected**. Если нет, нажмите **Connect**. Затем начните новый чат |
