<div align="left">

[![YaMDb workflow](https://github.com/Ivan-Grebennikov/yamdb_final/actions/workflows/yamdb_workflow.yml/badge.svg)](https://github.com/Ivan-Grebennikov/yamdb_final/actions)
[![Deployed service link](https://img.shields.io/badge/Service-deployed-green.svg)](http://greivan-yamdb.ddns.net/redoc)

</div>

## Сервис YaMDb

Проект **YaMDb** является сервисом, который собирает отзывы пользователей на произведения. Произведения делятся на категории, например «Книги», «Фильмы» и «Музыка». Список категорий может быть расширен администратором.
Произведениям может быть присвоен жанр из списка, который определяет администратор.
Пользователи могут оставлять текстовые отзывы к произведениям и ставить произведению оценку в диапазоне от одного до десяти — из пользовательских оценок формируется усреднённая оценка произведения — рейтинг.

В сервисе реализована ролевая модель — существуют как обычные пользователи, так и модераторы и администраторы.
Модераторы имеют право удалять и редактировать любые отзывы и комментарии, администраторы имеют максимальные права на изменение данных сервиса.

### Используемые технологии:

Сервис реализован с помощью фреймворков **Django**, **Django Rest Framework** и предоставляет REST API для доступа к своему функционалу. Сервис разворачивается с помощью **Docker** и использует веб-сервер **Nginx** и сервер приложений **Gunicorn**.

### Как запустить проект:

Клонировать репозиторий и перейти в него в командной строке:

```
git clone https://github.com/Ivan-Grebennikov/infra_sp2.git
```

```
cd infra_sp2
```

Перейти в папку, содержащую файлы для развертывания проекта:

```
cd infra
```

Создать файл ``` .env ```, содержащий значения переменных среды для развертывания проекта:

```
touch .env
```

Заполнить файл ``` .env ``` по образцу:

```
DJANGO_SECRET_KEY='p&l%385148kslhtyn^##a1)ilz@4zqj=rq&agdol^##zgl9(vs' # приватный ключ Django
DB_ENGINE=django.db.backends.postgresql # указываем, что работаем с postgresql
DB_NAME=postgres # имя базы данных
POSTGRES_USER=postgres # логин для подключения к базе данных
POSTGRES_PASSWORD=postgres # пароль для подключения к БД (установить свой)
DB_HOST=db # название сервиса (контейнера)
DB_PORT=5432 # порт для подключения к БД
```

Ключ ``` DJANGO_SECRET_KEY ``` можно сгенерировать с помощью сервиса https://djecrety.ir/

Значение ключа при заполении файла ``` .env ``` стоит обернуть в кавычки.

Создать и запустить docker контейнеры:

```
sudo docker compose up -d
```

Выполнить миграции:

```
sudo docker compose exec web python manage.py migrate
```

Заполнить базу данных тестовыми значениями из файла дампа БД:

```
sudo docker compose exec web python manage.py loaddata fixtures.json
```

Создать суперпользователя для доступа в админ зону проекта:

```
sudo docker compose exec web python manage.py createsuperuser
```

Настроить доступ к статическим файлам проекта:

```
sudo docker compose exec web python manage.py collectstatic --no-input 
```

Сервис будет запущен по локальному адресу:

http://localhost/

Для доступа в админ-зону сервиса необходимо перейти по адресу:

http://localhost/admin/

### Работа с REST API

Пример запроса:

```
GET /api/v1/categories/ - Получение списка всех категорий
```

Ответ сервиса:

```
{
    "count": 3,
    "next": null,
    "previous": null,
    "results": [
        {
            "name": "Музыка",
            "slug": "music"
        },
        {
            "name": "Книга",
            "slug": "book"
        },
        {
            "name": "Фильм",
            "slug": "movie"
        }
    ]
}
```

Пример запроса:

```
GET /api/v1/genres/ - Получение списка всех жанров
```

Ответ сервиса:

```
{
    "count": 15,
    "next": "http://localhost/api/v1/genres/?page=2",
    "previous": null,
    "results": [
        {
            "name": "Шансон",
            "slug": "chanson"
        },
        {
            "name": "Рок",
            "slug": "rock"
        },
        {
            "name": "Классика",
            "slug": "classical"
        },
        {
            "name": "Rock-n-roll",
            "slug": "rock-n-roll"
        },
        {
            "name": "Баллада",
            "slug": "ballad"
        },
        {
            "name": "Роман",
            "slug": "roman"
        },
        {
            "name": "Гонзо",
            "slug": "gonzo"
        },
        {
            "name": "Сказка",
            "slug": "tale"
        },
        {
            "name": "Триллер",
            "slug": "thriller"
        },
        {
            "name": "Детектив",
            "slug": "detective"
        }
    ]
}
```

Пример запроса:

```
GET /api/v1/titles/ - Получение списка всех произведений
```

Ответ сервиса:

```
{
    "count": 32,
    "next": "http://localhost/api/v1/titles/?page=2",
    "previous": null,
    "results": [
        {
            "id": 3,
            "rating": 7,
            "category": {
                "name": "Фильм",
                "slug": "movie"
            },
            "genre": [
                {
                    "name": "Драма",
                    "slug": "drama"
                }
            ],
            "name": "12 разгневанных мужчин",
            "year": 1957,
            "description": ""
        },
        {
            "id": 30,
            "rating": 10,
            "category": {
                "name": "Музыка",
                "slug": "music"
            },
            "genre": [
                {
                    "name": "Рок",
                    "slug": "rock"
                }
            ],
            "name": "Deep Purple — Smoke on the Water",
            "year": 1971,
            "description": ""
        },
        {
            "id": 29,
            "rating": 10,
            "category": {
                "name": "Музыка",
                "slug": "music"
            },
            "genre": [
                {
                    "name": "Rock-n-roll",
                    "slug": "rock-n-roll"
                }
            ],
            "name": "Elvis Presley - Blue Suede Shoes",
            "year": 1955,
            "description": ""
        },
        {
            "id": 24,
            "rating": 5,
            "category": {
                "name": "Книга",
                "slug": "book"
            },
            "genre": [
                {
                    "name": "Роман",
                    "slug": "roman"
                }
            ],
            "name": "Generation П",
            "year": 1999,
            "description": ""
        },
        {
            "id": 19,
            "rating": 9,
            "category": {
                "name": "Фильм",
                "slug": "movie"
            },
            "genre": [
                {
                    "name": "Драма",
                    "slug": "drama"
                }
            ],
            "name": "Generation П",
            "year": 2011,
            "description": ""
        },
        {
            "id": 28,
            "rating": 10,
            "category": {
                "name": "Музыка",
                "slug": "music"
            },
            "genre": [
                {
                    "name": "Баллада",
                    "slug": "ballad"
                }
            ],
            "name": "Jethro Tull - Aqualung",
            "year": 1971,
            "description": ""
        },
        {
            "id": 27,
            "rating": 1,
            "category": {
                "name": "Музыка",
                "slug": "music"
            },
            "genre": [
                {
                    "name": "Рок",
                    "slug": "rock"
                },
                {
                    "name": "Баллада",
                    "slug": "ballad"
                }
            ],
            "name": "Led Zeppelin — Stairway to Heaven",
            "year": 1971,
            "description": ""
        },
        {
            "id": 32,
            "rating": 10,
            "category": {
                "name": "Музыка",
                "slug": "music"
            },
            "genre": [
                {
                    "name": "Классика",
                    "slug": "classical"
                }
            ],
            "name": "Бах. Оркестровая Сюита №2 си минор",
            "year": 1739,
            "description": ""
        },
        {
            "id": 8,
            "rating": 2,
            "category": {
                "name": "Фильм",
                "slug": "movie"
            },
            "genre": [
                {
                    "name": "Триллер",
                    "slug": "thriller"
                }
            ],
            "name": "Бойцовский клуб",
            "year": 1999,
            "description": ""
        },
        {
            "id": 25,
            "rating": 10,
            "category": {
                "name": "Книга",
                "slug": "book"
            },
            "genre": [
                {
                    "name": "Сказка",
                    "slug": "tale"
                }
            ],
            "name": "Винни Пух и все-все-все",
            "year": 1926,
            "description": ""
        }
    ]
}
```

Для получения подробного описания REST API необходимо перейти на 

http://localhost/redoc/

### Остановка сервиса:

Для остановки сервиса необходимо выполнить команду:

```
sudo docker compose down -v
```
