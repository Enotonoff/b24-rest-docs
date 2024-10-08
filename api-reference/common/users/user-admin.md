# Определить права на управление настройками приложений user.admin

{% note warning "Мы еще обновляем эту страницу" %}

Тут может не хватать некоторых данных — дополним в ближайшее время

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _не выгружается на prod_" %}

- отсутствуют примеры
- отсутствует ответ в случае ошибки

{% endnote %}

{% endif %}

> Scope: [`базовый`](../../scopes/permissions.md)
>
> Кто может выполнять метод: любой пользователь

Метод `user.admin` определяет, обладает ли текущий пользователь правами на управление настройками приложений.

## Пример

Запрос
```http
https://my.bitrix24.ru/rest/user.admin.json?auth=d161f25928c3184678924ec127edd29a
```

{% include [Сноска о примерах](../../../_includes/examples.md) %}

## Ответ в случае успеха

> 200 OK
```json
{"result":true}
```

## Смотри также

- [BX24.isAdmin](../../bx24-js-sdk/additional-functions/bx24-is-admin.md)