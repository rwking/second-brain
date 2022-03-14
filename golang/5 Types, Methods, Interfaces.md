# Types

## User-defined Types
```go
type Person struct {
	FristName string
	LastName string
	Age int
}
```

Declaring a user-defined type _Person_ based on a struct literal (that the user also specifies). 

In addition to structs, user-defined types can piggy back off of primitives and compound types.

```go
type Score int
type Converter func(string)Score
type TeamScores map[string]Score
```

Note that the operators for these types are legal in the user-defined types. Ex: 

```go
type Age int
a := Age(10)
b := Age(20)
c := a + b
fmt.Println(c)
```

Output:

```console
> 30
```

Whether or not it is useful or logical. :-) 


## Methods

Go supports methods on user-defined types, which makes them particularly powerful.

```go
type Person struct {
	FirstName string
	LastName string
	Age int
}

func (p Person) String() string {
	return fmt.Sprintf("%s %s, age %d", p.FristName, p.LastName, p.Age)
}
```

The method is invoked in much the same way that you would reference a field:

```go
output := p.String()
```


## Pointer Receivers and Value Receivers

Go uses pointer receivers to indicate that a value may be modified by a function. The same is true of methods. Recall that arguments passed to functions are copied within the function, which does not modify the original value (unless explicitly specified with the use of a pointer receiver). 

Rules to abide by:

 1. If your method modifies the receiver, you _must_ use a pointer receiver
2. If your method needs to handle nil instances, it _must_ use a pointer receiver
3. If your method doesn't modify the receiver, you *can* use a pointer receiver


### Pointer synax - A primer

(_The syntax for pointers confused me initially, so documenting it for posterity._)

A pointer receiver is denoted by `*t Type`. The * tells Go that this parameter is a pointer receiver and the method may modify the value. When Go runs the code, it converts the `*t` to `&t`. The & symbol tells go the address in memory where the value is stored.


### Pointer receiver example

```go
package main

import (
	"fmt"
	"time"
)

type Counter struct {
	total       int
	lastUpdated time.Time
}

func (c *Counter) Increment() {
	c.total++
	c.lastUpdated = time.Now()
}

func (c Counter) String() string {
	return fmt.Sprintf("total: %d, last updated: %v", c.total, c.lastUpdated)
}

func main() {
	var c Counter
	fmt.Println(c.String())
	c.Increment()
	fmt.Println(c.String())
}
```

```console
> total: 0, last updated: 0001-01-01 00:00:00 +0000 UTC
> total: 1, last updated: 2009-11-10 23:00:00 +0000 UTC m=+0.000000001
```


## Embedding for Composition

Go provides an intuitive way to build complex user-defined types by combining smaller types.

An example is an Employee type and a Manager type. There are obvious relationships inherent just in the variable names, but the relationship can be made concrete using embedding.

```go
type Employee struct {
	Name  string
	ID    int
}

type Manager struct {
	Employee
	Reports []Employee
}

func (m Manager) FindNewEmployees() []Employee {
	// some business logic
}
```

Because the Manager type contains the type Employee (with no name assigned to it), we know that this is an embedded field. Any fields or methods declared on an embedded field are promoted to the containing struct, making them accessible directly on it. 

Example:

```go
m := Manager {
	Employee: Employee{
		Name:    "Bob Smith",
		ID:      "123",
	},
	Reports: []Employee{},
}

fmt.Println(m.ID).          // prints 123
fmt.Println(m.Description)  // prints Bob Bobson (123)
```

Note: you can embed any type within a struct, not just another struct. This promotes all methods on the embedded type to the containing struct. - Handy!


## Interfaces

Interfaces are the only abstract type in Go. Like other user-defined types, you declare them with the `type` keyword. 

```go
type Stringer interface {
	String() string
}
```

An interface literal should appear after the name of the interface type. The literal contains the methods that need to be implemented by a concrete type in order to meet the interface. 

Interfaces in Go are implemented *implicitly* - if a type implements the method set, it meets the requirements of the interface.

```go
package main

import "fmt"

type LogicProvider struct{}

func (lp LogicProvider) Process(data string) {
	fmt.Printf("Hello, %s\n", data)
}

type Logic interface {
	Process(data string)
}

type Client struct {
	L Logic
}

func (c Client) Program() {
	// get some data
	//c.L.Process(data)
}

func (c Client) Print() {
	fmt.Println("Client application")
}

func main() {

	c := Client{
		L: LogicProvider{},
	}
	c.L.Process("World")
	c.Print()
}
```

```console
> Hello World
> Client Application
```

Note: Types that meet the requirements of an interface can specify additional methods not part of the interface. The example above demonstrates this. 

A best practice is to end interface names in `er`. Like io.Reader, io.Closer, and io.ReadCloser. Let's use io.ReadCloser as our example:

```go
type Reader interface {
	Read(p []byte) (n int, err error)
}

type Closer interface {
	Closer() error
}

type ReadCloser interface {
	Reader
	Closer
}
```


## "Accept interfaces; return structs"

What does this mean? 

The business logic invoked by your functions should be invoked via interfaces. However, the output of your functions should be concrete types. 

When functions accept interfaces, your code becomes more flexible, and it explicitly declares what functionality is being used. 

When you return interfaces, you lose one main advantage of implicit interfaces: **decoupling.** You want to limit third part interfaces your code depends on because you create a permanent dependency on that module and it's dependencies. 

Another reason to avoid returning interfaces is versioning. If you return concrete types, new methods and fields can be added, but not if you return interfaces. Adding a new method to an interface means updating every implementation; otherwise, your code breaks. This would be a major version change to your code. 

*Note:* You cannot always avoid returning interfaces. Just try not to.


## Empty Interfaces Special Usage

In special cases, you may want a variable to store a value of any type. The empty interface enables this functionality:

```go
var i interface{}
i = 20
i = "hello"
i = struct{
	FirstName string
	LastName string
} {"Rich", "King"}
```

This is explained by the fact that an empty interface type simply states that the variable can store any value whose type implements zero or more methods - this is every type in Go. 

A common use of an empty interface is a data structure with an unknown schema, like a JSON file.

```go
// one set of braces for the interface{} type,
// the other to instantiate an instance of the map
data := map[string]interface{}{}
contents, err := ioutil.ReadFile("testdata/sample.json") {
	return err
}
defer contents.Close()
json.Unmarshall(contents, &data)
// the contetns are now in the map
```


## Type Assertions and Type Switches

A type assertion names the concrete type that implemented the interface, or names another interface that is also implemented by the concrete type underlying the interface. 

```go 

```



