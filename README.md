# Проектирование высоконагруженного сервиса интернет-магазина

# 1 Тема и целевая аудитория

## Тема
Маркетплейс Wildberries — российский международный интернет-магазин, предлагающий одежду, обувь, электронику, детские товары, товары для дома и другие категории товаров.

## MVP
1. Авторизация, регистрация по номеру телефона.
2. Поиск товаров по ключевым словам.
3. Главная страница персонализированных предложений.
4. Добавление товаров в корзину.
5. Редактирование количества и удаление товаров.
6. Оставление отзывов о приобретённых товарах.
7. Оформление заказа.
8. Статус заказа при доставке.
9. Добавление товаров продавцом на площадку с учётом фотографии.
10. Авторизация, регистрация для продавцов.
11. Открытие карточки товара.

## Продуктовые особенности
* Платежная система — удобная и безопасная система оплаты, которая поддерживает различные способы оплаты (банковские карты, электронные кошельки и другие), гарантируя комфорт и безопасность транзакций для пользователей.

## ЦА
Ниже представлена статистика исследования аудитории Wildberries.
Возраст целевой аудитории варьируется в диапазоне от 18 до 65+ лет. [[1]](https://www.similarweb.com/ru/website/wildberries.ru/#demographics)
Возрастная группа | Процентное соотношение
------------------| -------------
18-24 года        | 14,31% 
25-34 года        | 23,79% 
35-44 года        | 18,11% 
45-54 года        | 17,53% 
55-64 года        | 14,78% 
65+   года        | 11,48% 

Размер целевой аудитории: 58.7 млн. пользователей в месяц, 24.2 млн. пользователей в день на момент февраля 2024г. [[2]](https://oborot.ru/news/kolichestvo-posetitelej-marketplejsov-wildberries-i-ozon-v-mesyac-sravnyalos-kuda-eshhe-hodyat-pokupateli-i209527.html)

География пользователей:
Страна       | Процентное соотношение
-------------| ----------------------
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
Метрика                                                                                         | Значение
------------------------------------------------------------------------------------------------| -----------------------
Месячная аудитория MAU                                                                          | 58.7 млн. пользователей
Дневная аудитория DAU                                                                           | 24.2 млн. пользователей
Количество регистраций в день [[4]](https://adindex.ru/news/researches/2023/02/28/310879.phtml) | 40 тыс. пользователей
Средняя длительность сессии   [[5]](https://dzen.ru/a/ZkT8wKCKkBgtJMT-)                         | 11 минут

### Хранилище данных для пользователя

Параметр                                                | Число
--------------------------------------------------------| -----------------
Поиск товаров по ключевым словам                        | 3 запросов/день
Добавление товаров в корзину                            | 4 товаров/день
Оформление заказа                                       | 1 заказов/день
Среднее количество товаров в заказе                     | 3 товаров
Оставление отзывов                                      | 2 отзывов/день
Средняя длина отзыва                                    | 60 символов UTF-8
Средний количество фотографий у отзыва                  | 3 шт.
Добавление товаров продавцом                            | 1 товаров/день
Средний размер товара без учета фотографии              | 6.3 кБ
Средний количество фотографий у товара                  | 7 шт.
Средний размер фотографии товара                        | 86 кБ
Главная страница персонализированных предложений        | 20 товаров/день
Добавление товаров продавцом                            | 4 товаров/день
Редактирование количества и удаление товаров из корзины | 2 товаров/день
Открытие карточки товара                                | 10 товаров/день
Статус заказа при доставке                              | 3 запроса/день

Объём данных на одного пользователя в день:
* Добавление товаров в корзину: 4 товара * 6.3 кБ = 25.2 кБ
* Оформление заказа: 3 товара * 6.3 кБ = 18.9 кБ
* Оставление отзывов: 2 отзыва * (0.06 кБ + 3 * 86 кБ) = 467.72 кБ
* Главная страница персонализированных предложений: 20 товаров * 6.3 кБ = 126 кБ.
* Добавление товаров продавцом: 4 товаров * (6.3 кБ + 7 * 86 кБ) = 4 товаров * 2461.2 кБ = 9844.8 кБ.
* Редактирование количества и удаление товаров из корзины: 2 товаров * 6.3 кБ = 12.6 кБ.
* Открытие карточки товара: 10 товаров * (6.3 кБ + 17 отзывов * 0.06 кБ) = 10 товаров * 7.32 кБ = 73.2 кБ.
* Статус заказа при доставке: 3 запроса * 7.6 кБ = 22.8 кБ
* Загрузка медиа: 48 * 86 кБ = 4128 кБ

Тогда общий объем товаров в день, добавляемых в корзину, оформленых в заказе, оставление отзывов, получение главной ленты на одного пользователя равен: 
4 * 6.3 (кБ) + 3 * 6.3 (кБ) + 2 * (0.06 (кБ) + 3 * 86 (кБ)) + 20 * 6.3 (кБ) + 4 * (6.3 (кБ) + 7 * 86 (кБ)) + 2 * 6.3 (кБ) + 10 * (6.3 (кБ) + 7 * 86 (кБ)) + 3 * 7.6 (кБ) + 48 * 86 (кБ) = 14719.22 кБ = 14.7 МБ

### Динамический рост

Так как за год заказывается около 618 млн. [[7]](https://www.cnews.ru/news/line/2023-06-08_wildberries_itogi_i_kvartala_2023) товаров, то можно вычислить, какой объем данных будет занят пользователями:
* Расчёт данных на товары (всего): 6.3 (кБ) * 618 млн. = 3.9 ТБ
* Расчёт данных только на фотографии товаров: 7 * 86 (кБ) * 618 млн. = 372.04 ТБ

### Сетевой трафик
При расчете сетевого трафика не будем учитывать запросы, связанные с регистрацией пользователей, так как они не создают ощутимую нагрузку на наш сервис.
Основная нагрузка приходится на оформление заказа, добавление в корзину и оставление отзывов.

### Предварительные расчеты:
### Трафик по видам активности на одного пользователя:
1. Поиск товаров:

* 3 запроса/день. Примерный объем одного запроса можно оценить в 10 кБ (запрос + ответ от сервера). Итого: 3 ∗ 10 кБ = 30 кБ

2. Добавление товаров в корзину:

* 4 товара/день. Объем данных на один товар — 6.3 кБ. Итого: 25.2 кБ

3. Оформление заказа:

* 1 заказ в день с 3 товарами. Итого: 18.9 кБ

4. Оставление отзывов:

* 2 отзыва/день, 467.72 кБ (как рассчитано ранее). Итого: 467.72 кБ

5. Главная страница персонализированных предложений:

* 20 товаров/день, 126 кБ (как рассчитано ранее). Итого: 126 кБ

6. Добавление товаров продавцом:

* 4 товаров/день, 9844.8 кБ (как рассчитано ранее). Итого: 9844.8 кБ

7. Редактирование количества и удаление товаров из корзины:

* 2 товаров/день, 12.6 кБ (как рассчитано ранее). Итого: 12.6 кБ

8. Открытие карточки товара:

* 10 товаров/день и 17 отзывов для каждого товара, 73.2 кБ (как рассчитано ранее). Итого: 73.2 кБ

9. Статус заказа при доставке:

* 3 запроса/день, 22.8 кБ (как рассчитано ранее). Итого: 22.8 кБ

10. Медиа в день со всех ручек:

* 48 медиа/день, 4128 кБ (как рассчитано ранее). Итого: 4128 кБ

10. Трафик на одного пользователя в день:

* 30 кБ + 25.2 кБ + 18.9 кБ + 467.72 кБ + 126 кБ + 9844.8 кБ + 12.6 кБ + 73.2 кБ + 22.8 кБ + 4128 кБ = 14719.22 кБ = 14.7 МБ

### Сетевой трафик по видам активности (дневная аудитория 24.2 млн. пользователей):

Пиковое значение активности пользователей(соответственно RPS тоже) приблизительно в 1.66 раза выше среднего значения. Возьмем коэффициент запаса равный 2

Тип                           | Отправка (дневная аудитория 24.2 млн) | Отправка Гб/сек | Пиковое значение | Значение с коэффициентом запаса 2 |
------------------------------| --------------------------------------| ----------------| -----------------|---------------------------------- |
Поиск товаров                 | 24.2 млн * 30 кБ = 726 ГБ             | 0.0084          | 0.0139           | 0.0168                            |
Добавление товаров в корзину  | 24.2 млн * 25.2 кБ = 609.84 ГБ        | 0.007           | 0.0117           | 0.014                             |
Оформление заказа             | 24.2 млн * 18.9 кБ = 457.38 ГБ        | 0.0053          | 0.0088           | 0.0106                            |
Оставление отзывов            | 24.2 млн * 467.72 кБ = 11 314 ГБ      | 0.131           | 0.217            | 0.262                             |
Главная страница              | 24.2 млн * 126 кБ = 3049.2 ГБ         | 0.035           | 0.0581           | 0.07                              |
Редактирование корзины        | 24.2 млн * 12.6 кБ = 304.92 ГБ        | 0.0035          | 0.00581          | 0.007                             |
Добавление товаров продавцом  | 24.2 млн * 9844.8 кБ = 236.28 ТБ      | 2.734           | 4.538            | 5.468                             |
Открытие карточки товара      | 24.2 млн * 73.2 кБ = 1771.44 ГБ       | 0.021           | 0.035            | 0.042                             |
Статус заказа при доставке    | 24.2 млн * 22.8 кБ = 551.76 ГБ        | 0.0063          | 0.011            | 0.0126                            |
Медиа                         | 24.2 млн * 4128 кБ = 99.8976 ТБ       | 1.156           | 1.92             | 2.312                             |
Итого                         | 354961.98 ГБ = 354.96 	ТБ             | 4.1071          | 6.8179           | 8.2142                            |

### RPS
 
* Поиск товаров: 24.2 млн * 3 / 86400 = 839 RPS
* Добавление товаров в корзину: 24.2 млн * 4 / 86400 = 1120 RPS
* Оформление заказов: 24.2 млн. * 1 / 86400 = 280 RPS
* Оставление отзывов: 24.2 млн. * 2 / 86400 = 560 RPS
* Добавление товаров продавцом: 24.2 млн. * 1 / 86400 = 280 RPS
* Главная страница персонализированных предложений: 24.2 млн * 20 / 86400 = 5600 RPS
* Редактирование количества и удаление товаров из корзины: 24.2 млн * 2 / 86400 = 560 RPS
* Добавление товаров продавцом: 24.2 млн * 4 / 86400 = 1120 RPS
* Открытие карточки товара: 24.2 млн * 10 / 86400 = 2800 RPS
* Статус заказа при доставке: 24.2 млн * 3 / 86400 = 841 RPS
* Загрузка медиа: 24.2 млн * 48 / 86400 = 13445 RPS

Действие                            | RPS   | Пиковое значение | Пиковое значение с коэффициентом запаса 2  |
------------------------------------| ------| -----------------| ------------------------------------------ |
Поиск товаров                       | 839   | 1393             | 2786                                       |
Добавление товаров в корзину        | 1120  | 1859             | 3718                                       |
Оформление заказов                  | 280   | 465              | 930                                        |
Оставление отзывов                  | 560   | 930              | 1860                                       |
Добавление товаров продавцом        | 280   | 465              | 930                                        |
Главная страница                    | 5600  | 9296             | 11200                                      |
Редактирование корзины              | 560   | 930              | 1120                                       |
Добавление товаров продавцом        | 1120  | 1860             | 2240                                       |
Открытие карточки товара            | 2800  | 4648             | 5600                                       |
Статус заказа при доставке          | 841   | 1396             | 1682                                       |
Загрузка медиа                      | 13445 | 22318            | 26890                                      |
**Итого**                           | 27445 | 45558            | 54890                                      | 

# 3 Глобальная балансировка нагрузки

### География пользователей

Целевая аудитория Wildberries распределена в основном по странам СНГ, с преобладанием в России. Вот как распределяются пользователи по регионам:

Страна       | Процентное соотношение
-------------| ----------------------
Россия       | 93.05%
Беларусь     | 2.06% 
Казахстан    | 1.74%
Армения      | 0.53% 
Другие       | 2.63% 

Это определяет необходимость расположения датацентров (ДЦ) преимущественно в России и других странах СНГ для обеспечения низкой задержки и высокой производительности.

### Функциональное разбиение по доменам

Для оптимизации работы и распределения нагрузки по различным функциональным модулям можно применить функциональное разбиение по доменам. Это позволяет отдельным частям системы работать независимо друг от друга, улучшая масштабируемость и отказоустойчивость.

Примеры функциональных доменов:

* wildberries.ru — API-сервисы для мобильных приложений и веб-версии.
* cdn.wildberries.ru — доставка медиа-контента (фото товаров, видео, баннеры).
* wbxoofex.wildberries.ru — сервис оформления заказов и управления корзиной.
* search.wb.ru — отдельный домен для поиска товаров.
* seller.wildberries.ru — платформа для продавцов.

Такое разбиение позволяет улучшить управляемость трафика, распределять нагрузку на отдельные системы, а также внедрять более гибкие методы балансировки.

### Обоснование расположения ДЦ

На основе [географии пользователей](https://map.wb.ru/#2/53.05/127.13) можно обосновать расположение датацентров. Основная цель — снизить сетевые задержки и улучшить производительность для конечных пользователей.

Страна    | Локация ДЦ                | Обоснование
----------| --------------------------| ---------------------------------------------------------------------------------------------------
Россия    |	Москва                 	  | 93.05% пользователей из России, оптимизация для западной части страны
Россия    |	Санкт-Петербург	          | 93.05% пользователей из России, оптимизация для западной части страны
Россия    |	Новосибирск	              | Покрытие для пользователей Центральной России, Урала, Сибири, восточных регионов и Дальнего Востока
Беларусь  |	Минск	                    | Поддержка пользователей Беларуси (2.06%)
Казахстан |	Алматы	                   | Покрытие для пользователей Казахстана (1.74%)

Основное внимание уделяется России, так как большинство пользователей находится в этой стране. Однако наличие датацентров в соседних странах (Беларусь, Казахстан, Армения) помогает улучшить доступ к сервису и уменьшить задержки для пользователей этих регионов.

### Схема DNS-балансировки

Latency-Based DNS с использованием BGP-маршрутизации

Latency-Based DNS-балансировка направляет запросы пользователей на ближайший датацентр с наименьшей задержкой, оценивая это на основе метрик задержек в сети (latency) и BGP-маршрутов.

Алгоритм работы:
1. Пользовательский запрос: Пользователь отправляет запрос на домен.
2. Определение ближайшей BGP-подсети:
* DNS-сервер анализирует местоположение пользователя на основе IP-адреса. Это осуществляется за счёт баз данных, сопоставляющих IP с географической зоной и BGP-подсетью.
* DNS-сервер затем отправляет ICMP-запросы или использует протоколы мониторинга сети для замера latency (времени задержки) до ближайших датацентров в различных BGP-подсетях.
* По результатам измерений DNS выбирает BGP-подсеть с минимальной задержкой.
3. Маршрутизация на уровне BGP:
* После определения ближайшей BGP-подсети, запрос пользователя направляется в эту подсеть.
* Внутри выбранной BGP-подсети происходит более детальный анализ задержек между несколькими датацентрами. Например, если в выбранной подсети есть несколько датацентров, то на этом этапе измеряются задержки между пользователем и каждым из них.
4. Перенаправление на конкретный датацентр:
* После оценки задержек между всеми доступными датацентрами в выбранной BGP-подсети, запрос направляется в датацентр с наименьшей задержкой.
* Если ближайший датацентр перегружен или недоступен, механизм автоматически переключает запрос на следующий ближайший датацентр в этой же BGP-подсети с минимальной задержкой.

# 4 Локальная балансировка нагрузки

L3:
L3 будет использоваться в качестве первичной балансировки, чтобы распределить трафик по серверам внутри ЦОД. Балансировка будет по схеме Virtual Server via IP Tunneling

![image](https://raw.githubusercontent.com/Karpov-Ivan/Ivan_Karpov_Autumn2024_Highload/refs/heads/main/images/311767474-de3f99a0-3714-429c-b93a-7ac53e610c17.png)


Для обеспечаения отказоустойчивости системы стоит использовать framework *keepalived* благодаря следующим преимуществам:
* Производительность
* Открытость
* Простота настройки

Будем использовать **Virtual Router Redundancy Protocol(VRRP)**. Соответственно, мы получаем возможность отслеживать состояние узлов системы, а при отказе одного из них перенаправить трафик на другой узел.

L7:
На отдельно взятом сервере будет проходить балансировка с помощью L7. На этом уровне для балансировщика нагрузки важна поддержка HTTP-заголовков. Будем использовать балансировщик NGINX благодаря следующим преимуществам:
* Лучшая производительность
* Поддержка различных протоколов(в т.ч. gRPC и HTTP)
* Открытость
* Гибкость настройки

Также для решения проблемы отказоустойчивости на этом уровне будет использоваться k8s с использованием:
* Liveness probe
* Readiness probe

Для шифрования будем пользоваться услугами центра [Let's encrypt](https://letsencrypt.org)
Для оптимизации установки соединения будет пользоваться Session cache

# 5 Логическая схема базы данных

## Схема базы данных
[Ссылка на логическую схему БД](https://dbdiagram.io/d/6702ea1cfb079c7ebd82a4c2)

![Highload Ivan Karpov DB](https://github.com/Karpov-Ivan/Ivan_Karpov_Autumn2024_Highload/blob/main/images/db.png)

Тип       | Размер в байтах
----------| ----------
uuid      | 16
int       | 4
numeric   | 8
timestamp | 8
boolean   | 1
text      | Динамический, зависит от длины (1 символ ≈ 1 байт)

**Session**
```TeX
  token(32) + profile_id(16) = 48
``` 
---
**Profile**
```TeX
  id(16) + login(32) + description(128) + phone(19) + password_hash(64) + type(10) = 269 байт
``` 
---
**Address**
```TeX
  id(16) + profiel_id(16) + country(16) + city(16) + street(16) + house(8) + is_current(1) = 89 байт
``` 
---
**Product**
```TeX
  id(16) + name(128) + description(1024) + price(4) + rating(8) + category_id(16) + count_comments(4) + file_id(16) + company_id(16) = 1232 байт
``` 
---
**Category**
```TeX
  id(16) + name(16) + parent(16) = 48 байт
``` 
---
**File**
```TeX
  id(16) + link_file(64) + file_type(16) = 96 байт
``` 
---
**Comment**
```TeX
  id(16) + profile_id(16) + product_id(16) + comment(256) + rating(4) + file_id(16) = 324 байт
``` 
---
**Cart**
```TeX
  id(16) + profile_id(16) = 32 байт
``` 
---
**Shopping_Cart_Item**
```TeX
  id(16) + cart_id(16) + product_id(16) + quantity(4) = 52 байт
``` 
---
**Order_Item**
```TeX
  id(16) + product_id(16) + order_info_id(16) + quantity(4) + price(4) = 56 байт
``` 
---
**Order_Info**
```TeX
  id(16) + address_id(16) + profile_id(16) + status_id(16) + creation_at(8) + delivery_at(8) = 80 байт
``` 
---
**Status**
```TeX
  id(16) + name(16) = 32 байт
``` 
---
**Company**
```TeX
  id(16) + name(32) + profile_id(16) = 64 байт
``` 
---

Таблица            | Размер строки [byte]
-------------------| ---------------
Session            | 48
Profile            | 269
Address            | 89  
Product            | 1232
Category           | 48
File               | 96
Comment            | 324
Cart               | 32
Shopping_Cart_Item | 52
Order_Item         | 56
Order_Info         | 80
Status             | 32
Company            | 64

# Список источников

1. https://www.similarweb.com/ru/website/wildberries.ru/#demographics
2. https://oborot.ru/news/kolichestvo-posetitelej-marketplejsov-wildberries-i-ozon-v-mesyac-sravnyalos-kuda-eshhe-hodyat-pokupateli-i209527.html
3. https://dzen.ru/a/ZkT8wKCKkBgtJMT-
4. https://adindex.ru/news/researches/2023/02/28/310879.phtml
5. https://dzen.ru/a/ZkT8wKCKkBgtJMT-
6. https://www.vedomosti.ru/business/articles/2024/05/05/1035562-ozon-i-wildberries
7. https://www.cnews.ru/news/line/2023-06-08_wildberries_itogi_i_kvartala_2023
