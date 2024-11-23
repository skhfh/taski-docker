# Taski Docker
### Описание
Taski — это трекер задач, который помогает организовать процесс работы, 
отслеживать прогресс выполнения задач и управлять ими. 
Проект полностью запускается в контейнерах, что обеспечивает удобство 
деплоя и масштабируемость.

### Технологии
- **Backend**: Python 3.9, Django 3.2, DRF 3.12
- **DevOps**: Docker, Docker Compose
- **CI/CD**: GitHub Actions
- **Прочее**: PostgreSQL, Nginx, Telegram API

---

### Как работает автоматический деплой
Процесс автоматического деплоя включает следующие шаги:
1. **Пуш в ветку `main`**:
   - Запускаются тесты и линтеры.
   - Собираются Docker-образы.
   - Готовые образы отправляются в Docker Hub.

2. **Обновление на сервере**:
   - После успешного билда происходит подключение к серверу.
   - Копируется и перезапускается `docker-compose.production.yml`.
   - Выполняются миграции и сборка статики.

3. **Оповещение**:
   - Успешное завершение деплоя сопровождается отправкой уведомления в Telegram-бот.


---

### Разворачивание сервиса вручную на сервере
1. Подготовьте файл `.env` в корне проекта с переменными:
 ```env
   POSTGRES_DB=<название_базы_данных>
   POSTGRES_USER=<пользователь_базы>
   POSTGRES_PASSWORD=<пароль_базы>
   DB_HOST=<адрес_базы>
   DB_PORT=<порт_базы>
 ```
2. Запустите проект, используя файл `docker-compose.production.yml`:
 ```bash
    docker compose -f docker-compose.production.yml up -d
 ```
3. Выполните миграции базы данных:
 ```bash
    docker compose -f docker-compose.production.yml exec backend python manage.py migrate
 ```
4. Соберите и скопируйте статику:
 ```bash
    docker compose -f docker-compose.production.yml exec backend python manage.py collectstatic
    docker compose -f docker-compose.production.yml exec backend cp -r /app/collected_static/. /backend_static/static/
 ```
---
### GitHub Actions
Проект включает CI/CD пайплайн, который:
- Тестирует и линтует код.
- Собирает Docker-образы и пушит их на Docker Hub.
- Деплоит проект на сервер и отправляет уведомление в Telegram.

---
## Автор проекта
[skhfh](https://github.com/skhfh)
