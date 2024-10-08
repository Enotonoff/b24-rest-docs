# Условия в промптах

{% note warning "Мы еще обновляем эту страницу" %}

Тут может не хватать некоторых данных — дополним в ближайшее время

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _не выгружается на prod_" %}

- нужны правки под стандарт написания

{% endnote %}

{% endif %}

В препромпте, почти как в программировании, можно использовать `if` условия и `switch` ветвления.

## Условия

Сразу начнём с примера:

```js
@if (engine.code = ChatGPT)
	Говори от имени робота.
@else
	Говори от имени человека.
@endif
```

В результате, в зависимости от выбранного провайдера на портале, итоговый препромпт будет отличаться.

Что можно писать в условии `@if ()`? Посмотрим ниже. Регистр и лишние пробелы значения не имеют.


## Системные поля

- `engine.code` — код провайдера (ChatGPT, GigaChat, YandexGPT, [ThirdParty](*ThirdParty))
- `engine.category` — может принимать значения text или image. Но на текущий момент препромпты CoPilot работают только для текстов
- `context.module` — модуль, который вызывает CoPilot. Например, можно как-то иначе дополнить препромпт, если запрос идет для модуля CRM

Пример условия по системному полю был в начале страницы. Приведём ещё один:
```js
@if (engine.code = ChatGPT)
	Не забывай, что ты ChatGPT
@endif
```

## Проверка на пустоту

```js
@if (author.name = null)
	говори от имени анонимуса
@endif
```

## Отрицание

```js
@if (engine.code != ChatGPT)
	{context_messages}
@endif
```

## Работа с базовыми маркерами

Это [базовые маркеры](./markers.md), а также предустановленные разработчиком маркеры. Но в силу того, что в них содержится пользовательский контент, использовать их имеет смысл только для проверки на непустоту.

```js
@if (marker.original_message != null)
	похвали за {original_message}
@else
	похвали за {user_message}
@endif
```
С другой стороны через проверку маркера вы можете опираться на какой-то системный предустановленный маркер, который всегда устанавливается разработчиком при наступлении определенных условий.

```js
@if (marker.role = vip)
	пиши очень важно
@endif
```
## Работа с маркерами результата

Информацию о маркерах результата вы найдёте на странице документации о [маркерах](./markers.md).

Как применять их в условиях? По логике вещей, использовать вы их можете только в задачах проверки на пустоту. Например:

```js
@if (current_result0 != null)
	Это не первое уточнение пользователя, будь внимательнее, пожалуйста!
@endif
```
Важное уточнение — проверять доступность маркера нужно только для вашей логики. Запись такого вида:

```js
@if (current_result0 != null)
	{current_result0}
@endif
```

бессмысленна, так как если маркера нет, то он и так вырежется из текста автоматически.

## Функции внутри if-условий

Внутри условия if-выражения можно использовать функции.
Пока что поддерживается только определение длины `length()`. Внутрь этой функции, в принципе, можно передать любой маркер, в том числе пользовательский.

```js
@if (length(marker.user_message) < 10)
	Итоговый ответ должен быть очень лаконичным.
@else
	Пиши как Толстой.
@endif
{user_message}
```

## Функции сеттеры

Внутри промптов можно устанавливать температуру и токены. Причем разные, в зависимости от [условий](*условия).

- `@setTemperature()`
- `@setTokens()`

Дополним пример из предыдущего раздела про функции, использовав сеттеры:

```js
@if (length ( marker.user_message ) < 10)
	Итоговый ответ должен быть очень лаконичным.
	@setTemperature(0.12)
	@setTokens(200)
@else
	Пиши как Толстой.
	@setTemperature(1)
	@setTokens(1200)
@endif
```

## Ветвление

Это известный в программировании `switch`. Придет на помощь, когда у вас есть разные блоки промпта, но каждый из них выполняется строго в определенном порядке. Хороший пример, когда у вас разный текст промпта для разных провайдеров. Давайте на этом примере и рассмотрим.

```js
@switch (engine.code)
@case(ChatGPT)
	**инструкции для GPT**
@default
	**инструкции для остальных провайдеров**
@endswitch
```
Что можно вставлять в `switch`? Все то же, что и в `if`.

Условие ветвления имеет наивысший приоритет, а это значит, что внутри case-блоков могут содержаться в том числе if-выражения. Блоков ветвления может быть несколько, хотя это уже снизит читабельность.

Пример совместного использования switch и if:

```js
@switch (engine.code)
@case(ChatGPT)
	ты поросенок 
	@if(author.personalgender = m) розового цвета @else синего цвета @endif
@default
	ты волк
@endswitch
@switch (engine.code)
@case(ChatGPT)
	c Плутона
@default
	@if(author.personalgender = m) с Марса @else с Венеры @endif
@endswitch
```

[*ThirdParty]: ThirdParty — «третья сторона». Т.е. здесь может быть код стороннего провайдера, разработанного партнером.

[*условия]: Это могут быть как if-условия, так и switch ветвления.