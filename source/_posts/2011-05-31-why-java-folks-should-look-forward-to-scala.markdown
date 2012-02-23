---
date: '2011-05-31 05:16:42'
layout: post
slug: why-java-folks-should-look-forward-to-scala
status: publish
title: Why Java folks should look forward to Scala
comments: true
wordpress_id: '1091'
categories:
- functional programming
- java
- programming
- scala
- software
tags:
- C#
---

There's an interesting series of blog posts in progress: Why Java folks should stop looking down on C# : [Part 1](http://blog.kalistick.com/java/why-java-folks-should-stop-looking-down-on-c-differences-in-similarities/) and [Part 2](http://blog.kalistick.com/java/why-java-folks-should-stop-looking-down-on-csharp-a-brand-new-world/) (at the point of time of writing this post). It offers an interesting and detailed set of contrasts between Java and C#. It is a detailed analysis and makes for very worthwhile reading. What really intrigued me was this comment :



> _"We notice that Java developers generally tend to look scornfully at C#, as a copycat created by Microsoft and used by dummies. In theses blog series, I am going to try to sweep this nonsense and show some of the C# goodness."_



At least in my experience, I do not recollect Java developers looking scornfully at C# (notice that I did not mention .NET or Windows or Microsoft - I said C#). But perhaps there are some out there and the blog series does attempt to provide them sufficient evidence to reconsider their opinions. But reading through the posts made me think, there's at least one good reason Java programmers can choose to celebrate and look forward to rather than feel worried about Java. And all these capabilities are available for the asking on their preferred platform - the JVM and in an interoperable and incrementally migratable way from their current code bases. - Scala.

Now, believe me you, scala capabilities extend far beyond those I describe below. But that further discovery is an adventure which readers are encouraged to conduct later. Also - I am only a little along that path of Scala discovery and still have distances to cover. So I won't feel surprised if people are able to offer healthier and superior solutions to the ones I describe below. In fact, I would encourage them to do the same. But here's something for all ye java citizens to feel good about.

Please note that it will be helpful if you review these two blog posts referred to above - since almost every example I refer to below, that I write using Scala, is based on the examples in these posts which are written in Java and C#. Thus, even after reading those posts, it might be useful to keep them open in other tabs. So that should you prefer to do so, you can refer to those Java and C# examples simultaneously as you read the Scala examples.

**Unified Type System**

Scala has types for the java primitives eg. Byte, Short, Int. Thus you can continue to deal with them as objects instead of having to specifically deal with them as primitives. All value classes inherit from AnyVal, whereas all others inherit from AnyRef, both in turn inheriting from Any. In addition there are a number of additional methods that are available on these types (eg. [RichInt](http://www.scala-lang.org/api/current/scala/runtime/RichInt.html)). At the same time these types have _exactly_ the same ranges as java primitives. Thus the scala compiler can choose to transform instances of such value types into corresponding Java primitive types. 

**Farewell Checked Exceptions**

Goodbye. Adios. Au revoir, Vale. Checked Exceptions have been bid farewell. And I don't think anyone's less happy for it. But if you need to call Scala code from Java, you can choose to use the _@throws_ annotation to mark your methods so that java code may treat these as thrown exceptions.

**Double Rainbow Accessors**

Another heavy weight boilerplate thats been waived goodbye is java bean style getter setters. Every non private member declared automatically gets a getter and setter free by default. In situations where you would want to override the default behaviour, you can choose to do so as well. Besides a particular construct called a case class further simplifies this and helps you create a class trivially with reasonable implementation of equals, toString etc. already rolled in. It is likely, that new keyboards continue to retain their gloss and continue to function far longer when coding in scala than in Java. A simple example of double rainbow accessors :

``` scala Double rainbow accessors
case class Meme (var catchPhrase : String,  var url: String)

// Notice: class members are declared as default constructor parameters. No further
//         declaration required again.
class MemeAdvanced (private [this] var cp: String, private [this] var u: String){
  // Since these are private fields, they can be wrapped using 
  // getters / setters as follows which can be modified to suit any non-typical
  // expectations eg. validations
  
  // if the above parameter declarations did not have private qualifier,
  // the methods below would be automatically provided, whereas if declaration contained
  // val instead of var, only the getter would get provided.
  def catchPhrase = cp
  def catchPhrase_=(s: String) {cp = s}
  
  def url = u
  def url_=(s: String) { u = s}
}

object DoubleRainbow {
  
  def main(args : Array[String]) : Unit = {
    // using case classes
    var meme = new Meme("foo","bar")
    meme.catchPhrase = "Rick roll'd"
    meme.url = "http://www.youtube.com/watch?v=EK2tWVj6lXw"
    println(meme.catchPhrase)

    // using normal classes
    var meme2 = new MemeAdvanced("foo","bar")
    meme2.catchPhrase = "Rick roll'd"
    meme2.url = "http://www.youtube.com/watch?v=EK2tWVj6lXw"
    println(meme.catchPhrase)
  }
}
```

**Initialisers**

There's a bunch of new initialisation capabilities eg. initialising using field names

``` scala Initialising using field names
    var meme = new Meme(catchPhrase="blub", url="blub2")
    println(meme.catchPhrase)
```

or  you can initialise a collection by passing a series of arguments to the constructor

``` scala Initialising by passing a series of arguments
    val digits = List[Int](0,1,2,3,4,5,6,7,8,9)
```

or you could choose to load up a map with a bunch of associations at instantiation time

``` scala Initialising a map with associations
    val keywordsMapping = Map[String,String] ( "super" -> "base", "boolean" -> "bool", "import" -> "using" )
    println(digits.head + " " + keywordsMapping("boolean"))
```

**Verbatim (Multiline) Strings**

That isn't too hard with scala too. Just use triple consecutive double quotes delimiter (""")to make the string span multiple lines.

``` scala Multiline support
	val input = """Multiline
    	                    325-532-4521"""
	println(input)
```

**Methods as first class citizens**

Missing function pointers after you moved away from C / C++? Thats available too .. in a typesafe manner.

``` scala Pass functions around
    // declare a function pointer which takes a string as an input
    // and outputs the same
    var normalizeOp = (input: String) => input reverse;
    println(normalizeOp("abcd"))

    // compiler error : normalizeOp = (input: String) => 0;
    // thus the type of normalizeOp can no longer be changed

    // but it can be used to point to a different method instead
    normalizeOp = (input: String) => input trim;
    println(normalizeOp("  foo  bar  "))
```

**Event**

Now, scala doesn't have a built in Event class which has its automatic hardwired publish subscribe capabilities. But one of the themes you will find in this post is that Scala lives up to its name which really refers to it being a scalable language. So a whole set of language constructs can actually be built into the language by writing other code. Thats why sometimes it feels somewhat like a meta-language, a language to write your own language structures in. The construct below is rather simple and straight-forward, but we shall see creating your own language control structure like constructs later as well.

Here's a simple event class
``` scala 
class Event {
  var l = List[() => Unit]()

  def +=(f: () => Unit): Unit = l = f :: l   
  
  def apply() = l map (_())
}
```

It declares a list of listeners _l_ and allows listening functions to be registered against the event getting triggered (which in this case would be the += method. Finally since the event is triggered using event() construct in C#, we here define the apply function, which will get triggered whenever e() is invoked, e being the instance of any event. Note: I've deliberately used single character names for the fields to allow you to focus on the other constructs - not a good coding practice.

``` scala
class Button {
  val action = new Event();
  
  def performClick() = onClick()
  
  private def onClick() = {
    // this should call the apply() method
    action()
  }
}

object EventDriver {
  def performAction() {
    println("Work!")
  }
  
  def main(args : Array[String]) : Unit = {
    val button = new Button()
    // add a method as a listener
    button.action += performAction
    // trigger the event
    button.performClick()    
  }
}
```

So does one really need to have a Event baked into the language ? Apparently not really when it is a scalable language.

**Lambda expressions**

Indeed one of the strong points of this language. A simple lambda function is given below 

``` scala lambda expressions
    def isUniverseAnswer = (x: Int) => x == 42
    println(isUniverseAnswer(42))
    println(isUniverseAnswer(43))
```

There, it isn't so hard. But what when you want to define a lambda on the fly ? Turns out thats quite straight forward too.

``` scala anonymous functions
    val users = List[User](User("Five",5), User("Fifteen",15), User("TwentyFive", 25), 
    						User("Ten",10), User("Twenty", 20))
    // Note : the argument passed to the filter() method is the lambda, and the _ refers 
    //           to each User object passed in
    println (users filter (_.age < 18))
```

And if we did want to explore closures, there's good support there too.

``` scala closures
    var counter = 0
    val action = () => {counter += 1; println("counter = " + counter)}
    action()
    action()    
```

As Jack Sparrow would've said, _"savvy?"_

**Extension methods**

The exact details of how some of these capabilities are made to work are beyond the scope of this post, but scala allows for a lot of new extension methods on basic built in types through a variety of additional classes, and the same capabilities can be used by you to define any additional extension methods. As a teaser example, we'll use the reverse method defined in the WrappedString type which helps extend the same method to be used with strings.

``` scala Adding extension methods
println("Example" reverse)
```

**Return Multiple Values**

Thats very intuitive and simple too. 

``` scala Returning multiple values
object ReturnMultiple {
  // this method returns two values
  def addOneToAll(a: Int, b: Int) = (a + 1, b + 1)

  def main(args : Array[String]) : Unit = {
    val (c, d) = addOneToAll(3,4)
    println(c + "," + d)
  }
}
```

**Null-coalescing operator**

To the best of my knowledge scala doesn't have this operator. The C# code looks like follows which allows the first non-null value amongst a, b, and c to be set as the result.

``` scala
string result = a ?? b ?? c
```

Well, we'll build that operator (I'm sure someone will suggest a better option than the one below)

``` scala
class Nullable(val t: AnyRef) {
    def ??(b: AnyRef) = if (this.t != null) this.t else b
}

object NullCoalescing {
  implicit def objToNullable(a: AnyRef) : Nullable = { new Nullable(a) }

  def main(args : Array[String]) : Unit = {
    println("hello" ?? null ?? null)
    println((null: AnyRef) ?? "world" ?? null)
    println((null: AnyRef) ?? null ?? "foobar")
  }
}
```

If you see the three println statements, they show how the newly defined ?? operator can deliver the same semantics. Since null cannot help the type inferencing engine, in the latter two println statements - it has been explicitly cast to AnyRef.

**Automatic Resource Management**

Apparently C# has a built in capability of automatic resource management by using the keyword _using_. Alas Scala doesn't. But building it is hardly much effort. So here goes :

``` scala Automatic resource management
import java.io.FileInputStream
import java.io.DataInputStream
import java.io.InputStreamReader
import java.io.BufferedReader
object AutomaticResourceManagement {
  // This is a structural type. Any type which has methods which match the two
  // desired signatures specified below can be used wherever this type is expected
  // ** no inheritance required **
  type ReadableAndCloseable = { def close(): Unit ; def readLine() : String}
  
  // And to be able to define our own control structure, the second parameter to
  // using takes a function block which takes a ReadableAndCloseable as input
  // and returns nothing.
  def using(resource: ReadableAndCloseable)(f: ReadableAndCloseable => Unit) {
    try {
    	f(resource)
    } finally {
    	println("Closing resource")
    	resource.close()
    }
  }
  
  def main(args : Array[String]) : Unit = {
    val reader = new BufferedReader(new InputStreamReader(new DataInputStream(new FileInputStream("foo.txt"))))
    // Now you can be sure, the reader will get closed.
    using(reader){
      (resource) => println("First line is: " + resource.readLine())
    }
  }
}
```

**Summary**

Well there's more in Scala. Lots more. And it takes some patient effort to learn it. We've hardly started to talk about the functional programming capabilities. Or its parallel collections. Or for that matters its pattern matching. Or even its ability to deal with Generics with specified Co and Contravariance. We haven't gone that far. But all the seemingly distant capabilities - that seemed to be miles away in a different planet called .NET and a country called C#, are actually just a step away - using Scala. As a java programmer, I don't think you should look down at C# .. just look forward to Scala :)
