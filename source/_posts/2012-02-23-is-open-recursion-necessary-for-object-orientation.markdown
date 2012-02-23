---
layout: post
title: "Is Open Recursion necessary for Object Orientation?"
date: 2012-02-23 03:32
comments: true
categories: erlang, programming
---

Earlier today I heard the articulation that Open Recursion was a necessary attribute for an Object Oriented Language. And further since Erlang did not have open recursion, it could not be considered an OO language.

Note that I am not contesting the thought above. It could quite be right. Yet I am unable to convince myself of the same. And the rest of the post real deals with many of my confusions around the same. 

Let us first focus on the definitions. There is a broader explanation on the wikipedia page on [Object Oriented Programming](http://en.wikipedia.org/wiki/Object-oriented_programming). But that is not the one I am going to be referring to. There's another detailed [explanation](http://userpage.fu-berlin.de/~ram/pub/pub_jf47ht81Ht/doc_kay_oop_en) provided by Dr. Alan Kay who actually introduced the term itself. To specifically focus on his definition, 

> OOP to me means only messaging, local retention and protection and hiding of state-process, and extreme late-binding of all things. It can be done in Smalltalk and in LISP. There are possibly other systems in which this is possible, but I'm not aware of them. 

A definition for Open Recursion can be found at [Open Recursion. a definition](http://etymon.blogspot.in/2006/04/open-recursion-definition.html). To quote

> Open recursion. Another handy feature offered by most languages with objects and classes is the ability for one method body to invoke another method of the same object via a special variable called self or, in some langauges, this. The special behavior of self is that it is late-bound, allowing a method defined in one class to invoke another method that is defined later, in some subclass of the first.
> 
> *Types and Programming Lanauges, Benjamin C. Pierce, 2002, MIT Press, pg 227.*

Generally, I am not too keen to proactively call Erlang an OO language. Thats because it is very different from the many mainstream OO languages, and it is more likely to confuse than enlighten. However, I do believe that Erlang does promote many of the objectives of OO quite well. So I was particularly interested in threading through the logic of why it might be interpreted as being non-OO.

I would as an aside want to show some simplistic erlang code which can serve as a basis for discussion further down. The code below is that for a simple ATM which allows a user to create or delete an account, deposit or withdraw monies into or from the account, and check the account balance. To demonstrate a particular nuance it further supports two types of accounts - checking and savings. The only distinction between the two is that this particular implementation of a savings account will not allow any withdrawals if the subsequent account balance were to be less than 100 units of currency.

The three modules are:

** ATM **
*atm.erl*
``` erlang
-module(atm).
%% account life cycle
-export([open/2,close/1]).
%% balance check
-export([balance/1]).
%% account modification
-export([deposit/2,withdraw/2]).

open(checking,Id) ->
    Account = spawn(checking,init,[Id]),
    register(Id,Account);
open(savings,Id) ->
    Account = spawn(savings,init,[Id]),
    register(Id,Account);
open(_,_) ->
    {error, invalid_account_type}.

close(Id) -> dispatch(Id, close).
deposit (Id, Amount) -> dispatch(Id,{deposit  , Amount}).
withdraw(Id, Amount) -> dispatch(Id,{withdraw, Amount}).
balance(Id) -> dispatch(Id, balance).

dispatch(Id,Data) ->
    case whereis(Id) of
    undefined ->
        {error, no_such_account_exists, Id};
    Account -> 
        Account ! Data
    end.
```

** Checking Accout **
*checking.erl*
``` erlang
-module(checking).
-export([init/1]).

init(Id) ->
    loop(Id, 0.0).

loop(Id,Balance) ->
    receive
    {Activity, Arg} -> 
        NewBalance = handle_message(Activity,Balance, Arg),
        io:format("Id: ~w Activity: ~w, Arg:~w, NewBalance:~w~n",[Id,Activity,Arg,NewBalance]),
        loop(Id,NewBalance);
    close -> 
        io:format("Account ~w now closed~n",[Id]),
        ok;
    Activity -> 
        NewBalance = handle_message(Activity, Balance),
        io:format("Id: ~w Activity: ~w, Balance:~w~n",[Id,Activity,NewBalance]),
        loop(Id,NewBalance)
    end.

handle_message(deposit, Balance, Amount) ->
    Balance + Amount;

handle_message(withdraw, Balance, Amount) ->
    if  Balance >= Amount ->
        Balance - Amount;
    true ->
        io:format("Insufficient Funds~n",[]),
        Balance
    end.

handle_message(balance, Balance) ->
    Balance;

handle_message(Activity, Balance) ->
    io:format("Unknown message. Ignoring ~w~n",[Activity]),
    Balance.
```

** Savings Account **
*savings.erl*
``` erlang
-module(savings).
-export([init/1]).

init(Id) ->
    loop(Id, 0.0).

loop(Id,Balance) ->
    receive
    {Activity, Arg} -> 
        NewBalance = handle_message(Activity,Balance, Arg),
        io:format("Id: ~w Activity: ~w, Arg:~w, NewBalance:~w~n",[Id,Activity,Arg,NewBalance]),
        loop(Id,NewBalance);
    close -> 
        io:format("Account ~w now closed~n",[Id]),
        ok;
    Activity -> 
        NewBalance = handle_message(Activity, Balance),
        io:format("Id: ~w Activity: ~w, Balance:~w~n",[Id,Activity,NewBalance]),
        loop(Id,NewBalance)
    end.

handle_message(deposit, Balance, Amount) ->
    Balance + Amount;
handle_message(withdraw, Balance, Amount) ->
    if  Balance >= (Amount + 100) ->
        Balance - Amount;
    true ->
        io:format("Insufficient Funds~n",[]),
        Balance
    end.

handle_message(balance, Balance) ->
    Balance;
handle_message(Activity, Balance) ->
    io:format("Unknown message. Ignoring ~w~n",[Activity]),
    Balance.
```

If you look at the code, there are no classes. Yes, you have three modules *atm*, *checking*, and *savings*, but these are essentially namespaces. They do not have any member variables or state. They are similar to files in the C programming language containing just functions. To re-emphasise, when I call a method like *atm:balance(account1)*, *atm* is the namespace, and *balance* is the function in that name space. Just a simple function call. No sign of objects yet.

Something interesting does happen however inside the *atm:open* function. It internally calls a erlang built in function called *spawn*. The return value of the function call is a process identifier. And that points to an erlang construct called the process. The closest C++/Java/Python equivalent for the same would be a thread. And processes can encapsulate state. Yet they do so differently. In case of C++/Java/Python, the state is maintained by instances of classes, and can be concurrently acted upon by multiple threads. In case of Erlang, the state is maintained by the process. And this state is never directly shared between processes. Each process has its own state and can send messages to other processes. (You just saw another aspect of the OO definition come in, ie. message passing). 

And where exactly is the state maintained ? Two places. Each process has a process dictionary. But that is substantially frowned upon so we shall pretty much ignore it. Each process is typically running one function in a tail recursive loop. And the stack of the function is the state. Note that the process is neither tied to a particular function or even a module. It could switch from one function to another to yet another and back, each such function having a message handler. The stack of each such function is the state at that point in time. So the nature of the state could itself theoretically vary within the process.

To do a quick recap, we have modules which are namespaces. These modules have functions. Are these functions then similar to methods on classes in case of C++/Java/Python ? Not at all. They are just functions (perhaps a close equivalent in java could be a static method). But objects are supposed to interact with each other through messages - these messages conventionally presumed to be methods. In Erlang, processes interact with each other not by calling functions of the other process (if you have been following carefully, functions do not belong to processes, they belong to modules). Instead they implement messaging between the objects by .... **passing messages**. Those are the only handles processes have to interact with each other. 

In the above example if *spawn* created a process which had encapsulated state, the message passing is demonstrated within the *atm:dispatch* function. For each account, a new process is created, and registered using the account id. The dispatch function looks up the process corresponding to the relevant account and sends it message. The account process on the other hand receives the message within the receive block inside one of the two *loop()* methods.

And why did I define two different modules for checking and savings accounts? To demonstrate that the features implemented via late binding in case of conventional OO languages are just as possible to implement in erlang as well. Except that instead of the dynamism being delivered via types, it is made possible using different function handler. Any process can switch between multiple function handlers and in effect it has an ability to switch behaviours. 

Now let us run through the checklist of OO characteristics Dr. Alan Kay suggested


* *messaging* : Check. Messaging is done using an inherent construct called messages, not a proxy called functions
* *local retention and protection and hiding of state-process* : Check. Processes hide state. And in fact cannot even share state.
* *extreme late-binding* : Check Again. Many times. In classic OO instances cannot switch types. Here processes can perform similar feats by switching into different function handlers. It is absolute late late late binding. Much later than conventional OO. The processes are registered into a registry. They are looked up from the registry. Then a message is sent to them. The messages are not bound at compile time. They are not even bound at dispatch time. They are actually bound in a pattern matching block inside the handler. Again in terms of late binding this is much more late binding than what open recursion stipulates needs to happen. 

If you noticed, erlang does have a different model where open recursion using *this* or *self* would be completely nonsensical. Because *this.method* has no meaning, since functions belong to namespaces, and *this* or *self* would refer to a process. I believe that at least in terms of intent, Erlang meets and surpasses all other languages I am aware of with respect to Dr. Kay's definition. It also leads me to believe that therefore open recursion could not be a necessary criteria for deciding if an language is OO. Since it simply is not inclusive enough to be applied to languages like erlang.

*Blog Post Todos / Pending items*

* Sample session with the erlang code
* Need to explore the lack of definition of messaging as applied in context of OO
* Need to further elaborate on Kay's references to Smalltalk/Lisp messaging
* Need to find a way to make this post a lot smaller. 


