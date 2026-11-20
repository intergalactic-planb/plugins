# Claude в терминале

[English](../en/claude-terminal.md) · **Русский** · [中文](../cn/claude-terminal.md)

Для `claude` в терминале. Для приложения Claude: [Приложение Claude](claude-desktop.md).

Для плагинов нужна платная подписка Claude: Pro, Max, Team или Enterprise. Репозиторий `intergalactic-planb/plugins` должен быть публичным.

## Установить

В терминале выполните по очереди:

```bash
claude plugin marketplace add intergalactic-planb/plugins
```

```bash
claude plugin install quotalogic@quotalogic-plugins
```

Сообщение `SSH not configured, cloning via HTTPS` ошибкой не является.

## Войти в Quotalogic

1. Запустите `claude` и введите `/mcp`.
2. Стрелкой вниз выберите `plugin:quotalogic:quotalogic` (он в конце списка) и нажмите Enter.
3. Выберите **Authenticate**.
4. В браузере войдите в Quotalogic, проверьте рабочее пространство и подтвердите.
5. Вернитесь в терминал: статус `connected`. Спросите: «Покажи мои каталоги». Появится список ваших каталогов.

## В приложении Claude

Плагин, установленный через терминал, доступен и в приложении Claude, во вкладке **Code**. Устанавливать его там ещё раз не нужно.

## Обновить

```bash
claude plugin marketplace update quotalogic-plugins
```

Затем закройте `claude` (`/exit`) и запустите заново.

## Выключить и удалить

```bash
claude plugin disable quotalogic@quotalogic-plugins
```

```bash
claude plugin marketplace remove quotalogic-plugins
```

Вторая команда удаляет и плагин. Вход в Quotalogic сбрасывается в `/mcp`: выберите `plugin:quotalogic:quotalogic`, затем **Clear authentication**.

## Если не работает

| Что видите | Что делать |
| --- | --- |
| Нет `plugin:quotalogic:quotalogic` в `/mcp` | Закройте `claude` (`/exit`), запустите заново. Если не помогло: `claude plugin marketplace update quotalogic-plugins` и снова запустите `claude` |
| Статус `needs sign-in` | Выберите сервер, затем **Authenticate** |
| В меню **Re-authenticate**, а не **Authenticate** | Вход уже выполнен. Чтобы войти заново, выберите **Re-authenticate** или **Clear authentication** |
| Не удаётся добавить marketplace | Проверьте, что репозиторий публичный, и повторите команду |
