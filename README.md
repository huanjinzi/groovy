# Groovy

## Comments
single line comment
```groovy
// single line comment
```

multi-line comment
```groovy
/*
multi-line comment
*/
```

document comment
```groovy
/**
 * document comment.
 */
```

## Shebang line
need `groovy` in `PATH`.
```groovy
#!/usr/bin/env groovy
println "Hello from the shebang line"
```

## Quoted identifiers
```groovy
def map = [:]

map."double quotes" = "ALLOWED"
map.'single quotes' = "ALLOWED"

assert map."double quotes" == "ALLOWED"
assert map.'single quotes' == "ALLOWED"
```

## Double quoted string

Double quoted strings are a series of characters surrounded by double quotes:
```groovy
"a double quoted string"
```
>Double quoted strings are plain `java.lang.String` if there’s no interpolated expression, but are `groovy.lang.GString` instances if interpolation is present.

>To escape a double quote, you can use the backslash character: "A double quote: \"".

## Triple single quoted string

Triple single quoted strings are a series of characters surrounded by triplets of single quotes:
```groovy
'''a triple single quoted string'''
```
>Triple single quoted strings are plain `java.lang.String` and don’t support interpolation.

## String interpolation
```groovy
def first_name = 'yuan'
def last_name = 'xx'

def full_name = '${first_name}-${last_name}'
```

## Numbers
```groovy
// primitive types
byte  b = 1
char  c = 2
short s = 3
int   i = 4
long  l = 5

// infinite precision
BigInteger bi =  6

def a = 1
assert a instanceof Integer
```

## Boolean
Boolean is a special data type that is used to represent truth values: true and false.
Use this data type for simple flags that track true/false conditions.
```groovy
def myBooleanVariable = true
boolean untypedBooleanVar = false
booleanField = true
```

## Lists
Groovy lists are plain JDK `java.util.List`, as Groovy doesn’t define its own collection classes.
The concrete list implementation used when defining list literals are `java.util.ArrayList` by
default, unless you decide to specify otherwise.

```groovy
def numbers = [1, 2, 3]

assert numbers instanceof List
assert numbers.size() == 3
```

## Arrays
```groovy
def numArr = [1, 2, 3] as int[]
assert numArr instanceof int[]
```

## Map
Groovy creates maps that are actually instances of `java.util.LinkedHashMap`.
```groovy
def colors = [red: '#FF0000', green: '#00FF00', blue: '#0000FF']
assert colors instanceof java.util.LinkedHashMap
```

## Arithmetic operators
```groovy
** power
```

## Methods
```groovy
def someMethod() { 'method called' }
String anotherMethod() { 'another method called' }
def thirdMethod(param1) { "$param1 passed" }
static String fourthMethod(String param1) { "$param1 passed" }
```

## Closure
A closure definition follows this syntax:
```groovy
{ [closureParameters -> ] statements }
```

### Closures as an object
```groovy
Closure<Boolean> isTextFile = {
    File it -> it.name.endsWith('.txt')
}
```

### Calling a closure
```groovy
def code = { 123 }

assert code() == 123
assert code.call() == 123
```

### Implicit parameter
```groovy
def greeting = { "Hello, $it!" }
assert greeting('Patrick') == 'Hello, Patrick!
```

### Varargs
```groovy
def concat1 = { String... args -> args.join('') }
assert concat1('abc','def') == 'abcdef'
```

### Owner, delegate and this


To understand the concept of delegate, we must first explain the meaning of this inside a closure.
A closure actually defines 3 distinct things:

* `this` corresponds to the enclosing class where the closure is defined

* `owner` corresponds to the enclosing object where the closure is defined, which may be either a class or a closure

* `delegate` corresponds to a third party object where methods calls or properties are resolved whenever the receiver of the message is not defined

By default, the `delegate` is set to `owner`.

### Change delegate
```groovy
class Person {
    String name
}
class Thing {
    String name
}

def p = new Person(name: 'Norman')
def t = new Thing(name: 'Teapot')

def upperCasedName = { delegate.name.toUpperCase() }

upperCasedName.delegate = p
assert upperCasedName() == 'NORMAN'
upperCasedName.delegate = t
assert upperCasedName() == 'TEAPOT'
```

At this point, the behavior is not different from having a target variable defined in the lexical scope of the closure:
```groovy
def target = p
def upperCasedNameUsingVar = { target.name.toUpperCase() }
assert upperCasedNameUsingVar() == 'NORMAN'
```
However, there are major differences:

* in the last example, target is a local variable referenced from within the closure

* the delegate can be used transparently, that is to say without prefixing method calls with
delegate. as explained in the next paragraph.


