[![ЮKassa](/i/yookassalogo.png)](https://yookassa.ru/en/), [help](https://yookassa.ru/docs/support?lang=en), [api-documentation](https://yookassa.ru/en/developers), [Checkout.js](https://yookassa.ru/en/developers/payment-acceptance/integration-scenarios/checkout-js/basics), [SDK](https://yookassa.ru/en/developers/using-api/using-sdks)

# Replace host payment.yandex.net with api.yookassa.ru

> This page is intended for those counterparties who accept payments using **[payment protocol API YooMoney](https://yookassa.ru/en/developers/using-api/interaction-format)** and use the old host `payment.yandex.net` as the address for sending requests (API endpoint).  
> **Be sure to show this information to your technician or programmer** so that the necessary update can be performed on your side.

 ### Details
 
 Until the end of April 2022, if you (or your technical experts, developers) do not replace the old host in the code of your system with the new one, the payments made with [payment protocol API YooMoney](https://yookassa.ru/en/developers/using-api/interaction-format) will stop working.
 
### Briefly about what needs to be done
```
# API endpoint
- old: https://payment.yandex.net/api/v3/
+ new: https://api.yookassa.ru/v3/

# payment_method_data
- "yandex_money"
+ "yoo_money"

# cancellation_details
- "party" : "yandex_checkout"
+ "party" : "yoo_money"
```

# Answers to Questions

>> This information is for your software developers only, who will perform the update.

## Endpoint API replacement

If you are not using the [SDK](#sdk) (see below) and your site code is written by your developer. In the code of your software replace:

```
- old API endpoint: https://payment.yandex.net/api/v3/
+ new API endpoint: https://api.yookassa.ru/v3/
```

## Replacing payment_method_data yandex_money

When switching from payment.yandex.net host to api.yookassa.ru, the name of the payment method `"yandex_money"` changes to `"yoo_money"`. If you don't use this payment method, you can skip this block of information.

<details><summary>OLD POST payment.yandex.ru</summary>
    
```CSS
### yandex_money
POST https://payment.yandex.ru/v3/payments
authorization: Basic {{token}}
idempotence-key: {{$guid}}
content-type: application/json

{
    "amount": {
        "value": "10.00",
        "currency": "RUB"
    },
    "payment_method_data": {
        "type": "yandex_money"
    },
    "confirmation": {
        "type": "redirect",
        "return_url": "https://url.to.you_return_page"
    }
}
```
</details>

<details><summary>NEW POST api.yookassa.ru</summary>
 
> Upgrade to transfer requests to the new host, pass us `yoo_money` if payment by [e-wallet YooMoney](https://yookassa.ru/en/developers/payment-acceptance/integration-scenarios/manual-integration/yoo-money) is required.

 ```CSS
### yoo_money
POST https://api.yookassa.ru/v3/payments
authorization: Basic {{token}}
idempotence-key: {{$guid}}
content-type: application/json

{
    "amount": {
        "value": "10.00",
        "currency": "RUB"
    },
    "payment_method_data": {
        "type": "yoo_money"
    },
    "confirmation": {
        "type": "redirect",
        "return_url": "https://url.to.you_return_page"
    }
}
```
</details>

   * In payment notifications by the `yoo_money` method, as well as in our response to your GET request, in the payment object, you will receive `yoo_money`.
   * Important point: if you created payment `"yandex_money"` with old URL (https://payment.yandex.net/api/v3/), notifications on this payment will be unchanged (payment method will be `"yandex_money"`). But if you make a request to a new host (`GET https://api.yookassa.ru/v3/payments/{{id}}`), you will get a method with a new name - `"yoo_money"`.
   * If you send a request for creating a payment by the payment method `"yandex_money"` (the old name) to a new host https://api.yookassa.ru/api/v3/, you will get an error that there is no such method. That's why you should send a request to the new host only with the new method name -- `"yoo_money"`.

## Replacing cancellation_details party yoo_money

[In Reasons for error or payment cancellation](https://yookassa.ru/en/developers/payment-acceptance/after-the-payment/declined-payments#cancellation-details-party) in the payment notification or response to a GET request, the following change:

```
- "party" : "yandex_checkout"
+ "party" : "yoo_money"
```
---

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

### I've already upgraded

**Q:** I already updated my software and|or the CMS module, why are you writing to me?

**A:** The sample was taken from the logs for March 2022. If you've already updated, that's good. If you're not sure, email us and we'll see where your API requests are currently going.

### Can't update

**Q:** What if I don't have time to update?

**A:** Unfortunately, payments will stop working. We are doing our best to make all old hosts (which belong to Yandex) work as long as possible. **Try to upgrade by the end of April 2022**.

### Why do we have to do all this in the first place?

**Q:** Why do we have to do all this in the first place?

**A:** *Yandex.Money payment service, which is part of Sberbank's ecosystem, changed its legal entity name - Yandex.Money LLC was renamed to YooMoney NBCO LLC in the Unified State Register of Legal Entities. This was another step in the final rebranding.*

*On the 17th of September 2020 the company announced the renaming of the service into YooMoney, the payment solution for business has changed its name to YooKassa. Until December 15 2020 the service will use the old and new brands, after this date the service will use only the new names - YooMoney and YooKassa ...*.

* [Tech_Guide_2020_kassa_eng.pdf](https://yoomoney.ru/i/html-letters/Tech_Guide_2020_kassa_eng.pdf)
* Official News "[YooMoney payment service completes renaming](https://yoomoney.ru/page?id=536896&lang=en)" (November 12, 2020)

## Mailing text

```
Subject: Change the host of payment.yandex.net in your shopid=100500 settings
Body:
@Colleagues,

According to your shopid=100500 we see that for payments (sending requests via YooMoney payment protocol 
from your software) you use the old host payment.yandex.net (address belongs to Yandex).

You need to start using the new host api.yookassa.ru (belongs to YooMoney|Yookassa), otherwise payments 
can stop working until the end of April 2022.

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

## I have a question

If you still have questions or if something doesn't work, email us - at b2b_support@yoomoney.ru or in response to our email.
