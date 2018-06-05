# Kotlin Extension Methods
###### *05 June 2018*

![Kotlin](https://logos-download.com/wp-content/uploads/2016/10/Kotlin_logo_wordmark.png)

[Kotlin](https://kotlinlang.org/) is a great language, and one of its killer features is [extensions](https://kotlinlang.org/docs/reference/extensions.html).

The documentation stresses that [extensions are resolved statically](https://kotlinlang.org/docs/reference/extensions.html#extensions-are-resolved-statically). 
I recently came across an interesting situation that appears to be a side effect of this fact.

Here's my contrived, StackOverflow-appropriate example:
```kotlin

// extremely useful extension method
fun Int.nop() = this

fun main(args: Array<String>) {
    val x = 4

    println(x.nop())
}
```

I define an extension method on *Int* that simply returns the Int itself, and print the result of calling that method.
As you'd expect, the output is:
```shell
4
```

However, something interesting happens when trying to use a [reference](https://kotlinlang.org/docs/reference/reflection.html#function-references) to this extremely useful extension method:
```kotlin
// extremely useful extension method
fun Int.nop() = this

fun main(args: Array<String>) {
    val x = 4
    val nopRef = Int::nop
    println(x.nopRef())
}
```

This causes a compiler error of the form:
```shell
Extensions.kt:11:15: error: unresolved reference: nopRef
    println(x.nopRef())
              ^
```

After a bout of head-to-desk interaction and referencing the aforementioned documentation concerning static resolution, I found that using the following variation compiled just fine and produced the same ```4``` we saw earlier.
```kotlin
// extremely useful extension method
fun Int.nop() = this

fun main(args: Array<String>) {
    val x = 4
    val nopRef = Int::nop

    // comiler error
    // println(x.nopRef())

    // works
    println(nopRef(x))
}
```

The extension method seems to lose its extension-ness™, but otherwise [functions](https://www.youtube.com/watch?v=7uW47jWLMiY) the same. 
I'm still not sure what exactly causes this behavior, or if it's something that can be fixed by the Kotlin developers.

I'm also not sure how to conclude this post with the right mix of summary, brevity, and wit.
The end.

