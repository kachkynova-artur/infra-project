# Infra project

Учебный инфраструктурный репозиторий курса 

<!-- Допишите сюда -->
Учебный проект: Docker и Ansible.
Третья строка для демонстрации diff.
Проект содержит Docker и Ansible.
Строка для демонстрации git diff.
Строка, добавленная через веб-интерфейс.

## Запуск в Docker

```bash
docker build -t infra-web:1.0 docker/
docker run -d --name infra-web -p 8080:80 infra-web:1.0
curl http://localhost:8080
```
