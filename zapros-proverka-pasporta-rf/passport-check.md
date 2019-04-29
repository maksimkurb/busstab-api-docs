# Базовый уровень

## Введение

Для осуществления каких-либо изменений, в GraphQL существует понятие `Mutation`. По сути, это то же самое, что и `Query` \(запрос\), отличается `Mutation` лишь тем, что он изменяет какие-либо данные на сервере.

Нам нужно осуществить запрос на проверку паспорта, для этого нам необходимо выполнить мутацию `passportCheck`

Давайте посмотрим аргументы, которые принимает эта мутация:

![&#x41E;&#x43F;&#x438;&#x441;&#x430;&#x43D;&#x438;&#x435; &#x43C;&#x443;&#x442;&#x430;&#x446;&#x438;&#x438; &#x432; &#x430;&#x432;&#x442;&#x43E;&#x43C;&#x430;&#x442;&#x438;&#x447;&#x435;&#x441;&#x43A;&#x438; &#x433;&#x435;&#x43D;&#x435;&#x440;&#x438;&#x440;&#x443;&#x435;&#x43C;&#x43E;&#x439; &#x434;&#x43E;&#x43A;&#x443;&#x43C;&#x435;&#x43D;&#x442;&#x430;&#x446;&#x438;&#x438;](../.gitbook/assets/image%20%281%29.png)

Мутация принимает до 4 аргументов, однако в пояснительной записке сказано, что мы не должны сочетать поля `basic`, `complex` и `full` в одном запросе.  
Давайте посмотрим на поле `basic`. Оно имеет тип `PassportRequestBasicInput`

Этот тип имеет поля `passportSeries (String!)` и `passportNumber (String!)` \(постфикс "`!`" говорит о том, что поле обязательно\):

![](../.gitbook/assets/image%20%285%29.png)

## Базовый уровень проверки паспорта

Обязательные поля:

```text
{
    basic {
        passportSeries
        passportNumber
    }
}
```

Запрос:

```graphql
mutation($basic: PassportRequestBasicInput) {
  passportCheck(basic: $basic) {
    id
    stage
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
```

{% tabs %}
{% tab title="curl" %}
Пример выполнения запроса через cURL:

```bash
curl -X POST \
https://api-staging.busstab.ru/graphql \
-H 'Authorization: Bearer <access_token>' \
-H 'Cache-Control: no-cache' \
-H 'Content-Type: application/json' \
-H 'Postman-Token: 85082eea-02cf-6e90-4f6e-d7cff03b084e' \
-d '{
  "query":"mutation($basic: PassportRequestBasicInput) {passportCheck(basic: $basic) {id, stage, payload{passportIsDeprecated, remarks{severity, message, code}, INN, MRZ}}}",
  "variables": {
    "basic": {
      "passportSeries": "1234",
      "passportNumber": "567890"
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
    query: 'mutation($basic: PassportRequestBasicInput) {passportCheck(basic: $basic) {id, stage, payload{passportIsDeprecated, remarks{severity, message, code}, INN, MRZ}}}',
    variables: {
      basic: {
        passportSeries: '1234',
        passportNumber: '567890'
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

Результат запроса:

```javascript
{
  "data": {
    "passportCheck": {
      "id": "3421fc22-e6e0-4a47-9e05-3e06e2069fba",
      "stage": "FINISHED",
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
    }
  }
}
```

