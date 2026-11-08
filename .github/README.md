# Reusable workflows

Этот репозиторий содержит reusable workflows. Автоматические triggers здесь не определены: workflow запускается caller-файлом в repository-потребителе.

## Базовые файлы

Caller-файлы для Go quality, workflow lint, dependency review, CodeQL и Release Please находятся в `.github/workflows/`. Конфигурация Release Please находится в `configs/`.

Каждый caller сначала проверяет наличие файлов `*.go`. Если Go-кода нет, основной job получает статус skipped.

Значения `VALUE` — placeholders. Их нужно заменить настройками конкретного repository-потребителя.

## Secrets и permissions

GitHub repository variables для базовой конфигурации не нужны. Стабильные значения зафиксированы в YAML и generated callers.

Для Release Please нужен только secret `RELEASE_PLEASE_TOKEN`. Caller передаёт его в reusable workflow через `secrets.token`.

Reusable workflows:

- `go-quality-reusable.yml` — `go mod tidy`, tests, build, race detector, `go vet`, `gofumpt`, `gci`, `golangci-lint`, `govulncheck`;
- `workflow-lint-reusable.yml` — `actionlint`;
- `dependency-review-reusable.yml` — проверка новых зависимостей в pull request;
- `codeql-reusable.yml` — CodeQL Code Scanning;
- `release-reusable.yml` — Release Please.

Deployment workflows требуют отдельного target-specific reusable workflow и GitHub Environment. Универсальный workflow с произвольной shell-командой через input не используется.
