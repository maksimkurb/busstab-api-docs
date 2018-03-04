# Авторизация

Всего существует несколько методов авторизации OAuth.
В данный момент доступен метод `client_authorization`, когда приложение работает от имени его владельца, его мы рассмотрим ниже. Остальные методы авторизации (в т.ч. кнопка "Войти с помощью Busstab API") станут доступны в ближайшее время.


{% method %}
## `client_authorization`

Для такой авторизации нам необходимо сделать POST-запрос на сервер, передав **client_id** и **client_secret**

{% common %}
#### `POST https://api-staging.busstab.ru/oauth/token`

{% sample lang="curl" %}
```bash
curl -X POST \
  https://api-staging.busstab.ru/oauth/token \
  -H 'Content-Type: application/x-www-form-urlencoded' \
  -d 'grant_type=client_credentials&client_id=<CLIENT_ID>&client_secret=<CLIENT_SECRET>'
```
{% sample lang="js" %}
```js
var options = {
  method: 'POST',
  url: 'https://api-staging.busstab.ru/oauth/token',
  headers: {'Content-Type': 'application/x-www-form-urlencoded'},
  form: {
    grant_type: 'client_credentials',
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
Мы получили `access_token`, который нам необходимо будет прикладывать к каждому запросу на сервер, требующему аутентификацию.

{% endmethod %}
