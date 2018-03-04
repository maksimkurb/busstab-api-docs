# Введение

Запросы на сервер происходят по одному адресу:

```
POST https://api-staging.busstab.ru/graphql
```

При этом в заголовке запроса `Authorization` необходимо указывать **access\_token** \([как его получить?](//oauth/authorization.md)\) в следующем формате:

```
Authorization: Bearer <ваш_access_token>
```

Все запросы происходят с типом `application/json`, тело запроса состоит из следующих ключей:

| Ключ | Обязательный? | Описание |
| :--- | :---: | :--- |
| query | **Да** | Тело GraphQL-запроса |
| variables | Нет | Значения переменных |
| operationName | Нет | Название запроса, который необходимо выполнить |

Запросы пишутся на языке **GraphQL**, который очень похож на **JSON**. По сути, Вы описываете ключи JSON, которые Вам необходимо получить.

Подробнее про язык GraphQL можно узнать на сайте [https://graphql.org/learn/](https://graphql.org/learn/)

Перейдя по ссылке [https://api-staging.busstab.ru/graphql](https://api-staging.busstab.ru/graphql) в браузере, откроется браузерная IDE для построения и проверки запросов:

![](/assets/screen.jpg)

### Далее

Давайте попрактикуемся и сделаем несколько запросов



