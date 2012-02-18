---
date: '2008-09-25 23:25:32'
layout: post
slug: python-from-java-how-duck-typing-influences-class-design-and-design-principles
status: publish
title: Python from a Java perspective - Part 2 - How duck typing influences class
  design and design principles
wordpress_id: '105'
categories:
- java
- programming
- python
- software
tags:
- design principles
- java
- ood
- python
---

**Update:** Modified the title to make it a little shorter.

This post talks about applying Open Closed Principle, Liskov's Substitution Principle, Dependency Inversion Principle and  Interface Segregation Principle in Python, coming from a Java programming background. 

**Background : **
A few days ago I blogged about [ Commentary on Python from a Java programming perspective](http://blog.dhananjaynene.com/2008/09/commentary-on-python-from-a-java-programming-perspective/). In that post I avoided getting into the specific details with code snippets etc since I wanted to focus on how it feels. 

One of the observations I made was that I thought coming from a Java background, that background helped me from a class design perspective. I ran into a couple of posts from the [ObjectMentor blog](http://blog.objectmentor.com/), namely  [The Open-Closed Principle for Languages with Open Classes](http://blog.objectmentor.com/articles/2008/09/04/the-open-closed-principle-for-languages-with-open-classes), and [The Liskov Substitution Principle for "Duck-Typed" Languages](http://blog.objectmentor.com/articles/2008/09/06/the-liskov-substitution-principle-for-duck-typed-languages).  The gentleman behind ObjectMentor is Robert Martin, who wrote a number of articles in the mid 90s related to these design principles. (Links to PDF : [Open Closed Principle](http://www.objectmentor.com/resources/articles/ocp.pdf), [Liskov Substitution Principle](http://www.objectmentor.com/resources/articles/lsp.pdf),[ Dependency Inversion Principle](http://www.objectmentor.com/resources/articles/dip.pdf) and [Interface Segregation Principle](http://www.objectmentor.com/resources/articles/isp.pdf))



_Sidebar : A Hat Tip to Robert Martin : _ [Robert Martin](http://www.objectmentor.com/omTeam/martin_r.html) used to contribute heavily to a number of newsgroups including  [comp.lang.c++](http://groups.google.com/group/comp.lang.c++/topics) and [comp.object](http://groups.google.com/group/comp.object/topics) in the early and mid 90s. I must confess a tremendous debt to him since my view of OO Design in those days was substantially influenced by his writings, and the design principles he talked about and later published as articles. These continue to guide my thinking about Object Oriented Design to this day. A [hat tip](http://en.wikipedia.org/wiki/Hat_tip) to Robert Martin. He wouldn't know me or recall me, but I used to participate in [some](http://groups.google.com/group/comp.lang.c++/browse_thread/thread/f2addf623e4a6962/3d714eff3d76dbad?) [threads](http://groups.google.com/group/comp.object/browse_thread/thread/a98ca8faa1b71b0e/ab6fade8afe71934) occasionally, and read him regularly and that helped me tremendously learn so much about OOD and how to apply it in C++.



Having had applied these principles a countless number of times,in C++ and Java, I thought it would be an interesting exercise to document how class design changes even when the same underlying design principles are applied to a dynamic language (in this case - Python). The remainder of this post summarises the design principles and the examples that Robert Martin talked about in his articles, and how these get implemented perhaps a little differently when used in static typed (Java) and dynamically typed (Python) languages. His original source snippets in the C++ language can be found in the articles I have hyperlinked to earlier. 
 
**Open Closed Principle (OCP)**



> Software entities (classes, modules,functions etc.) should be open for extension but closed for modification



What this basically means is that the code you write should be in a manner where it does not need to be modified when you need to extend it - the design of the code should allow for the extension to be made by new code being added, not old code being modified. 

In the example below Circle and Square are two Shapes which can be rendered. The design essentially sets out with an objective that it should be easy to add newer shapes (say triangles) without modifying existing code. Lets straight away get into the Java version of the code. 


    
``` java
    public class Painter {
    
        public static void main(String[] args) {
            // We sould like the shapes to be drawn in the 
            // order of the shape types as found in this list
            List<class> classOrder = new LinkedList<class>();
            classOrder.add(Square.class);
            classOrder.add(Circle.class);
    
            // The shapes
            List<shape> shapes = new LinkedList<shape>();
            shapes.add(new Circle(1,1,5));
            shapes.add(new Circle(3,3,7));
            shapes.add(new Square(2,4,3));
            shapes.add(new Square(4,2,4));
    
            Collections.sort(shapes, new ShapeComparator(classOrder));
    
            for (Shape shape : shapes)
            {
                shape.draw();
            }
        }
    }
    
    // This declaration defines the contract across shapes
    public interface Shape {
        public void draw();
    }
    
    public class ShapeComparator implements Comparator<shape> {
        private List<class> orderedClasses;
        public ShapeComparator(List<class> orderedClasses) {
            this.orderedClasses = orderedClasses;
        }
    
        @Override
        public int compare(Shape s1, Shape s2) {
            return this.orderedClasses.indexOf(s1.getClass()) -
                this.orderedClasses.indexOf(s2.getClass()) ;
        }
    }
    
    public class Circle implements Shape {
        private int x;
        private int y;
        private int radius;
    
        public Circle(int x, int y, int radius) {
            super();
            this.x = x;
            this.y = y;
            this.radius = radius;
        }
    
        @Override
        public void draw() {
            System.out.println("Drawing Circle at (" +
                    x + "," + y + ") with radius " + radius);
        }
    }
    
    public class Square implements Shape {
        private int x;
        private int y;
        private int width;
    
        public Square(int x, int y, int width) {
            super();
            this.x = x;
            this.y = y;
            this.width = width;
        }
    
        @Override
        public void draw() {
            System.out.println(
                "Drawing Square at (" + x + "," + y +
                ") with width " + width);
        }
    }
```    



The Painter class here defines the order in which the various shapes need to be rendered (_classOrder_), creates a list of all the shapes (_shapes_), sorts the _shapes_ list using _classOrder_ and then renders all the shapes.

The ShapeComparator is a class which implements the comparator interface for Shapes (_public class ShapeComparator implements Comparator_) and implements the compare method which compares two shape instances and decides the relative order between them.

The remainder of the code should be rather self explanatory.

So where is OCP being applied here ? For that you have to imagine a new shape, say a triangle now being introduced into the mix. In this case, Triangle would be a new class which would implement Shape. No existing line of code will change. However the Painter main method will now add the new class in the classOrder list in the appropriate place, and if you want to modify the order in which the types should be rendered, just modify their corresponding class placement in the classOrder list. The essential thing to be noted is that Shape, Circle, Square and ShapeComparator are all "_open for extension but closed for modification_."

So what does the corresponding python code look like ?


    
``` python
    # Look ma ! No Shape class
    class Circle(object):
        def __init__(self,x,y,radius):
            self.x = x 
            self.y = y 
            self.radius = radius
        def draw(self):
            print "Drawing Circle at (%s,%s) with radius %s" % \ 
                    (self.x, self.y,self.radius)
    
    class Square(object):
        def __init__(self,x,y,width):
            self.x = x 
            self.y = y 
            self.width = width
        def draw(self):
            print "Drawing Square at (%s,%s) with width %s" % \ 
                (self.x, self.y,self.width)
    
    if __name__ == "__main__":
        order = [ Circle , Square] # the order in which to draw the shapes
        shapes = [Circle(1,1,5), Circle(3,3,7), \
                        Square(2,4,3), Square(4,2,4)] 
        for shape in sorted(shapes,
                # Comparison function is embedded inline using a lambda
                lambda s1,s2 : order.index(type(s1)) \
                         - order.index(type(s2))): 
            shape.draw()
```    



What learnings can we get from this ?

The first most apparent difference is - _No Shape Class_. Where did it go ? Well, static typed languages use [polymorphism](http://en.wikipedia.org/wiki/Polymorphism_(computer_science)) as a powerful mechanism of extensibility. In other words, in many cases the extensions are likely to be newer derived types. Thus design the rest of your code to work on the base type and introduce the newer derived types later as required without having to necessarily change existing code. However static languages primarily depend upon inheritance as the vehicle for delivering polymorphism. Dynamic languages on the other hand depend upon [duck typing](http://en.wikipedia.org/wiki/Duck_typing). Duck typing supports polymorphism without using inheritance. In this context you need the same set of relevant methods to be implemented in each of the extension classes. The role of the abstract base class or interface as the one which specifies the contract / api has been made redundant. You can still choose to define a base class / interface if you want to, but you no longer have to.

Another thing to be noted is the way the comparator is implemented. While java required us to create a new one method class implementing a required interface, and then required us to instantiate the same and then trigger its functionality, python allowed us to implement it inline as a lambda. In a very different way this is OCP being applied (okay okay .. for those insisting on theoretical correctness thats not true .. but close enough) inside the language design itself. A function is an object in python, and a lambda is a special kind (not a subtype) of a function, and there are capabilities built into python to easily manipulate function objects / lambdas. The sorter method functionality was extended to allow custom comparators by specifying a particular evaluation expression as lambda. In java we actually had to implement the inheritance hierarchy ourselves before being able to leverage the extensibility in the list class for custom comparisons.

In the context of OCP, the learning is that dynamic type languages allow you to build extensibility by leveraging duck typing instead of type inheritance.

**Dependency Inversion Principle (DIP)**



>   * High level modules should not depend upon low level modules. Both should depend upon abstractions.
>   * Abstraction should not depend upon details, details should depend upon abstractions



To understand the principle, the reference point should be structured and modular programming scenarios prior to OO. If I wanted to book a tour, the tour booking function would in turn call functions to book a flight, book a car and book a hotel. All four function implementations would be in the parlance of the definition - "details". And anytime you had to change the way any of them worked it would require a relatively high amount of effort to do the changes. If you applied DIP in such a scenario, you would perhaps have a function say TripBooker which would in turn call methods on other classes which implement the methods in an interface / abstract class to book a flight, car and a hotel. The actual detailed booking logic would now be implemented in another class. Thus TripBooker now depends upon an abstraction which specifies the contract, and it becomes much easier to change or plugin the concrete implementations.



As an aside I must note that I have found code which properly applies DIP creates enormous frustration amongst people coming from a procedural background and lesser oriented to this OO style when attempting to statically browse the source. (Java Eclipse users will understand the quote : _"I was attempting to understand the logic. I did F3, F3, F3 and then I reached a interface."_, thats because attempting to decipher the logic invariably leads to an abstraction and the programmer has to now separately figure out which was the detailed implementation which would now get triggered). But thats a learning curve issue - not really an issue with DIP.



In the following example which is largely similar to (but not identical to) the example in Robert Martin's article, we are having a Lamp with a button. All the class names from the example are retained. These are the two specific details - _Lamp_ and _ButtonImpl_. The lamp has capabilities to turn on and off, but one would like to to move the exact mechanism of turning on and off (which might be hardware specific) into the detail but treat the ability to turn on and off as an abstraction (_ButtonClient_). Meanwhile buttons can have a variety of possible implementations one of which is _ButtonImpl_, which all share the essential characteristic that a button has one state with two possible values, can toggle between the states and have a reference to a _ButtonClient_ to which they can communicate with. 

Thus the model instead of having two details _Lamp_ and _ButtonImpl_, with the details communicating with each other directly, one has the following design :

	
    * Detail _Lamp_ implements (depends upon) abstraction _ButtonClient_

	
    * Detail _ButtonImpl_ implements (depends upon) abstraction _Button_

	
    * Abstraction _Button_ has a reference to (depends upon) abstraction _ButtonClient_



Here's the code in Java.

    
``` java
    public class ButtonHappy {
        public static void main(String[] args) {
            Button button = new ButtonImpl(new Lamp());
            button.toggle();
            button.toggle();
            button.toggle();
        }   
    }
    
    public interface ButtonClient {
        public void on();
        public void off();
    }
    
    public class Lamp implements ButtonClient {
        @Override
        public void off() {
            System.out.println("Lamp turned off");
        }   
    
        @Override
        public void on() {
            System.out.println("Lamp turned on");
        }   
    }
    
    public abstract class Button {
        private ButtonClient client;
        public Button(ButtonClient client){
            this.client = client;
        }   
        public void toggle(){
            boolean newstatus = ! getStatus();
            if (newstatus) client.on();
            else client.off();
            setStatus(newstatus);
        }   
        public abstract boolean getStatus();
        public abstract void setStatus(boolean status);
    }
    
    public class ButtonImpl extends Button {
        private boolean status;
        
        public ButtonImpl(ButtonClient client) {
            super(client);
        }   
    
        @Override
        public boolean getStatus() {
            return status;
        }   
    
        @Override
        public void setStatus(boolean status) {
            this.status = status;
        }   
    }
```    



There's probably no additional explanation required, so here's the equivalent implementation in python.


    
``` python
    class Lamp(object):
        def on(self):
            print 'Lamp turned on'
        def off(self):
            print 'Lamp turned off'
    
    class Button(object):
        def __init__(self,client):
            self.client = client
        def toggle(self):
            status = self.get_status()
            if self.status : self.client.on()
            else: self.client.off()
            self.set_status(not status)
    
    class ButtonImpl(Button):
        def __init__(self,client):
            self.status = False
            super(ButtonImpl,self).__init__(client)
        def get_status(self):
            return self.status
        def set_status(self,status):
            self.status = status
    
    if __name__ == "__main__":
        btn = ButtonImpl(Lamp())
        btn.toggle()
    
    
        btn.toggle()
        btn.toggle()
```    



We can again see here that one class that is missing but is no longer being missed ( ;) ) is ButtonClient and the reason is the same as in OCP - Duck Typing. In terms of the definition of DIP itself - abstractions no longer necessarily have to be implemented. They exist implicitly in the detail classes but are no longer explicitly documented. 

**Liskov Substitution Principle (LSP)**



> What is wanted here is something like the following substitution property:   

If for each object o1of type S there is an object o2 of type T such that for all programs P defined in terms of T, the behavior of P is unchanged when o1 is substituted for o2 then S is a subtype of T.



Actually LSP is more of a test of whether two classes qualify to share an inheritance or is-a relationship, rather than a prescription for the design itself. In simple terms it sets out a requirement that if you define a new derived class, it should be possible to substitute an instance of a base class in a program (and though it doesn't state it, an instance of a peer derived class ie a class in the same inheritance hierarchy) with an instance of a derived class without introducing any negative or unexpected side effects whatsoever. A rather simple example would be if we were to attempt to define a derived class of java.lang.String (say ConstrainedString) which now had an additional constraint (max characters - say 20 for a particular instance).  To support this constraint a new runtime exception MaxLengthExceededException would need to be defined. It would have to be a runtime exception as you do not have the ability to add to the checked exceptions thrown by the java.lang.String class in ConstrainedString. Now programs that concatenated strings have no notion of having to react to a String overflow situation and this would create undesirable side effects in the program. Thus using LSP one could conclude that it would be incorrect to implement ConstrainedString as a derived class of java.lang.String.

I could not think of a good way of showing LSP in action in code since it is a test of candidate relationship and does not have any structural manifestation itself. However it is still a valid test to be applied in Dynamic Languages as well with one change as recommended in the post[ The Liskov Substitution Principle for "Duck-Typed" Languages ](http://blog.objectmentor.com/articles/2008/09/06/the-liskov-substitution-principle-for-duck-typed-languages).



> What is wanted here is something like the following substitution property:   

If for each object o1of type S there is an object o2 of type T such that for all programs P defined in terms of T, the behavior of P is unchanged when o1 is substituted for o2 then S is <del>a subtype of</del> substitutable for T.



Duck typing introduces looser coupling than inheritance but a coupling that has no static checks whatsoever. So the application of LSP in a designer's mind is a little more interesting. Because the modified LSP is now a rather obvious statement which in loose term says "If A can be substituted by B without any side effects then A is substitutable by B", on the face of things, it is no longer such a useful principle. But if you explore under the covers, the real interesting part has now shifted from "B is a subtype of A" to "If A can be substituted by B". In static type languages, the places where A was being used was clearly known and easily searchable or navigable into from IDEs. Because this is now so much more difficult even though you are no longer explicitly attempting to apply LSP, you are on your toes the time far more often to avoid the situations that LSP was setup to warn you about.



_A Sidebar : Refactorability : _ I received some interesting comments on my earlier post [ Commentary on Python from a Java programming perspective](http://blog.dhananjaynene.com/2008/09/commentary-on-python-from-a-java-programming-perspective/) on [this thread](http://www.reddit.com/r/programming/comments/71yqq/commentary_on_python_from_a_java_programming/). My submission is that it requires you to come from a static typing background into a dynamic typing background to realise how much more difficult refactoring is. And the above mentioned implication of LSP in dynamic typing languages just goes out to demonstrate that there are indeed situations where the burden on you as a programmer / designer just went up due to the lack of explicit type information. This is but one of many aspect of many pros and cons between the two (static and dynamic typing) paradigms. To acknowledge it allows you to deal with it and work with it. 



**Interface Segregation Principle (ISP) : **



> Clients should not be forced to depend upon interfaces that they do not use



This is a little anticlimactic. The good news here is that thanks to duck typing, the client is actually now making the choice of what interface they choose to use. This is one principle that no longer needs to be explicitly applied. No more discussion required here.

**Summary :**
As we have seen duck typing does imply some changes to your class design. While the first three design principles continue to be relevant, their relevance is now a little different in your design process. It is important to be aware of these changes to adjust your design models in a more appropriate and idiomatic way. One would typically create lesser complex class hierarchies, especially with all the interfaces / pure abstract classes now no longer mandatory. Not only type information but even some of your abstractions are now less explicit.
