---
marp: true
theme: default
---

# Kotlin Feature Highlights

---

# Feature Highlights

* Null safety
* Data Classes
* Functional Programming
* Interop with Java

---

# Null Safety - Nullable types and Non-Null types

```kotlin
var a: String = "abc"
a = null // compilation error

var b: String? = "abc" // nullable type
b = null // ok

val l = a.length // guaranteed not to cause an NPE

val l = b.length // compilation error: variable 'b' can be null

if (b != null) {
  val l = b.length // ok, since compiler knows there's a null check
}
```

---

# Null Safety - Safe Calls

```kotlin
// Kotlin safe calls
val deptHead = bob?.department?.head?.name

// How you might do it in Java
if (bob != null &&
    bob.department != null &&
    bob.department.head != null) {
      var deptHead = bob.department.head.name;
  }
}

// Elvis Operator
val deptHead = bob?.department?.head?.name ?: "<headless>"
```

---

# Data Classes

```
data class User(val name: String, val age: Int)
```

### generated functions
* `equals()`/`hashCode()` pair;
* `toString()` of the form `"User(name=John, age=42)"`;
* `componentN()` functions corresponding to the properties;
```
val jane = User("Jane", 35)
val (name, age) = jane
println("$name, $age years of age") // prints "Jane, 35 years of age"
```
* `copy()` function
```
val jack = User(name = "Jack", age = 1)
val olderJack = jack.copy(age = 2)
```

---

# Functional Programming

```
// function types and inline functions
val square: (Int) -> Int = { i -> i * i }

// passing functions as parameters
val arr = arrayOf(1, 2, 3)
val out = arr.map(square) // [1, 4, 9]

// higher-order functions
fun twice(f: (Int) -> Int): (Int) -> Int = {i -> f(f(i))}

val squaresquare = twice(square)
squaresquare(3) // 81

// trailing lambdas
arr.filter { i -> i%2 == 1 }  // omit parentheses
arr.filter { it%2 == 1 }  // implicit single param 'it'
```

---

# Interop with Java

---

# Other stuff

* Destructuring Declarations
* Named Arguments
* Extension Functions
