{% method %}
Давайте отправим подобный запрос на сервер:

```graphql
query {
  loggedInUser {
    id
    displayName
    imageUrl
  }
  
  latestRequests {
    id
    service {
      type
    }
    subject
    stage
  }
}
```

{% common %}
Мы получим подобный результат:
```json
{
  "data": {
    "loggedInUser": {
      "id": "895a2dfe-d293-11e7-bcd6-77930894ce64",
      "displayName": "Максим Курбатов (maksimkurb)",
      "imageUrl": "http://thecatapi.com/api/images/get?format=src&type=jpg"
    },
    "latestRequests": [
      {
        "id": "abcde-1",
        "service": {
          "type": "PASSPORT_CHECK"
        },
        "subject": "Джон Прекрасный, 5315 2***84",
        "stage": "FINISHED"
      },
      {
        "id": "abcde-2",
        "service": {
          "type": "PASSPORT_CHECK"
        },
        "subject": "3112 1***87",
        "stage": "FINISHED"
      },
      {
        "id": "abcde-3",
        "service": {
          "type": "PASSPORT_CHECK"
        },
        "subject": "Павел Овсянкин, 0109 1***02",
        "stage": "FINISHED"
      }
    ]
  }
}```

В ключе **data** у нас хранится результат с сервера. Он имеет такую же структуру, как и наш запрос.

{% endmethod %}