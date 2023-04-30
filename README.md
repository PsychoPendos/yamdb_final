![workflow](https://github.com/PsychoPendos/yamdb_final/actions/workflows/yamdb_workflow.yml/badge.svg)
## Api_YaMDb
Проект YaMDb собирает отзывы пользователей на произведения.
Сами произведения в YaMDb не хранятся, здесь нельзя посмотреть фильм или послушать музыку.
### Благодаря этому проекту пользователи смогут:
 - Зарегистрироваться
 - Оставить отзыв и оценку на произведение
 - Оставить комментарий к отзыву 
 - многое многое другое
### Запуск проекта в docker:
#### Подготовка .env файла:

Для запуска необходимо создать .env файл с наполнением по следующему шаблону:
```
SECRET_KEY=*** # Секрет для запуска проекта

DB_ENGINE=django.db.backends.postgresql # указание использовать PostgreSQL

DB_NAME=*** # имя базы данных

POSTGRES_USER=*** # логин для подключения к базе данных

POSTGRES_PASSWORD=*** # пароль для подключения к БД (установите свой)

DB_HOST=*** # название сервиса (контейнера)

DB_PORT=*** # порт для подключения к БД

```
#### Запуск Docker-compose:

Для запуска контейнера нужно выполнить следующую команду из папки ./infra/:
```
docker-compose up -d
```
После запуска необходимо выполнить миграции:
```
docker-compose exec web python manage.py migrate
```
Далее создаём суперпользователя:
```
docker-compose exec web python manage.py createsuperuser
```
И собираем статику:
```
docker-compose exec web python manage.py collectstatic --no-input 
```
Для наполнения базы данных из фикстур необходимо выполнить следующую комманду:
```
docker-compose exec web python manage.py loaddata fixtures/fixtures
```

Для остановки контейнера нужно выполнить следующую команду:
```
docker-compose stop
```
p.s. Доступен ReDoc – API, сгенерированный OpenAPI / Swagger по
ссылке "адрес_сервера/redoc"


### Авторы 
- Федор Петринчук
- Виктория Федорова
- Артур Костенков

### Примеры запросов:
***Регистрация***
- POST: 
http://127.0.0.1:8000/api/v1/auth/signup/
```
{
    "email": "user@example.com",
    "username": "user1"
}
```

***Получение токена***
- POST: 
http://127.0.0.1:8000/api/v1/auth/token/
```
{
    "username": "user1",
    "confirmation_code": "string"
}
```
Результат
```
{
    "token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ0b2tlbl90eXBlIjoiYWNjZXNzIiwiZXhwIjoxNjc0NjgwNjkwLCJpYXQiOjE2NzQ1OTQyOTAsImp0aSI6IjYyOGUxODliZTZkZTRlMWNiMGQ5MThiMDkwYTIyMTk1IiwidXNlcl9pZCI6MX0.yZldqDRF2jhLzqbAcO4X4u7C3Bki63IRgFZilu8d0tc"
}
```
***Получение информации о произведении***
- GET: 
http://127.0.0.1:8000/api/v1/titles/1/
```
{
    "id": 1,
    "name": "Кот в сапогах",
    "year": 2023,
    "rating": 6,
    "description": "Мультфильм о коте",
    "genre": ["Мультфильм"],
    "category": {
        "name": "Для детей",
        "slug": "baby"
    }
}
```
***Получение списка всех отзывов на произведение***
- GET: 
http://127.0.0.1:8000/api/v1/titles/{title_id}/reviews/
```
{
"count": 0,
"next": "string",
"previous": "string",
"results": [
        {
        "id": 0,
        "text": "string",
        "author": "string",
        "score": 1,
        "pub_date": "2019-08-24T14:15:22Z"
        }
    ]
}
```

***Добавление нового отзыва на произведение***
- POST: 
http://127.0.0.1:8000/api/v1/titles/{title_id}/reviews/
#### Payload
```
{
    "text": "string",
    "score": 1
}
```
#### Response (200)
```
{
    "id": 0,
    "text": "string",
    "author": "string",
    "score": 1,
    "pub_date": "2019-08-24T14:15:22Z"
}
```
***Получение списка всех комментариев к отзыву***
- GET: 
http://127.0.0.1:8000/api/v1/titles/{title_id}/reviews/{review_id}/comments/
```
{
    "count": 0,
    "next": "string",
    "previous": "string",
    "results": [
        {
            "id": 0,
            "text": "string",
            "author": "string",
            "pub_date": "2019-08-24T14:15:22Z"
        }
    ]
}
```
***Добавление комментария к отзыву***
- POST: 
http://127.0.0.1:8000/api/v1/titles/{title_id}/reviews/{review_id}/comments/
#### Payload
```
{
  "text": "string"
}
```
#### Response (200)
```
{
  "id": 0,
  "text": "string",
  "author": "string",
  "pub_date": "2019-08-24T14:15:22Z"
}
```
