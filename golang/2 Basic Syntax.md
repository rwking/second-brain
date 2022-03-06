# Declaring Variables

## var Versus :=

The most verbose way to declare a variable is with the keyword `var`. It uses the keyword `var`, a variable name, an explicit type, and an assignment.

```go
var x int = 10
```

Note: if the type on the righthand side of the = is the expected type of your variable, you can leave off the type from th eleft side of the =. 

```go
var x = 10
```

If you want to declare the variable and assign it to the zero value for that type, you can leave off the assignment.

```go
var x int
```

Declaring multiple variables at once with var:

```go
var x, y = 10, 15
```

The last way you can use var is with a _declaration list_:

```go
var (
	x     int
	y             = 20
	z     int     = 30
	d, e          = 40, "hello"
	f, g  string
)
```


## Short declaration with :=

### Equivalents:

Single assignment:

```go
var x = 10
x := 10
```

Multiple assignment:

```go
var x, y = 10, "hello"
x, y := 10, "hello"
```

Assigning values to existing variables - As long as there is at least one new variable on the lefthand side of the :=, then any of the other variables can already exist.

```go
x := 10
x, y := 15, "hello"
```

Note: the := operator is only legal inside of functions. Declaring variables at the package level requires the `var` keyword.







