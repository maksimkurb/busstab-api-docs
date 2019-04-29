# Аутентификация

Система поддерживает несколько методов авторизации OAuth: `Authorization code`, `Implicit` и `Client credentials`.

## `client_credentials`

_Подходит для внутреннего приложения. Клиент получает **token**, используя **id** и **секрет приложения**. В таком случае приложение всегда будет работать от имени владельца этого приложения._ [_\[rfc6749\#1.3.4\]_](https://tools.ietf.org/html/rfc6749#section-1.3.4)\_\_

Для такой авторизации нам необходимо сделать POST-запрос на сервер, передав следующие параметры:

1. **grant\_type**: `client_credentials`
2. **client\_id**: Свой ID клиента
3. **client\_secret**: Свой Секрет клиента
4. **scope**: [Права доступа приложения](scope.md)

### `POST https://api.busstab.ru/oauth/token`

{% tabs %}
{% tab title="curl" %}
```bash
curl -X POST \
  https://api.busstab.ru/oauth/token \
  -H 'Content-Type: application/x-www-form-urlencoded' \
  -d 'grant_type=client_credentials&scope=profile balance&client_id=<CLIENT_ID>&client_secret=<CLIENT_SECRET>'
```
{% endtab %}

{% tab title="JavaScript" %}
```javascript
var options = {
  method: 'POST',
  url: 'https://api.busstab.ru/oauth/token',
  headers: {'Content-Type': 'application/x-www-form-urlencoded'},
  form: {
    grant_type: 'client_credentials',
    scope: 'profile balance',
    client_id: '<CLIENT_ID>',
    client_secret: '<CLIENT_SECRET>'
  }
};

request(options, function (error, response, body) {
  if (error) throw new Error(error);
  console.log(body);
});
```
{% endtab %}
{% endtabs %}

Ответ сервера:

```javascript
{
    "access_token": "36302671a9353087f698ad9a63301c6fd6dae262",
    "token_type": "Bearer",
    "expires_in": 32140799999,
    "scope": "profile balance"
}
```

Мы получили **access\_token**, который нам необходимо будет прикладывать к каждому запросу на сервер, требующему аутентификацию.

