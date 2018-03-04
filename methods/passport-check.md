# Запрос "Проверка Паспорта РФ"

Для осуществления каких-либо изменений, в GraphQL существует понятие `Mutation`. По сути, это то же самое, что и `Query` (запрос), отличается `Mutation` лишь тем, что он изменяет какие-либо данные на сервере.

Нам нужно осуществить запрос на проверку паспорта, для этого нам необходимо выполнить мутацию `makePassportCheckRequest`
Давайте посмотрим аргументы, которые принимает эта мутация:
![](/assets/makePassportCheckRequest.png)
Мутация принимает 1 аргумент `input` с типом `MakePassportCheckInput!`

Сам `MakePassportCheckInput` принимает следующие параметры:
![](/assets/MakePassportCheckInput.png)
Выделенные параметры - обязательны (т.к. у них постфикс `!`)

В описании поля `label` подробно расписано, какие поля необходимы для разных уровней проверки (базовый, комплексной и полной):
![](/assets/MakePassportCheckInput_label.png)

{% module %}
### Базовый уровень проверки паспорта
Обязательные поля:
```
{
input {
passportSeries
passportID
}
}
```

{% common %}
Запрос:
```graphql
mutation($input: MakePassportCheckInput!) {
  makePassportCheckRequest(input: $input) {
    request {
      id
      payload {
        passportIsDeprecated
        basic {
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
-H 'Cache-Control: no-cache' \
-H 'Content-Type: application/json' \
-H 'Postman-Token: 85082eea-02cf-6e90-4f6e-d7cff03b084e' \
-d '{
"query":"mutation($input: MakePassportCheckInput!) {makePassportCheckRequest(input: $input) {request {id, payload{passportIsDeprecated, basic{type, message}}}}}",
"variables" :{
"input": {
"level": "BASIC",
"passportSeries": "1214",
"passportID": "123456"
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
  body: { = query: 'mutation($input: MakePassportCheckInput!) {makePassportCheckRequest(input: $input) {request {id, payload{passportIsDeprecated, basic{type, message}}}}}',
    variables: {
      input: {
        level: 'BASIC',
        passportSeries: '1214',
        passportID: '123456'
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
Результат запроса:
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

{% endmodule %}
