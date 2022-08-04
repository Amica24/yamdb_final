# yamdb_final
![yamdb_final workflow](https://github.com/Amica24/yamdb_final/actions/workflows/yamdb_workflow.yml/badge.svg)

Проект развернут по адресу http://51.250.21.244/redoc/
## Описание
Для приложения api_yamdb настроены Continuous Integration и Continuous Deployment:

* автоматический запуск тестов, 

* обновление образов на Docker Hub,

* автоматический деплой на боевой сервер при пуше в главную ветку main.

Проект **YaMDb** собирает отзывы пользователей на различные произведения.

### Алгоритм регистрации пользователей

    1. Пользователь отправляет POST-запрос на добавление нового пользователя с параметрами `email` и `username` на эндпоинт `/api/v1/auth/signup/`.
    2. **YaMDB** отправляет письмо с кодом подтверждения (`confirmation_code`) на адрес  `email`.
    3. Пользователь отправляет POST-запрос с параметрами `username` и `confirmation_code` на эндпоинт `/api/v1/auth/token/`, в ответе на запрос ему приходит `token` (JWT-токен).
    4. При желании пользователь отправляет PATCH-запрос на эндпоинт `/api/v1/users/me/` и заполняет поля в своём профайле (описание полей — в документации).


## Paths

Запросы к API начинаются с `/api/v1/`
      
Пример запроса:


**/titles/**

```
get:
```
* Получить список всех объектов
* Права доступа: Доступно без токена.

Пример ответа для GET-запроса:
```
[
  {
    "count": 0,
    "next": "string",
    "previous": "string",
    "results": [
      {
        "id": 0,
        "name": "string",
        "year": 0,
        "rating": 0,
        "description": "string",
        "genre": [
          {
            "name": "string",
            "slug": "string"
          }
        ],
        "category": {
          "name": "string",
          "slug": "string"
        }
      }
    ]
  }
]
```
```
post:
```
* Добавить новое произведение
* Права доступа: Администратор.

Пример POST-запроса:
```
{
  "name": "string",
  "year": 0,
  "description": "string",
  "genre": [
    "string"
  ],
  "category": "string"
}    
```
Пример ответа при удачном выполнении POST-запроса:
```
{
  "id": 0,
  "name": "string",
  "year": 0,
  "rating": 0,
  "description": "string",
  "genre": [
    {
      "name": "string",
      "slug": "string"
    }
  ],
  "category": {
    "name": "string",
    "slug": "string"
  }
}
```

**/titles/{titles_id}/**


    get:
* Информация о произведении
* Права доступа: Доступно без токена. 

Пример ответа для GET-запроса:
```
{
  "id": 0,
  "name": "string",
  "year": 0,
  "rating": 0,
  "description": "string",
  "genre": [
    {
      "name": "string",
      "slug": "string"
    }
  ],
  "category": {
    "name": "string",
    "slug": "string"
  }
}
```

    patch:
* Обновить информацию о произведении
* Права доступа: Администратор.

Пример PATCH-запроса:
```
{
  "name": "string",
  "year": 0,
  "description": "string",
  "genre": [
    "string"
  ],
  "category": "string"
}
```

Пример ответа при удачном выполнении PATCH-запроса:
```
{
  "id": 0,
  "name": "string",
  "year": 0,
  "rating": 0,
  "description": "string",
  "genre": [
    {
      "name": "string",
      "slug": "string"
    }
  ],
  "category": {
    "name": "string",
    "slug": "string"
  }
}
```
    delete:
* Удалить произведение
* Права доступа: Администратор.

## Шаблон наполнения env-файла

`DB_ENGINE` # указываем, что работаем с postgresql

`DB_NAME` # имя базы данных

`POSTGRES_USER` # логин для подключения к базе данных

`POSTGRES_PASSWORD` # пароль для подключения к БД

`DB_HOST` # название сервиса (контейнера)

`DB_PORT` # порт для подключения к БД

`SECRET_KEY` # секретный ключ

## Описание команд для запуска приложения в контейнерах

Приложения запускаются следующими командами:

`docker-compose up -d --build` # Пересобрать контейнеры

`docker-compose exec web python manage.py migrate` # Выполнить миграции

`docker-compose exec web python manage.py createsuperuser` # Создать суперпользователя

`docker-compose exec web python manage.py collectstatic --no-input` # Собрать статику

`docker-compose down -v` # Остановить и удалить собранные контейнеры

## Команда для заполнения базы данными

docker-compose exec web python manage.py dumpdata > fixtures.json