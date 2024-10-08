# Получить статус коннектора imconnector.status

{% note warning "Мы еще обновляем эту страницу" %}

Тут может не хватать некоторых данных — дополним в ближайшее время

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _не выгружается на prod_" %}

- не указаны типы параметров
- не указана обязательность параметров
- отсутствуют примеры
- отсутствует ответ в случае успеха
- отсутствует ответ в случае ошибки
  
{% endnote %}

{% endif %}

> Scope: [`imopenlines`](../../scopes/permissions.md)
>
> Кто может выполнять метод: любой пользователь

Метод возвращает текущее состояние коннектора.

## Параметры

#|
|| **Параметр** | **Описание** | **С версии** ||
|| **LINE**
[`unknown`](../../data-types.md) | ID открытой линии. | ||
|| **CONNECTOR**
[`unknown`](../../data-types.md) | ID коннектора (который был указан при регистрации обработчика). | ||
|| **ERROR**
[`unknown`](../../data-types.md) | Была ли ошибка в канале. | ||
|| **CONFIGURED**
[`unknown`](../../data-types.md) | Настроен ли канал. | ||
|| **STATUS**
[`unknown`](../../data-types.md) | Текущий статус канала (может ли передавать данные (настроен и не содержит ошибку)). | ||
|#

{% include [Сноска о параметрах](../../../_includes/required.md) %}
