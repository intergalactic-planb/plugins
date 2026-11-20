# Codex в терминале

[English](../en/codex-terminal.md) · **Русский** · [中文](../cn/codex-terminal.md)

Для `codex` в терминале. Для приложения ChatGPT: [Приложение ChatGPT](codex-desktop.md).

Репозиторий `intergalactic-planb/plugins` должен быть публичным.

## Установить

В терминале выполните по очереди:

```bash
codex plugin marketplace add intergalactic-planb/plugins
```

```bash
codex plugin add quotalogic@quotalogic-plugins
```

## Войти в Quotalogic

1. Выполните:

   ```bash
   codex mcp login quotalogic
   ```

2. Откроется браузер. Войдите в Quotalogic, проверьте рабочее пространство и подтвердите.
3. В терминале появится `Successfully logged in to MCP server 'quotalogic'`.
4. Проверьте вход: `codex mcp list`. У `quotalogic` в колонке `Auth` стоит `OAuth`.
5. Запустите `codex` и спросите: «Покажи мои каталоги». Появится список ваших каталогов.

## Обновить

```bash
codex plugin marketplace upgrade
```

```bash
codex plugin add quotalogic@quotalogic-plugins
```

Затем закройте `codex` и запустите заново.

## Выключить и удалить

Если нужно сбросить и вход, сначала выполните:

```bash
codex mcp logout quotalogic
```

Затем удалите плагин и маркетплейс:

```bash
codex plugin remove quotalogic@quotalogic-plugins
```

```bash
codex plugin marketplace remove quotalogic-plugins
```

## Если не работает

| Что видите | Что делать |
| --- | --- |
| В `codex mcp list` у `quotalogic` в колонке `Auth` стоит `Not logged in` | `codex mcp login quotalogic` |
| Codex отвечает, что нет инструментов Quotalogic | Проверьте `codex mcp list`: `quotalogic` должен быть `enabled` и `OAuth`. Если нет, войдите (раздел «Войти в Quotalogic»). Затем запустите `codex` заново |
| `codex plugin list` выводит очень длинный список | Покажите только нужный маркетплейс: `codex plugin list --marketplace quotalogic-plugins` |
| В `codex mcp list` две строки с адресом Quotalogic | Одну из них вы добавили вручную раньше. Удалите её: `codex mcp remove <name>`, где `<name>` берётся из `codex mcp list` (не `quotalogic`) |
