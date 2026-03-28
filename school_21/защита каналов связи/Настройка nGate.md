загрузиться с iso nGate, далее действовать по инструкции
https://cpdn.cryptopro.ru/content/ngate/admin-guide/source/13-setting-examples/task-display-stand-creation.html#task-display-stand-creation__clientless-access-scheme
#### Пояснения к инструкции:
Шаг 4:
	Один интерфейс (ens33) связывается с внешней сетью через сетевой мост. маска и шлюз такие же как на хосте, IP выдумать.
	Второй (ens36) с внутренней защищенной сетью - Здесь нужно создать **новую подсеть**, которой нет в нашей сети. Шлюз не заполнять.
Шаг 6:
	Не нужно активировать root по ssh
Шаг 7:
	программа выйдет в консоль. нужно ввести login: root и заданный ранее пароль. Для создания гаммы ввести в консоли `ng-certcfg`, далее по инструкции:
	https://cpdn.cryptopro.ru/content/ngate/admin-guide/source/04-ngate-installation-consol-setting/pki-infrasrtucture/task-gamma-netcfg-generate.html