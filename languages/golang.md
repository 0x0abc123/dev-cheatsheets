# Example Skeleton Main Program
```golang
// go.mod
module helloworld
go 1.20

// helloworld.go

package main

import (
    "fmt"
)

func main() {
    fmt.Printf("Hello world!\n")
}

```

# Comments
```golang
// single line comment
/* multi line...
    ...comment */
```

# Console Input/Output

## Output
```golang
    fmt.Println("Hello ",name,"!")
    fmt.Printf("%s %d\n", name, count) // var name string ; var count int
```

## Input
```golang
var cmd string
fmt.Scanln(&cmd)
```

# Namespaces
Go uses modules and packages. If your app has this structure:

```
mymodule
├── go.mod
├── mymodule.go
└── mypackage
    ├── foo.go
    └── bar.go
```

In go.mod:

```golang
module mymodule
go 1.20
```

In mypackage/foo.go:

```golang
package mypackage
const Something = 1 //...
```

In mymodule.go:

```golang
package main

import (
	"mymodule/mypackage"
	//other package imports...
)

var x = mypackage.Something
func main() {
    fmt.Printf("Something %d\n", x)
    //...
}
```

# Classes

Golang doesn't have classes like other OOP languages but instead achieves the same thing using structs, interfaces and composition via embedding other structs

## Declaration
```golang
type MyStruct struct {
    bar int
    foo string
}

// methods for the struct are defined by including the "receiver" before the method name
// when referencing the struct in the method, you don't need the & operator (Go automatically knows to dereference the pointer to MyStruct)

func (ms *MyStruct) GetBar() int {
    return ms.bar
}

func (ms *MyStruct) GetFoo() string {
    return ms.foo
}

func (ms *MyStruct) SetBar(newBar int) {
    ms.bar = newBar
}

func (ms *MyStruct) SetFoo(newFoo string) {
    ms.foo = newFoo
}

```

### Access Modifiers

In Go, there are no access modifiers like "public" or "private" as you might find in some other programming languages. Instead, the _intended_ visibility of an identifier (variable, constant, type, function, etc.) is denoted by its first letter being uppercase or lowercase.

- An identifier starting with an uppercase letter is exported and can be accessed from outside the package.
- An identifier starting with a lowercase letter is unexported and can only be accessed from within the same package.

This approach is known as the "exported" and "unexported" convention, and it provides a form of encapsulation.

_Note that the compiler won't enforce or flag violations of the convention_

While it doesn't enforce strict encapsulation like access modifiers in some other languages, it encourages developers to use exported identifiers for API boundaries and unexported identifiers for internal implementation details.

```golang
package mypackage

// ExportedStruct has exported fields
type ExportedStruct struct {
    ExportedField int    // Exported
    unexportedField string // Unexported
}

// ExportedFunction is an exported function
func (es *ExportedStruct) ExportedFunction() {
    // Can access ExportedField and unexportedField here
}

```

## Inheritance

### Embedding/Composition
Go does not support inheritance and instead uses composition to achieve the same goals. Inheritance uses the "is-a" concept (Dog is a Mammal), whereas composition uses the "has-a" concept(Dog has a Mammal). So, to re-use functionality and data from an ancestor struct, a Golang struct will embed the ancestor struct:
```golang
// Embedded struct
type Person struct {
   Name string
   Age  int
}

// Embedding struct
type Employee struct {
   Person      // Embedding the Person struct
   CompanyName string
}

func main() {
   // Creating an Employee object
   emp := Employee{
      Person: Person{
         Name: "John Doe",
         Age:  35,
      },
      CompanyName: "ACME Corp",
   }

   // Accessing the embedded Person object
   fmt.Println("Employee Name:", emp.Name)
   fmt.Println("Employee Age:", emp.Age)

   // Accessing the fields of the embedding object
   fmt.Println("Employee Company:", emp.CompanyName)
}
```

### Interfaces
To implement interfaces, use the interface type:

```golang
type Named interface {
    GetName(asSentence bool) string
}

type MyStruct struct {
    Bar int    // exported field
    Foo string // exported field
}

func (ms *MyStruct) GetName(asSentence bool) string {
    if asSentence {
        return fmt.Sprintf("My name is %s", ms.Foo)
    }
    return fmt.Sprintf("%s", ms.Foo)
}

func printName(n Named) {
    fmt.Println(n.GetName(true))
}

func main() {
    myInstance := MyStruct{
        Bar: 42,
        Foo: "Hello, Go!",
    }
    printName(&myInstance)
}

```

## Instantiation and Method Invocation
```golang
// Create an instance of MyStruct
myInstance := MyStruct{
    bar: 42,
    foo: "Hello, Go!",
}

myInstance.SetBar(99)
myInstance.SetFoo("Updated, Go!")
fmt.Printf("Updated values - bar: %d, foo: %s\n", myInstance.GetBar(), myInstance.GetFoo())
```

### Allocating memory

**make()**

- The make function is used to create and initialize slices, maps, and channels.
- It initializes and returns an initialized (not zeroed) value of a specified type.
- It is used with specific types that require initialization, such as dynamic data structures like slices, maps, and channels.

```golang
mySlice := make([]int, 5, 10) // Creates a slice with a length of 5 and a capacity of 10
myMap := make(map[string]int) // Creates an empty map

```

**new()**

- The new() function is a generic allocator that returns a pointer to a newly allocated zeroed value of a specified type.
- It is used with value types (e.g., structs) and returns a pointer to the zero value of the specified type.
- It does not initialize the memory; instead, it zeroes the memory block.

**Composite Literal**

- Does the same thing as new()
- Written like this: `var myInstancePtr = &MyStruct{}`
- This is essentially creating a new instance and obtaining its address in a single step.
- _the new() function is less commonly used in Go because it has a more explicit syntax and doesn't provide any advantage over the composite literal approach._

```golang
// dynamically creating the struct...
myInstancePtr := new(MyStruct)
// OR
var myInstancePtr = &MyStruct{}

// the statement above is a shorthand way of writing...
// var myInstancePtr *MyStruct
// myInstancePtr = new(MyStruct)
myInstancePtr.Foo = "myinstanceptr foo"

```

### Pointers and dereferencing

- A pointer to a type is written as: *typename
- Expressing a pointer to a variable: &instanceLabel
- Dereferencing a pointer to an instance: *ptrToInstance

```golang
func doubleIt(x *float64) {
    *x = *x + *x
}
func main() {
    x := 1.5
    doubleIt(&x)
}
```

**Note: pointers to structs are automatically dereferenced** (see https://golang.org/ref/spec#Selectors)

```golang
p := &SomeThing
// either of these two statements can be used
filename := p.Title + ".txt"
filename := (*p).Title + ".txt"
```

# Functions
```golang
func MyFunction(argument1 Type1, argument2 Type2) ReturnType {
    fmt.Println("arguments: ", argument1, argument2)
    var r ReturnType
    r = CreateIt(argument1, argument2)
    return r
}
```

**Variadic arguments**

```golang
func printNames(namedThings ...Named) {
    for _, n := range namedThings {
	    fmt.Println(n.GetName(true))
    }
}

// ...
printNames(myInstancePtr, &emp, &myInstance)
```

**Returning tuples**

```golang
// import ( "errors" )
func SampleFunction(arg int) (int, string, error) {
	result := 4 * arg
	message := "Success"
	if result > 50 {
		return 0, "", errors.New("SampleFunction failed: result is too large")
	}
	return result, message, nil
}
// result, message, err := SampleFunction(12)
```

**Default parameter values**

Golang doesn't support default values for function/method parameters. One option is to supply arguments bundled as a struct and to have default values for struct fields

```golang
// A declarative default value syntax
// Empty values will be replaced with defaults
type Parameters struct {
  A string `default:"default-a"` // this only works with strings
  B int // default is 5
}

func Concat3(prm Parameters) string {
  typ := reflect.TypeOf(prm)

  if prm.A == "" {
    f, _ := typ.FieldByName("A")
    prm.A = f.Tag.Get("default")
  }

  if prm.B == 0 {
    prm.B = 5
  }

  return fmt.Sprintf("%s%d", prm.A, prm.B)
}
```

# Variables

## Declaration/Assignment
```golang
var v1 int = 10
var v2 = 20 // compiler infers the appropriate type

// this can only be used inside a function body (i.e. not for global variables):
v3 := uint(30) // short-hand way of declaring the variable type and assigning a value
v4 := 40 // compiler infers the type, in this case it would be int

fmt.Println("vars:", v1, v2, v3, v4)
```
### Constants

Constants are declared like variables, but with the const keyword.

Constants can be character, string, boolean, or numeric values.

Constants cannot be declared using the := syntax.

```golang
const Pi = 3.14
```

### Ignoring parts of a tuple

Some functions return tuples, but you might want to ignore some of the values. You can use an underscore to ignore:

```golang
_, x := foo.bar()
```

## Ternary Operator
```golang
// Golang doesn't have a ternary operator so you have to use if/else:
someVariable := 4535
if otherVariable <= 0 { someVariable = 0 }
```

## Global Variables

Avoid using global variables for anything that needs to be secured. Any package can access them. For example, if there is a HTTP mux/router, an imported third-party package can access it and register a route. If the package is compromised it could use the mux/router to expose a malicious handler to the web.

```golang
const GlobalVar0 = 123 // cannot be changed

var GlobalVar1 = 12345

func ChangesGlobalVar(val int) {
	GlobalVar1 = GlobalVar1 + val
}
// ChangesGlobalVar(100)
```

## Data Basic Types
```golang
// integer types
var myInteger int = 42
myInt := 42            // int
myUInt := uint(42)      // uint32
myUInt64 := uint64(42)  // uint64

// float64 (default for floating-point literals)
myFloat64 := 3.14
var myFloat32 float32 = 3.14

myBoolean := true

```

## Data Basic Type Comparison
```golang
a := 10
b := 20

// Basic comparison examples
fmt.Println("a == b:", a == b)
fmt.Println("a != b:", a != b)
fmt.Println("a < b:", a < b)
fmt.Println("a > b:", a > b)
fmt.Println("a <= b:", a <= b)
fmt.Println("a >= b:", a >= b)

```

Comparisons are only allowed between values of the same type. Attempting to compare values of different types will result in a compilation error.

If you need to compare values of different types, you should perform a type conversion explicitly before the comparison. For example:

```golang
a := 10
c := 20.3
fmt.Println("a >= c:", float64(a) >= c)
```

## Logical Operators (Boolean Operands)

```golang
result := true && false // result is false
result := true || false // result is true
result := !true // result is false
```

# Data Structures

## list/array

### slices
```golang
var mySlice []int
mySlice = append(mySlice, 1, 2, 3, 4, 5)

// declare and initialise
days := []string{"Sunday","Monday","Tuesday","Wednesday"}

// Access individual elements
fmt.Println("First element:", mySlice[0]) //prints "1"

// Iterate over the slice
for _, value := range mySlice {
    fmt.Printf(" %d", value)
}

// slices of the slice
mySlice0_2 := mySlice[:3] // [1,2,3]
mySlice2_4 := mySlice[2:4] // [3,4]
mySlice3_end := mySlice[3:] // [4,5]

// slice length:
x := len(mySlice)

// doesn't have relative indexes, so you have to do this:
last3Elements := mySlice[len(mySlice)-3:]

// you can't remove an element, so you have to create a new slice from the original slice by concatenating the slice [:removeIndex] and [removeIndex+1:]

// concatenate two arrays
slice1 := []int{10, 11}
slice2 := []int{12, 13}
slice1 = append(slice1, slice2...)
```

### container/list

Elements in the list are of type interface{}, so you can store values of any type in the list. The Value field of the list's element is of type interface{}, and type assertions may be needed when retrieving values from the list.

The list type does not provide direct indexing like an array or slice. To access an element at a specific position, you would need to iterate through the list until you reach the desired position.

The `container/list` package in Go provides a doubly linked list implementation, which has some characteristics that differentiate it from slices and arrays. A few features and use cases where `container/list` might be more suitable:

1. **Dynamic Size:** Unlike arrays, the size of a linked list is dynamic. You can easily add or remove elements without worrying about the capacity.

2. **Constant-Time Insertion and Deletion:** Inserting or deleting an element in the middle of a linked list is a constant-time operation, while in slices, it's proportional to the number of elements after the insertion or deletion point.

3. **Efficient Element Removal:** Removing elements from the middle of a linked list using the `Remove` method is more efficient than slicing a portion out of a slice.

4. **No Need for Reallocation:** The linked list doesn't need to be reallocated when elements are added or removed, unlike slices which may need to be resized and reallocated in memory.

Linked lists also have some trade-offs compared to slices and arrays:

1. **No Direct Access by Index:** Linked lists don't support direct access by index. Accessing an element at a specific position requires iterating through the list, which can be less efficient than direct indexing in slices.

2. **Increased Memory Overhead:** Each element in a linked list requires additional memory for the pointers to the next and previous elements, which can lead to increased memory overhead compared to slices.

3. **Cache Locality:** Slices have better cache locality, which can result in better performance for certain types of operations, especially when iterating through consecutive elements.

```golang
import (
	"container/list"
)

myList := list.New()

// Push elements to the back of the list
myList.PushBack(1)
myList.PushBack(2)
myList.PushBack(3)

// Push elements to the front of the list
myList.PushFront(0)

// Print the elements by iterating through the list
for element := myList.Front(); element != nil; element = element.Next() {
    fmt.Printf(" %v", element.Value)
}

// Remove an element from the list
secondElement := myList.Front().Next()
myList.Remove(secondElement)

// length of list
middleIndex := myList.Len() / 2

// insert an element into the list
element := myList.Front()
for i := 0; i < middleIndex; i++ {
    element = element.Next()
}
myList.InsertBefore(value, element)

// function that accepts a list
func insertIntoMiddle(l *list.List, value int) {
    //...
}
```

## dictionary

Golang has the map built-in data type
```golang
myMap := make(map[string]string)
myMap2 := map[string]int{"apple": 5, "lettuce": 7} // using initializer
myMap["a"] = "entry 1"
myMap["b"] = "entry 2"
fmt.Println(myMap["b"]) // prints "entry 2"
val, exists := myMap["c"]
if exists {
    fmt.Println("Value for key 'c':", val)
}
for key, val := range myMap {
    fmt.Printf("%s: %s\n", key, val)
}
delete(myMap, "b") // remove a key/value
fmt.Println(myMap) // prints map[a:entry 1]
```

## set

Golang does not have a builtin Set type, but you can implement it using a map:

```golang
type Set map[string]bool

func main() {
	mySet := make(Set)

	// Add elements to the set
	mySet["apple"] = true
	mySet["banana"] = true
	mySet["orange"] = true

	// Check if an element is in the set
	fmt.Println("Is 'apple' in the set?", mySet["apple"])
	fmt.Println("Is 'grape' in the set?", mySet["grape"])

	// Print all elements in the set
	fmt.Println("Elements in the set:")
	for element := range mySet {
		fmt.Println(element)
	}

	// Remove an element from the set
	delete(mySet, "banana")

	// Print the updated set
	fmt.Println("Set after removing 'banana':", mySet) //prints "map[apple:true orange:true]"

}
```

# Control Flow Statements

## if/else

You don’t need parentheses around conditions in Go, but the curly braces are required.

```golang
if 7%2 == 0 {
    fmt.Println("7 is even")
} else {
    fmt.Println("7 is odd")
}

if 8%2 == 0 || 7%2 == 0 {
    fmt.Println("either 8 or 7 are even")
}

// you can have a statement that precedes conditionals
// any variables declared in this statement are available in the current and all subsequent branches.
if num := getRandomNum(); num < 0 {
    fmt.Println(num, "is negative")
} else if num < 10 {
    fmt.Println(num, "has 1 digit")
} else {
    fmt.Println(num, "has multiple digits")
}


```

## for
```golang

for i := 0; i <= 100; i++ {
    x := doSomething(i)
    if x == 123 {
        break
    } else if x == 111 {
        continue
    }
    doSomethingElse(x)
}



```

## while

Golang doesn't have while loops, so use for without any condition and break on condition to exit
```golang
// while (doSomething() != 123)
for {
    x := doSomething()
    if x == 123 {
        break
    }
    doSomethingElse(x)
}
```

## switch/case

```golang
switch i {
    case 1:
        fmt.Println("one")
    case 2:
        fmt.Println("two")
    case 3,4,5:
        fmt.Println("a few")
    default:
        fmt.Println("many")
}

// you can also use switch with non-constant case expressions to implement if/else logic more concisely:
hr := time.Now().Hour()
switch {
    case hr > 4 && hr <= 12:
        fmt.Println("It's breakfast")
    case hr > 12 && hr <= 17:
        fmt.Println("It's lunchtime")
    case hr > 17 && hr <= 23:
        fmt.Println("It's dinnertime")
    default:
        fmt.Println("It's time for bed")
}

```


# Exception Handling
```golang
import (
    "errors"
)

func doSomething() (int, error) {
    if retVal := doSomethingElse(); retVal < 0 {
        return -1, errors.New("Negative result")
    }
    return retVal, nil
}

func main() {
    //try:
    if result, err := doSomething(); err != nil {
        fmt.Println("An error occurred: ", err)
    }
}

```

### Defer

The defer keyword is used to delay the execution of a function call until the surrounding function (the one containing the defer statement) returns. Deferred functions are executed in Last In, First Out (LIFO) order, meaning the function calls are executed in reverse order of their appearance in the source code.

The primary use cases for defer include cleaning up resources, closing files, releasing locks, and other tasks that should be performed before a function exits.

The deferred function is executed after the other statements in the main function, but before the main function exits. This behavior is useful for ensuring that certain cleanup or finalization tasks are performed regardless of how the function exits, whether it's due to normal return or an early exit due to an error.
```golang
package main

import "fmt"

func main() {
	fmt.Println("Start of main function")
	defer deferredFunction()
	fmt.Println("End of main function")
	// deferred functions get called here, before main() exits
}

func deferredFunction() {
	fmt.Println("Deferred function called")
}

// this outputs:
// Start of main function
// End of main function
// Deferred function called

```

# Numbers

## Operators
```golang
x := 10
y := 5
fmt.Printf("%d\n", x+y) // 15
fmt.Printf("%d\n", x-y) // 5
fmt.Printf("%d\n", x*y) // 50
fmt.Printf("%d\n", x/y) // 2
fmt.Printf("%d\n", x%y) // 0 (Modulus)

// there is no exponent operator, but the math lib has func Pow(x, y float64) float64
XPowY := math.Pow(float64(x),float64(y))

// in-place operators can be used, eg.:
x += 1 // x = x + 1
x *= 4 // x = x * 4

// increment and decrement operators, eg.:
x++ // x = x + 1
x-- // x = x - 1

```

## Absolute Value
```golang
import "math"
// expects a float64 argument
// x == -10
absx := math.Abs(float64(x)) // == 10
```

## Min/Max

Golang math standard lib doesn't have min/max functions, so have to roll your own

```golang
func min(a int, b int) int {
	if a < b { return a }
	return b
}

func max(a int, b int) int {
	if a > b { return a }
	return b
}
```

## Floor/Ceiling
```golang
import "math"
f := 3.634
math.Floor(f) == 3
math.Ceil(f) == 4
```

## Convert Between Int/Float
```golang
int(f) == 3
float64(x) == 10.0
```

## Parse Number from String

The strconv.Parse* functions signature is `func ParseInt(s string, base int, bitSize int) (i int64, err error)`

_*Note that it returns int64, so you have to cast it to int_

`base`: is the radix

`bitSize`: must be 0, 8, 16, 32, or 64 (0 means infer from the string, otherwise it must be consistent with the target variable)

```golang
import "strconv"
f, err := strconv.ParseFloat("3.1415", 64)
i, err := strconv.ParseInt("-42", 10, 64) // int64
u, err := strconv.ParseUint("42", 10, 64)
uFromBinString, _ := strconv.ParseUint("10110110", 2, 64)
uFromHexString, _ := strconv.ParseUint("f3f08e", 16, 64)
```

## Random
```golang
import (
    "crypto/rand"
    "encoding/binary"
)

func generateRandomInt() (int, error) {

    randomBytes := make([]byte, 8)
    _, err := rand.Read(randomBytes)
	randomInt := int(binary.BigEndian.Uint64(randomBytes))
	return randomInt, err
}

// r, _ := generateRandomInt()

const charset = "abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789"

func generateRandomString(length int, charset string) string {

	// Generate the random string
	result := make([]byte, length)
	for i := range result {
		randInt, _ := generateRandomInt()
		abs := int(math.Abs(float64(randInt % len(charset))))
		result[i] = charset[abs]
	}

	return string(result)
}

// generateRandomString(12,charset)

```

# Strings

## Literals
```golang
singleLineStr := "single line" // must use double quotes as delimiter
multiLineStr := `multi line
string uses backticks as
the delimiter, and any backslashes are literal
and do not get escaped. These are good for when the string has "double quotes"`
```

## Concatenation
```golang
str1 := "foo"
str2 := "bar"
str1 + str2
```

## Formatting
```golang
import "fmt"

svar := "foo"
ivar := 123
fvar := 123.456
bvar := 0b10110011
msg0 := fmt.Sprintf("svar:%s, ivar:%d, fvar:%f, bvar:%08b", svar, ivar, fvar, bvar)

```

## Convert Types to a String Representation
```golang
stringRepresentation := fmt.Sprintf("%v", someStructVariable)
```

## Strip/Trim
```golang
import "strings"
teststr := "   __test__    "
strings.TrimSpace(teststr)  // returns: "__test__"
strings.TrimLeft(teststr," ")  // returns: "__test__    "
strings.Trim(teststr," _")  // returns: "test"

```

## Split
```golang
import "strings"
inputString := "one,two,three"
result := strings.Split(inputString, ",") // result == ["one","two","three"]
```

## Replace
```golang
import "strings"
inputString := "this is a string"
outputString := strings.Replace(inputString, "string", "replacement", -1) // The -1 indicates that all occurrences should be replaced.
// outputString == "this is a replacement"
```
## StartsWith
```golang
import "strings"
startsWithComma := strings.HasPrefix(inputString, ",") // True
```

## EndsWith
```golang
import "strings"
endsWithComma := strings.HasSuffix(inputString, ",") // True
```

## Change case
```golang
lstr := strings.ToLower(inputString)
ustr := strings.ToUpper(inputString)
```

## Find a Substring
```golang
containsCoat := strings.Contains(inputString, "coat")
index := strings.Index(inputString, "coat") // -1 if not found otherwise returns the start index of the substring match
```

## Select a Substring
```golang
// use the slice operators
inputString[:5] // "one,t"
```

## Regex
```golang
import "regexp"

// Check if a string matches a regex (true/false)
match, _ := regexp.MatchString("p([a-z]+)ch", "peach") // match == true

// most regex operations should use a compiled pattern:
r, _ := regexp.Compile("https?:\\/\\/[a-zA-Z0-9\\-\\.]+")

// return all matches
line := "link A [http://foo.com], link B [https://bar.net]"
m := r.FindAllString(line,-1) // ["http://foo.com","https://bar.net"]

/*
The Submatch variants include information about both the whole-pattern matches and the submatches within those matches. For example this will return information for both p([a-z]+)ch and ([a-z]+).
*/
line := "203.4.25.66 - 2016-05-22:14:52:03 GET /index.php HTTP/1.1 - Mozilla/5.0 (X11; Linux i686)"
r, _ := regexp.Compile("([0-9\\.]+).+GET (.+) HTTP")
m := r.FindStringSubmatch(line)
if len(m) > 0 {
    fmt.Println(m[1]) // "203.4.25.66"
    fmt.Println(m[2]) // "/index.php"
}

```

# Binary Bytes/Buffers

## Declaration
```golang
var key byte = 0x65 // hex literal
var val byte = 0b01111111 // binary literal
// uint8 == byte
var x uint8 = 0b01111111

header := []byte{0x01, 0xA2, 0xB3, 0xC4, 0xD5}

```

## Bytearray/Buffer Operations

Just use the normal slice operations (append, direct access eg. bytelist[3])

## Bitwise Operations 

```golang
var a byte = 0b11011001
var b byte = 0b10011101

```

### And
```golang
op := a & b
```

### Or
```golang
op := a | b
```

### Xor
```golang
op := a ^ b
```

### Not
```golang
op := ^a
```

### Shift Left/Right
```golang
op := a >> 4 // 00001101
op := a << 4 // 10010000
```

# CLI Argument Parsing

### Simple Arguments

```golang
import "os"

// arg[0] is the binary/program name
argsWithProg := os.Args
argsWithoutProg := os.Args[1:]
arg := os.Args[3]

```

### Flags

```golang
import "flag"

var intFlag int
flag.IntVar(&intFlag, "n", 0, "An integer flag")

var stringFlag string
flag.StringVar(&stringFlag, "s", "defaultValue", "A string flag")

flag.Parse()
```

### Subcommands
```golang
import "flag"

func main() {

    // ./subcmd --help
    // ./subcmd foo -enable -name feljsklef
    fooCmd := flag.NewFlagSet("foo", flag.ExitOnError)
    fooEnable := fooCmd.Bool("enable", false, "enable")
    fooName := fooCmd.String("name", "", "name")

    barCmd := flag.NewFlagSet("bar", flag.ExitOnError)
    barLevel := barCmd.Int("level", 0, "level")

    if len(os.Args) < 2 {
        fmt.Println("expected 'foo' or 'bar' subcommands")
        os.Exit(1)
    }

    switch os.Args[1] {

    case "foo":
        fooCmd.Parse(os.Args[2:])
        fmt.Println("subcommand 'foo'")
        fmt.Println("  enable:", *fooEnable)
        fmt.Println("  name:", *fooName)
        fmt.Println("  tail:", fooCmd.Args())
    case "bar":
        barCmd.Parse(os.Args[2:])
        fmt.Println("subcommand 'bar'")
        fmt.Println("  level:", *barLevel)
        fmt.Println("  tail:", barCmd.Args())
    default:
        fmt.Println("expected 'foo' or 'bar' subcommands")
        os.Exit(1)
    }
}

```

# Execute Shell Commands
```golang

import "os/exec"

execCmd := exec.Command("ls","-alh","/tmp")

cmdOut, err := execCmd.Output()
if err != nil {
    panic(err)
}
fmt.Println(string(cmdOut))

```

# File Operations

## Open File and Read
```golang
// ReadFile returns a []byte
dat, err := os.ReadFile("/etc/passwd")
if err != nil {
    panic(err)
}
defer dat.Close()
fmt.Print(string(dat))

// to iterate over lines of text...
lines := strings.Split(string(dat), "\n")
for _,line := range lines {
    fmt.Printf("'%s'\n",line)
}

// reading chunks of data
f, err := os.Open("/etc/passwd")
if err != nil { panic(err) }

const bytesToRead = 1024
b1 := make([]byte, bytesToRead)
n1, err := f.Read(b1)
if err != nil { panic(err) }

// num of bytes read may be less than the buffer size
fmt.Printf("%d bytes were read: %s\n", n1, string(b1[:n1]))

```

## Open File and Write
```golang
f, err := os.OpenFile("/tmp/test.dat", os.O_RDWR|os.O_CREATE, 0755)
if err != nil { panic(err) }

// write text
numBytesWritten, err := f.WriteString("text data written to file")

// write binary
header := []byte{0x01, 0xA2, 0xB3, 0xC4, 0xD5}
numBytesWritten, err := f.Write(header)

```

## Skip To File Location
```golang
const whence = 0 // 0 == absolute position from start, 1 == relative to current offset, 2 == relative to end of file
offset := 8
newOffsetAfterSeek, err := f.Seek(offset, whence)
```

## Close File
```golang
f.Close()
// ...but you would normally use defer right after opening the file:
defer f.Close()
```

# Networking

## Sockets
```golang
import (
 "net"
 "bufio"
)

// listen and read from socket
ln,err := net.Listen("tcp", ":8080")
if err != nil { panic(err) }
conn, _ := ln.Accept()
line, _ := bufio.NewReader(conn).ReadString('\n')
fmt.Printf("Received: %s\n", line)


// connect socket to server, send data and read response
conn, err := net.Dial("tcp", "10.20.30.40:8081")
if err != nil { panic(err) }
fmt.Fprintf(conn, "GET / HTTP/1.1\r\n\r\n")
line, _ := bufio.NewReader(conn).ReadString('\n')
fmt.Println(line)

```

## Higher Level Network Protocols (HTTP Client, Server)

### HTTP client

**GET request and read response**

```golang
import (
    "bufio"
    "fmt"
    "net/http"
)

func main() {

    resp, err := http.Get("http://10.20.0.6:8081")
    if err != nil {
        panic(err)
    }
    defer resp.Body.Close()

    fmt.Println("Response status:", resp.Status)

    scanner := bufio.NewScanner(resp.Body)
    for scanner.Scan() {
        fmt.Println(scanner.Text())
    }

    if err := scanner.Err(); err != nil {
        panic(err)
    }
}
```

You could also use "io" instead of bufio.Scanner

```golang
import "io"
body, _ := io.ReadAll(resp.Body) // ReadAll returns a []byte slice
fmt.Println("Response body:", string(body))
```

**POST request**

```golang
// import "bytes"
bufStr := []byte(`this is
a string POST body
to send
`)

buf := []byte{0x34,0x35,0x36,0x37,0x38,0x35,0x36,0x37,0x38,0x35,0x36,0x37,0x38}
reader := bytes.NewReader(buf)
resp, err := http.Post("http://10.20.0.7:8082/upload", "application/octet-stream", reader)
fmt.Println(resp, err)
```

**Using Client**

```golang
// import ("net/http","io")
func main() {

    url := "http://10.20.0.8:8082/"
    client := &http.Client{}
    req, _ := http.NewRequest("GET", url, nil)
    req.Header.Set("name", "value") // set request header values
    res, _ := client.Do(req)
    defer res.Body.Close()
    body, _ := io.ReadAll(res.Body)
    fmt.Println(res.Header.Get("Content-Type")) // get response headers
    fmt.Println(string(body))
}

// POST
    requestBody := strings.NewReader("This is the request body.")
	req, err := http.NewRequest("POST", url, requestBody)

```

### HTTP server

```golang
package main

import (
    "fmt"
    "net/http"
    "encoding/json"
)

func hello(w http.ResponseWriter, req *http.Request) {
    w.Header().Set("Content-Type", "text/html") // set response header
    fmt.Fprintf(w, "<html><body><h1>Hello</h1></body></html>")
}

func jsontest(w http.ResponseWriter, req *http.Request) { // json response example
    w.Header().Set("Content-Type", "application/json; charset=utf-8")
    enc := json.NewEncoder(w)
    d := map[string]int{"apple": 5, "lettuce": 7}
    enc.Encode(d)
}

func defaulthandler(w http.ResponseWriter, req *http.Request) {
    fmt.Fprintf(w, "METHOD: %v\n", req.Method) // request method
    fmt.Fprintf(w, "PATH: %v\n", req.URL.Path) // request path
    fmt.Fprintf(w, "HEADERS:\n")
    for name, headers := range req.Header { // request headers
        for _, h := range headers {
            fmt.Fprintf(w, "%v: %v\n", name, h)
        }
    }
}

func main() {
    http.HandleFunc("/hello/", hello) // trailing slash will match any path with the prefix
    http.HandleFunc("/json", jsontest) // without a trailing slash, must be exact match
    http.HandleFunc("/", defaulthandler) // slash by itself matches anything not matched by other routes
    http.ListenAndServe(":8090", nil)
}


/* ListenAndServe creates a new service goroutine for each incoming connection. The service goroutines read requests and then call srv.Handler to reply to them.
*/
```

# JSON Serialization

## Serialize

```golang
import (
    "encoding/json"
    "fmt"
)

// The struct must use uppercase fields so they are exported publicly
// the `json:"key"` maps the Field name to whatever json key
type FruitPage struct {
    Page   int      `json:"page"`
    Fruits []string `json:"fruits"`
}

func main() {

    // you can serialise arbitrary dicts, arrays, strings etc. to json without a struct definition...
    mapD := map[string]int{"apple": 5, "lettuce": 7}
    mapB, _ := json.Marshal(mapD)
    fmt.Println(string(mapB))

    // serialise with a struct defined...
    res2D := &FruitPage{
        Page:   1,
        Fruits: []string{"apple", "peach", "pear"}}
    res2B, _ := json.Marshal(res2D)
    fmt.Println(string(res2B))
    // prints:  {"page":1,"fruits":["apple","peach","pear"]}
```

## Deserialize

```golang
    byt := []byte(`{"num":6.13,"strs":["a","b"]}`)

	var dat map[string]interface{}

    if err := json.Unmarshal(byt, &dat); err != nil {
        panic(err)
    }
    fmt.Println(dat)
    // prints: map[num:6.13 strs:[a b]]

    // interface{} values require use of a type assertion, cast not allowed
    num := dat["num"].(float64)
    fmt.Println(num)

    strs := dat["strs"].([]interface{})
    str1 := strs[0].(string)
    fmt.Println(str1)

    str := `{"page": 1, "fruits": ["apple", "peach"]}`
    res := response2{}
    json.Unmarshal([]byte(str), &res)
    fmt.Println(res)
    fmt.Println(res.Fruits[0])
```

**JSON encoder (write straight to output writer)**

```golang

func jsontest(w http.ResponseWriter, req *http.Request) {
    w.Header().Set("Content-Type", "application/json")
    enc := json.NewEncoder(w)
    d := map[string]int{"apple": 5, "lettuce": 7}
    enc.Encode(d)
}


```

# Object Introspection

**Get Type**

```golang

import "reflect"
type MyStruct struct {
 // ...
}
mst := reflect.TypeOf(mystruct) // TypeOf returns reflect.Type
// mst.Name() == "MyStruct"
```

**Get Methods**

```golang
type Foo struct {
	Prop string
}

func (f Foo) Bar() string {
	return f.Prop
}

func (f Foo) Baz() {
}

func main() {
	fooType := reflect.TypeOf(Foo{})
	for i := 0; i < fooType.NumMethod(); i++ {
		method := fooType.Method(i)
		fmt.Println(method.Name)
	}
}
// prints:
// Bar
// Baz
```

# Date/Time
```golang
import "time"

// Get the current time and timezone
currentTime := time.Now()

// Format the current time as a datetime string with timezone
datetimeString := currentTime.Format("2006-01-02 15:04:05.999 -0700 MST")
fmt.Printf("Current Datetime %s\n", datetimeString)

// Parse the datetime string to create a datetime object
datetimeObj, err := time.Parse("2006-01-02 15:04:05.999 -0700 MST", datetimeString)
if err != nil {
    fmt.Println("Error parsing datetime:", err)
    return
}

// Add 2 hours 25 mins to the datetime object
updatedDatetime := datetimeObj.Add(2*time.Hour + 25*time.Minute)
fmt.Printf("Updated Datetime %s\n", updatedDatetime.Format("2006-01-02 15:04:05.999 -0700 MST"))

// Convert the datetime object to Unix Epoch timestamp (unsigned int)
unixTimestamp := updatedDatetime.Unix()
fmt.Printf("Unix Epoch Timestamp: %d\n", unixTimestamp)

```

# Sleep
```golang
import "time"
time.Sleep(800 * time.Millisecond)

// if you want a dynamic time value:
n := 12
time.Sleep(time.Duration(200*n) * time.Millisecond)
```

# Multithreading

Go uses goroutines and channels for concurrency

A goroutine is a function that is capable of running concurrently with other functions. To create a goroutine we use the keyword `go` followed by a function invocation

## Threads
```golang
func f(n int) {
    for i := 0; i < 10; i++ {
        fmt.Println(n, ":", i)
	time.Sleep(time.Duration(200*n) * time.Millisecond)
    }
}

func main() {
    go f(1)
    go f(2)
    var input string
    fmt.Scanln(&input) // pause to wait for thread completion
}
```

## Synchronization/Locking Primitives

Go uses channels to send/receive data between goroutines.

Go's philosphy is: "Do not communicate by sharing memory; instead, share memory by communicating."

Instead of locking and sharing a variable, communicate the result between goroutines using channels so you don't have to access shared memory.

A channel is bi-directional, so you can send/receive from it

```golang
// channels are synchronised, so pinger() and ponger() will block until printer() is ready
func pinger(c chan string) {
	for i := 0; ; i++ {
		msg := fmt.Sprintf("ping-%d",i)
		c <- msg // <- means send via c
	}
}

func ponger(c chan string) {
	for i := 0; ; i++ {
		msg := fmt.Sprintf("pong-%d",i)
		c <- msg // <- means send via c
	}
}

func printer(c chan string) {
	for {
		msg := <- c // recieve from c
		fmt.Println(msg)
		time.Sleep(time.Second * 1)
	}
}

func main() {
	var c chan string = make(chan string)
	go pinger(c)
	go ponger(c)
	go printer(c)
	var input string
	fmt.Scanln(&input)
}

```

**Wait Groups**

The canonical way to wait for goroutine completion is using sync.WaitGroup.

```golang
import "sync"

// ...other function defs...

func printer(c chan string, wg *sync.WaitGroup) {
	defer wg.Done()
	for {
		msg := <- c // recieve from c
		fmt.Println("got: ",msg)
		if msg == "ping-20" { break }
		time.Sleep(time.Second * 1)
	}
}

func main() {
	var c chan string = make(chan string, 5)
	var wg sync.WaitGroup

	go pinger(c)
	go ponger(c)

	wg.Add(1)
	go printer(c, &wg)

	wg.Wait()
}
```


**Buffered Channels (For Async Message Delivery)**

It's also possible to pass a second parameter to the make function when creating a channel:

`c := make(chan int, 100)`

This creates a buffered channel with a capacity of 100. Normally channels are synchronous; both sides of the channel will wait until the other side is ready. A buffered channel is asynchronous; sending or receiving a message will not wait unless the channel is already full.

**Passing Structs to Channels**

Pass values, and avoid passing pointers between channels and goroutines

If you pass a pointer, you need to implement mutex locks around the object to prevent RW panics.

**Mutexes**

If you really have to share a global variable between goroutines, there is a mutex:

```golang
import "sync"

type SharedThing struct {
	sync.RWMutex // embed the mutex
	x int
}

func (s *SharedThing) Get() int {
	s.RLock()
	s.RUnlock()
	return s.x
}

func (s *SharedThing) Set(me int) {
	s.Lock()
	s.x = me
	s.Unlock()
}

var st = &SharedThing{}

func main() {
	st.Set(2)
	fmt.Println(st.Get())
}
```
