# Ghost Lang 👻

A friendly little language for you and me.

## Motivation

Ghost is the language I wish we had.

There are so many newer programming languages that have brilliant ideas inside
them, but I think they have failed to make them _feel_ familiar to programmers.

Ghost steals as much of its design as possible from other programming
languages. If someone else did it really well, why do it any differently? As
such most of Ghost won't _feel_ new. If you've used some of these languages
before you will probably recognize features of Ghost.

There is no compiler or any tools for Ghost right now, I hope to someday work
on some. But for now, this is just an exercise in programming language design.

## Syntax

Ghost's syntax should feel light. There are no semicons, comments are a single
character (`#`), syntax avoid too many characters.

But it also avoids having "optional" syntax like optional parenthesis or curly
braces. A lot of this encourages a more "vertical" coding style.

Places where syntax could go multiple different ways, I've just picked what
seems to be most popular. That way it should feel familiar to lots of
developers.

### Comments

```coffee
# inline comments only
# combine to make multiline
```

### Declarations

```coffee
let name = expression
```

### Booleans

```coffee
let positive = true
let negative = false
```

### Strings

```coffee
let plain = "hello world"
let interpolated = "hello {target}"
let escapes = "hello \"world\""

let multiline =
  """
  The quick {color} fox           # comment
  {action} over the lazy dog     \# not comment
  """
let multilineWithNewlines =
  """
  This is the first line
  This is the second line\
  This is still the second line
  """
let multilineWithIndentation =
  """
  This is indented by 0 spaces.
    This is indented by 2 spaces.
      This is indented by 4 spaces.
  """
```

### Integers

```coffee
let integer = 42
let negative = -42
let hex = 0x2A
let octal = 0o52
let binary = 0b101010
let separators = 42_000 # may only be between any two digits
let byte = b'a' # u8 only
```

### Floats

```coffee
let float = 4.2 # `.` must be between digits (ex: 0.42 or 42.0)
let negative = -4.2
let separators = 4_000.000_002 # may only be between any two digits
```

### Regex

```coffee
let regex = /^one|^two|^three|^orfour/i # equivalent to below
let regexMultiline = ///
  ^ one   |  # all whitespace
  ^ two   |  # and comments
  ^ three |  # will be ignored
  ^ or four  # to make it readable
///i
```

### Structs

```coffee
struct Rect {
  x: Int32
  y: Int32
  width: Int32
  height: Int32
}

let rect = Rect {
  x: 10,
  y: 20,
  width: 1
}
```

### Enums

```coffee
enum Actions {
  Reset()
  Increment(amount: Int32 = 1)
  Decrement(amount: Int32 = 1)
}

let reset = Actions.Reset()
let increment = Actions.Increment()
let increment2 = Actions.Increment(2)
let decrement3 = Actions.Decrement(amount: 3)
```

### Operators

```coffee
# Arithmetic
let addition = a + b
let subtraction = a - b
let division = a / b
let multiplication = a * b
let remainder = a % b
let exponentiation = a ** b
let negation = -a

# Comparison
let equality = a == b
let referentialEquality = a === b

let lessThan = a < b
let lessThan2 = a < b < c # equivalent to `a < b && b < c`, except `b` is only evaluated once
let lte = a <= b
let lte2 = a <= b <= c
let lessThan3 = a <= b < c

let greaterThan = a > b
let greaterThan2 = a > b > c
let gte = a >= b
let gte2 = a >= b >= c
let greaterThan3 = a >= b > c

# Logical
let and = expr && expr
let or = expr || expr
let not = !expr

# Grouping
let either = (a && b) || (c && d)
```

### If-Else

```js
if condition {
  doSomethingIfTruthy()
}

if condition {
  doSomethingIfTruthy()
} else {
  doSomethingElse()
}

if condition {
  doSomethingIfTruthy()
} else if condition2 {
  doSomethingElseIf2Truthy()
} else {
  doSomethingElse()
}

let result = if n == 0 {
  "none"
} else if n == 1 {
  "one"
} else {
  "many"
}
```

### Is

```coffee
if value is true {}
if value is false {}
```

### Match

```coffee
let result = match value {
  is true { "true" }
  is false { "false" }
  else { "other" }
}
```

### For-As

```coffee
for iterable as item {
  if shouldBeSkipped(item) { continue }
  if shouldEndTheLoop(item) { break }
  doSomething()
}
```

### While

```coffee
while condition {
  onlyRunsIfConditionIsTrue()
  repeatsAsLongAsItRemainsTrue()
}
```

### Loop

```coffee
loop {
  if condition1 { break }
  if condition2 { continue }
  doSomethingInfinitelyUntilBreak()
}
```

### Functions

```coffee
fn add(a: Int32, b: Int32) {
  a + b
}
fn subtract(a: Int32, b: Int32) {
  let minuend = a
  let subtrahend = b
  minuend - subtrahend
}

let result = add(400, subtract(42, 22))
```

### Named Params

```coffee
fn divide(dividend: Int32, divisor: Int32) {
  dividend / divisor
}

# All of these are equivalent:
divide(400, 20)
divide(dividend: 400, divisor: 20)
divide(divisor: 20, dividend: 400)
divide(dividend: 400, 20)
divide(400, divisor: 20)
divide(divisor: 20, 400)

# Compiler Errors: Cannot pass multiple values to the same parameter
divide(dividend: 20, dividend: 400)
divide(20, dividend: 400)
```

### Iterable Functions

```coffee
fn doubles(iter): Iter<Int32> {
  for iter as value {
    yield value * 2
  }
}

fn invalid() {
  yield 42 # Error
}
```

### Blocks

```coffee
let value = {
  let a = 42
  let b = 10
  a * b
}
```

### Pipeline

```coffee
let results =
  | books
  | Iter.filter(^^, fn (book) {
      book.popularity > 0.8
    })
  | Iter.map(^^, fn (book) { Http.request(.get, book.url) })

# Equivalent code without pipelines:
let filtered = Iter.filter(books, fn (book) { book.popularity > 0.8 })
let results = Iter.map(filtered, fn (book) { Http.request(.get, book.url) })
```

### Junk

```coffee
let _ = "ignore me"
let [_, _, three] = [1, 2, 3]
let {a as _, b as _, ...rest as _} = { a = 1, b = 2, c = 3, d = 4 }

doSomething(fn (_, _, value) {
  value
})

log(_)
# SyntaxError: "Junk" bindings (_) are not valid
# identifiers and cannot be referenced. Give your
# binding a name instead.
```

### Elements (GSX)

```coffee
let view = OtherComponent(prop, bar: true) {
  let target = "world"
  Text("hello {target}")
  List {
    for items as item {
      Text(item.name)
    }
  }
}
```

### Use

```coffee
use std/time
```

### Pub

```coffee
pub let add = fn (a, b) { a + b }
pub let PI = 3.14
```

## Type Syntax

### Type Alias

```coffee
type MyType = Bool
```

### Primitive Types

```coffee
type T = Empty
type T = Never

type T = Boolean

type T = Int8
type T = Int16
type T = Int32
type T = Int64
type T = Int128
type T = IntSize

type T = Uint8
type T = Uint16
type T = Uint32
type T = Uint64
type T = Uint128
type T = USize

type T = Float32
type T = Float64
```

### Maybe Types

```coffee
type T = Maybe<Boolean>
```

### Result Types

```coffee
type T = Result<Boolean>
```

### Function Types

```coffee
type T = fn (param: String): USize
type T = fn (param: String): Iter<USize> 
```

### Generic Types

```coffee
type T<T> = Option<T>
type T = fn <T>(param: T): T
```

### Unions

```coffee
type MyType = Int8 | Int16 | Int32
type MyType =
  | Int8
  | Int16
  | Int32
```

## Types in Syntax

### Variables

```coffee
let value: Boolean = true
let value: Boolean | String = "cool"
```

### Functions

```coffee
let fn = fn (param1: String, param2: Number) {
  # ...
}

let fn = fn (...rest: Array<String>) {
  # ...
}

let fn = fn (): String {
  "nice"
}

let fn = fn (): Iter<String> iter {
  yield "good"
}
```
