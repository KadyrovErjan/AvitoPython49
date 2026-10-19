<div align="center">

# 🛒 Avito Python-49

**REST API для онлайн-доски объявлений в стиле Avito**

Каталог товаров · Корзина · Избранное · Отзывы · JWT-авторизация · Мультиязычность

![Python](https://img.shields.io/badge/Python-3.12-3776AB?style=for-the-badge&logo=python&logoColor=white)
![Django](https://img.shields.io/badge/Django-REST_Framework-092E20?style=for-the-badge&logo=django&logoColor=white)
![JWT](https://img.shields.io/badge/Auth-JWT-000000?style=for-the-badge&logo=jsonwebtokens&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-Compose-2496ED?style=for-the-badge&logo=docker&logoColor=white)
![Nginx](https://img.shields.io/badge/Nginx-009639?style=for-the-badge&logo=nginx&logoColor=white)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-17-4169E1?style=for-the-badge&logo=postgresql&logoColor=white)

</div>

---

## 📖 О проекте

**Avito Python-49** — это backend-часть маркетплейса, написанная на **Django** и **Django REST Framework**. Проект предоставляет полноценный API для работы с каталогом объявлений: категории и подкатегории, товары с фото и видео, отзывы с рейтингом, корзина и избранное. Документация API доступна через **Swagger UI**.

## ✨ Возможности

- 🔐 **Аутентификация** — регистрация, вход и выход через **JWT** (access 60 минут, refresh 7 дней, ротация и blacklist токенов)
- 🌐 **Социальный вход** — подключены GitHub и Google через `django-allauth`
- 🗂 **Каталог** — категории → подкатегории → товары, у каждого своё изображение
- 📦 **Товары** — несколько фото, видео-обзор, артикул, цена, описание, дата создания и обновления
- 🔎 **Поиск, фильтры, сортировка** — по названию, подкатегории, цене (`gt` / `lt`), артикулу; сортировка по цене и дате
- 📄 **Пагинация** списка товаров
- ⭐ **Отзывы** — оценка от 1 до 5, комментарий, фото; средний рейтинг и количество оценок считаются автоматически
- 🛍 **Корзина** — добавление, изменение количества, удаление, подсчёт общей суммы
- ❤️ **Избранное** — добавление и удаление товаров
- 🏅 **Статусы пользователей** — `gold`, `silver`, `bronze`, `simple`
- 🌍 **Мультиязычность (EN / RU)** — названия категорий, подкатегорий, товаров и описаний через `django-modeltranslation`
- 📚 **Swagger-документация** — интерактивная страница `/docs/`
- 🐳 **Docker-деплой** — `web` (Gunicorn) + `db` (PostgreSQL) + `nginx`

## 🧰 Технологии

| Область | Инструменты |
|---|---|
| Язык | Python 3.12 |
| Фреймворк | Django, Django REST Framework |
| Авторизация | SimpleJWT, django-allauth (GitHub, Google) |
| Фильтрация | django-filter, SearchFilter, OrderingFilter |
| Переводы | django-modeltranslation |
| Документация | drf-yasg (Swagger) |
| Телефоны | django-phonenumber-field |
| Медиа | Pillow |
| CORS | django-cors-headers |
| Деплой | Docker, Docker Compose, Gunicorn, Nginx, PostgreSQL 17 |

## 🗃 Модели данных

```
UserProfile ─┬─ Cart ──────── CartItem ───┐
             ├─ Favorite ──── FavoriteItem ┤
             └─ Review ────────────────────┤
                                           ▼
Category ── SubCategory ── Product ── ProductImage
```

| Модель | Описание |
|---|---|
| `UserProfile` | Пользователь (наследник `AbstractUser`): возраст, телефон (по умолчанию `+996`), аватар, статус |
| `Category` / `SubCategory` | Категории и подкатегории с изображениями |
| `Product` | Товар: название, цена, описание, артикул, тип, видео |
| `ProductImage` | Фотографии товара |
| `Review` | Отзыв: оценка 1–5, комментарий, фото |
| `Cart` / `CartItem` | Корзина пользователя и её позиции |
| `Favorite` / `FavoriteItem` | Избранное пользователя и его позиции |

## 🔌 API Endpoints

> Все маршруты находятся под языковым префиксом (`/en/...`, `/ru/...`), так как используется `i18n_patterns`.

### Аутентификация

| Метод | URL | Описание |
|---|---|---|
| `POST` | `/register/` | Регистрация |
| `POST` | `/login/` | Вход, получение JWT-токенов |
| `POST` | `/logout/` | Выход |

### Пользователи

| Метод | URL | Описание |
|---|---|---|
| `GET` | `/user/` | Список пользователей |
| `GET` `PUT` `PATCH` `DELETE` | `/user/<id>/` | Профиль пользователя |

### Каталог

| Метод | URL | Описание |
|---|---|---|
| `GET` | `/category/` · `/category/<id>/` | Категории |
| `GET` | `/subcategory/` · `/subcategory/<id>/` | Подкатегории |
| `GET` | `/product/` | Список товаров (поиск, фильтры, сортировка, пагинация) |
| `GET` | `/product/<id>/` | Детали товара |

### Отзывы, корзина, избранное

| Метод | URL | Описание |
|---|---|---|
| `GET` `POST` `PUT` `PATCH` `DELETE` | `/review/` · `/review/<id>/` | Отзывы (чтение — всем, запись — авторизованным) |
| `GET` | `/cart/` | Корзина текущего пользователя |
| `GET` `POST` | `/cart_item/` | Позиции корзины |
| `PUT` `DELETE` | `/cart_item/<id>/` | Изменить или удалить позицию |
| `GET` | `/favorite/` | Избранное текущего пользователя |
| `GET` `POST` | `/favorite_item/` | Позиции избранного |
| `DELETE` | `/favorite_item/<id>` | Удалить из избранного |

### Служебные

| URL | Описание |
|---|---|
| `/docs/` | Swagger UI |
| `/admin/` | Админ-панель Django |
| `/accounts/` | Страницы allauth (вход через GitHub / Google) |

### Примеры запросов к `/product/`

```http
GET /en/product/?search=iphone
GET /en/product/?price__gt=100&price__lt=1000
GET /en/product/?subcategory=1
GET /en/product/?ordering=-price
GET /en/product/?page=2
```

## 🚀 Быстрый старт

### Вариант 1. Локальный запуск

```bash
# 1. Клонируйте репозиторий
git clone https://github.com/KadyrovErjan/AvitoPython49.git
cd AvitoPython49/mysite

# 2. Создайте и активируйте виртуальное окружение
python -m venv venv
source venv/bin/activate        # Windows: venv\Scripts\activate

# 3. Установите зависимости
pip install -r req.txt

# 4. Примените миграции
python manage.py migrate

# 5. Создайте суперпользователя
python manage.py createsuperuser

# 6. Запустите сервер
python manage.py runserver
```

После запуска:

- API: <http://127.0.0.1:8000/en/>
- Swagger: <http://127.0.0.1:8000/en/docs/>
- Админка: <http://127.0.0.1:8000/en/admin/>

> По умолчанию в `settings.py` используется **SQLite**, поэтому для локального запуска ничего дополнительно настраивать не нужно.

### Вариант 2. Docker Compose

```bash
cd AvitoPython49/mysite
docker-compose up --build
```

Будут запущены три сервиса:

| Сервис | Назначение | Порт |
|---|---|---|
| `web` | Django + Gunicorn | `8000` |
| `db` | PostgreSQL 17 | — |
| `nginx` | Reverse proxy, раздача static и media | `80` |

Сервис `web` автоматически выполняет `collectstatic`, `makemigrations` и `migrate` при старте.

> ⚠️ В `docker-compose.yml` поднимается PostgreSQL, а в `settings.py` сейчас прописана SQLite. Чтобы Django работал с контейнером `db`, измените блок `DATABASES` (хост `db`, база/пользователь/пароль — `postgres`). Также проверьте путь к `staticfiles` в `volumes` — он указан под сервер `/home/ubuntu/...`.

## 📁 Структура проекта

```
AvitoPython49/
└── mysite/
    ├── avito_app/              # Основное приложение
    │   ├── migrations/
    │   ├── admin.py
    │   ├── filters.py          # Фильтры товаров
    │   ├── models.py           # Модели данных
    │   ├── pagination.py       # Пагинация
    │   ├── serializers.py      # DRF-сериализаторы
    │   ├── translation.py      # Переводы EN / RU
    │   ├── urls.py             # Маршруты API
    │   └── views.py            # Представления
    ├── mysite/                 # Настройки проекта
    │   ├── settings.py
    │   ├── urls.py
    │   ├── asgi.py
    │   └── wsgi.py
    ├── nginx/                  # Конфигурация Nginx
    │   ├── Dockerfile
    │   └── nginx.conf
    ├── media/                  # Загружаемые файлы (аватары, фото, видео)
    ├── Dockerfile
    ├── docker-compose.yml
    ├── manage.py
    └── req.txt                 # Зависимости
```

## 🔑 Авторизация в запросах

После входа через `/login/` передавайте access-токен в заголовке:

```http
Authorization: Bearer <your_access_token>
```

## 🗺 Планы по развитию

- [ ] Вынести секреты и настройки БД в `.env`
- [ ] Подключить PostgreSQL в `settings.py` для Docker-окружения
- [ ] Добавить тесты для основных эндпоинтов
- [ ] Оформление заказов и онлайн-оплата
- [ ] Чат между покупателем и продавцом

## 👤 Автор

**Kadyrov Erjan**

[![GitHub](https://img.shields.io/badge/GitHub-KadyrovErjan-181717?style=for-the-badge&logo=github)](https://github.com/KadyrovErjan)

---

<div align="center">

⭐ Если проект был полезен — поставьте звезду!

</div>
