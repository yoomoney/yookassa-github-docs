<!--
### N.B.

** Внимание! ** Начиная с 04.05.2018 изменился пароль (`api_key`). Если вы использовали старый пароль, обновите его. Старый пароль ~test_OGpIHQMdVeLp1giWoPn033vKRxNUAGcAdZizIymbOfg~.
-->

[![ЮKassa](/i/yookassalogo.png "ЮKassa")](https://yookassa.ru) / [помощь](https://yookassa.ru/docs/support) / [api-докуметация](https://yookassa.ru/developers), [Checkout.js](https://yookassa.ru/developers/payment-forms/other/yc-js), [SDK](https://yookassa.ru/developers#sdk)

Тестовое окружение API.ЮKassa
===================================
> Простое в установке и удобное в использовании окружение для того, чтобы освоить наше API разработчикам, выполняющим интеграцию или интересующимся. Внимание! Это статья только для программистов.

```
### Для самостоятельных тестов без Insomnia
	"shopid": "54401",
	"api_key": "test_Fh8hUAVVBGUGbjmlzba6TB0iyUbos_lueTHE-axOwM0"
```
<!--
### BUG FIX 21.02.2018

К сожалению, в процессе эксплуатации тестового магазина возникла одна техническая накладка. Раньше в запросах не требовалось передавать номер товара (и так оно и должно быть в обычном shopid), но мы тестировали многотоварный режим и теперь в запросе payments передавайте:

```
	"recipient": {
    		"gateway_id": "289250"
  	}
```
Иначе вы будете получать ошибку вида:
```
{
	"type": "error",
	"id": "fb202ed8-e975-46b1-991f-b5ec948d1fee",
	"code": "invalid_request",
	"description": "Failed to resolve gateway id, please provide recipient field in request",
	"parameter": "recipient"
}
```
Это временно. Поправим.
-->

### Overview

![пример тестового окружения для тестирования API.ЮKassa в REST клиенте Insomnia](/checkout-api/sample/rest/insomnia/api.yookassa.insomnia-sample.png "пример тестового окружения для тестирования API.ЮKassa в REST клиенте Insomnia")

 * В тестовом окружении представлены примеры для оплаты банковской картой  
 (реальных средств здесь нет, только тестовые).
 * Используйте *тестовую карту*: `5555555555554444`, срок действия: `12 20`, cvv: `000`. Или [другие тестовые карты](https://yookassa.ru/developers/using-api/testing#test-bank-card).
 * Вы будете взаимодействовать с нашей боевой системой в режиме real-time:
   * выполняя запрос, вы получите наглядное представление о том, какой запрос отправляется, какой ответ возвращается и т.д.;
   * cможете при необходимости изменить запрос.
 * На каждом шаге есть помощь на вкладке "Docs" и необходимые ссылки.
 * Уведомления о смене статуса заказа поступают на [URL для уведомлений](/checkout-api/031-01%20url%20для%20уведомлений.md), но вы их не увидите, т.к. он общий, закрытый, поэтому статус заказа в данном публичном магазине надо запрашивать через [запрос статуса](https://yookassa.ru/developers/api#информация_о_платеже).
 * **Регистрации не требуется**, всё работает из коробки.

---

## Инструкция по установке
- [ ] Скачайте дистрибутив программы [Insomnia](https://insomnia.rest/) и установите (~65 Мб; популярный, бесплатный REST-клиент, работающий на ОС Windows, OSX и Ubuntu);
- [ ] **[Скачайте коллекцию запросов API.ЮKassa](/checkout-api/sample/rest/insomnia/api-test-env.yookassa.for.insomnia.v.1.0_03.12.2020.zip)** (~10 Кб), разархивируйте и [импортируйте](#Импорт-коллекции) в Insomnia;
- [ ] Используйте настройки по-умолчанию или [пропишите](#Настройка) свой shopid и секретный ключ.

#### Импорт коллекции

Импортируйте файл с коллекцией запросов API.ЮKassa в Insomnia.

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
* Файл с коллекцией запросов API.ЮKassa
* Документация ЮKassa
* Гайды API.ЮKassa
:mortar_board: Тестовые окружение для работы с нашим API, это подготовленная нашими специалистами легкий у установке комлекс

-->
