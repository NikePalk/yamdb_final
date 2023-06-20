![Yamdb Workflow Status](https://github.com/nikepalk/yamdb_final/actions/workflows/yamdb_workflow.yml/badge.svg)
# CI и CD проекта api_yamdb
REST API проект для сервиса YaMDb — сбор отзывов о фильмах, книгах или музыке.

## Развёрнутый проект
- http://84.201.179.88/admin/
- http://84.201.179.88/redoc/


## Описание
Проект YaMDb собирает отзывы пользователей на произведения.
Произведения делятся на категории: «Книги», «Фильмы», «Музыка».
Список категорий может быть расширен (например, можно добавить категорию «Изобразительное искусство» или «Ювелирка»).
Настройка для приложения Continuous Integration и Continuous Deployment:
- автоматический запуск тестов,
- обновление образов на Docker Hub,
- автоматический деплой на боевой сервер при пуше в главную ветку main.


### Шаблон наполнения .env (не включен в текущий репозиторий) расположенный в infra/.env
```
DB_ENGINE=django.db.backends.postgresql
DB_NAME=postgres
POSTGRES_USER=postgres
POSTGRES_PASSWORD=postgres
DB_HOST=db
DB_PORT=5432
```

### Как запустить проект:
Все описанное ниже относится к ОС Windows.
Клонируем репозиторий и переходим в него:
```bash
git clone git@github.com:NikePalk/yamdb_final.git
cd yamdb_final
```

Создаем и активируем виртуальное окружение:
```bash
python -m venv venv
source venv/Scripts/activate
python -m pip install --upgrade pip
```

Ставим зависимости из requirements.txt:
```bash
pip install -r requirements.txt
```

Переходим в папку с файлом docker-compose.yaml:
```bash
cd infra
```

Поднимаем контейнеры (infra_db_1, infra_web_1, infra_nginx_1):
```bash
docker-compose up -d --build
```

Выполняем миграции:
```bash
docker-compose exec web python manage.py migrate
```

Создаем суперпользователя:
```bash
docker-compose exec web python manage.py createsuperuser
```

Собираем статику:
```bash
docker-compose exec web python manage.py collectstatic --no-input
```

Создаем дамп базы данных (нет в текущем репозитории):
```bash
docker-compose exec web python manage.py dumpdata > dumpPostrgeSQL.json
```

Останавливаем контейнеры:
```bash
docker-compose down -v
```
## Автор:
Никита Палкин
