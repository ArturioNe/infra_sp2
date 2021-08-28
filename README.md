# **[YaMDb](https://github.com/ArturioNe/api_yamdb.git).**

## **Описание проекта.**
Проект YaMDb собирает отзывы (Review) пользователей на произведения (Titles). 
Произведения делятся на категории: «Книги», «Фильмы», «Музыка». 
Список категорий (Category) может быть расширен администратором.
Сами произведения в YaMDb не хранятся, здесь нельзя посмотреть фильм или послушать музыку.
В каждой категории есть произведения: книги, фильмы или музыка.
Произведению может быть присвоен жанр (Genre) из списка предустановленных(например, «Сказка», «Рок» или «Артхаус»). 
Новые жанры может создавать только администратор.
Благодарные или возмущённые пользователи оставляют к произведениям текстовые отзывы (Review). 
На одно произведение пользователь может оставить только один отзыв.

## Подготовка.
Убедитесь, что у вас установлен Docker:
+ запустите командную строку
+ выполните команду ```docker --version```.
В ответе вы увидите какая версия Docker у вас установлена.

Если ранее вы не работали с Docker, то перейдите на [официальный сайт](https://www.docker.com/products/docker-desktop) 
и скачайте установочный файл Docker Desktop для вашей операционной системы.
Запустите его и следуйте инструкциям по установке. После установки вы увидите окно с сообщением 
Service is not running: программа установлена, но демон докера не запущен. Нажмите кнопку Start.

## Запуск приложения.

1. Клонируйте репозиторий:

``` git clone https://github.com/ArturioNe/infra_sp2.git```

2. Перейдите в папку с репозиторием.

3. Создайте файл `.env` и внесите следующие записи:
    + DB_ENGINE=указать базу данных с которой будете работать
    + DB_NAME=имя базы данных
    + POSTGRES_USER=логин для подключения к базе данных
    + POSTGRES_PASSWORD=пароль для подключения к БД (установите свой)
    + DB_HOST=название сервиса (контейнера)
    + DB_PORT=порт для подключения к БД

4. Для запуска docker-compose выполните команду:

``` docker-compose up -d --build ```

Сборка может занять некоторое время, по окончании работы docker-compose сообщит, 
что контейнеры собраны и запущены:
![](https://pictures.s3.yandex.net/resources/S18_03_03_1619103276.png)

5. Выполните миграции:

``` docker-compose exec web python manage.py migrate auth ```

``` docker-compose exec web python manage.py migrate --run-syncdb ```

6. Установите статику проекта

``` docker-compose exec web python manage.py collectstatic --no-input  ```

7. Создайте суперпользователя:

``` docker-compose exec web python manage.py createsuperuser ```

8. Заполните базу данных:

``` docker-compose exec web python3 manage.py loaddata fixture.json ```
   
Теперь проект доступен по адресу http://127.0.0.1/. 
При этом номер порта указывать уже не надо: умный nginx принимает запросы на стандартном порте 
и перенаправляет их в приложение.
Зайдите на http://127.0.0.1/admin/ и убедитесь, что страница отображается полностью: статика подгрузилась.
Авторизуйтесь под аккаунтом суперпользователя и убедитесь, что миграции прошли успешно.


