[![юkassa](/i/yookassalogo.png "юkassa")](https://yookassa.ru) / документация / [http](/demo/010%20интеграция%20для%20самописных%20сайтов.md), [cms](/demo/011%20интеграция%20для%20CMS%20и%20SaaS.md), [email](/010%20интеграция%20email.md):arrow_left:, [тестирование](/demo/030%20тестирование.md), [решение ошибок](/demo/031%20решение%20ошибок.md), [демо](/demo/032%20демо%20стенд.md),  [54-ФЗ](/demo/54-fz.md)

:mortar_board: Пример JS-скрипта для схемы E-mail + 54ФЗ
========================================================

> Это пример как сделать платежную форму на E-mail схеме и JS для вставки в customerContact электронной почты или телефона пользователя, который он вводит в вашей форме, чтобы получить электронный чек, если ваш магазин передает данные для 54ФЗ (технические подробности о реализации 54ФЗ [здесь](/demo/54-fz.md)).

### Рабочий пример кода

```html
<form action="https://yoomoney.ru/eshop.xml" method="post" onsubmit="formatReceipt(this);return false;">
	<input required name="shopId" value="123456" type="hidden"/>
	<input required name="scid" value="123456" type="hidden"/>
	<input required name="sum" value="1046.47" type="hidden">
	<input required name="customerNumber" value="test-fz-54-with-dynamic-customerContact" type="hidden"/>
	<input required name="paymentType" value="AC" type="hidden"/>		
	<input required name="customerContact" value="" placeholder="Укажите телефон +7NNNxxxXXxx или электронный адрес для получения чека" size="72"/><br>

    <!-- пропишите здесь свои товары, их количество и стоимость 
    помните, что сумма товаров в ym_merchant_receipt должна быть равна сумме в sum выше в коде -->
<input name="ym_merchant_receipt"
  value='{"customerContact": "",
        "taxSystem": 1,
          "items":[
            {"quantity": 1.154, "price": {"amount": 300.23},  "tax": 3,"text": "Зеленый чай \"Юн Ву\", кг",
                        "paymentMethodType": "full_prepayment",
                        "paymentSubjectType": "commodity"},
            {"quantity": 2,     "price": {"amount": 200.00},  "tax": 3,"text": "Кружка для чая, шт., скидка 10%",
                        "paymentMethodType": "full_prepayment",
                        "paymentSubjectType": "commodity"},
            {"quantity": 0.3,   "price": {"amount": 1000.00}, "tax": 3,"text": "Предоплата 30%, настольная игра \"Tea Time\"",
                        "paymentMethodType": "partial_prepayment",
                        "paymentSubjectType": "commodity"},
            {"quantity": 1,     "price": {"amount": 0.00},    "tax": 1,"text": "Бесплатная доставка",
                        "paymentMethodType": "full_prepayment",
                        "paymentSubjectType": "service"},
            {"quantity": 1,     "price": {"amount": 0.00},    "tax": 1,"text": "Пример одинарной кавычки can\u0027t",
                        "paymentMethodType": "full_prepayment",
                        "paymentSubjectType": "commodity"}
                  ]}'
type="hidden"/>
	<input type="submit" value="test-fz-54-with-dynamic-customerContact">	
</form>

<script>
    var  validateContact = function(value) {
        var emailReg = /^([\w-\.]+@([\w-]+\.)+[\w-]{2,4})?$/,
            phoneReg = /^\+7[0-9]{10,11}/;

        isEmail = value.match(emailReg);
        isPhone = value.match(phoneReg);

        return (isEmail || isPhone);
    }

    var formatReceipt = function (form) {
        
        var customerContactValue = form.customerContact.value,
            receipt = form.ym_merchant_receipt.value,
            receiptObject = JSON.parse(receipt);

        if(validateContact(customerContactValue)) {
            receiptObject.customerContact = customerContactValue;
            form.ym_merchant_receipt.value = JSON.stringify(receiptObject);
            form.submit();
        } else {
            alert('Неверно введен контакт покупателя. Ограничения: только цифры (+792100000000) или адрес электронной почты.')
            return false; 
        }
    };
</script>
```
