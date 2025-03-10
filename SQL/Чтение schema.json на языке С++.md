Для чтения JSON будем использовать библиотеку `nlohmann::json`
```cpp
#include <json.hpp>
using json = nlohmann::json;
```
Статический метод `parse` является одним из способов преобразования необработанной строки JSON в `nlohmann::json` объект.

## Синтаксический анализ документов в формате JSON
При чтении из объектов JSON мы бы передавали имя ключа, к которому хотим получить доступ.
```cpp
std::string Data{
      R"({ "name": "Roderick", "role": "Barbarian" })"};

  json Doc{json::parse(Data)};

  std::string Name{Doc["name"]};
  
  std::cout << "Name: " << Name;
```
При чтении из массивов JSON мы вместо этого передаем числовой индекс, который хотим прочитать
```cpp
std::string Data{R"(["Roderick", "Anna"])"};

  json Doc{json::parse(Data)};

  std::string Player1{Doc[0]};
  
  std::cout << "Player 1: " << Player1;
```

Мы можем связать `[]` оператор для чтения глубоко вложенных значений:
```cpp
std::string Data{R"(
    {
      "name": "Roderick",
      "role": "Barbarian",
      "guild": {
        "name": "The Bandits"
      },
      "equipment": [{
        "name": "Iron Sword",
        "damage": 5
      }]
    }
  )"};

  json Doc{json::parse(Data)};

  std::string GuildName{Doc["guild"]["name"]};
  int WeaponDamage{Doc["equipment"][0]["damage"]};
  
  std::cout << "Guild Name: " << GuildName;
```

## Сопоставление типов JSON с типами C ++
`get` Метод в `nlohmann::json` классе имеет параметр шаблона, который позволяет нам указать точное преобразование
```cpp
std::string Data{R"({
    "greeting": "Hello!",
    "numbers": [1, 2, 3]
  })"};

  json Doc{json::parse(Data)};

  auto Greeting{Doc["greeting"].get<std::string>()};

  auto Numbers{Doc["numbers"].get<std::vector<int>>()};

  std::cout << Greeting << '\n';

  for (int i : Numbers) {
    std::cout << i;
  }
```
Весь документ представляет собой массив JSON, который мы вставляем в `std::vector`:
```cpp
json Doc{json::parse(R"([1, 2, 3])")};

  auto Numbers{Doc.get<std::vector<int>>()};

  for (int i : Numbers) {
    std::cout << i;
  }
```

`get_to` используется, когда переменная уже создана
```cpp
json Doc{json::parse(R"([1, 2, 3])")};

  std::vector<int> Numbers;
  Doc.get_to(Numbers);

  for (int i : Numbers) {
    std::cout << i;
  }
```
