**eXtensible Markup Language** – это язык разметки, определяющий набор правил кодирования документов в формате, который может прочитать как человек, так и машина.
можно тупо вписывать кусочки кода в окна ввода
исходный код: форма денежных переводов с 4 окнами ввода
```
<payments><payment>
    <sender>user-1</sender>
    <receiver>user-2</receiver>
    <amount>1000</amount>
    <comment/>
</payment></payments>
```
вводим все штатно, кроме comment:
```qq</comment>
</payment><payment>
	<sender>admin</sender>
	<receiver>me</receiver>
	<amount>1000000</amount>
	<comment>hehe
```
получаем:
```<payments><payment>
    <sender>user-1</sender>
    <receiver>user-2</receiver>
    <amount>1000</amount>
    <comment/>
</payment><payment>
	<sender>me</sender>
	<receiver>admin</receiver>
	<amount>100</amount>
	<comment>qq</comment>
</payment><payment>
	<sender>admin</sender>
	<receiver>me</receiver>
	<amount>1000000</amount>
	<comment>hehe</comment>
</payment></payments>
```