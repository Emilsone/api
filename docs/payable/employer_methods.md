# Платный доступ для работодателей к некоторым методам API

Начиная с 16 июля 2018 года, некоторые методы API HH для работодателей стали платными.

Такие методы отмечены в [оглавлении](../README.md#headhunter-api) лейблом 
<img src="http://hhru.github.io/api/badges/emp_paid.png" alt="employer with paid access" /> 

Для покупки доступа к платным методам для работодателей обратитесь к своему персональному менеджеру. 

Обратите внимание, что при работе приложения от имени нескольких учетных записей работодателей (employer_id), у каждой учетной записи работодателя должен быть доступ к платным методам API. 
Проверить информацию о подключенных услугах работодателя можно с помощью [специального метода](employer_services.md#payable-api-actions).

В случае запроса платного метода без купленного доступа будет выдана [ошибка](errors.md#employer_payable_methods) `403 Forbidden`.

## Проверить, есть ли доступ к платному методу

Запрос необходимо выполнять с авторизацией работодателя, иначе вернется ошибка.

### Запрос

```
GET /employers/{employer_id}/manager/{manager_id}/method_access
```

где: 
* `employer_id` - идентификатор работодателя, который можно узнать в [информации о текущем пользователе](../me.md#employer-info).

Вернутся все существующие группы методов с информацией о доступе к ним.

В настоящее время существуют следующие группы:
1. Наличие доступа к методам:
    * просмотр резюме
    * отклики
    * переписка
2. Наличие доступа к методам:
    * поиск резюме
    * сохраненные поиски резюме
    * папки отобранных резюме
3. Наличие доступа к просмотру резюме, у которого есть отклик или приглашение
4. Наличие доступа к просмотру резюме

### Ответ

Успешный ответ приходит с кодом `200 OK` и содержит тело:

```json
{
    "items": [
        {
            "id": "3",
            "description": "Получение резюме, если есть топик",
            "access": {
                "has_access": true
            }
        },
        {
            "id": "4",
            "description": "Поиск резюме из базы",
            "access": {
                "has_access": false
            }
        },
        ...
    ]
}
```

Каждый элемент массива items содержит в себе описание группы методов.

Имя | Тип | Описание
--- | --- | --------
id | string | идентификатор группы методов
description | string | описание группы методов
access.has_access | boolean | есть ли доступ к группе методов

### Ошибки

* `403 Forbidden` – если текущий пользователь - не работодатель.
* `404 Not Found` – если указанный работодатель или менеджер не найден.