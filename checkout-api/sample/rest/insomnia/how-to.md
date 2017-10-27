[![яндекс касса](/i/yakassalogo.png "Яндекс Касса")](https://kassa.yandex.ru) / документация / api

Тестовое окружение
==================

### Overview

![пример тестового окружения для тестирования API.Яндекс.Кассы в REST клиенте Insomnia](/checkout-api/sample/rest/insomnia/api.yandex.checkout.insomnia-sample.png "пример тестового окружения для тестирования API.Яндекс.Кассы в REST клиенте Insomnia")

> * Платежи тестовые: примеры для оплаты банковской картой, отмены или проведению возврата оплаченных средств (реальных средств здесь не используется).
> * Во время тестов используйте *карту*: `1111111111111026`, срок действия: `12 25`, cvv: `000`).
> * Вы будете взаимодействовать с нашей боевой системой в режиме real-time (выполняя запрос, вы получите наглядное представление, что отправляется, какой ответ возвращается и т.д.).
> * Кликаете по нужному запросу, получаете результат, читайте аннотацию на вкладке "Docs".
> * Никакой регистрации для работы не требуется, всё работает из коробки.

---

### Инструкция по установке
- [ ] Скачайте дистрибутив программы [Insomnia](https://insomnia.rest/) (~65 Мб; популярный, бесплатный REST-клиент, работающий на ОС Windows, OSX и Ubuntu) и установите;
- [ ] Скачайте [коллекцию запросов API.Яндекс.Кассы](#) (~50 Кб) и [импортируйте](#Импорт-коллекции) полученный файл в Insomnia;
- [ ] Используйте настройки по-умолчанию или пропишите свой shopid и секретный ключ.

#### Импорт коллекции

Импортируйте файл с коллекцией запросов API.Кассы в Insomnia.

> ##### Шаг 1
> ![Insomnia import step1](/checkout-api/sample/rest/insomnia/insomnia-import-step1.png "Insomnia import step1")

> ##### Шаг 2
> ![Insomnia import step2](/checkout-api/sample/rest/insomnia/insomnia-import-step2.png "Insomnia import step2")

> ##### Шаг 3
> ![Insomnia import step3](/checkout-api/sample/rest/insomnia/insomnia-import-step3.png "Insomnia import step3")

#### Настройка

В настройках пропишите ваши тестовые shopid и секретный ключ (если их нет, оставьте те, что прописаны по-умолчанию). Тестовое окружение готово.

> ![Insomnia settings step1](/checkout-api/sample/rest/insomnia/settings-step1.png "Insomnia settings step1")
> ![Insomnia settings step2](/checkout-api/sample/rest/insomnia/settings-step2.png "Insomnia settings step2")

<!--
#### Ссылки
* [Insomnia](https://insomnia.rest/) - удобный, бесплатный REST-клиент под все операционные системы.
* Файл с коллекцией запросов API.Кассы
* Документация API.Кассы
* Гайды API.Кассы
:mortar_board: Тестовые окружение для работы с нашим API, это подготовленная нашими специалистами легкий у установке комлекс

-->
