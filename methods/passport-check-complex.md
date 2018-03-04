# Запрос "Проверка Паспорта РФ"

## Комплексный уровень проверки паспорта
Поля запроса:
```
{
  input {
    passportSeries
    passportID
    lastName
    firstName
    patronymic (не требуется, если нет отчества)
    birthDate
    birthPlace
    passportIssueDate
    captcha
    captchaToken
  }
}
```

{% module %}
Запрос:
```graphql
mutation($input: MakePassportCheckInput!) {
  makePassportCheckRequest(input: $input) {
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

{% sample lang="curl" %}
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

{% sample lang="js" %}
Пример выполнения запроса на JavaScript:
```js
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
{% common %}
#### Результат запроса:
```json
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

#### Результат, если введён неправильный код с картинки:
```json
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

{% endmodule %}
