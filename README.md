# Проектирование высоконагруженного сервиса интернет-магазина

# 1 Тема и целевая аудитория

## Тема

Маркетплейс Wildberries — российский международный интернет-магазин, предлагающий одежду, обувь, электронику, детские товары, товары для дома и другие категории товаров.

## MVP
1. Авторизация, регистрация по номеру телефона.
2. Поиск товаров по ключевым словам.
3. Функции сортировки по разным критериям, включая цену, популярность и новизну.
4. Добавление товаров в корзину.
5. Редактирование количества и удаление товаров.
6. Оставление отзывов о приобретённых товарах.
7. Оформление заказа.
8. Статус заказа при доставке.
9. Добавление товаров продавцом на площадку с учётом фотографии.
10. Авторизация, регистрация для продавцов.

## Продуктовые особенности
* Персонализированные предложения — пользователи получают индивидуальные рекомендации и специальные скидки на основе их предыдущих покупок и предпочтений. Система анализирует поведение клиента и предлагает наиболее релевантные товары, что повышает вероятность совершения покупок.
* Платежная система — удобная и безопасная система оплаты, которая поддерживает различные способы оплаты (банковские карты, электронные кошельки и другие), гарантируя комфорт и безопасность транзакций для пользователей.

## ЦА
Ниже представлена статистика исследования аудитории Wildberries.
Возраст целевой аудитории варьируется в диапазоне от 18 до 65+ лет. [[1]](https://www.similarweb.com/ru/website/wildberries.ru/#demographics)
Возрастная группа | Процентное соотношение
------------ | -------------
18-24 года| 14,31% 
25-34 года| 23,79% 
35-44 года| 18,11% 
45-54 года| 17,53% 
55-64 года| 14,78% 
65+   года| 11,48% 

Размер целевой аудитории: 58.7 млн. пользователей в месяц, 24.2 млн. пользователей в день на момент февраля 2024г. [[2]](https://oborot.ru/news/kolichestvo-posetitelej-marketplejsov-wildberries-i-ozon-v-mesyac-sravnyalos-kuda-eshhe-hodyat-pokupateli-i209527.html)

География пользователей:
Страна       | Процентное соотношение
-------------| -------------
Россия       | 93.05%
Беларусь     | 2.06% 
Казахстан    | 1.74%
Армения      | 0.53% 
Другие       | 2.63% 

Гендерное распределение аудитории Wildberries [[3]](https://dzen.ru/a/ZkT8wKCKkBgtJMT-):
Пол          | Процентное соотношение
-------------| -------------
Мужской      | 30.00%
Женский      | 70.00% 

# 2 Расчёт нагрузки

Продуктовые метрики:
Метрика      | Значение
-------------| -------------
Месячная аудитория MAU        | 58.7 млн. пользователей
Дневная аудитория DAU         | 24.2 млн. пользователей
Количество регистраций в день [[4]](https://adindex.ru/news/researches/2023/02/28/310879.phtml) | 40 тыс. пользователей
Средняя длительность сессии   [[5]](https://dzen.ru/a/ZkT8wKCKkBgtJMT-) | 11 минут
Количество заходов в день     [[6]](https://www.vedomosti.ru/business/articles/2024/05/05/1035562-ozon-i-wildberries) | 11 млн.

### Хранилище данных для пользователя

Параметр          | Число
-------------| -------------
Поиск товаров по ключевым словам           | 3 запросов/день
Добавление товаров в корзину               | 4 товаров/день
Оформление заказа                          | 1 заказов/день
Среднее количество товаров в заказе        | 3 товаров
Оставление отзывов                         | 2 отзывов/день
Средняя длина отзыва                       | 60 символов UTF-8
Средний количество фотографий у отзыва     | 3 шт.
Добавление товаров продавцом               | 1 товаров/день
Средний размер товара без учета фотографии | 6.3 кБ
Средний количество фотографий у товара     | 7 шт.
Средний размер фотографии товара           | 86 кБ

Тогда общий объем товаров в день, добавляемых в корзину, оформленых в заказе, оставление отзывов, на одного пользователя равен: 
4 * (6.3 (кБ) + 7 * 86 (кБ)) + 3 * (6.3 (кБ) + 7 * 86 (кБ)) + 2 * (0.06 (кБ) + 3 * 86 (кБ)) = 4774,82 кБ

### Динамический рост

Так как за год заказывается около 618 млн. товаров, то можно вычислить, какой объем данных будет занят пользователями:
* Расчет для товаров в общем: (6.3 (кБ) + 7 * 86 (кБ)) * 618 млн. = 375.93 ТБ
* Расчет для фотографий: 7 * 86 (кБ) * 618 млн. = 372.04 ТБ

### Сетевой трафик
При расчете сетевого трафика не будем учитывать запросы, связанные с регистрацией пользователей, так как они не создают ощутимую нагрузку на наш сервис.
Основная нагрузка приходится на оформление заказа, добавление в корзину и оставление отзывов.

# Список источников

1. https://www.similarweb.com/ru/website/wildberries.ru/#demographics
2. https://oborot.ru/news/kolichestvo-posetitelej-marketplejsov-wildberries-i-ozon-v-mesyac-sravnyalos-kuda-eshhe-hodyat-pokupateli-i209527.html
3. https://dzen.ru/a/ZkT8wKCKkBgtJMT-
4. https://adindex.ru/news/researches/2023/02/28/310879.phtml
5. https://dzen.ru/a/ZkT8wKCKkBgtJMT-
6. https://www.vedomosti.ru/business/articles/2024/05/05/1035562-ozon-i-wildberries
