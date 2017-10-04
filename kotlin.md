# A Kotlin Krash Kourse - for java coders
An introduction to Kotlin from a Nodes Android App Development perspective.
This is by no means meant to be a complete reference to Kotlin. Instead
its a crash course to the various parts of Kotlin that are most relevant to Android developers.
And no, you cannot have a goddamn youtube video instead :)

Contents
========

  * [General coding](#general-coding)
	 * [Type inference](#type-inference)
	 * [Type Casting and Checking](#type-casting-and-checking)
		* [Safe casts](#safe-casts)
		* [Type checking (instanceof in java)](#type-checking-instanceof-in-java)
		* [Number conversions](#number-conversions)
	 * [Conditional assigment](#conditional-assigment)
	 * [Knowing When to switch - a more flexible condition multiplexer](#knowing-when-to-switch---a-more-flexible-condition-multiplexer)
	 * [Ranges](#ranges)
  * [Data objects](#data-objects)
  * [A Tale of Nullity and her sisters Mutability and Immutability](#a-tale-of-nullity-and-her-sisters-mutability-and-immutability)
	 * [Mutability](#mutability)
		* [Immutable declaration:](#immutable-declaration)
		* [mutable declaration](#mutable-declaration)
	 * [Nullability](#nullability)
		* [Normal reference](#normal-reference)
		* [Nullable reference](#nullable-reference)
	 * [Safe call operator - A safe voice in an endless void*](#safe-call-operator---a-safe-voice-in-an-endless-void)
	 * [Elvis operator](#elvis-operator)
	 * [The !! operator (unsafecall?)](#the--operator-unsafecall)
	 * [Fashionable lateinit](#fashionable-lateinit)
  * [Listeners and Callbacks and starships named Enterprise](#listeners-and-callbacks-and-starships-named-enterprise)
	 * [Object Expressions](#object-expressions)
	 * [Using Lambda's for callbacks instead of listeners](#using-lambdas-for-callbacks-instead-of-listeners)
  * [Contemplating String Templates](#contemplating-string-templates)
	 * [Combining with String.format()](#combining-with-stringformat)
  * [Typealias - one type many names](#typealias---one-type-many-names)
  * [Singletons and static members - Open from 10PM](#singletons-and-static-members---open-from-10pm)
	 * [Using objects from Java](#using-objects-from-java)
	 * [Static members](#static-members)
	 * [Android Application class (with companion object)](#android-application-class-with-companion-object)
  * [Accessors (getters and setters)](#accessors-getters-and-setters)
	 * [Automatic accessors](#automatic-accessors)
	 * [Custom accessors](#custom-accessors)
  * [Extensions](#extensions)
  * [Collections](#collections)
  * [Coroutines](#coroutines)
  * [Kotlin Android Extension gradle plugin](#kotlin-android-extension-gradle-plugin)
  * [Class objects](#class-objects)
      

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
This is operator (which apparently doesn't have a name) makes a nullable reference behave like in java,
in the way that you can use it in situations where the compiler normally wouldn't allow it:

```kotlin
var myNullableString : String? = null
var myNormalString : String = "Hola Gruppo"
// this won't compile because the rvalue can be null
myNormalString = myNullableString 
// This compiles, but will result in a null pointer exception at runtime if dereferenced while containing null (like java)
myNormalString = myNullableString!!
```
Try avoid using it.
_The android studio java <-> kotlin transpiler REALLY likes using it_

See also:
https://kotlinlang.org/docs/reference/null-safety.html

### Fashionable lateinit
Sometimes you REALLY don't want to use a nullable reference, but you also don't wan't to assign a value right away.
This is where you can make use of lateinit, but beware, if used before being initializated it will throw a 
NPE at you.
It's included for those situations where you cannot initialize a variable right away but would rather prefer not to make it
nullable because you'd then have to deal with it in the remainder of your code.
(alternatives are lazy initialization)

```kotlin
lateinit var imNotNullableButCanBeAssignedLater : Int

// sometime later...
imNotNullableButCanBeAssignedLater = 10 // a-ok
```
A better example of the use of lateinit is provided in the paragraph 
[Android Application class (with companion object)](#android-application-class-with-companion-object)


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
Accessors are automatically generated for you whenever you declare a object member variable which is public (which is the default in kotlin)
However in kotlin you don't call them as functions but instead how you would access a public variable in java.

If its a mutable public variable (declared with var) both setter and getter is generated, where as if its an immutable reference (declared with val)
only a getter is generated.

```kotlin
class Person
{
	var firstName = ""
}
```
In the above example the kotlin compiler autogenerates the methods **setFirstName()** and **getFirstName()** for you.
From java code you would use the above class like this:

```java
Person p = new Person();
p.setFirstName("Gertrude Powershield")
String name = p.getFirstName()
```

In kotlin the accessors are automatically called for you, hence you can use the following syntax (for easier to read code):

```kotlin
val p = Person()
p.firstName = "Gertrude Powershield"
val name = p.firstName
```

### Custom accessors
In case you want to do custom accessor they must be written immediately after the variable declaration:

```kolin
// declare isEmpty variable as well as its getter
val isEmpty: Boolean
    get() = this.size == 0
    
// alternative syntax
val isEmpty: Boolean
    get() { 
    	return this.size == 0
    }
```

Setters are basically the same deal but its useful to know that you can refer to the variable you're setting with the **field** keyword
value is an optional name for the argument:

```kolin
var counter = 0
    set(value) {
        if (value >= 0) field = value
    }
```

## Extensions
Extensions are a way to add a member function to class without changing the definition.
This can be very powerful because it can be applied to library classes as well.

Following code adds a method formatAmount on the builtin String class:

```kolin
fun String.formatAmount(amount : Double) : String
{
	return String.format(Locale.getDefault(), "%.2f", amount)
}

// use
val formattedAmountString = String.formatAmount(21.5)
```
formattedAmountString is now "21.00".
Inside the extension you can refer to other member variables and use this.

## Collections
There is a lot to be said about collections and kotlin which is unfortunately out of scope of this introduction.

One big difference from java is that the mutability concept have been extended to collections.
Kotlin contains a Mutable and Immutable versions of the run-of-the mill collection interfaces (List, Map etc)
where as the underlying containers (ArrayList, HashMap) are always mutable.

```kotlin
var myImmutableList : List<Int> = ArrayList() // my list has no add or remove methods
myImmutableList.add(10) // will not compile

var myMutableList : MutableList<Int> = ArrayList() // Contains all the jazz you're used to from java's List interface
myMutableList.add(10) // compiles a-ok
```

## Coroutines
Coroutines are an experimental feature, first add the following to gradle.properties to suppress annoying
linter messages:
```
kotlin.coroutines=enable
```

Coroutines are an advanced topic and as such a topic for a workshop in itself. There is plenty of good documentation available online. The following
are a few simple usages:

Use **launch()** to break off into a separate coroutine
```kotlin
launch(CommonPool)
{

}
```
launch returns a Job which you can do exciting things, like cancelling the coroutine or poll for completion.
CommonPool means use a preallocated threadpool to run the coroutines, other possible values are UI if you want it
to run on the main thread. You can nest launch:

```kotlin
launch(CommonPool)
{
	val result = longRunningOperation() // this line is executed in a background thread
	lauch(UI)
	{
		showResult(result)				// this line is being executed on the main/ui thread
	}
}
```
Refer to this guide for more advanced uses:
https://github.com/Kotlin/kotlinx.coroutines/blob/master/coroutines-guide.md


## Kotlin Android Extension gradle plugin
If you've have included the Kotlin Android Extension you can skip the boring process of binding views defined and instantiated
by the LayoutInflater and instead having it done automatically for you by simple "importing the layout". Place the following
line somewhere in your import statements and replace <layout_name> with the name of a layout in your project (ex: fragment_home)

```kotlin
import kotlinx.android.synthetic.main.<layout_name>.*
```

To use this feature you have to apply the gradle plugin in your app's gradle.build like this:

```groovy
apply plugin: 'kotlin-android-extensions'
```

_If you use this feature, which I think you should, adopt camelCaseNaming for your XML view id's since they're getting turned in variable names directly._

## Class objects
A lot of java libraries use the class object. In kotlin there is both a kotlin class object (a klass object if you will :D)
and the regular ole' java class object. This is how you access the java class object in kotlin:

```kotlin
val myClassName = MyClass::class.java.name
```

