# Запрос "Проверка Паспорта РФ"

## Получение токена капчи
Для запроса "Проверка Паспорта РФ" комплексного или полного уровня необходимо вводить код с картинки с [сайта налоговой](https://service.nalog.ru/inn.do)

{% method %}
Запрос:

```graphql
query {
  service(type: PASSPORT_CHECK) {
    ...on PassportCheckService {
      captcha {
        imageUrl
        token
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
  -H 'Authorization: Bearer 55b58c0b40011512c1fe4a27c9f81acabf1017d6' \
  -H 'Content-Type: application/json' \
  -d '{"query":"query {service(type: PASSPORT_CHECK) {...on PassportCheckService {captcha {imageUrl, token}}}}"}'
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
     query: 'query {service(type: PASSPORT_CHECK) {...on PassportCheckService {captcha {imageUrl, token}}}}'
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
        "service": {
            "captcha": {
                "imageUrl": "https://service.nalog.ru/static/captcha.html?a=1B0E63A565697036C3AC6AD7CB07671A4D0A8CDF574293BCF92BFAF5D109089599B43D6906AB263117DAFF3B28A49F03B1276140E831A6527F88CD98D9BA47EF",
                "token": "1B0E63A565697036C3AC6AD7CB07671A4D0A8CDF574293BCF92BFAF5D109089599B43D6906AB263117DAFF3B28A49F03B1276140E831A6527F88CD98D9BA47EF"
            }
        }
    }
}
```
{% endmethod %}
