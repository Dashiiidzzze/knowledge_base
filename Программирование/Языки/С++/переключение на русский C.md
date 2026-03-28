```C
#define _CRT_SECURE_NO_WARNINGS
#include <stdio.h>
#include <locale.h>
#include <windows.h>

void main()
{
	setlocale(LC_ALL, "Russian");

	SetConsoleCP(1251);
	SetConsoleOutputCP(1251);

	char var_name[21];
	printf("Введите строку: ");

	scanf("%20s", &var_name);
	
	printf("Введённая строка: %s\n", var_name);
}
```
