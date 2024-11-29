---
title: "Function Properties in Data Classes are Code Smells"
date: 2024-11-29T15:34:00+01:00
draft: true
toc: false
images:
- /logo.png
categories:
- software development
tags:
  - android
  - kotlin
  - kmp
---

To me, using functions as properties in the primary constructor of a data class is a **code smell**. Here's why:

- **Data classes represent data.** Data is a value. Data is never executed.
- **Functions are not data.** They produce values when executed.

A **function** returns a value. A **procedure** runs a command. In either case, these are behaviors, not data.  

### Why It Matters

Kotlin generates key methods for data classes based on the properties in the primary constructor, such as:

- `equals()` and `hashCode()` for comparing instances.
- `toString()` for readable output like `"Person(name=John Doe, age=30)"`.

When you add functions as properties, you breaks the `data class` contract of holding only data, and these generated methods will misbehave.

### Example: Data vs. Functions

#### Expected Behavior: Data Class with Data.

```
data class Person(
  val name: String,
  val surname: String,
  val age: Int,
)

val p1 = Person("John", "Doe", 30)
val p2 = Person("John", "Doe", 30)
println(p1 == p2) // true
println(p1.hashCode() == p2.hashCode()) // true
println(p1.toString()) // Person(name=John, surname=Doe, age=30)
```

#### Unexpected Behavior: Data Class with Functions

```
data class UiState(
  val people: List<Person>,
  val eventSink: (UiEvent) -> Unit,
)
sealed class UiEvent

val s1 = UiState(listOf(), eventSink = {})
val s2 = UiState(listOf(), eventSink = {})
println(s1 == s2) // false
println(s1.hashCode() == s2.hashCode()) // false
println(s1.toString()) // UiState(people=[], eventSink=Function1<UiEvent, kotlin.Unit>)
```

You can find the example above at [this link](https://pl.kotl.in/pZmKMjxCy).

### Issues in Detail

1. **Equality (`equals`)**: Functions are never equal, even if they are duplicates of each other. 
2. **Hash Codes (`hashCode`)**: Different instances with same data generates different hashes.
3. **String Representation (`toString`)**: Functions produce generic output, leading to misleading comparisons.

For example, using an assertion library? You might encounter:

> "(non-equal value with same string representation)"

This happens because function properties can't be meaningfully compared or represented, causing `equals` and `toString` to behave unpredictably.

### The Solution

**Use data classes only for data.** If you need to include functions or behaviors like callbacks:

- Use a regular class.
- Override `equals()`, `hashCode()`, and `toString()` manually.

### References

- [Kotlin Data Classes Documentation](https://kotlinlang.org/docs/data-classes.html)
- [A Curious Case of Mistaken Identity](https://blog.mmckenna.me/a-curious-case-of-mistaken-identity)
