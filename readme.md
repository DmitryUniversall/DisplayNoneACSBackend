# НТО 2024. II отборочный этап. Командные задания — серверная часть

## 📖 Предыстория

В компании S контроль доступа в офис осуществляется с помощью СКУД (системы контроля управления доступом). На данный момент у каждого сотрудника компании есть карта-пропуск с NFC меткой. А у каждой входной двери есть считыватель с обеих сторон. При поднесении карты к считывателю, дверь открывается, а информация о времени входа или выхода сотрудника фиксируется в базе данных. 
Администрации компании S требуется мобильное приложение, как для рядовых сотрудников, так и для администрации с возможностью просмотра посещений и работой электронного пропуска как временной замены обычного (при помощи сканировании QR кода, который находится на считывателе). Поскольку в приложении есть возможность использовать телефон как пропуск - то к данному приложению повышенные требования к безопасности всех данных, находящихся внутри него.



## 🛠️ Техническое задание

Требуется разработать серверное приложение на Java (Java 11) с использованием Spring Boot, которое работает на основе протоколов TCP/IP и взаимодействует с клиентами благодаря RESTful API. Для хранения данных о сотрудниках и их посещениях должна использоваться реляционная база данных (H2). Сотрудникам не нужно регистрироваться, все данные в базе должны быть предзаполнены. Картинки для аватаров пользователей не должны храниться в БД. Должны быть сохранены лишь URL-адреса на ресурсы, откуда в последующем мобильное приложение загрузит соответствующий аватар. 

Сервер разрабатывается на основе предоставляемой заготовки проекта. Версии зависимостей и сами зависимости изменяться не должны. 



## 📂 Правила работы с проектом-заготовкой

В предоставленном проекте необходимо изучить, но никак не модифицировать, не перемещать и не удалять следующие файлы:
- `pom.xml`
- `application.yml`
- все файлы из `db.changelog`

Кроме описанных выше файлов, в проекте уже созданы основные классы-сущности и добавлены пустые классы всех слоев. В этих классах необходимо будет написать программный код и добавить аннотации для реализации описанного задания. Наименования классов и прочий код уже написанный в предоставляемом проекте **изменять/удалять не нужно, необходимо их доработать**. Добавлять свои дополнительные классы в проект можно.

Создание таблиц в БД и предзаполнение их требуемыми данными уже реализовано в заготовке при помощи liquibase.  


## 🌐 Где необходимо разместить сервер

Серверное приложение должно быть разработано и протестировано локально (не требуется размещать сервер удаленно и осуществлять его функционирование 24/7).



## 📋 Технические требования к серверу и его ответам клиенту

Для конфигурирования Вашего сервера (его хоста/IP адреса) используйте константы из файла `Constants.kt`.

| **Тип запроса** | **Путь**              | **Параметры/Тело**                                                             | **Ответы**                                                                                                                                              |
|------------------|-----------------------|--------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------|
| **GET**          | `api/<LOGIN>/auth`   | `<LOGIN>` - никнейм/имя пользователя                                           | `400` - что-то пошло не так<br>`401` - логина не существует или неверный<br>`200` - данный логин существует - можно пользоваться приложением            |
| **GET**          | `api/<LOGIN>/info`   | `<LOGIN>` - никнейм/имя пользователя                                           | `400` - что-то пошло не так<br>`401` - логина не существует или неверный<br>`200` - ОК<br>{<br> "id": 1,<br> "login": "pivanov",<br> "name": "Иванов Петр Федорович",<br> "photo": "https://funnyducks.ru/upload/iblock/0cd/0cdeb7ec3ed6fddda0f90fccee05557d.jpg",<br> "position": "Разработчик",<br> "lastVisit": "2024-02-12T08:30:00"<br>} |
| **PATCH**        | `api/<LOGIN>/open`   | `<LOGIN>` - никнейм/имя пользователя<br>Тело: `{“value”: <CODE>}`, где <br> `<CODE>` - это код, полученный из экрана сканирования QR кода | `400` - что-то пошло не так<br>`401` - логина не существует или неверный<br>`200` - дверь открылась                                                    |



## 📊 Данные, которыми необходимо наполнить базу данных:



| **id** | **login**  | **name**                        | **photo**                                                                                   | **position**     | **lastVisit**       |
|--------|------------|---------------------------------|---------------------------------------------------------------------------------------------|------------------|---------------------|
| 1      | pivanov    | Иванов Петр Федорович          | https://funnyducks.ru/upload/iblock/0cd/0cdeb7ec3ed6fddda0f90fccee05557d.jpg               | Разработчик      | 2024-02-12T08:30:00    |
| 2      | ipetrov    | Петров Иван Константинович      | https://funnyducks.ru/upload/iblock/0cd/0cdeb7ec3ed6fddda0f90fccee05557d.jpg               | Аналитик         | 2024-02-30T08:35:00    |
| 3      | asemenov   | Семенов Анатолий Анатольевич   | https://funnyducks.ru/upload/iblock/0cd/0cdeb7ec3ed6fddda0f90fccee05557d.jpg               | Разработчик      | 2024-02-31T08:31:00    |
| 4      | afedorov   | Федоров Александр Сергеевич    | https://funnyducks.ru/upload/iblock/0cd/0cdeb7ec3ed6fddda0f90fccee05557d.jpg               | Тестировщик      | 2024-02-30T08:36:00    |



| **id** | **code**               |
|--------|-------------------------|
| 1      | 1234567890123456789     |
| 2      | 9223372036854775807     |
| 3      | 1122334455667788990     |
| 4      | 998877665544332211      |
| 5      | 5566778899001122334     |



## 📝 Решение

Необходимо загрузить свое решение в систему [по ссылке](https://innovationcampus.ru/lms/mod/quiz/view.php?id=2078).

Отметим, что работу необходимо осуществлять в представленных проектах-заготовках (шаблонах).



## ✅ Особенности оценивания

Оценивание происходит с помощью автоматической системы тестирования, которая в автоматическом режиме находит элементы и взаимодействует с ними (именно для этого у каждого элемента указан уникальный идентификатор, по которому будет производится поиск). Каждый тест происходит с чистой установки приложения. В случае тестирования сервера на него поочередно отправляются команды, описанные в API и ожидаются определенные корректные ответы. Сервер и приложение тестируются независимо.