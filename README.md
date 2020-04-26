# optionGen
optionGen is a fork of [XSAM/optionGen](https://github.com/XSAM/optionGen), a tool to generate go Struct option for test, mock or more flexible. The purpose of this fork is to provide more powerful and flexible option generation. 
## Functional Options
Functional options are an idiomatic way of creating APIs with options on types. The initial idea for this design pattern can be found in an article published by Rob Pike called [Self-referential functions and the design of options](https://commandcenter.blogspot.com/2014/01/self-referential-functions-and-design.html).

## Install
Install using go get, and this will build the optionGen binary in $GOPATH/bin.
```bash
go get github.com/timestee/optionGen/...
```

optionGen require [goimports](https://godoc.org/golang.org/x/tools/cmd/goimports) to format code which is generated. So you may confirm that `goimports` has been installed

```bash
go get golang.org/x/tools/cmd/goimports
```

## Using optionGen
To generate struct option, you need write a function declaration to tell optionGen how to generate.struct name and `OptionDeclareWithDefault` suffix. In this function, just return a variable which type is `map[string]interface{}`.

The key of the map means option name, and the value of the map should consist of two parts, one for option type(except func type), and the other option default value.

Here is an example.
```go
//go:generate optionGen --option_with_struct_name=false

func ConfigOptionDeclareWithDefault() interface{} {
	return map[string]interface{}{
		"TestNil":           nil,
		"TestBool":          false,
		"TestInt":           32,
		"TestInt64":         int64(32),
		"TestSliceInt":      []int{1, 2, 3},
		"TestSliceInt64":    []int64{1, 2, 3},
		"TestSliceString":   []string{"test1", "test2"},
		"TestSliceBool":     []bool{false, true},
		"TestSliceIntNil":   []int(nil),
		"TestSliceIntEmpty": []int{},

		"TestMapIntInt":       map[int]int{1: 1, 2: 2, 3: 3},
		"TestMapIntString":    map[int]string{1: "test"},
		"TestMapStringInt":    map[string]int{"test": 1},
		"TestMapStringString": map[string]string{"test": "test"},

		"TestString": "Meow",
		"Food":       (*string)(nil),
		"Walk": func() {
			log.Println("Walking")
		},
		"TestNilFunc": (func())(nil),
	}
}

```

To use a optionGen, you must tell optionGen that you want to use it using a special comment in your code. For example
```
//go:generate optionGen --option_with_struct_name=false
```
This tells go generate to run optionGen and that you want to ignore the struct name for option function.

Here is the sample result generate by `optionGen`

```go
// Code generated by optionGen. DO NOT EDIT.
// optionGen: github.com/timestee/optionGen

package example

import "log"

type Config struct {
	TestNil             interface{}
	TestBool            bool
	TestInt             int
	TestInt64           int64
	TestSliceInt        []int
	TestSliceInt64      []int64
	TestSliceString     []string
	TestSliceBool       []bool
	TestSliceIntNil     []int
	TestSliceIntEmpty   []int
	TestMapIntInt       map[int]int
	TestMapIntString    map[int]string
	TestMapStringInt    map[string]int
	TestMapStringString map[string]string
	TestString          string
	Food                *string
	Walk                func()
	TestNilFunc         func()
}

type ConfigOption func(oo *Config)

func WithTestNil(v interface{}) ConfigOption       { return func(oo *Config) { oo.TestNil = v } }
func WithTestBool(v bool) ConfigOption             { return func(oo *Config) { oo.TestBool = v } }
func WithTestInt(v int) ConfigOption               { return func(oo *Config) { oo.TestInt = v } }
func WithTestInt64(v int64) ConfigOption           { return func(oo *Config) { oo.TestInt64 = v } }
func WithTestSliceInt(v []int) ConfigOption        { return func(oo *Config) { oo.TestSliceInt = v } }
func WithTestSliceInt64(v []int64) ConfigOption    { return func(oo *Config) { oo.TestSliceInt64 = v } }
func WithTestSliceString(v []string) ConfigOption  { return func(oo *Config) { oo.TestSliceString = v } }
func WithTestSliceBool(v []bool) ConfigOption      { return func(oo *Config) { oo.TestSliceBool = v } }
func WithTestSliceIntNil(v []int) ConfigOption     { return func(oo *Config) { oo.TestSliceIntNil = v } }
func WithTestSliceIntEmpty(v []int) ConfigOption   { return func(oo *Config) { oo.TestSliceIntEmpty = v } }
func WithTestMapIntInt(v map[int]int) ConfigOption { return func(oo *Config) { oo.TestMapIntInt = v } }
func WithTestMapIntString(v map[int]string) ConfigOption {
	return func(oo *Config) { oo.TestMapIntString = v }
}
func WithTestMapStringInt(v map[string]int) ConfigOption {
	return func(oo *Config) { oo.TestMapStringInt = v }
}
func WithTestMapStringString(v map[string]string) ConfigOption {
	return func(oo *Config) { oo.TestMapStringString = v }
}
func WithTestString(v string) ConfigOption  { return func(oo *Config) { oo.TestString = v } }
func WithFood(v *string) ConfigOption       { return func(oo *Config) { oo.Food = v } }
func WithWalk(v func()) ConfigOption        { return func(oo *Config) { oo.Walk = v } }
func WithTestNilFunc(v func()) ConfigOption { return func(oo *Config) { oo.TestNilFunc = v } }

func NewConfig(opts ...ConfigOption) *Config {
	ret := newDefaultConfig()
	for _, o := range opts {
		o(ret)
	}
	return ret
}

var defaultConfigOptions = [...]ConfigOption{
	WithTestNil(nil),
	WithTestBool(false),
	WithTestInt(32),
	WithTestInt64(32),
	WithTestSliceInt([]int{1, 2, 3}),
	WithTestSliceInt64([]int64{1, 2, 3}),
	WithTestSliceString([]string{"test1", "test2"}),
	WithTestSliceBool([]bool{false, true}),
	WithTestSliceIntNil(nil),
	WithTestSliceIntEmpty(nil),
	WithTestMapIntInt(map[int]int{1: 1, 2: 2, 3: 3}),
	WithTestMapIntString(map[int]string{1: "test"}),
	WithTestMapStringInt(map[string]int{"test": 1}),
	WithTestMapStringString(map[string]string{"test": "test"}),
	WithTestString("Meow"),
	WithFood(nil),
	WithWalk(func() {
		log.Println("Walking")
	}),
	WithTestNilFunc(nil),
}

func newDefaultConfig() *Config {
	ret := &Config{}
	for _, o := range defaultConfigOptions {
		o(ret)
	}
	return ret
}


```

See a complete example in the [example](https://github.com/timestee/optionGen/blob/master/example/cat.go) directory.
