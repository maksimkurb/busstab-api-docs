{% method %}
Давайте отправим подобный запрос на сервер:

```graphql
query {
  # Получим информацию о текущем пользователе
  loggedInUser {
    id
    displayName
    imageUrl
  }
  
  # и его последние запросы (до 25 штук)
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

{% method %}
Теперь давайте запросим более подробную информацию о запросах.

Судя по документации API, поле `latestRequests` возвращает массив типа `Request`. `Request` в свою очередь - интерфейс, который наследуется типом `PassportCheckRequest`, у которого есть дополнительные поля: `input` и `payload`.
В поле `input`, очевидно, находятся входные данные, которые передавали для этого запроса, а в `payload` - результат.
Чтобы запросить более подробные данные, выполним следующий запрос:

```graphql
query {
  latestRequests {
    id
    service {
      type
    }
    subject
    stage
    
    # Если тип текущего запроса - PassportCheckRequest, то запрашиваем дополнительные данные у этого запроса
    ...on PassportCheckRequest {
      input {
        level
      }
      payload {
        passportIsDeprecated
        itn
        mrz
        basic {
          type
          message
        }
      }
    }
  }
}
```

{% common %}
В ответе мы получим следующий результат:

```json
{
	"data": {
		"latestRequests": [{
				"id": "UmVxdWVzdDo5NDU5MWRlOC1lOThlLTExZTctYTg3My1lZmY4ZDM2ZmU3MzE=",
				"service": {
					"type": "PASSPORT_CHECK"
				},
				"subject": "ИВАНОВ ПЕТР, 4500 1***56",
				"stage": "FINISHED",
				"input": {
					"level": "FULL"
				},
				"payload": {
					"passportIsDeprecated": false,
					"itn": "123456789",
					"mrz": "PNRUSIVANOV<<PETR<<SIDOROVICH<<<<<<<<<<<<<<<\n450123456XRUS780613XM110531X01105010000000<X8",
					"basic": [{
						"type": "INFO",
						"message": "Согласно серии бланка паспорта, регионом выдачи является *Московская область*"
					}],
					"complex": [{
							"type": "WARNING",
							"message": "Паспорт выдан в выходной и/или праздничный день. Нарушение требования статьи **193 ГК РФ**: «Если последний день срока приходится на нерабочий день, днём окончания срока считается ближайший следующий за ним рабочий день». Рекомендуется проведение дополнительной проверки."
						},
						{
							"type": "INFO",
							"message": "Данные паспорта проходят верификацию в ФНС. Лицо с указанными данными прошло постановку на налоговый учёт"
						}
					]
				}
			},
			{
				"id": "UmVxdWVzdDo1MWRkYThkOC1kYzI1LTExZTctYjZiNi0xYjNkMTdhMzBhYmQ=",
				"service": {
					"type": "PASSPORT_CHECK"
				},
				"subject": "Павел Овсянкин, 0109 1***02",
				"stage": "FINISHED",
				"input": {
					"level": "COMPLEX"
				},
				"payload": {
					"passportIsDeprecated": false,
					"itn": null,
					"mrz": null,
					"basic": [{
						"type": "INFO",
						"message": "Согласно серии бланка паспорта, регионом выдачи является *Алтайский край*"
					}],
					"complex": [{
							"type": "FATAL",
							"message": "Согласно введённой даты рождения, выдача паспорта указанному лицу невозможна, поскольку оно не достигло 14-летнего возраста на момент выдачи паспорта."
						},
						{
							"type": "WARNING",
							"message": "Данные паспорта не проходят верификацию в ФНС. Учитывая возраст лица **14** лет возможно постановка на учёт ещё не произведена. При наличии проверяемого документа на руках, рекомендуется проверить страницу № 19, где должны быть указаны сведения о ранее выданных паспортах. В случае выявления данных прежнего паспорта проверьте также и его."
						}
					],
					"full": []
				}
			}
		]
	}
}
```
{% endmethod %}