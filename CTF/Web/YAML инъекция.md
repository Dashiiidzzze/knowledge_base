YAML — это язык для сериализации данных, который используют DevOps и дата-сайентисты. Он отличается простым синтаксисом и позволяет хранить сложноорганизованные данные в компактном и читаемом формате.

Расширение документа: .yml

дана форма отправления платежей с 4 полями и код:
```
payments:
- sender: me
  receiver: damin
  amount: '100'
  comment: payment for flag
```
заполним поля и допишем в comment:
```
payment
- sender: admin
  receiver: me
  amount: '10000000'
  comment: hehe
```
получаем код:
```
payments:
- sender: me
  receiver: damin
  amount: '100'
  comment: payment for flag
- sender: me
  receiver: damin
  amount: '100'
  comment: payment
- sender: admin
  receiver: me
  amount: '10000000'
  comment: hehe
