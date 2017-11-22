[![яндекс касса](/i/yakassalogo.png "Яндекс Касса")](https://kassa.yandex.ru) / [помощь](https://yandex.ru/support/checkout/) / [api-докуметация](https://kassa.yandex.ru/docs/checkout-api/#api-yandex-kassy), [api-гайды](https://kassa.yandex.ru/docs/guides/#bystryj-start), [YandexCheckout.js](https://kassa.yandex.ru/docs/checkout-js/#yandexcheckout-js)

Тестовое окружение API.Яндекс.Кассы
===================================
> Простое в установке и удобное в использовании окружение для того, чтобы освоить наше API разработчикам, выполняющим интеграцию или интересующимся.

Для самостоятельных тестов:
```
	"shopid": "54401",
	"api_key": "test_OGpIHQMdVeLp1giWoPn033vKRxNUAGcAdZizIymbOfg"
```

### Overview

![пример тестового окружения для тестирования API.Яндекс.Кассы в REST клиенте Insomnia](/checkout-api/sample/rest/insomnia/api.yandex.checkout.insomnia-sample.png "пример тестового окружения для тестирования API.Яндекс.Кассы в REST клиенте Insomnia")

 * В тестовом окружении представлены примеры для оплаты банковской картой  
 (реальных средств здесь нет, только тестовые).
 * Используйте *тестовую карту*: `1111111111111026`, срок действия: `12 25`, cvv: `000`.
 * Вы будете взаимодействовать с нашей боевой системой в режиме real-time:
   * выполняя запрос, вы получите наглядное представление, что отправляется, какой ответ возвращается и т.д.;
   * cможете при необходимости изменить запрос.
 * На каждом шаге есть помощь на вкладке "Docs" и необходимые ссылки.
 * **Регистрации не требуется**, всё работает из коробки.

---

### Инструкция по установке
- [ ] Скачайте дистрибутив программы [Insomnia](https://insomnia.rest/) и установите (~65 Мб; популярный, бесплатный REST-клиент, работающий на ОС Windows, OSX и Ubuntu);
- [ ] Скачайте [коллекцию запросов API.Яндекс.Кассы](/checkout-api/sample/rest/insomnia/API.Yandex.Kassa_test-env-for-insomnia.v1-001.ru.json.zip) (~10 Кб), разархивируйте и [импортируйте](#Импорт-коллекции) в Insomnia;
- [ ] Используйте настройки по-умолчанию или [пропишите](#Настройка) свой shopid и секретный ключ.

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
