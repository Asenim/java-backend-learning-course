Проект "Планировщик задач"

Многопользовательский планировщик задач. Пользователели могут использовать его в качестве TODO листа. Источником вдохновения для проекта является Trello.

## Что нужно знать

- Java - коллекции, ООП
- Maven/Gradle
- Spring Boot, Spring Security, Spring Sessions, Spring AMQP, Spring Scheduler, Spring SMTP
- Базы данных - SQL, Spring Data JPA
- Брокер сообщений RabbitMQ - очереди
- HTML/CSS, Bootstrap
- JS - jQuery, Ajax
- Docker - контейнеры, образы, volumes, докеризация Spring Boot приложения, Docker Compose

## Мотивация проекта

- Знакомство с микросервисами на практике - разработка проекта, разделённого на несколько сервисов
- Использование новых модулей Spring Boot - AMQP, Scheduler, SMTP
- Практика с Docker и Docker Compose, докеризация Spring Boot приложение (упаковка его в Docker образ)
- Знакомство и практика с RabbitMQ
- Самостоятельная разработка части схемы REST API

Дисклеймер. Планировщик задач - довольно простой и легковесный проект для микросервисного подхода. Но проект достаточной сложности вышел бы за рамки учебного по объёму и сложности.

## Функционал приложения

Работа с пользователями:

- Регистрация
- Авторизация
- Logout

Работа с задачами:

- Создание, редактирование (каждая задача имеет заголовок и описание)
- Пометка задачи как сделанной
- Удаление

Оповещение пользователей по email:
- Приветственное письмо
- Каждый день в полночь оповещение о том, сколько задач было сделано за прошедшие сутки, и какое количество осталось невыполнено

## Интерфейс приложения

Веб-приложение является одностраничным, всё общение с сервером происходит с помощью Javascript/Ajax, интерфейс обновляется через jQuery.

- Заголовок
    - Для неавторизованных пользователей - кнопки регистрации и авторизации
    - По нажатию на кнопки открывается модальный диалог с формой регистрации или авторизации
        - Поля диалога регистрации - email, password, repeat password
        - Поля диалога авторизации - email, password
        - В случае ошибки регистрации или авторизации, приложение должно отобразить ошибку, подобно Thymeleaf
    - Для авторизованных пользователей - логин текущего пользователя и кнопка Logout
- Контент (только для авторизованных пользователей)
    - Поле ввода заголовка новой задачи с кнопкой "Добавить"
    - Список несделанных задач
    - Список сделанных задач

При клике на задачу открывается модальное окно со следующими элементами:
- Поле ввода заголовка задачи
- Многострочное поле ввода описания задачи
- Чекбокс чтобы пометить задачу сделанной (и обратно - пометить несделанной)
- Кнопка "удалить"

У формы модального окна нет кнопки "сохранить". Javascript должен сохранять состояние задачи с помощью Ajax запросов, каждый раз когда пользователь нажимает на чекбокс, или редактирует поля ввода.

## Сервисы

### Бэкенд 

Spring Boot приложение, реализующее REST API для работы с пользователям, сессиями, создаваемые при авторизации и задачами.

####  POST `/users` - регистрация пользователя

- Тело запроса - поля формы в формате `multipart/form-data`
- Зарегистрированный пользователь сразу же автоматически авторизуется, без отдельного заполнения логин формы 
- В случае успешной регистрации, код ответа HTTP 200, HTTP заголовок ответа устанавливает куку авторизованного пользователя
- В случае неуспешной регистрации, код и тело ответа описывают ошибку

Пример ответа с ошибкой - "this email is already taken" - HTTP code 409:
```
{
    "message" - "This email is already taken"
}
```

Структура ответа с ошибкой - JSON объект с полем message. Код ответа должен быть наиболее подходящим к ситуации из 4хх 5хх кодов ошибок - https://datatracker.ietf.org/doc/html/draft-ietf-httpbis-p2-semantics-18#section-7.4.

#### POST `/session` - авторизация пользователя

- Тело запроса - поля формы в формате `multipart/form-data`
- В случае успешной авторизации, код ответа HTTP 200, HTTP заголовок ответа устанавливает куку авторизованного пользователя
- В случае неуспешной авторизации, код и тело ответа описывают ошибку

#### DELETE `/session` - logout

- Прекращение сессии пользователя

### GET `/tasks` - получение списка заметок текущего пользователя

- Успешный ответ с кодом HTTP 200 содержит список моделей задач в виде JSON массива
- В случае, если пользователь неаторизован, его сессия истекла или прервана через logout - тело ответа содержит сообщение об ошибке, код ответа - HTTP 401

---

Дизайн методов для создания, редактирования, удаления задач студенту предлагается разработать самостоятельно.

### Фронтенд



### Планировщик

### Рассыльщик писем