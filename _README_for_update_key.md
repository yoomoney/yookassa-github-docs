Версия документа от 25.01.2021

> :mortar_board: Внимание! Если вы читаете этот документ после получения рассылки о необходимости обновления ключа, и вам, как техническому специалисту не достаточно информации для понимания "какой ключ надо обновить", то перейдите к ["Шагу 3"](#шаг-3-дешифровка-ответа) ниже. Если требуется дополнительная информация, ответьте на полученную рассылку или напишите нам на адрес b2b_support@yoomoney.ru.

> **Если НЕ выполнить обновление ключа, то перестанут работать ваши выплаты, зачисление на кошелек или процесс идентификации.**

## Как обновить ключ?

1. Найти в вашей системе где размещен старый ключ. Вот копия этого файла https://yadi.sk/d/crccbNzRy933MQ, она поможет вам найти этот файл в вашей системе.
2. Взять файл new_key_cer_YooMoney_deposit_response_verify_with_all_chain по ссылке https://yadi.sk/d/MOfnA1IlB1F2Yg и во вторник, 09.02.2021 в 12:00 (время московское) замените в своей системе старый ключ на новый.
3. Также мы рекомендуем в первом полугодии 2021 выполнить [обновление хоста](#старый-и-новый-хосты) для передачи запросов по используемому у вас протоколу.

#### [Содержимое директории](https://yadi.sk/d/MOfnA1IlB1F2Yg):

1. old_key_cer_payouts_response_verify_until_12_02_21 - текущий ключ, которым вы проверяете нашу подпись;
2. new_key_cer_YooMoney_deposit_response_verify_with_all_chain - новый ключ, которым надо заменить старый ключ в вашей системе, 9 февраля 2021 года в 12:00;
3. new_key_p7b_YooMoney_deposit_response_verify_with_all_chain - этот же ключ, что в п.2, но в формате p7b (некоторым системам нужен ключ не в формате cer, а в формате p7b);
4. yoomoney-all-chain - все наши цепочки сертификации (на случай, если у вас в системе они требуются); эти цепочки добавлены в файл new_key_cer_YooMoney_deposit_response_verify_with_all_chain.

### Старый и новый хосты

> Напоминаем, что до декабря 2020 использовался хост calypso.yamoney.ru и этот хост будет работать весь 2021 год, но мы рекомендуем перейти на новый хост согласно используемому у вас протоколу.

| Протокол | Старый хост | Новый хост |
| -------- | ----------- | ---------- |
| [Протокол выплат](https://yookassa.ru/docs/payouts) | calypso.yamoney.ru | payouts.yookassa.ru |
| [Протокол зачислений](https://yoomoney.ru/docs/depositions) | calypso.yamoney.ru | deposit.yoomoney.ru |
| Протокол идентификации кошелька `*` (`makeIdentificationDeposition`) | calypso.yamoney.ru | deposit.yoomoney.ru |

#### Документация

* [Протокол выплат](https://yookassa.ru/docs/payouts). Для контрагентов, кто выполняет зачисления на кошельки, карты, р/с или мобильные телефоны.
* [Протокол зачислений](https://yoomoney.ru/docs/depositions). Для контрагентов банков-партнёров.
* `*` Протокол идентификации кошелька направляется контрагентам по запросу (в открытом доступе документации нет).

### Requirements

Для интеграции протокола выплат, зачислений или идентификации, нужно:

1. Зарегистрироваться в ЮKassa как юридическое лицо (или ИП).
2. Сгенерировать CSR запрос по инструкции https://yookassa.ru/docs/payouts/api/using-api/security#creating-csr (см. Получение SSL-сертификата для взаимодействия с серверами Яндекс.Денег) шаги 1-5.
3. Сообщить IP, с которых вы будете выполнять запросы в нашу систему (это должны быть статические IPv4, не более 5 IP).
4. Получить идентификатор для выплатного шлюза (AgentID).

Если у вас нет все этого, то мы надеемся, что прочтение примеров объяснит вам как работают эти протоколы.

## Шаги 

###  Используемые в примерах файлы

* req.xml - тело запроса makeDeposition до упаковки в PKCS7 (см. пример ниже);
* req_signed.txt - упакованный в PKCS7 запрос req.xml;
* you.cer - сертификат, который выдан контрагенту (см. п.2 выше);
* private.key - ваш файл секретного ключа, на основе которого вы сделали CSR-запрос, и мы (ЮKassa) на основе него сделали you.cer;
* deposit.cer - ключ для дешифровки из PKCS7 формата ответов от ЮKassa на запросы makeDeposition или makeIdentificationDeposition;

### Модификация параметров в curl и openssl 

1. -passin
Использование пароля в private.key. Модификация `-passin pass:HIDDEN` в запросах: если вы создавали в п.2 (см. выше) приватный ключ (private.key) с паролем, то вместо HIDDEN напишите свой пароль, например, `-passin pass:coolpassword`; если без пароля, то передавайте `-passin pass:`, без HIDDEN.
2. req_signed.txt
В lunix|unix указываем файл так: "-F file=@req_signed.txt". В Windows "--data-ascii @req_signed.txt".

#### N.B.

* Ниже представлены примеры запросов makeDeposition или makeIdentificationDeposition с помощью утилиты curl и дешифровка полученного ответа c помощью утилиты openssl.
* Раскрытые в документе примеры не заменяют документацию. Они необходимы для случаев, когда программисту контрагента необходимо быстро вспомнить как организован процесс протокольного взамодействия с ЮKassa. Конечно вы можете работать без утилиты curl и openssl, используя свои инструменты, в данных примерах они используются как наиболее простые для понимания схемы работы.

### Шаг 1. Упаковка запроса

Сформируйте содержимое запроса и упакуйте его в PKCS7
```
openssl smime -sign -in req.xml -nointern -nodetach -nocerts -nochain -outform PEM -out req_signed.txt -signer you.cer -inkey private.key -passin pass:HIDDEN
```

### Шаг 2. Отправка запроса

Отправьте запрос makeDeposition или makeIdentificationDeposition на сервер payouts.yookassa.ru:
```
curl -X POST --insecure -F file=@req_signed.txt --header "Content-type:application/pkcs7-mime" --cert you.cer --key private.key --pass HIDDEN --url https://payouts.yookassa.ru:9094/webservice/deposition/api/makeDeposition
```
 
#### Пример тела запроса для makeDeposition (файл req.xml)
```
<?xml version="1.0" encoding="UTF-8"?>
<makeDepositionRequest agentId="200807" dstAccount="257003392579" requestDT="2020-11-26T12:09:36.000Z" amount="200.00" clientOrderId="7093BCF45E8046ECB675" currency="10643" contract="Договор средств N 123 от 22.08.2020">
        <paymentParams>
                <skr_destinationCardSynonim>dN3VdS7WchWKmWIUfPmcMUUQpikZ.SC.201511</skr_destinationCardSynonim>
                <pdr_birthDate>01.01.2000</pdr_birthDate>
                <pdr_birthPlace>поселок Забайкальского края</pdr_birthPlace>
                <pdr_lastName>Петров</pdr_lastName>
                <pdr_firstName>Андрей</pdr_firstName>
                <pdr_middleName>Александрович</pdr_middleName>
                <pdr_docNumber>123123123</pdr_docNumber>
                <pdr_docIssueYear>2015</pdr_docIssueYear>
                <pdr_docIssueMonth>8</pdr_docIssueMonth>
                <pdr_docIssueDay>12</pdr_docIssueDay>
                <pdr_docIssuedBy>УВД РОВД, город Саратов</pdr_docIssuedBy>
                <pdr_city>Новосибирск</pdr_city>
                <pdr_postcode>630092</pdr_postcode>
                <pdr_address>Новосибирская обл,  Новосибирск,  ул. Ольги Ивановны,  дом N 4,  кв. 5</pdr_address>
                <smsPhoneNumber>79529234933</smsPhoneNumber>
                <pof_offerAccepted>1</pof_offerAccepted>
                <pdr_country>643</pdr_country>
        </paymentParams>
</makeDepositionRequest>
```

#### Возможные ошибки

- Используйте в качестве номера знак N (не используйте спецсимвол №).
- `<paymentParams></paymentParams>`: не забывайте, что все дополнительные параметры (`pdr_birthDate`, `pof_offerAccepted` и т.д.) должны быть внутри paymentParams.

### Шаг 3. Дешифровка ответа

Если вы пришли к этому пункту после рассылки про обновление ключа 09.02.2021, то вам потребуется данный "Шаг 3" и возможно "Шаг 1" и "Шаг 2".

Задача "Шага 3" - это дешифровка полученного ответа от payouts.yookassa.ru на ваш запрос makeDeposition или makeIdentificationDeposition (см. "Шаг 2" выше) и проверка подписи ответа. Напоминаем, что до декабря 2020 использовался хост calypso.yamoney.ru и этот хост будет работать весь 2021 год, но мы рекомендуем перейти на новый хост -- payouts.yookassa.ru.

#### Пример проверки подписи

В документации об этом шаге: https://yookassa.ru/docs/payouts/api/using-api/format#response. Не смотря на то, что в документации процесс указан как "проверка подписи", по факту ЮKassa отдает ответ в PKCS7 формате (base64) и чтобы его прочесть вашей системе, требуется сконвертировать ответ из base64 и проверить подпись:

```
$ openssl smime -verify -noverify -inform PEM -nointern -certfile "deposit.cer" -CAfile "deposit.cer" < resp.txt
	<?xml version="1.0" encoding="UTF-8"?>
	<makeDepositionResponse clientOrderId="12345" status="0" error="30" processedDT="2021-01-21T13:23:07.197+03:00" balance="1000.00" />
	Verification successful
```

В ответе "Verification successful" как раз указывает, что подпись корректна.

Если ключ для дешифровки не валидный, или он пришел не от нас и подписан поддельным ключем. При проверке вы получите что-то вида:
```
$ openssl smime -verify -noverify -inform PEM -nointern -certfile "deposit-incorrect.cer" -CAfile "deposit-incorrect.cer" < resp.txt
	Verification failure
	139971507589568:error:2107C080:PKCS7 routines:PKCS7_get0_signers:signer certificate not found:../crypto/pkcs7/pk7_smime.c:421:
```

#### Пример отправки запроса makeDeposition и проверки подписи

1. Отправьте запрос на сервер ("Шаг 2", см. выше) и сохраните полученный ответ в файл resp.txt
`curl -X POST --insecure -F file=@req_signed.txt --header "Content-type:application/pkcs7-mime" --cert you.cer --key private.key --pass HIDDEN --url https://payouts.yookassa.ru:9094/webservice/deposition/api/makeDeposition > resp.txt`

Обратите внимание, что в вашем случае содержимое запроса makeDeposition или makeIdentificationDeposition может быть другим, но это не отмяет того, что ответ ЮKassa на запрос makeDeposition или makeIdentificationDeposition будет в base64 и его необходимо дешифровать.
  
2. Выполните дешифровку полученного ответа (в данном случае из файла resp.txt)
`openssl smime -verify -noverify -inform PEM -nointern -certfile "deposit.cer" -CAfile "deposit.cer" < resp.txt`

Как видно, в дешифровке используется deposit.cer. Это файл открытого ключа, с помощью которого нужно выполнить дефишровку ответа от ЮKassa. Актуальный ключ можно взять здесь https://yadi.sk/d/MOfnA1IlB1F2Yg.

P.S. Обратите внимание, что 09.02.2021 в 09:00 МСК ЮKassa выполнит обновление этого ключа и будет действовать new_key_cer_YooMoney_deposit_response_verify_with_all_chain.cer (см. ссылку выше); до этого времени расшифровка ответа должна выполняться с помощью ключа old_key_cer_payouts_responsverify_production_until_12_02_21 (см. https://yadi.sk/d/MOfnA1IlB1F2Yg). 

#### Содержимое ответа из "Шаг 3, п.1" (см файл resp.txt; напоминаем, это пример)
```
-----BEGIN PKCS7-----
MIAGCSqGSIb3DQEHAqCAMIACAQExCzAJBgUrDgMCGgUAMIAGCSqGSIb3DQEHAaCA
JIAEgcU8ZXJyb3JEZXBvc2l0aW9uTm90aWZpY2F0aW9uUmVxdWVzdCBjbGllbnRP
cmRlcklkPSIyNTNhMzk1Ny0xNjA5LTQ3ODQtYjRlZC1iYjRhNmY2ODgzYzciIHJl
cXVlc3REVD0iMjAxNi0wNC0wNFQxMjowMzowMi43ODlaIiBkc3RBY2NvdW50PSIy
NTcwMDMzOTI1NzkiIGFtb3VudD0iMTM2LjYyIiBjdXJyZW5jeT0iMTA2NDMiIGVy
cm9yPSIzMSIvPgAAAAAAADGCAjcwggIzAgEBMIGEMHwxCzAJBgNVBAYTAlJVMQ8w
DQYDVQQIEwZSdXNzaWExGTAXBgNVBAcTEFNhaW50LVBldGVyc2J1cmcxGDAWBgNV
BAoTD1BTIFlhbmRleC5Nb25leTEQMA4GA1UECxMHVW5rbm93bjEVMBMGA1UEAxMM
WWFuZGV4Lk1vbmV5AgRND18oMAkGBSsOAwIaBQCggYgwGAYJKoZIhvcNAQkDMQsG
CSqGSIb3DQEHATAcBgkqhkiG9w0BCQUxDxcNMTYwNDA0MTIwMzAyWjAjBgkqhkiG
9w0BCQQxFgQUBeVNe6OQrdPV0tgnIMRchvOeMSUwKQYJKoZIhvcNAQk0MRwwGjAJ
BgUrDgMCGgUAoQ0GCSqGSIb3DQEBAQUAMA0GCSqGSIb3DQEBAQUABIIBAJnDA4FJ
w9xd3aH+Q5Zvubrbaf9jci89coST+ySqXbyCI/htgEqHXlapsu256wF+bzTQJM/X
QXj14aP/X7v8JIE937GbldpeyUlWIhbyaw1odL8pnw60nNd9hG11Rav3OmF2l4Xe
jH9VelknKY/IXFWPUK25NkTSHtR28Ze9C1NIE87hmsgq2M6QOnoWITzYL2BM6KtG
1PZKDB0HpqRDw89SjdSeMI+dNet8VSyvmLtR/i8Dr7EWN7+G1tqCi7V4UveWlTKt
O7MZyy3xg64XeaDqG+eD6/3D9GN8Q7J3KDNK9SdjqbmQY6qFCNq1BGEb0zHOs7Ae
9iRii8sV+d9dLywAAAAAAAA=
-----END PKCS7-----
```
Есть вопросы? Пишите на b2b_support@yoomoney.ru

<!--
===============================================================================================================

### [errorDepositionNotificationRequest](https://yookassa.ru/docs/payouts/api/error-deposition-notification)

Функциональность дешифровки ответа errorDepositionNotificationRequest (ошибки при зачислениях на карты) не относится к "Шагу 3" (см. выше)

```curl -X POST --insecure --data-ascii @req_signed.txt --header "Content-type:application/pkcs7-mime" --url http://app.merchant-site.ru/errorDepositionNotificationRequest
resp.txt
-----BEGIN PKCS7-----
MIAGCSqGSIb3DQEHAqCAMIACAQExCzAJBgUrDgMCGgUAMIAGCSqGSIb3DQEHAaCA
JIAEgcU8ZXJyb3JEZXBvc2l0aW9uTm90aWZpY2F0aW9uUmVxdWVzdCBjbGllbnRP
cmRlcklkPSIyNTNhMzk1Ny0xNjA5LTQ3ODQtYjRlZC1iYjRhNmY2ODgzYzciIHJl
cXVlc3REVD0iMjAxNi0wNC0wNFQxMjowMzowMi43ODlaIiBkc3RBY2NvdW50PSIy
NTcwMDMzOTI1NzkiIGFtb3VudD0iMTM2LjYyIiBjdXJyZW5jeT0iMTA2NDMiIGVy
cm9yPSIzMSIvPgAAAAAAADGCAjcwggIzAgEBMIGEMHwxCzAJBgNVBAYTAlJVMQ8w
DQYDVQQIEwZSdXNzaWExGTAXBgNVBAcTEFNhaW50LVBldGVyc2J1cmcxGDAWBgNV
BAoTD1BTIFlhbmRleC5Nb25leTEQMA4GA1UECxMHVW5rbm93bjEVMBMGA1UEAxMM
WWFuZGV4Lk1vbmV5AgRND18oMAkGBSsOAwIaBQCggYgwGAYJKoZIhvcNAQkDMQsG
CSqGSIb3DQEHATAcBgkqhkiG9w0BCQUxDxcNMTYwNDA0MTIwMzAyWjAjBgkqhkiG
9w0BCQQxFgQUBeVNe6OQrdPV0tgnIMRchvOeMSUwKQYJKoZIhvcNAQk0MRwwGjAJ
BgUrDgMCGgUAoQ0GCSqGSIb3DQEBAQUAMA0GCSqGSIb3DQEBAQUABIIBAJnDA4FJ
w9xd3aH+Q5Zvubrbaf9jci89coST+ySqXbyCI/htgEqHXlapsu256wF+bzTQJM/X
QXj14aP/X7v8JIE937GbldpeyUlWIhbyaw1odL8pnw60nNd9hG11Rav3OmF2l4Xe
jH9VelknKY/IXFWPUK25NkTSHtR28Ze9C1NIE87hmsgq2M6QOnoWITzYL2BM6KtG
1PZKDB0HpqRDw89SjdSeMI+dNet8VSyvmLtR/i8Dr7EWN7+G1tqCi7V4UveWlTKt
O7MZyy3xg64XeaDqG+eD6/3D9GN8Q7J3KDNK9SdjqbmQY6qFCNq1BGEb0zHOs7Ae
9iRii8sV+d9dLywAAAAAAAA=
-----END PKCS7-----```

#### Дешифровка ответа
```curl -X POST --insecure --data-ascii @req_signed.txt --header "Content-type:application/pkcs7-mime" --url https://app/ya/errorDepositionNotificationRequest<!DOCTYPE HTML PUBLIC "-//IETF//DTD HTML 2.0//EN">
<html><head>
<title>403 Forbidden</title>
</head><body>
<h1>Forbidden</h1>
<p>You don't have permission to access /errorDepositionNotificationRequest
on this server.</p>
</body></html>```

==========

### Старый пример в одну строку

```A=$(echo "<makeDepositionRequest agentId=\"0000\" clientOrderId=\"123\" requestDT=\"`date +%Y-%m-%dT%H:%M:%S`\" dstAccount=\"257003392579\" amount=\"110.00\" currency=\"10643\" contract=\"test_test\"><paymentParams><skr_destinationCardSynonim>7yJteyIZuXB4B6mOMurvxyAjUKAZ.SC.201710</skr_destinationCardSynonim><pdr_firstName>Гнилица</pdr_firstName><pdr_middleName>Иван</pdr_middleName><pdr_lastName>Иванов</pdr_lastName><pdr_docNumber>1111111111</pdr_docNumber><pdr_postcode>020202</pdr_postcode><pdr_country>643</pdr_country><pdr_city>Город</pdr_city><pdr_address>улица улица1</pdr_address><pdr_birthDate>01.01.1990</pdr_birthDate><pdr_birthPlace>Город</pdr_birthPlace><pdr_docIssueYear>2008</pdr_docIssueYear><pdr_docIssueMonth>01</pdr_docIssueMonth><pdr_docIssueDay>10</pdr_docIssueDay><pdr_docIssuedBy>ОВД г. улица</pdr_docIssuedBy><pof_offerAccepted>1</pof_offerAccepted><smsPhoneNumber>7999999999</smsPhoneNumber></paymentParams></makeDepositionRequest>" | openssl smime -sign -signer /var/www/www-root/data/www/apikassa.tk/KeysForApi/gnilitsa_gates.cer -inkey /var/www/www-root/data/www/apikassa.tk/KeysForApi/private.key -nochain -nocerts -outform PEM -nodetach -passin pass:*****) && curl -v -H "Content-Type: application/pkcs7-mime" -k --cert /var/www/www-root/data/www/apikassa.tk/KeysForApi/gnilitsa_gates.cer --key /var/www/www-root/data/www/apikassa.tk/KeysForApi/private.key --pass ***** --request POST "https://payouts.yookassa.ru:9094/webservice/deposition/api/makeDeposition" --data "$A"
```
-->
