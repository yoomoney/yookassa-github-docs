##Публичные сертификаты продуктов Яндекс.Кассы##

В данном разделе расположены ключницы и сертификаты, используемые Яндекс.Деньги в первую очередь для подписи публичных частей сертификатов для сервисов, участвующих в работе (penelope.yamoney.ru, calypso.yamoney.ru, etc), а также для подписи запросов в различных продуктах компании.  
* [NBCO_YM_Chain.p7b][] - ключница ЦС, которым подписываются все новые сертификаты с 2016 года  
* [NBCO_YOO_Chain.p7b][] - ключница ЦС, которым подписываются все новые сертификаты с декабря 2020 года
* [payouts_responsverify_production.cer][] - сертификат, которым подписываются ответы на запросы контрагента в рамках протокола выплат в боевой [среде][], действует до 12.02.2021  
* [pkcs7_verification.cer][] - сертификат для проверки подписи уведомлений о платежей при подключении по протоколу уведомлений с использованием формата [PKCS#7][], действует до 27 декабря 2020 17:12:15

[NBCO_YM_Chain.p7b]: https://github.com/yandex-money/yandex-money-joinup/blob/master/sertificates/NBCO_YM_Chain.p7b  
[NBCO_YOO_Chain.p7b]: https://github.com/yandex-money/yandex-money-joinup/blob/master/sertificates/NBCO_YOO_Chain.p7b 
[payouts_responsverify_production.cer]: https://github.com/yandex-money/yandex-money-joinup/blob/master/sertificates/payouts_responsverify_production.cer  
[pkcs7_verification.cer]: https://github.com/yandex-money/yandex-money-joinup/blob/master/sertificates/pkcs7_verification.cer
[среде]: https://yookassa.ru/docs/payouts
[PKCS#7]: https://yookassa.ru/docs/payment-solution/notifications/http/basics
