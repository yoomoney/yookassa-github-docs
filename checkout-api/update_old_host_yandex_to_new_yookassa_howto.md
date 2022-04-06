[![ЮKassa](/i/yookassalogo.png "ЮKassa")](https://yookassa.ru) / [помощь](https://yookassa.ru/docs/support) / [api-докуметация](https://yookassa.ru/developers), [Checkout.js](https://yookassa.ru/developers/payment-forms/other/yc-js), [SDK](https://yookassa.ru/developers/using-api/using-sdks)

# Переход со старого payment.yandex.net на новый api.yookassa.ru

> Эта страница предназначена для тех контрагентов, кто принимает платежи с помощью **[платежного протокола API ЮKassa](https://yookassa.ru/developers/using-api/interaction-format)** и в качестве адреса для отправки запросов (API endpoint) использует старый хост `payment.yandex.net`.  
> **Обязательно покажите это письмо вашему техническому специалисту или программисту**, чтобы чтобы он выполнил необходимое обновление на вашей сторонее.

### Подробности

До конца апреля 2022, в случае, если вы (или ваши технические специалисты, разработчики) не замените старый хост в коде вашей системы на новый, платежи, выполняемые с помощью [платежного протокола API ЮKassa](https://yookassa.ru/developers/using-api/interaction-format), перестанут работать.

### Кратко о том, что необходимо сделать
```
- old API endpoint: https://payment.yandex.net/api/v3/
+ new API endpoint: https://api.yookassa.ru/v3/
```

# Ответы на вопросы

## SDK

Если для взаимодействия вы используете устаревшую библиотеку SDK, 
