[![ЮKassa](/i/yookassalogo.png)](https://yookassa.ru/en/) | [help](https://yookassa.ru/docs/support?lang=en) | [api-documentation](https://yookassa.ru/en/developers), [Checkout.js](https://yookassa.ru/en/developers/payment-acceptance/integration-scenarios/checkout-js/basics) | [SDK](https://yookassa.ru/en/developers/using-api/using-sdks)

# Replace host payment.yandex.net with api.yookassa.ru

> This page is intended for those counterparties who accept payments using **[payment protocol API YooMoney](https://yookassa.ru/en/developers/using-api/interaction-format)** and use the old host `payment.yandex.net` as the address for sending requests (API endpoint).  
> **Be sure to show this information to your technician or programmer** so that the necessary update can be performed on your side.

 ### Details
 
 Until the end of April 2022, if you (or your technical experts, developers) do not replace the old host in the code of your system with the new one, the payments made with [payment protocol API YooMoney](https://yookassa.ru/en/developers/using-api/interaction-format) will stop working.
 
### Briefly about what needs to be done
```
- old API endpoint: https://payment.yandex.net/api/v3/
+ new API endpoint: https://api.yookassa.ru/v3/
```

# Answers to Questions

>> This information is for your software developers only, who will perform the update.

## SDK

If you are using an outdated [SDK](https://yookassa.ru/en/developers/using-api/using-sdks) library for communication, upgrade. New SDK libraries do not use the old address, they use the new address. 

### SDK update 

In general, the procedure for updating the [SDK](https://yookassa.ru/en/developers/using-api/using-sdks) library is:

1. **Save the current shopid and secret key value** from the SDK configuration file. In case you can't find the old values, you can reissue the secret key in your personal account in the *Integration -- API keys* section.
2. Delete old SDK files (previously make an archive of old files).
3. Replace the old SDK files with new ones.
4. Write the shopid and secret key in the SDK configuration file.

## Comparison of old and new hosts

>> This table will come in handy for your programmer if you use more than one protocol when working with YooMoney.  
>> For each protocol, only the host is updated. And if you update it (see the table for the new value), prescribing the new host in the code of your software, it is a guarantee that when the old host stops working, you in terms of payments, refunds, payments, etc., everything will continue to work as normal.

| Protocol | Old end point | New end point |
| -------- | ----------- | ---------- |
|  |
| [API new payment protocol](https://yookassa.ru/en/developers/using-api/interaction-format) | `payment.yandex.net/api/v3` | `api.yookassa.ru/v3/` |
| [Old payment protocol](https://yookassa.ru/docs/payment-solution/payment-process/basics#merchant-scenario-http) | `money.yandex.ru/eshop.xml` | `yoomoney.ru/eshop.xml`
| [MWS addition to the old payment protocol](https://yookassa.ru/docs/payment-solution/payment-management/basics) | `penelope.yamoney.ru` | `shop.yookassa.ru` |
|  |
| [Payout protocol](https://yookassa.ru/docs/payouts) | `calypso.yamoney.ru` | `payouts.yookassa.ru` |
| [Enrollment protocol](https://yoomoney.ru/docs/depositions) | `calypso.yamoney.ru` | `deposit.yoomoney.ru` |
| Wallet identification protocol `*` (`makeIdentificationDeposition`) | `calypso.yamoney.ru` | `deposit.yoomoney.ru` |
|Host2host PCI DSS protocol | `paymentcard.yamoney.ru` | `paymentcard.yoomoney.ru` | 

---

## Other issues

### // I've already upgraded

**Q:** I already updated my software and|or the CMS module, why are you writing to me?

**A:** The sample was taken from the logs for March 2022. If you've already updated, that's good. If you're not sure, email us and we'll see where your API requests are currently going.

### // Can't update

**Q:** What if I don't have time to update?

**A:** Unfortunately, payments will stop working. We are doing our best to make all old hosts (which belong to Yandex) work as long as possible. **Try to upgrade by the end of April 2022**.

### // Why do we have to do all this in the first place?

**Q:** Why do we have to do all this in the first place?

**A:** *Yandex.Money payment service, which is part of Sberbank's ecosystem, changed its legal entity name - Yandex.Money LLC was renamed to YooMoney NBCO LLC in the Unified State Register of Legal Entities. This was another step in the final rebranding.*

*On the 17th of September 2020 the company announced the renaming of the service into YooMoney, the payment solution for business has changed its name to YooKassa. Until December 15 2020 the service will use the old and new brands, after this date the service will use only the new names - YooMoney and YooKassa ...*.

* [Tech_Guide_2020_kassa_eng.pdf](https://yoomoney.ru/i/html-letters/Tech_Guide_2020_kassa_eng.pdf)
* Official News "[YooMoney payment service completes renaming](https://yoomoney.ru/page?id=536896&lang=en)" (November 12, 2020)

### Mailing text

```
Subject: Change the host of payment.yandex.net in your shopid=100500 settings
Body:
@Colleagues,

According to your shopid=100500 we see that for payments (sending requests via YooMoney|YooKassa payment protocol from your software) you use the old host payment.yandex.net (address belongs to Yandex).

You need to start using the new host api.yookassa.ru (belongs to YooMoney|Yookassa), otherwise payments can stop working until the end of April 2022.

Show this letter to your technician or programmer so he/she can perform the necessary update on your side.

### What you need to do

(the developer will understand these two lines below)

- old API endpoint: https://payment.yandex.net/api/v3/
+ new API endpoint: https://api.yookassa.ru/v3/

### Details

https://clck.ru/enrpY (eng)
https://clck.ru/emJBU (ru)

(we've compiled all the answers you need for this case at this link; the page is updated as new questions come in) 

If you don't have enough information, please reply to this email.
```
