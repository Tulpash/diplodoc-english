# Основные команды Git

## Инициализация и настройка
{% cut "init" %}

Создаёт новый Git-репозиторий в текущей директории.
```bash
git init
```
{% endcut %}

{% cut "clone" %}

Клонирует удалённый репозиторий на локальный компьютер.
```bash
git clone https://github.com/user/repo.git
```
{% endcut %}

{% cut "config" %}

Настраивает параметры Git (например, имя, email).
```bash
git config --global user.name "John Doe"
git config --global user.email "john@example.com"
```
{% endcut %}

## Работа с изменениями
{% cut "status" %}

Показывает состояние рабочей директории и индекса (какие файлы изменены, добавлены в staging).
```bash
git status
```
{% endcut %}

{% cut "add" %}

Добавляет изменения файла в индекс (staging area) для следующего коммита.
```bash
git add Program.cs
git add .  # Добавляет все изменённые файлы
```
{% endcut %}

{% cut "commit" %}

Создаёт коммит с изменениями из индекса, добавляя сообщение.
```bash
git commit -m "Add login feature"
```
{% endcut %}

{% cut "diff" %}

Показывает разницу между рабочей директорией, индексом и последним коммитом.
```bash
git diff Program.cs
```
{% endcut %}

## Работа с ветками
{% cut "branch" %}

Показывает список веток, текущая отмечена звёздочкой (*).
```bash
git branch
```
{% endcut %}

{% cut "branch new" %}

Создаёт новую ветку.
```bash
git branch feature/login
```
{% endcut %}

{% cut "checkout" %}

Переключается на указанную ветку.
```bash
git checkout feature/login
```
{% endcut %}

{% cut "checkout new" %}

Создаёт и сразу переключается на новую ветку.
```bash
git checkout -b feature/login
```
{% endcut %}

{% cut "merge" %}

Сливает указанную ветку в текущую.
```bash
git checkout main
git merge feature/login
```
{% endcut %}

{% cut "delete" %}

Удаляет ветку (если она уже слита).
```bash
git branch -d feature/login
```
{% endcut %}

## Работа с удалённым репозиторием
{% cut "add" %}

Добавляет удалённый репозиторий.
```bash
git remote add origin https://github.com/user/repo.git
```
{% endcut %}

{% cut "push" %}

Отправляет локальные коммиты в удалённый репозиторий.
```bash
git push origin main
```
{% endcut %}

{% cut "pull" %}

Забирает изменения из удалённого репозитория и сливает их в текущую ветку.
```bash
git pull origin main
```
{% endcut %}

{% cut "fetch" %}

Забирает изменения из удалённого репозитория, но не сливает их.
```bash
git fetch origin
```
{% endcut %}

## Работа с историей
{% cut "log" %}

Показывает историю коммитов.
```bash
git log --oneline
```
{% endcut %}

{% cut "show" %}

Показывает детали конкретного коммита.
```bash
git show a1b2c3d
```
{% endcut %}

{% cut "revert" %}

Создаёт новый коммит, отменяющий изменения указанного коммита.
```bash
git revert a1b2c3d
```
{% endcut %}

{% cut "reset" %}

Отменяет коммиты до указанного, изменяя историю (осторожно!).
```bash
git reset --hard a1b2c3d
```
{% endcut %}

## Работа с конфликтами и временным сохранением
{% cut "stash" %}

Сохраняет незакоммиченные изменения во временное хранилище.
```bash
git stash
```
{% endcut %}

{% cut "pop" %}

Восстанавливает последние сохранённые изменения.
```bash
git stash pop
```
{% endcut %}

{% cut "rebase" %}

Переносит коммиты текущей ветки на другую, создавая линейную историю.
```bash
git rebase main
```
{% endcut %}

## Полезные дополнительные команды
{% cut "cherry-pick" %}

Применяет изменения из конкретного коммита к текущей ветке.
```bash
git cherry-pick a1b2c3d
```
{% endcut %}

{% cut "blame" %}

Показывает, кто и когда изменил каждую строку файла.
```bash
git blame Program.cs
```
{% endcut %}

{% cut "clean" %}

Удаляет неотслеживаемые файлы и папки.
```bash
git clean -fd
```
{% endcut %}