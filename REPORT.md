# Мидтерм: Инфраструктурный репозиторий

Студент: kachkynov artur
Репозиторий: https://github.com/kachkynova-artur/infra-project

## Задание 1.1. Настройка и первый коммит

Команды:
```
git config --global user.name "kachkynova artur"
git config --global user.email "kachkynov_a@iuca.kg"
git config --global init.defaultBranch main
git clone git@github.com:kachkynova-artur/infra-project.git
cd infra-project
[IO.File]::WriteAllText("$PWD\.gitignore", ".env`n*.log`n*.retry`n")
cat .gitignore
git add .gitignore
git commit -m "chore: add .gitignore"
git push -u origin main
git show --stat HEAD
```

Вывод:
```
.env
*.log
*.retry

commit dad32a71733b5ec71ed4fb6f73f92f13a941b295
    chore: add .gitignore
 .gitignore | 3 +++
 1 file changed, 3 insertions(+)
```

Что произошло: склонировал пустой репозиторий по SSH, создал `.gitignore` с тремя строками и сделал им первый коммит. В коммите только один файл.

## Задание 1.2. Файлы проекта

Команды:
```
Get-ChildItem "$HOME\Downloads\starter\infra-project-starter" -Force | Copy-Item -Destination . -Recurse -Force
Set-Content .env "DB_PASSWORD=secret123" -Encoding ascii
Set-Content app.log "test log" -Encoding ascii
git status
git add README.md .env.example
git commit -m "docs: add README and .env.example"
git add docker/
git commit -m "feat(docker): add Dockerfile and index page"
git add ansible/
git commit -m "feat(ansible): add inventory, playbook and vars"
git push
git ls-files
```

Три отдельных коммита:
```
[main 2210145] docs: add README and .env.example
 2 files changed, 7 insertions(+)
[main c17ff8e] feat(docker): add Dockerfile and index page
 2 files changed, 7 insertions(+)
[main cdafdbf] feat(ansible): add inventory, playbook and vars
 4 files changed, 29 insertions(+)
```

Вывод `git ls-files` (`.env` и `app.log` нет):
```
.env.example
.gitignore
README.md
ansible/ansible.cfg
ansible/group_vars/all.yml
ansible/inventory.ini
ansible/playbook.yml
docker/Dockerfile
docker/index.html
```

Что произошло: файлы `.env` и `app.log` созданы, но Git их не показывает, потому что они в `.gitignore`. Проект закоммичен тремя отдельными коммитами и отправлен на GitHub. Из-за случайного повторного клона в истории появились лишние коммиты и merge-коммит `b34d198`; историю я не переписывал.

## Задание 1.3. git diff

Команды:
```
Add-Content -Path README.md -Value "Третья строка для демонстрации diff." -Encoding utf8
git diff
git add README.md
git diff
git diff --staged
git commit -m "docs: add diff demo line"
```

Вывод `git diff` до `git add`:
```
PS C:\Users\HP\work\infra-project> git diff
warning: in the working copy of 'README.md', LF will be replaced by CRLF the next time Git touches it
diff --git a/README.md b/README.md
index 27ca4e3..f3ab61a 100644
--- a/README.md
+++ b/README.md
@@ -3,3 +3,4 @@
 Учебный инфраструктурный репозиторий курса

 <!-- Допишите сюда -->
+Учебный проект: Docker и Ansible.
Вывод `git diff` после `git add`:
```
(пусто)
```

Вывод `git diff --staged`:

```
PS C:\Users\HP\work\infra-project> git diff --staged
diff --git a/README.md b/README.md
index 27ca4e3..f3ab61a 100644
--- a/README.md
+++ b/README.md
@@ -3,3 +3,4 @@
 Учебный инфраструктурный репозиторий курса

 <!-- Допишите сюда -->
+Учебный проект: Docker и Ansible.```

Почему второй `git diff` пустой: `git diff` без параметров сравнивает рабочие файлы с индексом. После `git add README.md` изменение попало в индекс и совпало с рабочим файлом, поэтому разницы нет. Сравнение индекса с последним коммитом показывает `git diff --staged`.

## Задание 2.1. История

Команды:
```
git log --oneline
git log --oneline --graph --all
git show HEAD~1
```

Вывод `git log --oneline --graph --all` (из файла outputs.txt):
```
PS C:\Users\HP\work\infra-project> git log --oneline --graph --all
* f2a2d22 (HEAD -> main, origin/main, origin/HEAD) docs: add diff demo
*   b34d198 Merge branch 'main' of github.com:kachkynova-artur/infra-project
|\
| * 459e9b9 docs: extend project description
| * 373e50e docs: add README and .env.example
* | 161330a docs: add diff demo line
|/
* 8cae40d docs: describe project in README
* cdafdbf feat(ansible): add inventory, playbook and vars
* c17ff8e feat(docker): add Dockerfile and index page
* 2210145 docs: add README and .env.example
* dad32a7 chore: add .gitignore
PS C:\Users\HP\work\infra-project> git show HEAD~1
commit b34d198ae3d6322c6125b05c9d8b118b5c73bf79
Merge: 161330a 459e9b9
Author: kachkynova artur <kachkynov_a@iuca.kg>
Date:   Mon Oct 26 15:55:23 2026 +0600

    Merge branch 'main' of github.com:kachkynova-artur/infra-project

    # Conflicts:
    #       README.md

diff --cc README.md
index ed3234e,2ae64b1..3e72525
--- a/README.md
+++ b/README.md
@@@ -3,5 -3,4 +3,9 @@@
  Учебный инфраструктурный репозиторий курса

  <!-- Допишите сюда -->
++<<<<<<< HEAD
 +Учебный проект: Docker и Ansible.
 +Третья строка для демонстрации diff.
++=======
+ Проект содержит Docker и Ansible.
```

`HEAD~1` на момент выполнения соответствует коммиту `b34d198 Merge branch 'main' of github.com:kachkynova-artur/infra-project`: это родитель текущего коммита `f2a2d22 docs: add diff demo`.

## Задание 2.2. Ветка и слияние

Команды:
```
git switch -c feature/healthcheck
Add-Content -Path docker\Dockerfile -Value "`nHEALTHCHECK CMD wget -qO- http://localhost/ || exit 1" -Encoding utf8
git add docker/Dockerfile
git commit -m "feat(docker): add healthcheck"
git switch main
git merge feature/healthcheck
git branch -d feature/healthcheck
git push
```

Вывод `git merge`:
```
PS C:\Users\HP\work\infra-project> git merge feature/healthcheck
Updating f2a2d22..1f877bb
Fast-forward
 docker/Dockerfile | 2 ++
 1 file changed, 2 insertions(+)```

Что произошло: создал ветку, добавил HEALTHCHECK, слил в main без merge-коммита (fast-forward) и удалил ветку.

## Задание 2.3. Конфликт

Команды:
```
git switch -c fix/app-port
(Get-Content ansible\group_vars\all.yml -Encoding utf8) -replace '^app_port:.*','app_port: 9090' | Set-Content ansible\group_vars\all.yml -Encoding utf8
git commit -am "fix(ansible): set app_port to 9090"
git switch main
(Get-Content ansible\group_vars\all.yml -Encoding utf8) -replace '^app_port:.*','app_port: 8000' | Set-Content ansible\group_vars\all.yml -Encoding utf8
git commit -am "fix(ansible): set app_port to 8000"
git merge fix/app-port
cat ansible\group_vars\all.yml
```

Содержимое `all.yml` с маркерами конфликта (то, что было видно в терминале при разрешении):
```
---
<<<<<<< HEAD
app_port: 8000
=======
```

Итоговый `all.yml` (из outputs.txt, маркеров нет):
```
PS C:\Users\HP\work\infra-project> cat ansible\group_vars\all.yml
---
<<<<<<< HEAD
app_port: 8000
=======
app_port: 9090
>>>>>>> fix/app-port
app_user: deploy

Вывод `git log --oneline --graph` (виден merge-коммит `de68a93`):
```
*   de68a93 Merge branch 'fix/app-port'
|\
| * 002d15a fix(ansible): set app_port to 9090
* | 45b57d9 fix(ansible): set app_port to 8000
|/
* 1f877bb feat(docker): add healthcheck
```

Что произошло: в двух ветках изменил одну строку `app_port`, Git поставил маркеры конфликта. При ручном разрешении я по ошибке удалил не все маркеры и закоммитил, поэтому отдельным коммитом `d636137` убрал остатки. Историю не переписывал.

## Задание 3.1. Pull с сервера

Команды и результат: в веб-интерфейсе GitHub отредактировал README.md (коммит `5e48f09 Update README.md` в main), затем локально:
```
git pull
 README.md | 1 +
 Fast-forward
git log --oneline
```
Что произошло: коммит из веб-интерфейса появился локально после `git pull`.

## Задание 3.2. Merge Request

Команды:
```
git switch -c feature/ansible-curl
Add-Content -Path ansible\playbook.yml -Value "`n    - name: Install curl`n      ansible.builtin.package:`n        name: curl`n        state: present" -Encoding utf8
git commit -am "feat(ansible): install curl"
git push -u origin feature/ansible-curl
git switch main
git pull
```

Pull Request #1: https://github.com/kachkynova-artur/infra-project/pull/1 (слит в main, коммит `66b3f6f`).
Описание: «Добавлена задача установки curl в playbook. Curl нужен для проверки доступности сервисов на хостах.»

Что произошло: изменение оформлено через PR с описанием и слито через веб-интерфейс.

## Задание 3.3. Уборка веток

Команды:
```
git branch -d feature/ansible-curl
git push origin --delete feature/ansible-curl
git fetch --prune
git branch -a
```

Вывод `git branch -a`:
```
  fix/app-port
* main
  remotes/origin/HEAD -> origin/main
  remotes/origin/main
```
Что произошло: ветка `feature/ansible-curl` удалена локально и на сервере и больше не отображается.

## Задание 4.1. Docker

Команды:
```
git switch -c feature/docker-page
docker build -t infra-web:1.0 docker/
docker run -d --name infra-web -p 8080:80 infra-web:1.0
curl.exe http://localhost:8080
docker rm -f infra-web
git commit -m "feat(docker): add personal page and expose port"
git push -u origin feature/docker-page
```

Последние строки `docker build`:
```
 => [2/2] COPY index.html /usr/share/nginx/html/
 => exporting to image
 => => naming to docker.io/library/infra-web:1.0
 => => unpacking to docker.io/library/infra-web:1.0
```

Вывод `curl.exe http://localhost:8080`:
```
<!DOCTYPE html>
<html lang="ru">
<head><meta charset="UTF-8"><title>infra-project</title></head>
<body>
  <h1>Kachkynova Artur</h1>
  <p>Проект: infra-project</p>
</body>
</html>
```

Pull Request #2: https://github.com/kachkynova-artur/infra-project/pull/2 (слит, коммит `a343061`). В `docker/Dockerfile` есть `EXPOSE 80`, в README есть раздел «Запуск в Docker».

Что произошло: собрал образ, запустил контейнер, проверил страницу через curl, удалил контейнер и оформил изменения через PR.

## Задание 5.1. restore и amend

Команды:
```
Add-Content -Path docker\Dockerfile -Value "# lishnyaya stroka" -Encoding utf8
git restore docker/Dockerfile
git commit -m "docs: updte readme"
git commit --amend -m "docs: update readme"
git push
```
Что произошло: лишняя строка отменена `git restore`. Сообщение коммита с опечаткой исправлено через `--amend` до push, в истории только `d16c309 docs: update readme`.

## Задание 5.2. revert

Команды:
```
git commit -am "chore: remove *.log from .gitignore"
git push
git revert 696043b --no-edit
git push
```
Вывод:
```
[main ee177cc] Revert "chore: remove *.log from .gitignore"
```
Содержимое `.gitignore` после revert: `.env`, `*.log`, `*.retry`.

Почему revert: коммит уже на сервере и мог попасть к другим людям. Удаление из истории разойдётся с их копиями. `git revert` добавляет новый коммит, отменяющий изменения, и история остаётся согласованной.

## Задание 5.3. Контрольные вопросы

1. **Коммит** это снимок состояния проекта с автором, датой, сообщением и ссылкой на родителя. **Индекс** (staging area) это промежуточная область, где я выбираю, какие изменения войдут в следующий коммит, и могу коммитить только часть правок.
2. **Ветка** это подвижный указатель на коммит, по сути отдельная линия разработки.
3. **HEAD** это указатель на текущее положение: обычно на текущую ветку, а через неё на последний коммит.
4. `git fetch` только скачивает изменения с сервера, не меняя мои файлы и ветки. `git pull` это fetch плюс слияние в текущую ветку.
5. В `.env` лежат пароли и токены. Попав в репозиторий, они остаются в истории навсегда и доступны всем, у кого есть доступ к репозиторию.