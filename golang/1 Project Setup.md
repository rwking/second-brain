# Tools for new projects

## Useful Commands
`go run .`
_Runs the current project (denoted by .)_

`go build .`
_Builds an executable of the current project_

`go build -o hello_world hello.go`
Builds an executable with a different name than your package


## Makefiles

Makefiles enable the creation of repeatable, automated softwar builds. By creating and distributing a makefile with your project, you can ensure the build steps remain consistent between machines / platforms / operating systems. 

Simple makefile example:

```make
.DEFAULT_GOAL := build

fmt:
		go fmt ./...
.PHONY:fmt

lint: fmt
		golint ./...
.PHONY:lint

vet: fmt
		go vet ./...
.PHONY:vet

build: vet
		go build hello.go
.PHONY: build
```

`.DEFAULT_GOAL` runs when no target is specified.

The word before the colon `:` is the target. Any words after the target are the other targets that mus run before the specified target runs.

The tasks run for the target are on the indented lines after the target line. 

`.PHONY` keeps `make` from getting confused when you have directories in your project with the same name as the targets. 

Invoking `make` in the project directory for the above sample file will produce the following output:

```console
> go fmt ./...
> go vet ./...
> go build hello.go
```

The single command, `make`, ensures the code is formatted, vetted for potential non-obvious errors, and compiled.


## Documentation

Go has a built-in utility that shows package / module documentation in the console. To run:

```console
> go doc module-name
```

Example output:

```console
~/code/godoc > go doc Money                                             

package money // import "."
 
type Money struct {
	Value    decimal.Decimal
	Currency string
}
    Money represents the combination of an amount of money and the currency the
    money is in.

func Convert(from Money, to string) (Money, error)
```

Source:
_Learning Go: An Idiomatic Approach to Real-Worled Go Programming by Jon Bodner_
[Link](https://www.oreilly.com/library/view/learning-go/9781492077206/)

Next up:
[[2 Basic Syntax]]