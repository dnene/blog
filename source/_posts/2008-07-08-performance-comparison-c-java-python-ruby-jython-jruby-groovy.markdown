---
date: '2008-07-08 18:40:04'
layout: post
slug: performance-comparison-c-java-python-ruby-jython-jruby-groovy
status: publish
title: Performance Comparison - C++ / Java / Python / Ruby/ Jython / JRuby / Groovy
comments: true
wordpress_id: '37'
categories:
- java
- php
- python
- ruby
- software
---

**Update (README CAREFULLY) :** I am starting to see hyperlinks to his post with only some of the findings being treated as the link title (eg. X is 100 times faster than Y, X faster than Z). I emphasise once again that I have carefully indicated in the original post that this is but one of many possible microbenchmarks and that you should treat the results as one of many data points. Given the comments I've received and some of the links I've seen to this post, if I was to make this posting anew, I would choose to assign the title of this post as **"Implementing an identical object oriented solution to the Josephus Problem in Java / C++ / Ruby / JRuby / Python / Jython / Groovy and measuring the performance results thereof."**

This post compares performance across various languages for a specific micro benchmark _(actually it isn't really a microbenchmark - it is simply a benchmark for a specific piece of logic - but thats the closest word I could think of)_.

Last week, while preparing for a presentation - [Contrasting Java and Dynamic Languages](http://blog.dhananjaynene.com/2008/07/presentation-contrasting-java-and-dynamic-languages/),  I came across this interesting [Perl/Python/Ruby Comparison](http://danvk.org/josephus.html) which focused on comparing the code style of different languages. I thought it would be interesting to use the same to get some actual benchmarks based on the same. Note that you could also use the code segments below to get a feel for different syntactic flavours. However since I have strived to keep the code as similar as possible to each other, some of the advanced syntactic sugar of the dynamic languages is not on display here.



### Problem Statement

 

Quoting from the post linked to above : 



> Flavius Josephus was a roman historian of Jewish origin. During the Jewish-Roman wars of the first century AD, he was in a cave with fellow soldiers, 40 men in all, surrounded by enemy Roman troops. They decided to commit suicide by standing in a ring and counting off each third man. Each man so designated was to commit suicide...Josephus, not wanting to die, managed to place himself in the position of the last survivor.  

In the general version of the problem, there are n soldiers numbered from 1 to n and each k-th soldier will be eliminated. The count starts from the first soldier. What is the number of the last survivor


  



### Design



I actually changed the design of the solution as compared to the original post. Instead of using the deeply recursive calls as used in the earlier post, I decided to split the logic into two classes, and use loop iteration instead of recursion. It is my belief that we tend to do loop iterations far more frequently than recursions, and the resultant class design having two classes - one to indicate a Chain and one reflecting a Person seemed more appropriate to me.

**Logic**
The Chain object contains a reference to one person (first) who is but one member in a circular linked list. Each person object has a reference to its previous (prev) and next (next) person in the circle. When the kill loop starts, it sets a threshold (nth). The count starts with 1 from the first person. Each person when asked to shout, checks if the shout count (shout) is less than the threshold (nth). If less, the person just returns an incremented count. If the two are same, the person in effect commits suicide. In doing so the person, updates the next reference of its prev, and prev reference of its next to take himself off the circle and keep the circle consistent, finally returning a shout of 1 (which is what the next person in the list will shout).

The code does not have any comments (sorry!) and all the console outputs have been removed so that the benchmarking activity is not interfered with by the IO overheads.




### The results



All the results are as observed on my notebook with the following config
OS : Ubuntu Gutsy Gibbon 7.10
Kernel : 2.6.22-15-generic
CPU : Intel(R) Core(TM) Duo CPU      T2600  @ 2.16GHz
RAM : 2GB





<table>
    <tr>
        <th>Language</th><th>Version</th><th>Lines of Code</th><th>Time per iteration (microseconds)</th>
    </tr>
    <tr>
        <td>Java</td>
        <td>Sun JDK 1.6.0.03</td>
        <td>86</td>
        <td>1.6 </td>
    </tr>
    <tr>
        <td rowspan="2">C++</td>
        <td>4.1.3 20070929 (prerelease)<br/>
        (Ubuntu 4.1.2-16ubuntu2)  <br/>
        Compiled with optimisation -O3</td>
        <td>86</td>
        <td>3</td>
    </tr>
    <tr>
        <td>gcc version 4.2.3  <br/>
        (Ubuntu 4.2.3-2ubuntu7)<br/>
        Compiled with optimisation -O3<br/>
        [Alberto Bignotti's modified code with customised memory reuse and management](http://www.bigno.it/speed/cppspeed.html)</td>
        <td>124</td>
        <td>approx 0</td>
    </tr>
    <tr>
        <td rowspan="3">Ruby</td>
        <td>ruby 1.9.0 (2008-04-14 revision 16006) [i686-linux]</td>
        <td rowspan="3">63</td>
        <td>89 </td>
    </tr>
    <tr>
        <td>ruby 1.8.6 (2007-06-07 patchlevel 36) [i486-linux]</td>
        <td>380</td>
    </tr>
    <tr>
        <td>jruby : ruby 1.8.6 (2008-05-28 rev 6586) [i386-jruby1.1.2]</td>
        <td>80</td>
    </tr>
    <tr>
        <td rowspan="3">Python</td>
        <td>2.5.1</td>
        <td rowspan="3">41</td>
        <td>192 </td>
    </tr>
    <tr>
        <td>2.5.1 with psyco</td>
        <td>33</td>
    </tr>
    <tr>
        <td>Jython 2.2.1 on JRE 1.6.0.03</td>
        <td>632</td>
    </tr>
    <tr>
        <td rowspan="3">Groovy</td>
        <td>Groovy Version: 1.5.6 JVM: 1.6.0_03-b05 uncompiled</td>
        <td rowspan="3">81</td>
        <td>363 </td>
    </tr>
    <tr>
        <td>Compiled to bytecode and run using java</td>
        <td>360</td>
    </tr>
    <tr>
        <td>_Update_Groovy Version: 1.6-beta-1 JVM: 1.6.0_03</td>
        <td>104</td>
    </tr>
    <tr>
        <td>PHP</td>
        <td>PHP 5.2.3-1ubuntu6.3 (cli)</td>
        <td>85</td>
        <td>593 </td>
    </tr>
    <tr>
</table> 

**Updates :** 



	
  * Ken suggested syntactic improvements (see comments below) which lead to even faster ruby execution times : jruby : 80 microseconds, ruby 1.9 : 89 microseconds, ruby 1.8.6 : 380 microseconds. The table above has been updated

	
  * Cato requested a run using Groovy 1.6 beta 1 - have updated the same. Big improvement

	
  * Nicholas Riley suggested introducing slots and using "is not" and "is" in the if conditions for the python code. Updated the results to reflect the figure of 192 and 632 micro seconds for CPython and Jython. The figure was 182 microseconds for CPython and 131 microseconds for Jython if I did not use the new style classes, however I did not reflect the same, since most new code is likely to be using new style classes. However this does indicate one possible performance optimisation if your code does not depend upon new style classes. Moreover makes me really interested in waiting for the Jython performance optimisations for new style classes that Nicholas suggests are on their shortlist.

	
  * Tim Fountain in a comment below indicates that on his hardware (core 2 Quad) with Ubuntu Hardy Heron, Ruby 1.8.6 (same version as above) performs somewhat faster (15%) whereas upgraded version of Python and PHP run much faster(63% for python and 83% for ruby). Another difference in config- he is running 64 bit.

	
    * python - version 2.5.2: - 138 microseconds

	
    * ruby - 1.8.6 (2007-09-24 patchlevel 111) [x86_64-linux] :321 microseconds

	
    * PHP - 5.2.4-2ubuntu5.1 with Suhosin-Patch 0.9.6.2 (cli): 323 microseconds.




        
  * Peter Lupo requested a reduction in line count for Java since the conventional way is to have the opening braces not on a separate independent line. Given the fact that it is a fair comment, I have reduced the line count of java to 86 (didn't physically change the code - 86 = 101 - 15 opening curly braces).

	
  * Added another finding of using python with psyco

        
  * C++ - Added results using Alberto Bignotti's alternative code with customised memory reuse management


  



### Summarisation



The following are the results. Given the long code blocks, I am presenting the summarisation first followed by the code.

**Note:** This can only be treated as one particular benchmark. The results are a little atypical with respect to my general understanding. Advise caution against drawing broad conclusions based on this benchmark alone but would suggest that you could treat this as one data point amongst many. People better versed than me in the details of language runtimes might be able to suggest why some of the results seem surprising or atypical. 




	
  * **Java / C++ Rock** : The performance of Java and C++ was head and shoulders beyond other languages (nearly 100 times faster). My thought is that while a difference of 10x was only to be expected - this difference was just way too massive

	
  * **Java is faster than C++** : Though I had read about other microbenchmarks reaching the same conclusions, it is the first time I actually ran one where Java was faster. There are many others I have run where C++ beats Java quite handsomely. More importantly - the performance of C++ worsened by almost 40% once I added code which started freeing memory that was being allocated (there's still a small memory leak in the code - there is no Chain destructor which will clean up first). I would later definitely want to look at the impact of garbage collection in this context, and whether the Java garbage collector simply was much faster than the hand crafted new - delete calls in C++. **Update:**Using the customised memory management (which is not used in any of the other examples) but the same algorithm as in the [code written by Alberto](http://www.bigno.it/speed/cppspeed.html), C++ is much faster than Java

	
  * **Ruby 1.9 is twice as fast as Python** : While it has been known for a while Ruby 1.9 is much faster than Ruby 1.8.6, heres one more supporting data point. I was expecting ruby 1.9 to give python a run for its performance money. But at least in this particular context it seems to be much much faster.

	
  * **JRuby is faster than Ruby** : Even ruby 1.9. Very interesting indeed.
	
  * **Jython still has some catching up to do** : Though in the ballpark as the other languages, it was the slowest in the pack.

	
  * **Overhead of dynamism is dominant** : I have no idea if JRuby ran much faster because of the java bytecode or because of its implementation (though its performance was not even remotely close to that of Java). However even after I compiled groovy code, to java bytecode, it still ran much slower than python and ruby. It seems the overhead of supporting dynamic constructs is much more dominant than any benefits that one gets out of compilation (whether to java byte code or to intermediate compiled  files). I think the argument that because something compiles to java bytecode it is likely to be fast should be looked at a little carefully. 

	
  * **PHP stays at the rear end** : Though I benchmarked PHP for the first time, I wasn't completely surprised by the fact that PHP could only manage to be faster than Jython.



**Update : ** There are many comments to this post including those from cwilbur who benchmarks perl using a idiomatic method, Paddy3118 who offers an optimised algorithm for python, and peter lawrey who offers an optimised algorithm for Java. I would like to state that each of their solutions offer superior performance than that what has been described here. However I believe any benchmark comparison should compare apples to apples. Should these contributions be taken into account and be reflected in the table above ? I certainly believe there is a case to do so as an exercise using a different algorithm. However to ensure that it is a fair comparison, one has to modify all the code in all other languages also to reflect the same algorithm. Only then can we get an apple to apples comparison. That is probably an exercise for another post. Is the algorithm I have chosen the fastest - No. However I believe it is a very readable algorithm and if one ignores the IO with networks and databases and files, it is probably close to the kind of code many programmers write (and maintain) on a day to day basis.  It has been consistently implemented in all the languages. Readers should be aware that there are algorithms which will deliver much superior performance - but they will also make the performance superior in all the languages (perhaps to slightly differing extents and thus possibly somewhat different results). 



### The code



For all you who are either interested in running it for yourself or would like to perhaps explore this in more detail, .. here's the code. Note that I am not equally competent across all languages. So if you believe there is something that could be more appropriate way to code the same, do post a comment. One of the things I have tried to do is to ensure that the code remains more or less similar across all languages. Also I have used getter - setters or skipped them based on my understanding of the generally accepted convention for users of the language.


**Java : **


    
``` java    
    package com.dnene.josephus;
    
    public class Chain
    {
    	private Person first = null;
    	
    	public Chain(int size)
    	{
    		Person last = null;
    		Person current = null;
    		for (int i = 0 ; i < size ; i++)
    		{
    			current = new Person(i);
    			if (first == null) first = current; 
    			if (last != null)
    			{
    				last.setNext(current);
    				current.setPrev(last);
    			}
    			last = current;
    		}
    		first.setPrev(last);
    		last.setNext(first);		
    	}
    
    	public Person kill(int nth)
    	{
    		Person current = first;
    		int shout = 1;
    		while(current.getNext() != current)
    		{
    			shout = current.shout(shout, nth);
    			current = current.getNext();
    		}
    		first = current;
    		return current;
    	}
    	
    	public Person getFirst()
    	{
    		return first;
    	}
    	public static void main(String[] args)
    	{
    		int ITER = 100000;
    		long start = System.nanoTime();
    		for (int i = 0 ; i < ITER ; i++)
    		{
    			Chain chain = new Chain(40);
    			chain.kill(3);
    		}
    		long end = System.nanoTime();
    		System.out.println("Time per iteration = " + ((end - start) / (ITER )) + " nanoseconds.");
    	}
    }
    package com.dnene.josephus;
    
    public class Person
    {
    	int count;
    	private Person prev = null;
    	private Person next = null;
    	
    	public Person(int count)
    	{
    		this.count = count;
    	}
    	
    	public int shout(int shout, int deadif)
    	{
    		if (shout < deadif) return (shout + 1);
    		this.getPrev().setNext(this.getNext());
    		this.getNext().setPrev(this.getPrev());
    		return 1;
    	}
    	
    	public int getCount()
    	{
    		return this.count;
    	}
    	
    	public Person getPrev()
    	{
    		return prev;
    	}
    
    	public void setPrev(Person prev)
    	{
    		this.prev = prev;
    	}
    
    	public Person getNext()
    	{
    		return next;
    	}
    
    	public void setNext(Person next)
    
    	{
    		this.next = next;
    	}
    }
```    



**C++**


    
``` cpp    
    #include <stdio.h>
    #include <stdlib.h>
    #include <time.h>
    #include < sys/time.h>
    
    class Person
    {
    
        public:
    
            Person(int count) : _next(NULL), _prev(NULL) { _count = count; }
            int shout(int shout, int nth)
            {
                if (shout < nth) return (shout + 1);
                _prev->_next = _next;
    
    
                _next->_prev = _prev;
                return 1;
            }
            int count() { return _count; }
            Person* next() { return _next; }
            void next(Person* person) { this->_next = person ; }
            Person* prev() { return _prev; }
            void prev(Person* person) { this->_prev = person; }
        private:
            int _count;
            Person* _next;
            Person* _prev;
    };
    
    class Chain
    {
        public:
            Chain(int size) : _first(NULL)
            {
                Person* current = NULL;
                Person* last = NULL;
                for(int i = 0 ; i < size ; i++)
                {
                    current = new Person(i);
                    if (_first == NULL) _first = current;
                    if (last != NULL)
                    {
                        last->next(current);
                        current->prev(last);
                    }
                    last = current;
                }
                _first->prev(last);
                last->next(_first);
            }
            Person* kill(int nth)
            {
                Person* current = _first;
                int shout = 1;
                while(current->next() != current)
    
    
    
                {
                    Person* tmp = current;
                    shout = current->shout(shout,nth);
                    current = current->next();
                    if (shout == 1)
                    {
                        delete tmp;
                    }
                }
                _first = current;
                return current;
            }
            Person* first() { return _first; }
        private:
            Person* _first;
    };
    
    int main(int argc, char** argv)
    {
        int ITER = 1000000;
        Chain* chain;
        struct timeval start, end;
        gettimeofday(&start;,NULL);
        for(int i = 0 ; i < ITER ; i++)
        {
            chain = new Chain(40);
            chain->kill(3);
    
    
            delete chain;
        }
        gettimeofday(&end;,NULL);
        fprintf(stdout,"Time per iteration = %d microsecondsnr", (((end.tv_sec - start.tv_sec) * 1000000) + (end.tv_usec - start.tv_usec)) / ITER);
        //fprintf(stdout,"Last man standing is %dnr", (chain->first()->count() + 1));
        return 0;
    }
```    



**Python**


    
``` python    
    class Person(object):
        def __init__(self,count):
            self.count = count;
            self.prev = None 
    
    
            self.next = None
        def shout(self,shout,deadif):
            if (shout < deadif): return (shout + 1)
            self.prev.next = self.next
            self.next.prev = self.prev
            return 1
    
    class Chain(object):
        def __init__(self,size): 
            self.first = None
            last = None
            for i in range(size):
                current = Person(i)
                if self.first == None : self.first = current
                if last != None :
                    last.next = current
                    current.prev = last
                last = current
            self.first.prev = last
            last.next = self.first
        def kill(self,nth):
            current = self.first
            shout = 1
            while current.next != current:
                shout = current.shout(shout,nth)
                current = current.next
            self.first = current
            return current
        
    import time 
    ITER = 100000
    start = time.time()
    for i in range(ITER):
        chain = Chain(40)
        chain.kill(3)
    end = time.time()
    print 'Time per iteration = %s microseconds ' % ((end - start) * 1000000 / ITER)
```    



**Ruby**


    
``` ruby    
    class Person
        attr_reader :count, :prev, :next
        attr_writer :count, :prev, :next
    
        def initialize(count)
            #puts 'Initializing person : ' + count.to_s()
            @count = count
            @prev = nil
            @next = nil
        end
    
        def shout(shout, deadif)
            if shout < deadif
                return shout + 1
            end
            @prev.next = @next
            @next.prev = @prev
            return 1
        end
    end      
                    
    class Chain 
        attr_reader :first
        attr_writer :first
        
        def initialize(size)
            @first = nil
            last = nil
            for i in (1..size)
                current = Person.new(i)
                if @first == nil
                    @first = current
                end
                if last != nil
                    last.next = current
                    current.prev = last
                end
                last = current
            end
            @first.prev = last
            last.next = @first
        end
    
        def kill(nth)
            current = @first
            shout = 1
            while current.next != current
                shout = current.shout(shout,nth)
                current = current.next
            end
            @first = current
            return current
        end
    end
    
    ITER=100000
    start = Time.now
    ITER.times { |i|
    chain = Chain.new(40)
    chain.kill(3)
    }
    ends = Time.now
    puts 'Time per iteration = ' + ((ends - start) * 1000000 / ITER).to_s() + " microseconds"
``` 



**Groovy**


    
``` java    
    class Chain
    {
        def size
        def first
    
        def init(siz)
        {
            def last
            size = siz
            for(def i = 0 ; i < siz ; i++)
            {
                def current = new Person()
                current.count = i
                if (i == 0) first = current
                if (last != null)
                {
                    last.next = current
                }
                current.prev = last
                last = current
            }
            first.prev = last
            last.next = first
        }
    
        def kill(nth)
        {
            def current = first
            def shout = 1
            while(current.next != current)
            {
                shout = current.shout(shout,nth)
                current = current.next
            }
            first = current
        }
    }
    
    class Person
    {
        def count
        def prev
        def next
    
        def shout(shout,deadif)
        {
            if (shout < deadif)
            {
                return (shout + 1)
            }
            prev.next = next
    
            next.prev = prev
            return 1
        }
    }
    
    def main(args)
    {
        println "Starting"
        def ITER = 100000
        def start = System.nanoTime()
        for(def i = 0 ; i < ITER ; i++)
        {
            def chain = new Chain()
            chain.init(40)
            chain.kill(3)
        }
        def end = System.nanoTime()
        println "Total time = " + ((end - start)/(ITER * 1000)) + " microseconds"
    }
    
    def ITER = 100000
    def start = System.nanoTime()
    for(def i = 0 ; i < ITER ; i++)
    {
        def chain = new Chain()
        chain.init(40)
        chain.kill(3)
    }
    def end = System.nanoTime()
    println "Time per iteration = " + ((end - start)/(ITER * 1000)) + " microseconds"
```    



**PHP**


    
``` php    
    class Person
    {   
        function __construct($c)
        {   
            $this->count = $c;
        }       
    
                    
        function getPrev()
        {       
            return $this->prev; 
        }           
                
        function setPrev($pr)
        {   
            $this->prev = $pr;
        }   
        
        function getNext()
        {
            return $this->next;
        }
    
        function setNext($nxt)
        {
            $this->next = $nxt;
        }
    
        function shout($shout, $nth)
        {
            if ($shout < $nth)
            {
                return $shout + 1;
            }
            $this->getPrev()->setNext($this->getNext());
            $this->getNext()->setPrev($this->getPrev());
            return 1;
        }
    }
    
    class Chain
    {
        var $first;
    
        function __construct($size)
        {
            for($i = 0; $i < $size ; $i++)
            {
                $current = new Person($i);
                if ($this->first == null) $this->first = $current;
                if ($last != null)
                {
                    $last->setNext($current);
                    $current->setPrev($last);
                }
                $last = $current;
            }
            $this->first->setPrev($last);
            $last->setNext($this->first);
        }
    
        function kill($nth)
        {
            $current = $this->first;
            $shout = 1;
            while($current->getNext() !== $current)
            {
                $shout =  $current->shout($shout,$nth);
                $current = $current->getNext();
            }
            $this->first = $current;
        }
    }
    
    $start = microtime(true);
    $ITER = 100000;
    for($i = 0 ; $i < $ITER ; $i++)
    {
        $chain = new Chain(40);
        $chain->kill(3);
    }
    $end = microtime(true);
    printf("Time per iteration = %3.2f microsecondsnr",(($end -  $start) * 1000000 / $ITER));
```    



