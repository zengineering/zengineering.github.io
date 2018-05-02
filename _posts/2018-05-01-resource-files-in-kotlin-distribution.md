# Resource Files in a Kotlin Distribution

I'm working on a Kotlin project, and decided to include a data file in the project rather than supplying it as a command line argument. Having not packaged many Java projects before, I learned that the process is not terribly straightforward.

### Place the data file in *src/main/resources*
I'm using a Gradle build system, and it automagically takes care of packaging resource files in a jar if the files are placed here. I added subdirectories under *src/main/resources*, but that isn't strictly necessary.

### Retrieve the resource
I started with [```java.lang.Class.getResource```](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#getResource-java.lang.String-). 
This obviously requires a Class object, but I was using a top-level function in Kotlin. StackOverflow and the Kotlin forums were helpful in learning how to get the class for the current Kotlin file: 

```kotlin
private val topLevelClass = object: Any() {}.javaClass
```

This line defines an anonymous object and gets its class. Said object needs to be defined at the top-level of the file.
The ```private``` modifier is necessary to satisfy the compiler's encapsulation requirements; without it we get:

```
'public' property exposes its 'local' type argument <no name provided>
```


Now that we can ```getResource```, here we go: 

```kotlin
File(topLevelClass.getResource("/resource.txt").file).forEachLine {
    ...
}
```
Breaking this down:
- getResource(filename) accepts a String and returns a [```java.net.URL```](https://docs.oracle.com/javase/8/docs/api/java/net/URL.html). The string is the name of the resource file we are trying to retrieve, e.g. */resource.txt*. It's important to include the preceding "*/*", which says "look in the root directory of the resource path" (in our case, src/main/resources/). Alternatively you could specify */subdir/resource.txt*.
- ```java.lang.URL.getFile``` (Kotlin-ified to just ```.file``` here) ["Gets the file name of this URL"](https://docs.oracle.com/javase/8/docs/api/java/net/URL.html#getFile--) and returns a String.
- Pass the file name to the ```File``` constructor.
- Iterate through each line of the file

Unfortunately, this results in:

```
Exception in thread "main" java.io.FileNotFoundException: file:/<path to project>/build/install/<project name>/lib/<project name>.jar!/resource.txt (No such file or directory)
```

*Ok...*

I should note that I was running this from Gradle's [distribution](https://docs.gradle.org/current/userguide/distribution_plugin.html) install. I assumed that Gradle didn't package up the resources with the distribution jar. That theory went down with an equally unsuccessful ```./gradlew run```, and some digging that revealed the aforementioned automatic packing of resource files in *src/main/resources*.

I unzipped the installed jar to find... *src/main/resources/resource.txt* sitting there in all it's of glory.

With a mild amount of infuriation, I continued to scour the internet for the answer. I found a few instances of using [```java.lang.Class.getResourceAsStream```](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#getResourceAsStream-java.lang.String-) instead of ```java.lang.Class.getResource```. For a sanity check, I tried it out.

It worked. 

***WHAT IS HAPPENING?!***

It turns out that (as usual), compiler error messages are sometimes more helpful than they seem. Remember this?:

```
Exception in thread "main" java.io.FileNotFoundException: file:/<path to project>/build/install/<project name>/lib/<project name>.jar!/resource.txt (No such file or directory)
```

I assumed */.../lib/project_name.jar!/resource.txt* was just pretty-printing the fact that the file was inside a jar. Howver, since that was a String being passed to the contructor of ```File```, ```File``` takes it literally, and indeed there is no file at that path. In contrast, ```java.lang.Class.getResourceAsStream``` handles the file I/O itself rather than simply returning a URL to the file, thus avoiding my earlier string buffoonery.

I ended up with this gem:

```kotlin
topLevelClass.getResourceAsStream("resource.txt").bufferedReader().lineSequence().forEach {
    ...
}
```

This uses some [kotlin.io](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.io/index.html) extension methods to read the file line-by-line.

I'm sure there is some other fix for resolving the file path, but this works just fine. Hopefully my journey will save other jar-distribution-newbies some time and sanity.


