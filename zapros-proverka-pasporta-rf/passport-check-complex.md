# Комплексный уровень

## Комплексный уровень проверки паспорта

### _Outdated: статья относится к старой версии API, в новой версии запрос делается немного по-другому_

Поля запроса:

```text
{
  input {
    passportSeries
    passportNumber
    lastName
    firstName
    patronymic (не требуется, если нет отчества)
    birthDate
    birthPlace
    passportIssueDate
  }
}
```

Запрос:

```graphql
mutation($input: MakePassportCheckInput!) {
  passportCheck(input: $input) {
    request {
      id
      payload {
        passportIsDeprecated
        itn
        basic {
          type
          message
        }
        complex {
          type
          message
        }
      }
    }
  }
}
```

{% tabs %}
{% tab title="curl" %}
Пример выполнения запроса через cURL:

```bash
curl -X POST \
  https://api-staging.busstab.ru/graphql \
  -H 'Authorization: Bearer <access_token>' \
  -H 'Content-Type: application/json' \
  -d '{
    "query":"mutation($input: MakePassportCheckInput!) {\nmakePassportCheckRequest(input: $input) {\nrequest {\nid\npayload {\npassportIsDeprecated\nitn\nbasic {\ntype\nmessage\n}\ncomplex {\ntype\nmessage\n}\n}\n}\n}\n}",
    "variables" :{
        "input": {
            "level": "COMPLEX",
            "passportSeries": "1214",
            "passportID": "123456",
            "lastName": "Фёдоров",
            "firstName": "Иван",
            "patronymic": "Петрович",
            "birthDate": "04.06.1985",
            "birthPlace": "гор. Новокузнецк",
            "passportIssueDate": "04.06.1992",
            "captcha": "123456",
            "captchaToken": "ABCDEFGHABCDEFGHABCDEFGH"
        }
    }
}'
```
{% endtab %}

{% tab title="JavaScript" %}
Пример выполнения запроса на JavaScript:

```javascript
var options = {
  method: 'POST',
  url: 'https://api-staging.busstab.ru/graphql',
  headers: {
    Authorization: 'Bearer <access_token>',
    'Content-Type': 'application/json'
  },
  body: {
    query: 'mutation($input: MakePassportCheckInput!) {\nmakePassportCheckRequest(input: $input) {\nrequest {\nid\npayload {\npassportIsDeprecated\nitn\nbasic {\ntype\nmessage\n}\ncomplex {\ntype\nmessage\n}\n}\n}\n}\n}',
    variables: {
      input: {
        level: 'COMPLEX',
        passportSeries: '1214',
        passportID: '123456',
        lastName: 'Фёдоров',
        firstName: 'Иван',
        patronymic: 'Петрович',
        birthDate: '04.06.1985',
        birthPlace: 'гор. Новокузнецк',
        passportIssueDate: '04.06.1992',
        captcha: '123456',
        captchaToken: 'ABCDEFGHABCDEFGHABCDEFGH'
      }
    }
  },
  json: true
};
request(options, function(error, response, body) {
  if (error) throw new Error(error);
  console.log(body);
});
```
{% endtab %}
{% endtabs %}

### Результат запроса:

```javascript
{
    "data": {
        "makePassportCheckRequest": {
            "request": {
                "id": "UmVxdWVzdDpiZTY0ZjE1NC0xZmQzLTExZTgtODRhZi1lN2ViYTM2ODM0MmQ=",
                "payload": {
                    "passportIsDeprecated": false,
                    "basic": [
                        {
                            "type": "INFO",
                            "message": "Согласно серии бланка паспорта, регионом выдачи является *Астраханская область*"
                        },
                        {
                            "type": "INFO",
                            "message": "Исключение могут составлять паспорта, выдаваемые жителям полуострова Крыма в 2014 году на бланках паспортов из других регионов РФ"
                        }
                    ]
                }
            }
        }
    }
}
```

### Результат, если введён неправильный код с картинки:

```javascript
{
    "errors": [
        {
            "error": "GraphQLError",
            "message": "The request is invalid.",
            "state": {
                "captcha": [
                    "Введён неправильный код с картинки"
                ]
            },
            "path": [
                "makePassportCheckRequest"
            ]
        }
    ],
    "data": {
        "makePassportCheckRequest": null
    }
}
```

