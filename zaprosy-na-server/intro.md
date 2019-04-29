# Введение

Запросы на сервер происходят по одному адресу:

```text
POST https://api.busstab.ru/graphql
```

При этом в заголовке запроса `Authorization` необходимо указывать **access\_token** \([как его получить?](../autentifikaciya/authorization.md)\) в следующем формате:

```text
Authorization: Bearer <ваш_access_token>
```

Все запросы происходят с типом `application/json`, тело запроса состоит из следующих ключей:

| Ключ | Обязательный? | Описание |
| :--- | :---: | :--- |
| query | **Да** | Тело GraphQL-запроса |
| variables | Нет | Входные данные \(переменные\) |
| operationName | Нет | Название запроса, который необходимо выполнить |

Запросы пишутся на языке **GraphQL**, который очень похож на **JSON**. По сути, Вы описываете ключи JSON, которые Вам необходимо получить.

Подробнее про язык GraphQL можно узнать на сайте [https://graphql.org/learn/](https://graphql.org/learn/)

Перейдя по ссылке [https://api.busstab.ru/graphql](https://api-staging.busstab.ru/graphql) в браузере, откроется браузерная IDE для построения и проверки запросов:

![&#x418;&#x43D;&#x442;&#x435;&#x440;&#x444;&#x435;&#x439;&#x441; GraphQL Playground](../.gitbook/assets/image%20%284%29.png)

## Далее

Давайте попрактикуемся и сделаем [запрос основной информации пользователя](querying-user-profile.md)

