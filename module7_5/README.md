## Задача 1. Установите golang.
Выполнено 

## Задача 2. Знакомство с gotour.
Выполнено

## Задача 3. Написание кода. 
1. Напишите программу для перевода метров в футы (1 фут = 0.3048 метр). Можно запросить исходные данные 
у пользователя, а можно статически задать в коде.
```
package main
import "fmt"
func main() {
	fmt.Print("Enter meters: ")
	var input float64
	var output float64
	fmt.Scanf("%f", &input)
	output = input / 0.3048
	fmt.Println("In feet are: ", output)
}
``` 
Результат:
```
PS C:\Users\user\projects\netology\go\task_1> go run .
Enter meters: 2
In feet are:  6.561679790026246
PS C:\Users\user\projects\netology\go\task_1> 
```
2. Напишите программу, которая найдет наименьший элемент в любом заданном списке, например:
    ```
    x := []int{48,96,86,68,57,82,63,70,37,34,83,27,19,97,9,17,}
    ```
Код:
```
package main
import (
	"fmt"
	"math"
)
func main() {
	x := []int{48, 96, 86, 68, 57, 82, 63, 70, 37, 34, 83, 27, 19, 97, 9, 17}
	min := math.MaxInt
	fmt.Print("Array of integers given: ", "\n")
	for i := 0; i < (len(x)); i++ {
		fmt.Print(x[i], " ")
		if x[i] < min {
			min = x[i]
		}
	}
	fmt.Print("\n", "The minimum value in array is: ", min)
}
```
результат:
~~~
Array of integers given: 
48 96 86 68 57 82 63 70 37 34 83 27 19 97 9 17 
The minimum value in array is: 9
PS C:\Users\user\projects\netology\go\task_2>
~~~
3. Напишите программу, которая выводит числа от 1 до 100, которые делятся на 3. То есть `(3, 6, 9, …)`.
Код:
~~~
package main

import (
	"fmt"
)

func main() {
	fmt.Print("Numbers from range 1-100 devided by 3 gives 0 reminder:", "\n")
	for i := 1; i <= 100; i++ {
		if i%3 == 0 {
			fmt.Print(i, " ")
		}
	}
}

~~~
результат:
~~~
PS C:\Users\user\projects\netology\go\task_3> go run .
Numbers from range 1-100 devided by 3 gives 0 reminder:
: 3 6 9 12 15 18 21 24 27 30 33 36 39 42 45 48 51 54 57 60 63 66 69 72 75 78 81 84 87 90 93 96 99 
PS C:\Users\user\projects\netology\go\task_3>
~~~
