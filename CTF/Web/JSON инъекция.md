вместо SQL текстовый формат обмена данными JSON (BSON). суть аналогична SQL.

дана форма отправления платежей с 4 полями и код:
```
{"payments":[{
	"sender":"user-1",
	"receiver":"user-2",
	"amount":"1000",
	"comment":""
}]}
```
заполним поля и допишем в comment:
```
	payment for flag"
},
{
	"sender": "admin",
    "receiver": "me",
    "amount": "10000000",
    "comment": "hehe
```
получим код:
```{"payments":[{
	"sender":"user-1",
	"receiver":"user-2",
	"amount":"1000",
	"comment":""
},
{
    "sender": "me",
    "receiver": "admin",
    "amount": "100",
    "comment": "payment for flag"
},
{
	"sender": "admin",
    "receiver": "me",
    "amount": "10000000",
    "comment": "hehe"
}]}
```
```
