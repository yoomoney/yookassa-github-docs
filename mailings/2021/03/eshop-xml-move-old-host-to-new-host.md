<!--
...
-->

Смена хоста money.yandex.ru на yoomoney.ru
==========================================

> В 2020 году, в связи с [ребрендином сервиса](https://yoomoney.ru/page?id=536896), также стало необходимым и обновление хостов, куда вы по платежному протоколу отправляете ваши запросы. Если не обновить хост в коде вашей системы, то платежи перестанут работать.  
> На этой странице представлены пояснения к рассылке.

#### Пример

```
- <form action="https://money.yandex.ru/eshop.xml">
+ <form action="https://yoomoney.ru/eshop.xml">
```

### Документация

* [Документация по старому платежному протоколу](https://yookassa.ru/docs/payment-solution)
* [Подробности о смене бренда](https://new.yookassa.ru/)
* [Гайд по всем техническим изменениям](https://yoomoney.ru/i/html-letters/Tech_Guide_2020_kassa_rus.pdf)


### Что изменится 

1. После смены хоста в `action` вашей платежной формы, платежи продолжат работать как и прежде. Смена хоста - это единственное изменение, которое требуется, чтобы вы внесли в код своей системы, где есть взаимодействие с нами.  
В какой-то момент старый хост `money.yandex.ru` перестанет работать (точных сроков, к сожалению, сказать не можем) и как следствие, перестанут работать платежи. Поэтому наилучшим решением будет - обновить ваши скрипты заранее, прописав в них новый хост `yoomoney.ru`.
3. Если вы интегрировали еще какие-то наши протоколы, в них также надо обновить хост. См. таблицу хостов ниже.

<!-- ### Как протестировать
> вводный текст
> 
1. пункты
#### N.B.
...
-->

### Старые и новые хосты

| Протокол | Старый хост | Новый хост |
| -------- | ----------- | ---------- |
|  |
| [API новый платежный](https://yookassa.ru/developers/using-api/basics) | `payment.yandex.net/api/v3` | `api.yookassa.ru/v3/` |
| [Старый платежный протокол](https://yookassa.ru/docs/payment-solution) | `money.yandex.ru/eshop.xml` | `yoomoney.ru/eshop.xml`
| [MWS](https://yookassa.ru/docs/payment-solution/payment-management/basics) | `penelope.yamoney.ru` | `shop.yookassa.ru` |
|  |
| [Протокол выплат](https://yookassa.ru/docs/payouts) | `calypso.yamoney.ru` | `payouts.yookassa.ru` |
| [Протокол зачислений](https://yoomoney.ru/docs/depositions) | `calypso.yamoney.ru` | `deposit.yoomoney.ru` |
| Протокол идентификации кошелька `*` (`makeIdentificationDeposition`) | `calypso.yamoney.ru` | `deposit.yoomoney.ru` |

#### Текст рассылки

```
#### Обновите ваше платежное ПО для работы с ЮKassa

По вашему магазину платежи идут на старый URL платежной системы ЮKassa.

#### Что нужно сделать?

Платежи должны начать ходить на новый адрес yoomoney.ru, а не на старый money.yandex.ru.

Подробнее о том, как обновиться
https://github.com/yoomoney/yookassa-github-docs/blob/master/mailings/2021/03/eshop-xml-move-old-host-to-new-host.md

#### А если не обновлять?

Если обновление не сделать, то платежи, к сожалению, перестанут работать в ближайшие несколько недель.

#### У меня есть вопрос

Если остались вопросы или что-то не получается, напишите нам — на b2b_support@yoomoney.ru или в ответ на это письмо.
```
