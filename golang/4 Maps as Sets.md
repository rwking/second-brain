# Using maps as sets

Since Go doesn't natively support sets, you can use maps to mimic the behavior. Sets can be very helpful structures for holding a list of similar, but unique values. Sets do not guarantee order, but do guarantee uniqueness.

```go
package main

import "fmt"

func main() {
	intSet := map[int]bool{}
	vals := []int{5, 10, 2, 5, 8, 7, 3, 9, 1, 2, 10}
	for _, v := range vals {
		intSet[v] = true
	}
	fmt.Println(len(vals), len(intSet))
	fmt.Println(intSet[5])
	fmt.Println(intSet[500])
	if intSet[100] {
		fmt.Println("100 is in the set")
	}
}
```

Output:
```console
11 8
true
false
```

Next up:
[[5 Types, Methods, Interfaces]]





