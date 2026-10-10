# Obsidian skills for Codex

Три скилла для переноса в Codex на другой компьютер:

- `obsidian-cli` — работа с хранилищем через Obsidian CLI.
- `obsidian-markdown` — Markdown, ссылки, вставки, свойства и callouts Obsidian.
- `obsidian-worklog` — ограниченное восстановление инженерного контекста и сохранение полезных выводов.

## Установка на Windows

Скачайте репозиторий или клонируйте его, откройте PowerShell в его каталоге и выполните:

```powershell
$codexSkillsHome = if ($env:CODEX_HOME) { Join-Path $env:CODEX_HOME 'skills' } else { Join-Path $env:USERPROFILE '.codex\skills' }
New-Item -ItemType Directory -Path $codexSkillsHome -Force | Out-Null
foreach ($skillName in @('obsidian-cli', 'obsidian-markdown', 'obsidian-worklog')) {
    $targetPath = Join-Path $codexSkillsHome $skillName
    if (Test-Path -LiteralPath $targetPath) { throw "Скилл уже существует: $targetPath. Сначала сохраните или переименуйте его." }
    Copy-Item -LiteralPath (Join-Path '.\skills' $skillName) -Destination $codexSkillsHome -Recurse
}
```

Начните новый чат Codex после установки. Скиллы можно вызвать явно: `$obsidian-cli`, `$obsidian-markdown`, `$obsidian-worklog`.

## Настройка Worklog на новом ПК

Пути к хранилищу и личные заметки не включены. Вызовите `$obsidian-worklog` и попросите настроить его, указав абсолютный путь к существующему хранилищу Obsidian.

Конфигурация хранится вне скилла: `%USERPROFILE%\.codex\obsidian-worklog.json` (или в `CODEX_HOME`, если он задан):

```json
{
  "vault_path": "C:\\path\\to\\your\\vault",
  "worklog_root": "AI/Worklog"
}
```

Само хранилище переносится отдельно. `obsidian-cli` требует установленного Obsidian с доступным CLI. Worklog поддерживает работу с файлами, если CLI недоступен.
