---
marp: true
theme: default
---

# *Always* use **Kotlin** instead of Java

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

# Interop with Java - just import and use

```
import java.util.*

fun demo(source: List<Int>) {
    val list = ArrayList<Int>()
    // 'for'-loops work for Java collections:
    for (item in source) {
        list.add(item)
    }
    // Operator conventions work as well:
    for (i in 0..source.size - 1) {
        list[i] = source[i] // get and set are called
    }
}
```

---

# Interop with Java - getters and setters

```
import java.util.Calendar

fun calendarDemo() {
    val calendar = Calendar.getInstance()
    if (calendar.firstDayOfWeek == Calendar.SUNDAY) {  // call getFirstDayOfWeek()
        calendar.firstDayOfWeek = Calendar.MONDAY      // call setFirstDayOfWeek()
    }
    if (!calendar.isLenient) {                         // call isLenient()
        calendar.isLenient = true                      // call setLenient()
    }
}
```

---

# Interop with Java - calling Kotlin from Java

```
var firstName: String
```
gets compiled to the following Java declarations:
```
private String firstName;

public String getFirstName() {
    return firstName;
}

public void setFirstName(String firstName) {
    this.firstName = firstName;
}
```

---


# Other stuff

* Destructuring Declarations
```
val jane = User("Jane", 35)
val (name, age) = jane
```
* Named Arguments and Default Values
```
val jack = User(name = "Jack", age = 1)
```
* Extension Functions
```
fun String.singlishify() {
    // implementation
}
```
* No Checked Exceptions

---

# Other interesting reads

* How Kotlin defaults support the ideas in "Effective Java" - https://www.lukle.at/blog/2016/12/how-effective-java-influenced-the-design-of-kotlin-part-1/
* How to use Kotlin with Spark - https://tomstechnicalblog.blogspot.com/2016/11/using-kotlin-language-with-spark.html
