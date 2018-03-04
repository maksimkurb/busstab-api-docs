# Авторизация

Всего существует несколько методов авторизации OAuth.
В данный момент доступен метод `client_authorization`, когда приложение работает от имени его владельца, его мы рассмотрим ниже. Остальные методы авторизации (в т.ч. кнопка "Войти с помощью Busstab API") станут доступны в ближайшее время.


{% method %}
## `client_authorization`

Для такой авторизации нам необходимо сделать POST-запрос на сервер, передав следующие параметры:

1. **grant_type**: `client_credentials`
1. **client_id**: Свой ID клиента
1. **client_secret**: Свой Секрет клиента
1. **scope**: [Права доступа приложения](/oauth/scope.md)

{% common %}
#### `POST https://api-staging.busstab.ru/oauth/token`

{% sample lang="curl" %}
```bash
curl -X POST \
  https://api-staging.busstab.ru/oauth/token \
  -H 'Content-Type: application/x-www-form-urlencoded' \
  -d 'grant_type=client_credentials&scope=profile:read profile.balance:read&client_id=<CLIENT_ID>&client_secret=<CLIENT_SECRET>'
```

{% sample lang="js" %}
```js
var options = {
  method: 'POST',
  url: 'https://api-staging.busstab.ru/oauth/token',
  headers: {'Content-Type': 'application/x-www-form-urlencoded'},
  form: {
    grant_type: 'client_credentials',
    scope: 'profile:read profile.balance:read',
    client_id: '<CLIENT_ID>',
    client_secret: '<CLIENT_SECRET>'
  }
};

request(options, function (error, response, body) {
  if (error) throw new Error(error);
  console.log(body);
});
```

{% common %}
Ответ сервера:
```json
{
    "access_token": "36302671a9353087f698ad9a63301c6fd6dae262",
    "token_type": "Bearer",
    "expires_in": 32140799999,
    "scope": "profile:read profile.balance:read"
}
```

Мы получили **access_token**, который нам необходимо будет прикладывать к каждому запросу на сервер, требующему аутентификацию.

{% endmethod %}
