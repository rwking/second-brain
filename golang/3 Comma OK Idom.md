# The comma ok Idiom

Maps return two values when accessing data with a key. The value, if it exists, and a boolean indicating whether the map contains an entry for the given key. 

```go
m := map[string]int{
	"hello": 5,
	"world": 0,
}

v, ok := m["hello"]
fmt.Println(v, ok)

v, ok := m["world"]
fmt.Println(v, ok)

v, ok := m["goodbye"]
fmt.Println(v, ok)

```

The comma ok idiom in Go lets us distinguish between reading a value and getting back the zero value.

This is also a good practice to implement with user defined types.

Next up:
[[4 Maps as Sets]]