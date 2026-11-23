# Приложение Claude

[English](../en/claude-desktop.md) · **Русский** · [中文](../cn/claude-desktop.md)

Для `claude` в терминале: [Claude в терминале](claude-terminal.md).

Для вкладки **Chat**: [Чат Claude](claude-chat.md).

Для плагинов нужна платная подписка Claude: Pro, Max, Team или Enterprise. Репозиторий `intergalactic-planb/plugins` должен быть публичным.

## Установить

1. В боковом меню приложения откройте **Plugins**.
2. Нажмите **Add**, затем **Add marketplace**, затем **Add from a repository**.
3. В поле **URL** введите `intergalactic-planb/plugins` и выберите строку **Use "intergalactic-planb/plugins"**.
4. Найдите в списке `Quotalogic` и нажмите **Add to this computer**. Рядом появится **Added**.

## Войти в Quotalogic

1. Откройте вкладку **Code** и начните новую сессию.
2. Введите `/mcp` и отправьте. После `/mcp` ничего не пишите.
3. В окне **MCP servers** нажмите **Connect** у `quotalogic`.
4. Приложение спросит «Open mcp-staging.quotalogic.io to authorize quotalogic?». Нажмите **OK**.
5. В браузере войдите в Quotalogic, проверьте рабочее пространство и подтвердите.
6. В новой сессии спросите: «Покажи мои каталоги». Появится список ваших каталогов.

## Импорт файла: разрешить домен

Без этого шага импорт из чата не работает. Claude отправляет файл командой из своей песочницы, а песочница по умолчанию не пускает запросы к серверу Quotalogic. Claude ответит, что адреса нет в списке разрешённых.

1. Откройте **Settings**, затем **Capabilities**.
2. В поле **Additional allowed domains** добавьте домен сервера без `https://` и пути: `mcp-staging.quotalogic.io`.
3. Вернитесь в чат и напишите: «Домен добавил, повтори импорт».

На платном личном тарифе домен добавляет сам пользователь, на Team и Enterprise — администратор организации. Файл прикладывают к сообщению скрепкой: «Создай каталог Поставщик Б из этого файла».

## Обновить

1. **Plugins**, **Add**, **Manage marketplaces**, у `quotalogic-plugins` нажмите **...**, затем **Check for updates**.
2. Закройте окно, откройте карточку Quotalogic в **Plugins** и нажмите **Update**.
3. Перезапустите приложение и начните новую сессию.

## Выключить и удалить

Откройте карточку Quotalogic в **Plugins**. Переключатель справа выключает плагин. **...**, затем **Remove** удаляет его.

## Если не работает

| Что видите | Что делать |
| --- | --- |
| «Marketplace source format is invalid» при добавлении | Закройте окно. **Add**, **Manage marketplaces**: если `quotalogic-plugins` уже есть, нажмите **...**, **Check for updates**. Если плагина всё равно нет в списке, нажмите **...**, **Remove** и добавьте заново с шага 2 |
| В списке нет `Quotalogic` | Обновите marketplace: **Add**, **Manage marketplaces**, **...**, **Check for updates** |
| В окне **MCP servers** нет `quotalogic` | Обновите плагин (раздел «Обновить»), перезапустите приложение, начните новую сессию |
| «isn't a recognized /mcp action» | Введите только `/mcp`, без названия сервера |
| `/mcp` открывает Settings, а не **MCP servers** | Откройте сессию, в которой окно **MCP servers** уже открывалось, и введите `/mcp` там |
| `/mcp` не работает во вкладке **Chat** | Введите `/mcp` во вкладке **Code** |
