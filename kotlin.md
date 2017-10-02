# A Kotlin Krash Kourse - for java coders
An introduction to Kotlin from a Nodes Android App Development perspective.
This is by no means meant to be a complete reference to Kotlin. Instead
its a crash course to the various parts of Kotlin that are most relevant to Android developers.
And no, you cannot have a goddamn youtube video instead :)

## General coding
The brilliant leader and strategist we all aspire to be.

### Type inference
Kotlin as most modern languages (even C++11) implements type inference, which
basically means that your don't have to write out the type, in case it can be inferred.

The declaration and assignment:
```kotlin
val i : Integer = 10
```

Can be written as:
```kotlin
val i = 10 // i is of the type Int
val f = 10f // f is a Float
val d = 10.0 // d is a Double
val str = "Hi Per" // str is a string
```
Because the type of _i_ can be inferred based on the value of the assignment.

In this example the type can be inferred because the return type of myFancyCollection.iterator() is defined somewhere:

```kotlin
val i = myFancyCollection.iterator()
```

This feature is purely syntactic sugar but often leads to easier to read code (although it can also
hide information).

### Type Casting and Checking
Use as to cast between to different types (using type inference) casting an object of the type KoolKeith to DoctorOctagon:
```kotlin
val performer = KoolKeith as DoctorOctagon
performer.perform("blue flowers")
```
If the cast cannot be performed the a ClassCastException is thrown (and will crash your app if you do not handle it).

#### Safe casts
There is also a safe version of the cast operator as? unlike as it returns null instead of throwing an exception:
```kotlin
val aInt: Int? = a as? Int
// if a is not an Int aInt == null
```

####  Type checking (instanceof in java)
Use the _is_ operator to check the type of an object instance:
```kotlin
if(obj is String)
	doSomething()
	
if(obj !is String)
	doSomethingElse()
```

#### Number conversions
Unlike Java **DO NOT** use casts to convert between number formats, instead use the family of to*() functions defined
on all numeral types:

- toByte(): Byte
- toShort(): Short
- toInt(): Int
- toLong(): Long
- toFloat(): Float
- toDouble(): Double
- toChar(): Char

There is also .toAllOfTheAforementionedTypes() functions on the String class (but use string templates instead)

### Conditional assigment
In kotlin if statements can be written as the rvalue of an assigment:
```kotlin
val no = 6
var isBetweenZeroAndFive = if(no >= 0 && no <= 5) true else false
```

### Knowing When to switch - a more flexible condition multiplexer
Kotlin provides a more flexible alternative to java's switch called when:
```kotlin
when(str)
{
	"on" -> itsOn()
	"off" -> itsOff()
	else -> whoKnows()
}
```

### Ranges

## Data objects
Data objects are a convenience feature for writing entity classes (classes containing data.. mostly) hence you should use them.
Compared to regular classes data classes:

- Have a shorter and more convenient syntax
- Have a builtin toString() which unlike yours is always up to date.
- Have a built in copy() method, finally its possible to do a deep copy off an object without manually assigning members.
- Have a built in equals() that compare the actual values of the classes member variables instead of comparing the object reference (pointer).
- Doesn't give a crap about France

Syntax:
```kotlin
data class Person(var firstName : String, var lastName : String, var age : Int)

// with default values (and default values inferring the type)
data class Person(var firstName = "", var lastName "", var age = 0)

// Upon instantiating you need to pass values with no defaults as a minimum to the constructor, for the first example:
val person = Person("Nemma", "Nameson", 40)

// if the data class have default values only the ones without defaults are required, eg for the second example:
val person = Person("Nemma", "Nameson")
```


## A Tale of Nullity and her sisters Mutability and Immutability
Kotlin and other modern languages are based heavily around the concept of declaring mutability and specifying _nullability_
up front.
What that means is that when you declare your variables you have to specify whether they are ever gonna be changed (mutability) and whether they're allowed to be/contain null. For other and in my opinion failed attempts at this, try googling "c++ const correctness" :D

### Mutability
When you declare a variable in kotlin you specify mutability:

#### Immutable declaration:
```kotlin
val i = 10 // I can never be changed
i = 5 // won't compile
```

#### mutable declaration
```kotlin
var j = 1
j = 5 // will compile just fine and dandy (specially dandy)
```

### Nullability
It sounds stupid, but it is a great way of reducing null pointer exceptions due to programmer error.
We all forget to check our nulls once in a while :)

Since we cannot easily do away with the whole null concept, modern languages instead takes the sensible
approach of requiring the programmer, to specify nullability at declaration:

#### Normal reference
```kotlin
// a string:
var nullAbleRef : String = "Go weekend!!"
// this cannot be set to null, the following line will not compile:
nullAbleRef = null
```

#### Nullable reference
```kotlin
// Today the gang is declaring a nullable string:
var nullAbleRef : String? = null
// this can be set to either null or a string:
nullAbleRef = "Go weekend!!"
nullAbleRef = null
```

### Safe call operator - A safe voice in an endless void*
The safe call operator ? wraps call in a invisible if statement:
```kotlin
var view : Activity? = null
view?.showHelloJpg() // will thankfully never get called
```
Is functionally equivalent to writing:
```kotlin
var view : Activity? = null
if(view != null)
	view?.showHelloJpg()
```

What is even better is that you can use it all the way along a call chain of nullable references.
For instance the object Person has a nullable reference to Uncle which in turns hold a nullable refence
to Cousin. Now you wan't to get the cousins name provided that both he and the uncle actually it exists (this might
not be the case if their object references are null):

```kotlin
val cousinName : String? = person.uncle?.cousin?.name
```
In the above example person is the only reference that cannot be null

### Elvis operator
The elvis operator could be - less colorfully - renamed to the IfNullThen operator. Basically if the result of any statement
on the left hand side of the operator (the lvalue) is null, whatever is on the right hand side of the operator (the rvalue) is substituted/used instead:

```kotlin
var name : String? = null
val greeting = "Hello ${name ?: "world"}
```
String contains "Hello world" if name is null, otherwise it will contain the value of name.

Safe call operator is often used in combination with the elvis operator, a practice known as "safe calling Elvis" ?:p
```kotlin
// Safe calling Elvis
val cousinName : String = person.uncle?.cousin?.name ?: "" 
```
In this case cousinName isn't nullable, if we do not use the elvis operator to specify an alternative (in this case an empty string)
the statement won't compile.

### The !! operator (unsafecall?)

### Fashionable lateinit

## Listeners and Callbacks and starships named Enterprise
In java listeners and callbacks are generally implemented as interfaces (or abstract base classes).
Often anonymous class instantiations are used to write the code "in place", instead of having
to declare a separate class.

Example of the listener pattern in java:
```java
view.setOnClickListener(new View.OnClickListener() {
	@Override
	public void onClick(View v) {
		// do something in response to button clicks
	}
});
```

### Object Expressions

Because most apps - which are event driven - do this a lot Kotlin has a special syntax for this called
object expression. 

Rewriting the above code in kotlin using a object expression:
```kotlin
view.setOnClickListener(object : View.OnClickListener {
	override fun onClick(v: View?) {
		// do something
	}
})
```
object expressions are executed (and initialized) immediately, where they are used.

### Using Lambda's for callbacks instead of listeners
Instead of using a listener object expression we can use lambda notation and
declare and pass a anonymous function directly:

```kotlin
view.setOnClickListener { view: View? -> doSomething() }
```
In the above example the function could be defined as:

```kotlin
(View?) -> Unit
```
Which is a function that takes a View? (nullable) and returns void (Unit).
This is a valid type and be used everywhere like a regular old Int:

```kotlin
class MyClass {
	// this function takes a boolean as an argument and returns an integer
    var myCallbackFunction : (success: Boolean) -> Int
}

// Declare and assign a function to the variable, This implementation calls doSomething() and then return 10
instanceOfMyclass.myCallbackFunction = {success: Boolean ->
	doSomething()
	10
}

// call function
val myInteger = instanceOfMyClass.myCallbackFunction(true)
// myInteger is now 10
```

As usual we can shorten the above with type inference. In the above example we already declared success
as a Boolean, hence that can be inferred with making the assignment:

```kotlin
instanceOfMyclass.myCallbackFunction = {success ->
	doSomething()
	10
}
```

The above lambda's can be written shorter. If you don't need the function parameters, you don't need to specify them.
For instance if you don't care about the view parameter on View.OnClickListener, simply leave it out:

```kotlin
view.setOnClickListener { doSomething() }
```
## Contemplating String Templates
Use string templates, they are awesome :). You basically use them like this:

```kotlin
val no = 10
val str = "$no good reasons to use string templates ;)"
```

If you need to refer to an object member or even do a calculation ${} syntax allows you to evaluate the expression inside:
```kotlin
val person = Person("Nemma", "Nameson", 40)
val str = "The person ${person.firstName) ${person.lastName) is ${person.age} years old"
```

This can be combined with the safe call and the elvis operator, so you can null check and provide an alternative all inside the string template:
```kotlin
val person : Person? = null
val str = "The person ${person?.firstName ?: "Unknown") ${person?.lastName ?: "Person") is ${person?.age :? "some"} years old"
```
If person happens to be null (and the person reference is nullable either specified or inferred) the string will contain:

"The person Unknown Person is some years old"

### Combining with String.format()
The power of the format() is still available in kotlin:
```kotlin
val dollars = 198.50
val str = "Ole has ${String.format(Locale.getDefault(), "%.2f", dollars)} russerdollars!!"
```
**Output**: "Ole has 198.50 russerdollars!!" (depending on your devices current locale the decimal separator might be a comma)

And if dollars is nullable:
```kotlin
val str = "Ole has ${String.format(Locale.getDefault(), "%.2f", dollars ?: 0.0)} russerdollars!!"
```
**Output**: "Ole has 0.00 russerdollars!!"
**Do not use nullable types unless you really have to or are getting object references from calling java code**

## Typealias - one type many names
typealias is a keyword that works a bit like _typedef_ from the C language family (a powerful keyword from a more civilized age). It basically allows you
to substitute/alias a type name for another like this:

```kotlin
typealias MyStringList = List<String>
```

With the above typealias you could write the follow code:

```kotlin
val stringList : MyStringList = ArrayList()
```

Instead of:
```kotlin
val stringList : List<String> = ArrayList()
```

A better example of its usage would be define name for lambda definitions, so if we revisit the earlier example
it could be written like this:

```kotlin
typealias CallbackFunction = (success: Boolean) -> Int

class MyClass {
	// this function takes a boolean as an argument and returns an integer
    var myCallbackFunction : CallbackFunction 
}
```
Now CallbackFunction can be used instead of typing out: (success: Boolean) -> Int, much more readable. 

## Singletons and static members - Open from 10PM
Singletons are basically a way of allowing only one instance of a class. Because this is such an often used
pattern Kotlin supports it directly using Object Declaration:

```kotlin
object MySingleton
{
	fun myFunction() {
		...
	}
}
```
Objects can have supertypes but no constructors (as its always constructed for you).
Object declarations are initialized lazily, when accessed for the first time.

**Remember singletons often lead to higher coupling, which in turn leads to the dark side.**

### Using objects from Java
Accessing object members from java uses a special syntax.
Calling myFunction in the object declaration example above looks like this in java:

```java
MySingleton.INSTANCE.myFunction()
```

### Static members 
Kotlin does not have a static keyword like java (same thing can be achieved with the **@JvmStatic** annotation though, since it is still supported on the JVM level ofc).
Instead it has a construct called a companion object. Easy way to think of it is that everything inside the companion
object is shared between all instances of the class.

Example of companion object:
```kotlin
class MyClass {
    companion object Factory {
        fun create(): MyClass = MyClass()
    }
}
```

### Android Application class (with companion object)
The classic use of the singleton pattern in android apps is to provide access to a custom Application object.
Custom in the regard that its your own classed derived from the Android framework application class.

It might be tempting to declare the Application class as an Object Declaration. This unfortunately does not
work since the Application class is instantiated _for us_, by the android runtime and objects are instantiated
by the kotlin runtime. 

To get around this you declare the Application class as a regular class but save the instance inside a companion object,
that way its easily accessible to the rest of the app:

```kotlin
class App : Application()
{
	override fun onCreate() {
        instance = this
        super.onCreate()
    }
    
	companion object {
        lateinit var instance : App
    }
}
```
App.instance() will return the instance. a companion object is initialized when the corresponding class is loaded.

## Accessors (getters and setters)
### Automatic accessors
### Custom accessors

## Extensions

## Collections

## Coroutines

## Kotlin Android Extension

## Class objects




