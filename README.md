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

### Null

```coffee
let missing = null
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

### Numbers

```coffee
let float = 4.14
let float32 = 4.14f
let decimal = 0.15d
let bigint = 9999i

let largeNumber = 98_521_391_124i
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

### Ranges

```coffee
let range = 0..3                   # 0,1,2
let rangeInclusive = 0...3         # 0,1,2,3
let rangeExpr = 0..num             # 0 to whatever `num` is
let reverseWithNegatives = 2..-3   # 2,1,0,-1,-2
```

### Properties

```coffee
let property1 = .name # properties are global
let property2 = .name # both of these are equal
```

### Lists

```coffee
let list = [1, 2, 3]
let trailingCommas = [
  1,
  2,
  3,
]

let combineList = [1, ..list, 3] # fast
let combineIter = [1, ...iter, 3] # slow

let getterIndex = list[3]
let getterRange = list[0..3]
```

### Arrays

```coffee
let array = Array [1, 2, 3]
let trailingCommas = Array [
  1,
  2,
  3,
]

let getterIndex = array[3]
let getterIndexNegative = array[-3]
let getterRange = array[0..3]
```

### Sets

```coffee
let set = Set [
  "one",
  "two",
  "three",
]
```

### Records

```coffee
let three = .three

let record = [
  one = 1,
  two = 2,
  [three] = 3,
}

let one = record.one
let two = record[.two]
let three = record[three]

let newRecord = {
  one = "default",
  ...record,
  three = "new value",
}
```

### Maps

```coffee
let map = Map {
  "key1" = "value1",
  "key2" = "value2",
  3.1415 = "π"
}
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

# Bitwise
let and = a & b
let or = a | b
let xor = a ^ b
let not = ~a
let leftShift = a << b
let signPropRightShift = a >> b
let zeroFillRightShift = a >>> b

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
if (condition) { doSomethingIfTruthy() }

if (condition) {
  doSomethingIfTruthy()
} else {
  doSomethingElse()
}

if (condition) {
  doSomethingIfTruthy()
} else if (condition2) {
  doSomethingElseIf2Truthy()
} else {
  doSomethingElse()
}

let result = if (n == 0) {
  "none"
} else if (n == 1) {
  "one"
} else {
  "many"
}
```

### Destructuring

```coffee
let [one, two, ...rest] = [1, 2, 3, 4]
# one = 1
# two = 2
# rest = [3, 4]

let [one, two, ...rest] = Array [1, 2, 3, 4]
# one = 1
# two = 2
# rest = [|3, 4|]

let {one, two, ...rest} = {one = 1, two = 2, three = 3, four = 4}
# one = 1
# two = 2
# rest = {three = 3, four = 4}

let {one as uno, two as dos, ...rest as resto} = {one = 1, two = 2, three = 3, four = 4}
# uno = 1
# dos = 2
# resto = {three = 3, four = 4}
```

### Is

```coffee
value is any
value is true # assert any static type
value is false
value is Number
value is Number.Float # return true or false if it matches
value is { prop: any }
value is { prop: Number }
```

### Match

```coffee
let result = match (value) {
  is true { "true" }
  is false { "false" }
  else { "other" }
}
```

### Throw

```coffee
throw Error("message")
throw Error("message", reason)
```

### Try-Catch-Finally

```coffee
let result = try {
  doSomething()
} catch (err is SpecialError) {
  doSomethingSpecial()
} catch (err) {
  doSomethingElse()
} finally {
  doSomethingAlways()
}
```

### Effects

```coffee
let doSomething = fn () {
  let myResult = effect "myEffect"
}

let result = try {
  doSomething()
} catch (myEffect is "myEffect") {
  resume "myResult"
}
```

```coffee
let inner = fn () {
  log("inner start")
  let result = effect "myEffect"
  log("inner end", result)
  result
}

let outer = fn () {
  log("outer start")
  let result = inner()
  log("outer end", result)
  result
}

let finalResult = try {
  log("try start")
  let result = outer()
  log("try end", result)
  result
} catch (effect is "myEffect") {
  log("catch start", effect)
  let result = resume "myEffectHandlerResult"
  log("catch end", result)
}

# log: try start
# log: outer start
# log: inner start
# log: catch start "myEffect"
# log: inner end "myEffectHandlerResult"
# log: outer end "myEffectHandlerResult"
# log: try end "myEffectHandlerResult"
# log: catch end "myEffectHandlerResult"

finalResult == "myEffectHandlerResult"
```

### For-As

```coffee
for (iterable as item) {
  if (item.shouldBeSkipped) { continue }
  if (item.shouldEndTheLoop) { break }
  doSomething()
}
```

### While

```coffee
while (condition) {
  onlyRunsIfConditionIsTrue()
  repeatsAsLongAsItRemainsTrue()
}
```

### Do-While

```coffee
do {
  runsOnceImmediatelyRegardlessIfConditionIsTrue()
  repeatsAsLongAsItRemainsTrue()
} while (condition)
```

### Loop

```coffee
loop {
  if (condition1) { break }
  if (condition2) { continue }
  doSomethingInfinitelyUntilBreak()
}
```

### Functions

```coffee
let add = fn (a, b) { a + b }
let add = fn (a, b) {
  let addend = a
  let augend = b
  a + b
}

let result = add(400, 20)
```

### Named Params

```coffee
let divide = fn (dividend, divisor) {
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
let doubles = fn (range) iter {
  for (range as index) {
    yield index * 2
  }
}
```

### Do

```coffee
let value = do {
  let a = 42
  let b = 10
  a * b
}
```

### Pipeline

```coffee
let results =
  |> books
  |> Iter.filter(^^, fn (book) { book.popularity > 0.8 })
  |> Iter.map(^^, fn (book) { Http.request(.get, book.url) })

# Equivalent code without pipelines:
let filtered = Iter.filter(books, fn (book) { book.popularity > 0.8 })
let results = Iter.map(filtered, fn (book) { Http.request(.get, book.url) })
```

### Use

```coffee
let append = fn (fileName, buffer) {
  let file = use File.open(fileName)
  File.append(file, buffer)
}

# Effectively:
let append = fn (fileName, buffer) {
  let file = File.open(fileName)
  try {
    let result = File.append(file, buffer)
  } finally {
    if (file) {
      file.dispose()
    }
  }
  result
}
```

```coffee
let append = fn (fileName, buffer) {
  let file = use File.open(fileName)
  File.append(file, buffer)
  file.dispose() # manually call
}
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
let MyComponent = fn (props) {
  let {prop, items} = props

  <OtherComponent { prop, bar: true }>
    let target = "world"
    <.h1>"hello {target}"</.h1>
    <.ul>
      for (items as item) {
        <.li { key: item.id }>item.name</.li>
      }
    </.ul>
  </OtherComponent>
}
```

### Blocks

```coffee
let fn = fn () {
  # block
}

for (iter as item) {
  # block
}

if (cond) {
  # block
} else if (cond) {
  # block
} else {
  # block
}

try {
  # block
} catch (err) {
  # block
} finally {
  # block
}
```

### Block Scoping

```coffee
let a = 1
if (cond) {
  let a = 2
  let b = 3
  log(a) # > 2
}
log(a) # > 1
log(b) # Error! There's no variable named "b" in this scope!
```

### Imports

```coffee
import Time
Time.Instant()

import Time as MyTime
MyTime.Instant()

import Math as { cos, PI }
cos(PI)

import ./utils/currency
currency.convert(42, .usd, .aud)

import ../lib/utils/i18n as { t }
t("Hello $1", "World")
```

### Exports

```coffee
export let add = fn (a, b) { a + b }
export let PI = 3.14
```

## Type Syntax

### Type Alias

```coffee
type MyType = type
```

### Basic Types

```coffee
type MyType = any
type MyType = null
type MyType = Boolean
type MyType = true
type MyType = false
type MyType = String
type MyType = "string"
type MyType = Number
type MyType = Number.Float
type MyType = Regex
type MyType = Range
type MyType = Property
type MyType = .property
type MyType = List<String>
type MyType = Array<Boolean>
type MyType = Map<Number, Regex>
type MyType = Set<Range>
type MyType = Iter<String>
```

### Record Types

```coffee
type MyType = { prop: String }
type MyType = { prop?: String }
type MyType = { ...OtherRecordType }
```

### Optional Types

```coffee
type MyType = Boolean?
type MyType = Boolean | null # same
```

### Function Types

```coffee
type MyType = fn (param: String): Number
type MyType = fn (param: String): Iter<Number>       # fn () iter {}
```

### Generics

```coffee
type MyType<T> = Array<T>
type MyType = fn <T> (param: T): T
```

### Unions

```coffee
type MyType = String | Boolean | Range
type MyType =
  | String
  | Boolean
  | Range
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
