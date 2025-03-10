**`for`** - это единственный цикл доступный в Go.

пример:
```Go
package main

import "fmt"

func main() {

    i := 1
    for i <= 3 {               // аналог while
        fmt.Println(i)
        i = i + 1
    }

    for j := 7; j <= 9; j++ {  // классика (инициализ/усл/выраж)
        fmt.Println(j)
    }

    for {                      // бесконечный
        fmt.Println("loop")
        break       // выход из цикла
    }

    for n := 0; n <= 5; n++ {  // классика
        if n%2 == 0 {
            continue   // переход к сл. итерации
        }
        fmt.Println(n)
    }

	poslanie := []string{"hello", "world"}
	for i, s := range poslanie { // for-each цикл
		fmt.Println(i, s)
	}
	//вывод:
	//0 hello
	//1 world
}
```