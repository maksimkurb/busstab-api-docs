# Чтение основной информации пользователя

Давайте отправим подобный запрос на сервер:

```graphql
query {
  # Получим информацию о пользователе
  loggedInUser {
    id
    email
    nickname
  }
  
  # И его последние 15 запросов
  requests(take: 15, skip: 0) {
    id
    stage
    createdAt
    __typename
  }
}
```

Мы получим подобный результат:

```javascript
{
  "data": {
    "loggedInUser": {
      "id": "63e9c464-6699-42bd-ac9e-d5e0ab5180a8",
      "email": "test@github.com",
      "nickname": "19majkel94"
    },
    "requests": [
      {
        "id": "263d7cf3-1da3-4bad-a7cb-c4101411ed09",
        "stage": "FINISHED",
        "createdAt": "1556197225014",
        "__typename": "PassportRequest"
      },
      {
        "id": "e4b6d127-0e23-413e-9d56-25f798960cdd",
        "stage": "PENGING",
        "createdAt": "1556563958885",
        "__typename": "CompanySearchRequest"
      }
    ]
  }
}
```

В ключе **data** у нас хранится результат с сервера. Он имеет такую же структуру, как и наш запрос.

Теперь давайте запросим более подробную информацию о запросах.

Судя по документации API \(зелёная кнопочка SCHEMA в GraphQL Playground\), поле `requests` возвращает массив типа `Request`. `Request` в свою очередь - интерфейс, который наследуется типом `PassportCheckRequest`, у которого есть дополнительные поля: `input` и `payload`. В поле `input`, очевидно, находятся входные данные, которые передавали для этого запроса, а в `payload` - результат. Чтобы запросить более подробные данные, выполним следующий запрос:

```graphql
query {
  requests(take: 15, skip: 0) {
    id
    stage
    createdAt
    __typename
    
    # Если запрос - PassportRequest
    ...on PassportRequest {
      payload {
        passportIsDeprecated
        remarks {
          severity
          message
          code
        }
        INN
        MRZ
      }
    }
  }
}
```

В ответе мы получим следующий результат:

```javascript
{
  "data": {
    "requests": [
      {
        "id": "263d7cf3-1da3-4bad-a7cb-c4101411ed09",
        "stage": "FINISHED",
        "createdAt": "1556197225014",
        "__typename": "PassportRequest",
        "payload": {
          "passportIsDeprecated": false,
          "remarks": [
            {
              "severity": "INFO",
              "message": "Согласно серии бланка паспорта, регионом выдачи является **Астраханская область**",
              "code": "ARCPS_REGION_DETECTED"
            },
            {
              "severity": "FATAL",
              "message": "В данном паспорте использован бланк документа, предусмотренный к выдаче в **2034** году.  \nЭто противоречит правилам выдачи и замены паспортов. Рекомендуется проведение дополнительной проверки.",
              "code": "ARCPS_PASSPORT_FROM_FUTURE"
            }
          ],
          "INN": null,
          "MRZ": null
        }
      },
      {
        "id": "e4b6d127-0e23-413e-9d56-25f798960cdd",
        "stage": "PENGING",
        "createdAt": "1556563958885",
        "__typename": "CompanySearchRequest"
      }
    ]
  }
}
```

